# Firebase PHP Client with curl and context support

Based on the [Firebase REST API](https://www.firebase.com/docs/rest-api.html).

Available on [Packagist](https://packagist.org/packages/aspertw/firebase-php).

### Adding Firebase PHP to your project using Composer

```bash
cd <your_project>
composer require aspertw/firebase-php dev-master
```

More info about Composer at [getcomposer.org](http://getcomposer.org).

### Forked from [ktamas77/firebase-php](https://github.com/ktamas77/firebase-php).

Original version require curl extension.

I made some change to support curl and stream context(using file_get_contents).

if curl extension loaded, using curl to send request.

if not, using file_get_contents and [stream contexts](http://php.net/manual/en/stream.contexts.php) to send.

because GAE (Google App Engine) does not support curl extension, so I fork this project.

↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓   ktamas77's document  ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓

### Example
```php
const DEFAULT_URL = 'https://kidsplace.firebaseio.com/';
const DEFAULT_TOKEN = 'MqL0c8tKCtheLSYcygYNtGhU8Z2hULOFs9OKPdEp';
const DEFAULT_PATH = '/firebase/example';

$firebase = new \Firebase\FirebaseLib(DEFAULT_URL, DEFAULT_TOKEN);

// --- storing an array ---
$test = array(
    "foo" => "bar",
    "i_love" => "lamp",
    "id" => 42
);
$dateTime = new DateTime();
$firebase->set(DEFAULT_PATH . '/' . $dateTime->format('c'), $test);

// --- storing a string ---
$firebase->set(DEFAULT_PATH . '/name/contact001', "John Doe");

// --- reading the stored string ---
$name = $firebase->get(DEFAULT_PATH . '/name/contact001');
```

### Supported Commands
```php
// -- Firebase API commands

$firebase->set($path, $value);   // stores data in Firebase
$value = $firebase->get($path);  // reads a value from Firebase
$firebase->delete($path);        // deletes value from Firebase
$firebase->update($path, $data); // updates data in Firebase
$firebase->push($path, $data);   // push data to Firebase

// -- Firebase PHP Library commands

$firebase->setToken($token);     // set up Firebase token
$firebase->setBaseURI($uri);     // set up Firebase base URI (root node)
$firebase->setTimeOut($seconds); // set up maximum timeout / request
```

Please refer to the [Firebase REST API documentation](https://www.firebase.com/docs/rest/api/) for further details.

### Firebase PHP Stub
A Firebase PHP Stub has been created to allow for integration with phpunit without actually interacting with FirebaseIO.

To use the firebaseLib and firebaseStub in your application and testing, you must pass in a firebase object to your application.

For example, if your current code is:

```php
public function setFirebaseValue($path, $value) {
  $firebase = new Firebase('https://radiant-fire-2427.firebaseio.com', 'czvEX8vMU8FZn4wYCvf466P3J6zH5ZlKQeuwxmEZ');
  $firebase->set($path, $value);
}
```

You will change it to be:

```php
public function setFirebaseValue($path, $value, $firebase) {
  $firebase->set($path, $value);
}
```

Then your phpunit tests will look like:

```php
<?php
  require_once '<path>/lib/firebaseInterface.php';
  require_once '<path>/lib/firebaseStub.php';

  class MyClass extends PHPUnit_Framework_TestCase
  {
    public function testSetFirebaseValue() {
      $myClass = new MyClass();
      $firebaseStub = new FirebaseStub($uri, $token);
      $myClass->setFirebaseValue($path, $value, $firebaseStub);
    }
  }
?>
```

### Unit Tests
All the unit tests are found in the "/tests" directory. Due to the usage of an interface, the tests must run in isolation.

The firebaseLib tests have inherent latency due to actual cURL calls to a live firebaseIO account. The firebaseLib tests can be executed by running the following command:

```bash
$ phpunit tests/firebaseTest.php
```

The FirebaseStub tests can be executed by running the following command:

```bash
$ phpunit tests/firebaseStubTest.php
```


#### The MIT License (MIT)
```
Copyright (c) 2012-2015 Tamas Kalman

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```
