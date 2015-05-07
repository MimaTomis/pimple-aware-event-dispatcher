Pimple Aware Event Dispatcher
=============================

Added support [Pimple v3.0](http://pimple.sensiolabs.org/)

Installation
------------

```
composer.phar require "davedevelopment/pimple-aware-event-dispatcher:3.0.*"
```

Usage
-----

To use in a [Silex](http://silex.sensiolabs.org) application:

``` php
<?php

use PimpleAwareEventDispatcher\PimpleAwareEventDispatcher;
use Silex\Application;

$app = new Application;

// override the dispatcher
$app['dispatcher'] = $app['pimple_aware_dispatcher'] = $app->share(
    $app->extend('dispatcher', function($dispatcher) use ($app) {
        return new PimpleAwareEventDispatcher($dispatcher, $app);
    }
));

// define our application services
$app['some.service'] = $app->share(function() use ($app) {
    // let's assume this takes a bit of doing and/or is dependant on several other
    // services
    sleep(1);
    return new SomeService;
});

// add a listener, that will lazily fetch the service when needed
$app['pimple_aware_dispatcher']->addListenerService(
    "some.event",
    array("some.service", "serviceMethod")
);
```

