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
include 'path/to/mysqli-wrapper/mywrap.php'
```

## Open a connection
The default constructor will 
```php
$connection = new mywrap_con();
```

Alternatively, you can use a specific database by specifying the parameters:
```php
$connection = new mywrap_con('my_db_host', 'my_db_user', 'my_db_user_password', 'my_db_name');
```

## Run a statement
To run a MySQL query, use the ```run()``` method on the mywrap_con object. For example, to execute a simple statement with no arguments:
```php
$results = $connection->run('select * from songs order by tracknumber');
```

The ```$results``` object is a mywrap_result() object. This object provides several helper methods for handling the data that was retrieved. Typically, you will loop through the results in a while loop:
```php
while ($row = $result->fetch()) {
  // the data must be used within this loop when the fetch() method is used,
  // because each time the fetch() method is called, the previous $row will
  // get overwritten.
  echo $row['artist'];
}
```

Retreive the results as an array, one row at a time:
```php
while ($row = $results->fetch_array()) {
  echo $row['artist'];
}
```

Alternatively, or retrieve all of the results at once:
```php
$songs = $results->fetch_all_array();
```

### Prepared statements
It is recommended to use prepared statements when querying a database. This is made extremely easy with mysqli-wrapper.

```php
$artist  = 'Frank Sinatra';
$results = $con->run('select * from songs where artist = ?', 's', $artist);
```
In this example we are querying the database where ```artist = ? ```, this will get populated by ```$artist``` which must be a ```'s'```, which is short for ```string```.

In pure mysqli, this query would be a lot longer. It would look like this:
```php
$artist = 'Frank Sinatra';
if ($stmt = $mysqli->prepare('select * from songs where artist = ?')) {
  $stmt->bind_param('s', $artist);
  $stmt->execute();
  $stmt->bind_result($results);
  $stmt->fetch();
  printf($results); // $results will not be a consistant format
  $stmt->close();
}
```
As you can see, mysqli-wrapper removes a lot of boilerplate code.

