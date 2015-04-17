Rollbar Yii Component
=====================
[![GitHub release](https://img.shields.io/github/release/baibaratsky/yii-rollbar.svg)](https://github.com/baibaratsky/yii-rollbar/releases)
[![Dependency Status](https://www.versioneye.com/user/projects/55315cb410e7141211000fc8/badge.svg?style=flat)](https://www.versioneye.com/user/projects/55315cb410e7141211000fc8)
[![Packagist](https://img.shields.io/packagist/v/baibaratsky/yii-rollbar.svg)](https://packagist.org/packages/baibaratsky/yii-rollbar)
[![Packagist](https://img.shields.io/packagist/dt/baibaratsky/yii-rollbar.svg)](https://packagist.org/packages/baibaratsky/yii-rollbar)

Rollbar Yii Component is the way to integrate [Rollbar](http://rollbar.com/) service with your Yii application.
Rollbar aggregates and analyzes your application errors and deploys.

The code of this project has been forked from
[Ratchetio Component](https://github.com/yiiext/ratchetio-component/tree/5e09ebc042d3c6ec0f69a208395831f05520f88f).

Installation
------------

1. a. Easy way is to use [Composer](http://getcomposer.org/). Just run `composer install`.

   OR

   b. Download [rollbar.php](https://raw.github.com/rollbar/rollbar-php/master/rollbar.php)
   and put it somewhere you can access, then add the following code at your application's entry point:
   ```php
   require_once 'rollbar.php';
   ```

2. Add `rollbar` component to the `main.php` config:
    ```php
    // ...
    'components' => array(
        // ...
        'rollbar' => array(
            'class' => 'application.vendor.baibaratsky.yii-rollbar.RollbarComponent', // adjust path if needed
            'accessToken' => 'your_serverside_rollbar_token',
        ),
    ),
    ```

3. Adjust `main.php` config to preload the component:
    ```php
    'preload' => array('log', 'rollbar'),
    ```

4. Set `RollbarErrorHandler` as error handler:
    ```php
    'components' => array(
        // ...
        'errorHandler' => array(
            'class' => 'application.vendor.baibaratsky.yii-rollbar.RollbarErrorHandler',
            // ...
        ),
    ),
    ```

    You can also pass some additional rollbar options in the component config:
    `environment`, `branch`, `maxErrno`, `baseApiUrl`, etc.

    A good idea is to specify `environment` as:

    ```php
    'environment' => isset($_SERVER['HTTP_HOST']) ? $_SERVER['HTTP_HOST'] : 'cli_' . php_uname('n'),
    ```

    You can specify alias of your project root directory for linking stack traces (`application` by default):
    ```php
    'rootAlias' => 'root',
    ```


Rollbar Log Route
-----------------
You may want to collect your logs produced by `Yii::log()` in Rollbar. Put the following code in your config and enjoy:
```php
'components' => array(
    // ...
    'log' => array(
        // ...
        'routes' => array(
            array(
                'class' => 'application.vendor.baibaratsky.yii-rollbar.RollbarLogRoute',
                'levels' => 'error, warning, info',

                // You may specify the name of the Rollbar Yii Component ('rollbar' by default)
                'rollbarComponentName' => 'rollbar',
            ),
        ),
    ),
),
```
