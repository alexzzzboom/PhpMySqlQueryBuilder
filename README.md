# MySQL Query Builder

Simple Query Builer to fetch MySQL Queries into PHP Objects

#### Table of contencts
**[Set up](#set-up)**  
**[Info](#info)**  
**[Usage examples](#usage-expamples)**
- **[Select](#select)**
- **[Select Where](#select-where)**
- **[Update Where](#update-where)**
- **[Insert Into](#insert-into)**
- **[Delete Where](#delete-where)**

<a name="set-up"></a>
## Set Up

```php
require 'vendor/autoload.php';

$database = new \Kantas_net\Database\QueryBuilder(\Kantas_net\Database\Connection::make(require 'Data/configDB.php'));
```

or

```php
require 'vendor/autoload.php';

use Kantas_net\Database\{QueryBuilder,Connection} ;

$database = new QueryBuilder(Connection::make(require 'Data/configDB.php'));
```

- `QueryBuilder` class constuctor takes as an agrument a `PDO` object

- `Connection::make` static method takes as an agrument an associative array of this format:
```php
[
        'dbname' =>'myDB',
        'username' => 'root',
        'password' => '123',
        'host' => '127.0.0.1',
        'options' =>[
            PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION
        ]
]
```
and returns a `PDO`

<a name="info"></a>
## Info

I created this Query Builder for learning purposes and to have it as bootstrap in my applictions

There are a lot of reacher implementations for SQL Query Builder with PHP but if you like mine, you can use it free under [MIT licence](LICENCE.txt).

<a name="usage-expamples"></a>
## Usage examples

Let's say we've got the following table

<a name="user-table"></a>
##### user
| id | employee_id | user_type | username | password | 
| -: | -: | - | - | - | 
| 1 | \N | SUPER ADMIN | admin | admin | 
| 2 | 1 | NORMAL | robin | robin | 
| 3 | 2 | ADMIN | taylor | taylor |

Created by the following commands
```sql
CREATE TABLE IF NOT EXISTS `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `employee_id` int(11) DEFAULT NULL,
  `user_type` varchar(50) DEFAULT NULL,
  `username` varchar(100) NOT NULL,
  `password` varchar(100) NOT NULL,
  PRIMARY KEY (`id`)
)

INSERT INTO `user` (`id`, `employee_id`, `user_type`, `username`, `password`) VALUES
	(1, NULL, 'SUPER ADMIN', 'admin', 'admin'),
	(2, 1, 'NORMAL', 'robin', 'robin'),
	(3, 2, 'ADMIN', 'taylor', 'taylor');
```
<a name="select"></a>
### Select 

```sql
$database = new QueryBuilder(Connection::make(require 'Data/configDB.php'));

$users = $database->select('user');
```

if `var_dump($users)` we take the following output

```
(array) [Number of elements: 3]
0: 
(object) stdClass [Object ID: 5][Number of Properties: 5]
id: (string) 1
employee_id: (null) NULL
user_type: (string) SUPER ADMIN
username: (string) admin
password: (string) admin
1: 
(object) stdClass [Object ID: 6][Number of Properties: 5]
id: (string) 2
employee_id: (string) 1
user_type: (string) NORMAL
username: (string) robin
password: (string) robin
2: 
(object) stdClass [Object ID: 7][Number of Properties: 5]
id: (string) 3
employee_id: (string) 2
user_type: (string) ADMIN
username: (string) taylor
password: (string) taylor
```

We can use the `$users` variable like this

```php
echo "Username of user with id {$users[0]->id} is {$users[0]->username}" ;
```

and take the following output

```
Username of user with id 1 is admin
```

<a name="select-where"></a>
### Select Where

The `user` table look like this [above](#user-table)

```php
$database = new QueryBuilder(Connection::make(require 'Data/configDB.php'));

$users = $database->selectWhere('user','id','2');
```

After the execution of commands above `var_dump($users)` output is

```
(array) [Number of elements: 1] 0: 
  (object) stdClass [Object ID: 5][Number of Properties: 5]
    id: (string) 2
    employee_id: (string) 1
    user_type: (string) NORMAL
    username: (string) robin
    password: (string) robin
```

<a name="update-where"></a>
### Update Where

The `user` table look like this [above](#user-table)

```php
$database = new QueryBuilder(Connection::make(require 'Data/configDB.php'));

$setData = 

$database->updateWhere('user','id','2');
```

After the above command execute `user` table looks like this below

##### user
| id | employee_id | user_type | username | password | 
| -: | -: | - | - | - | 
| 1 | \N | SUPER ADMIN | admin | admin | 
| 2 | 1 | NORMAL | robin | robin | 
| 3 | 2 | ADMIN | Alex | $Alex$ | 

<a name="insert-into"></a>
### Insert Into

 The `user` table look like this [above](#user-table)

 We excecute

 ```php
$database = new QueryBuilder(Connection::make(require 'Data/configDB.php'));

$insertData = ["employee_id" => '3','user_type' => 'ADMIN', 'username' => 'vivian', 'password' => 'vivian'];

$database->insertInto('user',$insertData);
 ```

Our table now look like this

<a name="user-table-1"></a>
##### user
| id | employee_id | user_type | username | password | 
| -: | -: | - | - | - | 
| 1 | \N | SUPER ADMIN | admin | admin | 
| 2 | 1 | NORMAL | robin | robin | 
| 3 | 2 | ADMIN | taylor | taylor | 
| 4 | 3 | ADMIN | vivian | vivian | 

<a name="delete-where"></a>
### Delete Where

The `user` table look like this [above](#user-table-1)

```php
$database = new QueryBuilder(Connection::make(require 'Data/configDB.php'));

$database->deleteWhere('user','id','4');
```

After the above command execute `user` table looks like this below

##### user
| id | employee_id | user_type | username | password | 
| -: | -: | - | - | - | 
| 1 | \N | SUPER ADMIN | admin | admin | 
| 2 | 1 | NORMAL | robin | robin | 
| 3 | 2 | ADMIN | taylor | taylor | 
