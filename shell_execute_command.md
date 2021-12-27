> vim /root/cd.sh

    #!/bin/bash  
      
    show() {  
      echo "project 1. platform1 2. platform2 3. platform3 "  
      read -p 'project: ' project  
      
      if [ "$project" = 1 ]; then  
      cd /var/www/platform1
      elif [ "$project" = 2 ]; then 
      cd /var/www/platform2
      elif [ "$project" = 3 ]; then 
      cd /var/www/platform3
      else 
      show 
      fi
    }  
    show

> source cd.sh
