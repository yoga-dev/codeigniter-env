# Codeigniter env

[![Codeigniter](https://img.shields.io/badge/Codeigniter-v3.1+-orange.svg)](http://codeigniter.com/)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/c6a897f69d6c4929bdc5220555576a14)](https://www.codacy.com/manual/yoga-dev/codeigniter-env)
[![StyleCI](https://github.styleci.io/repos/213576220/shield?branch=master)](https://github.styleci.io/repos/213576220)

Simple env setup for your codeigniter application with single or multiple _.env_ configuration. Configure your application based on your need.

Load environment variables from `.env` for your application using `getenv()` or `$_ENV`.

## Installation

1.  Install Composer

```sh
$ curl -s http://getcomposer.org/installer | php
```

2.  copy the `.env.example` and `.htaccess` from method1 or method2 in repo and paste it to your CodeIgniter root.
3.  Install package on root directory

```sh
$ composer require symfony/dotenv
```

## Configuration

1.  Enable your Composer Autoload and Hooks: `application/config/config.php`

`$config['enable_hooks'] = FALSE;` to `$config['enable_hooks'] = TRUE;`

`$config['composer_autoload'] = FALSE;` to `$config['composer_autoload'] = 'vendor/autoload.php';`

2.  Add this code to your application hooks: `application/config/hooks.php`

### Method 1 (Single env)

```php
$hook['pre_system'] = function () {
    $dotenv = new Symfony\Component\Dotenv\Dotenv();
    $dotenv->load(__DIR__.'/../../.env');
};
```
### Method 2 (Multiple env)

```php
$hook['pre_system'] = function () {
    $dotenv = new Symfony\Component\Dotenv\Dotenv();
    $dotenv->load(__DIR__.'/../../.env.'.getenv('CI_ENV'));
};
```

3.  Create your _.env_ files

### Using Method 1(Single env)

```sh
$ cp .env.example .env
```

> Define your application environment `CI_ENV` variable in _.env_ only for this method.

### Using Method 2 (Multiple env) 
#### Predefined Environments (development, testing, production)

```sh
$ cp .env.example .env.development
```

> Use these environment names only. For additional environments add a switch case in `index.php` Line No. : 66
> Set the environment in `.htaccess` file. Line No. : 3 & 12. If environment is not set in `.htaccess` file the application won\'t run.

```apacheconf
<IfModule mod_env.c>
    #Set Environment(development,testing,production)
    SetEnv CI_ENV development
</IfModule>
<IfModule mod_rewrite.c>
    #if mod_env module not enabled in server
    #Set Environment(development,testing,production)
    RewriteRule ^ - [E=CI_ENV:development]
</IfModule>
```

## Usage Example

### Database Configuration

1.  Edit the `application/config/database.php`
2.  Replace this code:

```php
$db['default'] = [
	'dsn'	=> '',
	'hostname' => 'localhost',
	'username' => '',
	'password' => '',
	'database' => '',
	'dbdriver' => 'mysqli',
	'dbprefix' => '',
	'pconnect' => FALSE,
	'db_debug' => (ENVIRONMENT !== 'production'),
	'cache_on' => FALSE,
	'cachedir' => '',
	'char_set' => 'utf8',
	'dbcollat' => 'utf8_general_ci',
	'swap_pre' => '',
	'encrypt' => FALSE,
	'compress' => FALSE,
	'stricton' => FALSE,
	'failover' => [],
	'save_queries' => TRUE

];
```

to

```php
$db['default'] = [
    'dsn'          => '',
    'hostname'     => getenv('DB_HOSTNAME'),
    'username'     => getenv('DB_USERNAME'),
    'password'     => getenv('DB_PASSWORD'),
    'database'     => getenv('DB_DATABASE'),
    'dbdriver'     => 'mysqli',
    'dbprefix'     => getenv('DB_PREFIX'),
    'pconnect'     => false,
    'db_debug'     => (ENVIRONMENT !== 'production'),
    'cache_on'     => false,
    'cachedir'     => '',
    'char_set'     => getenv('DB_CHAT_SET'),
    'dbcollat'     => getenv('DB_COLLATION'),
    'swap_pre'     => '',
    'encrypt'      => false,
    'compress'     => false,
    'stricton'     => false,
    'failover'     => [],
    'save_queries' => true,

];
```