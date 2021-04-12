> composer require imsglobal/lti

edit /app/Util/IMSOAuthDataStore.php<br>

```php
<?php
/**
 * Created by PhpStorm.
 * User: mmestre
 * Date: 4/17/17
 * Time: 4:14 PM
 *
 * from IMSGlobal
 *
 */

namespace App\Util;

use IMSGlobal\LTI\OAuth\OAuthDataStore;
use IMSGlobal\LTI\OAuth\OAuthConsumer;
use IMSGlobal\LTI\OAuth\OAuthToken;

class IMSOAuthDataStore extends OAuthDataStore
{
    private $consumer_key = NULL;
    private $consumer_secret = NULL;

    public function __construct($consumer_key, $consumer_secret)
    {

        $this->consumer_key = $consumer_key;
        $this->consumer_secret = $consumer_secret;

    }

    function lookup_consumer($consumer_key)
    {

        return new OAuthConsumer($this->consumer_key, $this->consumer_secret);

    }

    function lookup_token($consumer, $token_type, $token)
    {

        return new OAuthToken($consumer, '');

    }

    function lookup_nonce($consumer, $token, $nonce, $timestamp)
    {

        return FALSE;  // If a persistent store is available nonce values should be retained for a period and checked here

    }

    function new_request_token($consumer, $callback = null)
    {

        return NULL;

    }

    function new_access_token($token, $consumer, $verifier = null)
    {

        return NULL;

    }

}
```

edit /app/Http/Controllers/Lti_launch.php<br>

```php
<?php

namespace App\Http\Controllers;

use App\Util\IMSOAuthDataStore;
use Illuminate\Support\Facades\Input;
use IMSGlobal\LTI\OAuth\OAuthServer;
use IMSGlobal\LTI\OAuth\OAuthSignatureMethod_HMAC_SHA1;
use IMSGlobal\LTI\OAuth\OAuthRequest;
use IMSGlobal\LTI\OAuth\OAuthException;

function pre($stuff)
{
    echo '<pre>';
    print_r($stuff);
    echo '</pre>';
}

class Lti_launch extends Controller
{
    public $message = false;
    public $ok = true;

    // constructor including secret validation via OAuth
    function __construct()
    {
        $tool_consumer_secrets['key'] = 'secret';

        // Check it is a POST request
        $this->ok = $this->ok && $_SERVER['REQUEST_METHOD'] === 'POST';

        // Check the LTI message type
        $this->ok = $this->ok && isset($_POST['lti_message_type']) && ($_POST['lti_message_type'] === 'basic-lti-launch-request');

        // Check the LTI version
        $this->ok = $this->ok && isset($_POST['lti_version']) && ($_POST['lti_version'] === 'LTI-1p0');

        // Check a consumer key exists
        $this->ok = $this->ok && !empty($_POST['oauth_consumer_key']);

        // Check a resource link ID exists
        $this->ok = $this->ok && !empty($_POST['resource_link_id']);

        // If all checks have passed, validate signature with OAuth
        if ($this->ok) {
            try {
                $store = new IMSOAuthDataStore($_POST['oauth_consumer_key'], $tool_consumer_secrets['key']);
                $server = new OAuthServer($store);
                $method = new OAuthSignatureMethod_HMAC_SHA1();
                $server->add_signature_method($method);
                $request = OAuthRequest::from_request();
                $server->verify_request($request);
                $this->ok = true;
            } catch (OAuthException $e) {
                $this->ok = false;
                $this->message = $e->getMessage();
            }
        }
    }

    function launch()
    {
        // if lti is valid, launch
        if ($this->ok) {
            echo 'LTI launched successfully';
            echo '<br />';
            pre($_POST);
        } else {
            die ('LTI launch failed. Contact itg@brown.edu for assistance');
        }
    }
}
```

delete /routes/api.php all routes<br

edit /routes/web.php route<br>

    Route::post('/lti', 'Lti_launch@launch');

edit /app/Http/Middleware/VerifyCsrfToken.php<br>

    protected $except = [
        //
        'lti'
    ];

