    mysqldump -u root -p laravel lnovel_line --skip-triggers --no-create-db --no-create-info --compact --where 'id > 50000 and id <= 75000'> t_lnovel_line_75000.sql
