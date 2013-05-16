Silex Notes
===========

Some notes on getting started with the Silex PHP microframework.

# Install with composer

## Create composer.json

    {
        "require": {
            "silex/silex": "1.0.*@dev"
        },
        "require-dev": {
            "symfony/browser-kit": ">=2.1,<2.3-dev"
            , "phpunit/phpunit": "~3.7"
        }
    }

## Run `composer install --dev` (the `--dev` causes the `require-dev` packages to be installed as well.)
  If you don't already have composer, install it with ...

# Create a hello world

## Create app.php

    <?php

    require_once __DIR__ . '/vendor/autoload.php'; 

    $app = new Silex\Application();

    $app->get('/hello/{name}', function ($name) use ($app) {
        return '<h1>Hello, ' . $app->escape($name) . '</h1>';
    });)

    return $app;

## Create web/index.php

    <?php

    $app = require __DIR__ . '/../app.php';

    $app->run();

## Configure the web server. Ex. for Apache:

web/.htaccess:

    FallbackResource /index.php

Apache conf file:

# Configure testing

## Install PHPUnit. Add the following to composer, then run `composer update --dev`:

    {
        "require-dev": {
            "phpunit/phpunit": "~3.7"
            , "symfony/browser-kit": ">=2.1,<2.3-dev"
            , "symfony/css-selector": ">=2.1,<2.3-dev"
        }
    }

## Create `phpunit.xml`

    <?xml version="1.0" encoding="UTF-8"?>
    <phpunit backupGlobals="false"
             backupStaticAttributes="false"
             colors="true"
             convertErrorsToExceptions="true"
             convertNoticesToExceptions="true"
             convertWarningsToExceptions="true"
             processIsolation="false"
             stopOnFailure="false"
             syntaxCheck="false"
             bootstrap="./tests/bootstrap.php"
            >
        <testsuites>
            <testsuite name="MyApp Test Suite">
                <directory>./tests/</directory>
            </testsuite>
        </testsuites>

        <php>
            <env name="env" value="test" />
        </php>
    </phpunit>

## Create `tests/bootstrap.php`:

    <?php

    $loader = require_once __DIR__ . '/../vendor/autoload.php';

    $loader->add('MyApp\Test', __DIR__);

## Create `tests/MyApp/Test/WebTestCase.php`:

    <?php

    namespace MyApp\Test;

    use Silex\WebTestCase as BaseWebTestCase;

    class WebTestCase extends BaseWebTestCase
    {
        public function createApplication()
        {
            // Don't use require_once, because this gets executed before every test.
            $app = require __DIR__ . '/../../../app.php';

            // Throw errors as raw exceptions instead of HTML pages.
            $app['debug'] = true;
            $app['exception_handler']->disable();

            // If the app uses sessions, uncomment the following to simulate sessions.
            // $app['session.test'] = true;

            return $app;
        }
    }

## Create `tests/MyApp/Test/HelloTest.php`:

    <?php

    namespace MyApp\Test;

    class HelloTest extends WebTestCase
    {
        public function testHelloName()
        {
            // The client represents a browser.
            $client = $this->createClient();

            // The crawler allows you to parse the contents of the page.
            $crawler = $client->request('GET', '/hello/joe');

            // Check that the HTTP status code was 200 OK.
            $this->assertTrue($client->getResponse()->isOk());

            // Inspect the page DOM with CSS selectors.
            $this->assertCount(1, $crawler->filter('h1:contains("joe")'));

            // Alternately, just parse the raw page content with a regular expression.
            $this->assertRegExp('/joe/', $client->getResponse()->getContent());

        }

    }

You can also access the Application through `$this->app`.

## Run `phpunit` from the application root directory.

# Services

## Dependency injection.

Pass in dependencies instead of instantiating them within the class.

Uses Pimple, a super simple dependency injection container that acts in many ways like a simple PHP array full of closures.

## Creating a service

### Create a class for the "hello" service:

`src/MyApp/Hello.php`:

    <?php

    namespace MyApp;

    class Hello
    {
        protected $end_punctuation;

        public function __construct($end_punctuation)
        {
            $this->end_punctuation = $end_punctuation;
        }

        public function greeting($name)
        {
            return 'Hello, ' . $name . $this->end_punctuation;
        }
    }

### Set up autoloading by adding to composer.json:

    "autoload": {
        "psr-0": {
            "MyApp": "src/"
        }
    }

Then run `composer dump-autoload`


### Define the service in app.php:

    // Set parameters as keys on the app container.
    $app['hello.end_punctuation'] = '!';

    // Define services with closures, which are invoked when first accessed (i.e. the services are lazy loaded).
    // Optionally use $app->share() to share the same instance for all accesses.
    $app['hello'] = $app->share(function($app) { return new MyApp\Hello($app['hello.end_punctuation']); });

    // Access services and parameters by accessing the app keys.
    $app->get('/hello/{name}', function ($name) use ($app) {
        return '<h1>' . $app['hello']->greeting($name) .'</h1>';
    });

# Service providers

## Configuration service provider

* Add to composer.json
* Add config/config.php (common), plus environment specific: dev,php, prod.php, test.php
* Add to app.php

## Logging

composer.json:
Could also use monolog/monolog, but symfony/monolog-bridge installs monolog as the default logger, so it will be used automatically by various other services.

    "require": {
        "symfony/monolog-bridge": ">=2.1,<2.3-dev"
    }

app.php:

    $app->register(new Provider\MonologServiceProvider(), array(
        'monolog.logfile' => __DIR__ . '/' . $env . '.log',
    ));

config/dev.php:

    'monolog.loglevel' => Monolog\Logger::DEBUG,

Use: `$app['logger']->debug('Hello.');`

## Doctrine DBAL

### Add to composer.json

Include symfony/doctrine-bridge to get support for query logging (if monolog-bridge is installed and monolog.level is set to Logger::DEBUG).

    "require": {
        "doctrine/dbal": "~2.2"
        , "symfony/doctrine-bridge": "~2.2"
    }

### Add to config files

If using the config provider. Otherwise, pass as an argument when registering the service (see below).

config/config.php:

    'db.options' => array(
        'driver'   => 'pdo_mysql',
        'dbname' => 'my_app',
        'host' => 'localhost',
        'user' => '',
        'password' => '',
    ),

Override your environment-specific config in config/dev.php:

    'db.options' => array(
        'user' => 'my_app_user',
        'password' => 'myDbPassword',
    ),

### Add to app.php

    $app->register(new Provider\DoctrineServiceProvider());

# Error handling

# Middleware

# MVC

# JSON API server

# User authentication and access control

# OAuth2