> cd /storage<br>
> chmod -R 777 *<br>
> php artisan view:clear;<br>
> php artisan route:clear;<br>
> php artisan cache:clear;<br>
> php artisan clear-compiled;<br>
> php artisan optimize;<br>
> php artisan config:cache;<br>
> php artisan route:cache;<br>

edit /public/test.php<br>

```php
<?php

$launch_url = "http://dr-chuck.com/ims/php-simple/tool.php";
$launch_url = "http://192.168.0.14/lti";
$consumer_key = "12345";
$secret = "secret";

$launch_data = array(
    "user_id" => "292832126",
    "roles" => "Instructor",
    "resource_link_id" => "120988f929-274612",
    "lis_person_name_full" => "Johnny Depp",

    "lis_person_contact_email_primary" => "user@school.edu",
    "lis_person_sourced_id" => "school.edu:user",

    "context_id" => "456434513",
    "context_title" => "Design of Personal Environments",
    "context_label" => "SI182",

    "tool_consumer_instance_guid" => "lmsng.school.edu",
    "tool_consumer_instance_description" => "University of School (LMSng)"
);
#
# END OF CONFIGURATION SECTION
# ------------------------------
$launch_data["lti_version"] = "LTI-1p0";
$launch_data["lti_message_type"] = "basic-lti-launch-request";

# Basic LTI uses OAuth to sign requests
# OAuth Core 1.0 spec: http://oauth.net/core/1.0/
$launch_data["oauth_callback"] = "about:blank";
$launch_data["oauth_consumer_key"] = $consumer_key;
$launch_data["oauth_version"] = "1.0";
$launch_data["oauth_nonce"] = uniqid('', true);
$launch_data["oauth_timestamp"] = time();
$launch_data["oauth_signature_method"] = "HMAC-SHA1";

# In OAuth, request parameters must be sorted by name
$launch_data_keys = array_keys($launch_data);
sort($launch_data_keys);
$launch_params = array();

foreach ($launch_data_keys as $key) {
    array_push($launch_params, $key . "=" . rawurlencode($launch_data[$key]));
}

$base_string = "POST&" . urlencode($launch_url) . "&" . rawurlencode(implode("&", $launch_params));
$secret = urlencode($secret) . "&";
$signature = base64_encode(hash_hmac("sha1", $base_string, $secret, true));

?>

<html>
<head></head>
<!-- <body onload="document.ltiLaunchForm.submit();"> -->
<body>
<form id="ltiLaunchForm" name="ltiLaunchForm" method="POST" action="<?php printf($launch_url); ?>">
    <?php foreach ($launch_data as $k => $v) { ?>
        <input type="hidden" name="<?php echo $k ?>" value="<?php echo $v ?>">
    <?php } ?>
    <input type="hidden" name="oauth_signature" value="<?php echo $signature ?>">
    <button type="submit">Launch</button>
</form>
<body>
</html>
```

[127.0.0.1/test.php](http://127.0.0.1/test.php)

### REFERECE

[LTI-Tool-Provider-Library-PHP](https://github.com/IMSGlobal/LTI-Tool-Provider-Library-PHP/wiki/Installation)<br>
[LTI Integration + Caliper MediaEvent Metric Profile](https://www.imsglobal.org/ims-app-note-lti-integration-caliper-mediaevent-metric-profile)<br>
[Recipe for Making LTI® 1 Tool Providers](https://www.imsglobal.org/recipe-making-lti-1-tool-providers)<br>
[Learning Tools Interoperability®: Sample Code](http://www.imsglobal.org/learning-tools-interoperability-sample-code)<br>
[IMS Global Learning Tools Interoperability™ Basic LTI Implementation Guide](https://www.imsglobal.org/specs/ltiv1p0/implementation-guide#toc-21)<br>
[laravel-lti](https://github.com/MarcLightning/laravel-lti-github)<br>
[Sample code for Basic LTI Consumer in PHP](https://gist.github.com/matthanger/1171921)<br>
