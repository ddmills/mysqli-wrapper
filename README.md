# mysqli-wrapper
A wrapper for mysqli that makes prepared statements easier to create, and results easier to parse.

## Setup
Fill in your database information in the Constants.php file:
```php
/* MYSQL DATABASE SERVER */
define('DB_SERVER', 'localhost');
/* MYSQL DATABASE USERNAME */
define('DB_USER', 'admin');
/* MYSQL DATABASE PASSWORD */
define('DB_PASS', 'secret');
/* MYSQL DATABASE NAME */
define('DB_NAME', 'my_db_name');
```

Include mywrap.php in your file
```php
include 'path/to/myswli-wrapper/mywrap.php'
```

## Open a connection
The default constructor will 
```php
$connection = new mywrap_con();
```
Alternatively, you can use specify a database to use:
```php
$connection = new mywrap_con('my_db_host', 'my_db_user', 'my_db_user_password', 'my_db_name');
```

## Run a statement
Run a simple statement with no arguments:
```php
$results = $connection->run('select * from songs order by tracknumber');
```
Loop through the results
```php
while ($row = $result->fetch()) {
  echo $row['artist'];
}
```


