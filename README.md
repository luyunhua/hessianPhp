HessianPHP 2
============

HessianPHP 2 is a library that implements the Hessian binary web services protocol for PHP 5. 

The Hessian binary web service protocol makes web services usable without requiring a large framework, and without learning yet another alphabet soup of protocols. Because it is a binary protocol, it is well-suited to sending binary data without any need to extend the protocol with attachments.

Hessian was created by Caucho Technology in the Java programming language. This protocol was designed to be fast and simple to learn and use. It uses HTTP as transport by sending and receiving POST requests to remote services.

HessianPHP 2 is a complete rewrite of the original HessianPHP library published a few years ago to make it compatible with the newest versions of the protocol and PHP.

## Installation

    composer require tomato/hessian-php

## Features

- PHP 5 only
- Hessian protocol version 1 and 2 with autodetection
- Can create both clients and servers
- Support CURL and standard http stream wrappers


Quick Start
-----------

### Consuming a Hessian web service

To start consuming remote Hessian web services all you need to do is:

1. Create a HessianClient object passing the url of the service and additional options if required
2. Call methods

This is an example code that creates a proxy to a remote service, calls several methods and prints the results:

	$testUrl = 'http://localhost/mathService.php';
	$proxy = new HessianClient($testUrl);
	try {
	    echo $proxy->div(2, 5); 
	} catch (Exception $e) {
	   // ...handle error
	}

### Creating a Hessian web service

You have to create a script in the web server following this steps:

1. Create a HessianService wrapper object
2. Register a previously created or new object in the service constructur or by calling registerObject().
3. Execute the handle() method.

Thus, for example, if we want to publish a calculator service that is compatible with the previous example, we have to create a class something like this:

    class Math {
      function add($n1,$n2) {
        return $n1+$n2;
      }    
      function sub($n1,$n2) {
        return $n1-$n2;
      }    
      function mul($n1,$n2) {
        return $n1*$n2;
      }    
      function div($n1,$n2) {
        return $n1/$n2;
      }
    }

Then create the service wrapper and register a Math object to make it accesible through Hessian:

    $service = new HessianService(new Math());
    $service->handle();

That's it. Now we have successfully published a Hessian web service from a common PHP object. The url of the service is the same url of the script.

If you try to access the url using a web browser, you will get a 500 error because Hessian requires POST to operate.

### Status

This is a dev master version. It's compatible with PHP 5.3. Deployment requires a little over 100KB.

HessianPHP is licensed under the MIT license, so you can comfortably use it in commercial applications.