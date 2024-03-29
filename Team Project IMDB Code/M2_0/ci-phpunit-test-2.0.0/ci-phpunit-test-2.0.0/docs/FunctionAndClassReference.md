# ci-phpunit-test for CodeIgniter 3.x

version: **v2.0.0** |
[v1.0.0](https://github.com/kenjis/ci-phpunit-test/blob/v1.0.0/docs/FunctionAndClassReference.md)

## Function/Class Reference

- [*function* `reset_instance()`](#function-reset_instance)
- [*function* `set_is_cli($return)`](#function-set_is_clireturn)
- [*function* `load_class_instance($classname, $instance)`](#function-load_class_instanceclassname-instance)
- [*class* TestCase](#class-testcase)
	- [`TestCase::resetInstance($use_my_controller = false)`](#testcaseresetinstanceuse_my_controller--false)
	- [`TestCase::request($method, $argv, $params = [])`](#testcaserequestmethod-argv-params--)
		- [`request->setHeader()`](#request-setheader)
		- [`request->setFiles($files)`](#request-setfilesfiles)
		- [`request->setCallable()`](#request-setcallable)
		- [`request->addCallable()`](#request-addcallable)
		- [`request->setCallablePreConstructor()`](#request-setcallablepreconstructor)
		- [`request->addCallablePreConstructor()`](#request-addcallablepreconstructor)
		- [`request->enableHooks()`](#request-enablehooks)
	- [`TestCase::ajaxRequest($method, $argv, $params = [])`](#testcaseajaxrequestmethod-argv-params--)
	- [`TestCase::assertResponseCode($code)`](#testcaseassertresponsecodecode)
	- [`TestCase::assertRedirect($uri, $code = null)`](#testcaseassertredirecturi-code--null)
	- [`TestCase::assertResponseHeader($name, $value)`](#testcaseassertresponseheadername-value)
	- [`TestCase::assertResponseCookie($name, $value, $allow_duplicate = false)`](#testcaseassertresponsecookiename-value-allow_duplicate--false)
	- [`TestCase::assertLogged($level, $message)`](#testcaseassertloggedlevel-message)
	- [`TestCase::getDouble($classname, $params, $constructor_params = false)`](#testcasegetdoubleclassname-params-constructor_params--false)
	- [`TestCase::verifyInvoked($mock, $method, $params)`](#testcaseverifyinvokedmock-method-params)
	- [`TestCase::verifyInvokedOnce($mock, $method, $params)`](#testcaseverifyinvokedoncemock-method-params)
	- [`TestCase::verifyInvokedMultipleTimes($mock, $method, $times, $params)`](#testcaseverifyinvokedmultipletimesmock-method-times-params)
	- [`TestCase::verifyNeverInvoked($mock, $method, $params)`](#testcaseverifyneverinvokedmock-method-params)
	- [`TestCase::warningOff()`](#testcasewarningoff)
	- [`TestCase::warningOn()`](#testcasewarningon)
- [*class* DbTestCase](#class-dbtestcase)
	- [`DbTestCase::seeInDatabase($table, $where)`](#dbtestcaseseeindatabasetable-where)
	- [`DbTestCase::dontSeeInDatabase($table, $where)`](#dbtestcasedontseeindatabasetable-where)
	- [`DbTestCase::seeNumRecords($expected, $table, $where = [])`](#dbtestcaseseenumrecordsexpected-table-where--)
	- [`DbTestCase::hasInDatabase($table, $data)`](#dbtestcasehasindatabasetable-data)
	- [`DbTestCase::grabFromDatabase($table, $column, $where)`](#dbtestcasegrabfromdatabasetable-column-where)
- [*class* UnitTestCase](#class-unittestcase)
	- [`UnitTestCase::newModel($classname)`](#unittestcasenewmodelclassname)
	- [`UnitTestCase::newLibrary($classname, $args)`](#unittestcasenewlibraryclassname-args)
	- [`UnitTestCase::newController($classname)`](#unittestcasenewcontrollerclassname)
- [*class* ReflectionHelper](#class-reflectionhelper)
	- [`ReflectionHelper::getPrivateProperty($obj, $property)`](#reflectionhelpergetprivatepropertyobj-property)
	- [`ReflectionHelper::setPrivateProperty($obj, $property, $value)`](#reflectionhelpersetprivatepropertyobj-property-value)
	- [`ReflectionHelper::getPrivateMethodInvoker($obj, $method)`](#reflectionhelpergetprivatemethodinvokerobj-method)
- [*class* MonkeyPatch](#class-monkeypatch)
	- [`MonkeyPatch::patchFunction($function, $return_value, $class_method)`](#monkeypatchpatchfunctionfunction-return_value-class_method)
	- [`MonkeyPatch::resetFunctions()`](#monkeypatchresetfunctions)
	- [`MonkeyPatch::patchMethod($classname, $params)`](#monkeypatchpatchmethodclassname-params)
	- [`MonkeyPatch::resetMethods()`](#monkeypatchresetmethods)
	- [`MonkeyPatch::patchConstant($constant, $value, $class_method)`](#monkeypatchpatchconstantconstant-value-class_method)
	- [`MonkeyPatch::resetConstants()`](#monkeypatchresetconstants)
	- [`MonkeyPatch::verifyInvoked($class_method, $params)`](#monkeypatchverifyinvokedclass_method-params)
	- [`MonkeyPatch::verifyInvokedOnce($class_method, $params)`](#monkeypatchverifyinvokedonceclass_method-params)
	- [`MonkeyPatch::verifyInvokedMultipleTimes($class_method, $times, $params)`](#monkeypatchverifyinvokedmultipletimesclass_method-times-params)
	- [`MonkeyPatch::verifyNeverInvoked($class_method, $params)`](#monkeypatchverifyneverinvokedclass_method-params)

### *function* `reset_instance()`

Resets CodeIgniter instance. You must create a new controller instance after calling this function.

~~~php
reset_instance();
$controller = new Welcome();
$this->CI =& get_instance();
~~~

Normally, you don't have to use this function. Use [`TestCase::resetInstance()`](#testcaseresetinstanceuse_my_controller--false) method instead.

**Note:** Before you create a new controller instance, `get_instance()` returns `CIPHPUnitTestNullCodeIgniter` object.

### *function* `set_is_cli($return)`

| param   | type | description         |
|---------|------|---------------------|
|`$return`| bool | return value to set |

Sets return value of `is_cli()` function.

~~~php
set_is_cli(FALSE);
~~~

### *function* `load_class_instance($classname, $instance)`

| param      | type   | description     |
|------------|--------|-----------------|
|`$classname`| string | class name      |
|`$instance` | object | object instance |

Injects an instance directly into `load_class()` function.

~~~php
$email = $this->getMockBuilder('CI_Email')
	->setMethods(['send'])
	->getMock();
$email->method('send')
	->willReturn(TRUE);
load_class_instance('email', $email);
~~~

### *class* TestCase

#### `TestCase::resetInstance($use_my_controller = false)`

Resets CodeIgniter instance and assign new CodeIgniter instance as `$this->CI`.

~~~php
public function setUp(): void
{
	$this->resetInstance();
	$this->CI->load->model('Category_model');
	$this->obj = $this->CI->Category_model;
}
~~~

If you want your `MY_Controller` as `$this->CI`, use `$this->resetInstance(true)`.

**Note:** When you call [$this->request()](#testcaserequestmethod-argv-params--), you don't have to use this method. Because `$this->request()` resets CodeIgniter instance internally.

**Upgrade Note for v0.6.0**

Before v0.6.0, we write `setUp()` method like this:

~~~php
public function setUp(): void
{
	$this->CI =& get_instance();
	$this->CI->load->model('Category_model');
	$this->obj = $this->CI->Category_model;
}
~~~

When you use the way, you use the same CodeIgniter instance and the same `Category_model` instance in every test method.

In contrast, if you use `$this->resetInstance()`, it resets CodeIgniter instance and `Category_model`. So you use new CodeIgniter instance and new `Category_model` instance in every test method.

#### `TestCase::request($method, $argv, $params = [])`

| param     | type         | description                                   |
|-----------|--------------|-----------------------------------------------|
|`$method`  | string       | HTTP method                                   |
|`$argv`    | array/string | controller, method [, arg1, ...] / URI string |
|`$params`  | array/string | POST params or GET params / raw_input_stream  |

`returns` (string) output strings (view)

Runs a controller method or make a request to URI string, after `reset_instance()`.

If you want to invoke routing, specify URI string:

~~~php
$output = $this->request('GET', 'products/shoes/show/123');
~~~

You could add query string in URI string:

~~~php
$output = $this->request('GET', 'users/detail?name=John+O%27Reilly');
~~~

If you want to make POST request:

~~~php
$output = $this->request(
	'POST',
	'form/index',
	['name' => 'John Smith', 'email' => 'john@example.com']
);
~~~

If you want to call a controller method directly:

~~~php
$output = $this->request('GET', ['Form', 'index']);
~~~

**Note:** If you pass an array to the 2nd argument, it does not invoke routing, `_remap()` and `_output()` methods.

**Upgrade Note for v0.16.0**

v0.16.0 has changed the default behavior of `$this->request()`. It detects all warnings and notices during the execution, and throws exceptions. If you want to disable the checking, you must set `$strictRequestErrorCheck` false in your test case classes:

~~~php
protected $strictRequestErrorCheck = false;
~~~

##### `request->setHeader()`

Sets HTTP request header.

~~~php
$this->request->setHeader('Accept', 'application/csv');
~~~

##### `request->setFiles($files)`

Sets `$_FILES` superglobal variable.

~~~php
$filename = 'ci-phpuni-test-downloads-777.png';
$filepath = TESTPATH.'fixtures/'.$filename;

$files = [
	'userfile' => [
		'name'     => $filename,
		'type'     => 'image/png',
		'tmp_name' => $filepath,
	],
];
$this->request->setFiles($files);
~~~

##### `request->setCallable()`

Sets (and resets) a function (callable) to run after controller instantiation.

~~~php
$this->request->setCallable(
	function ($CI) {
		$CI->load->library('user_agent');
	}
);
$output = $this->request('GET', ['Bbs', 'index']);
~~~

You can set one callable with `$this->request->setCallable()`. If you want to add more than one callable, you can use `$this->request->addCallable()` below.

##### `request->addCallable()`

Adds a function (callable) to run after controller instantiation.

~~~php
$this->request->addCallable(
	function ($CI) {
		$CI->load->library('user_agent');
	}
);
$output = $this->request('GET', ['Bbs', 'index']);
~~~

##### `request->setCallablePreConstructor()`

Sets (and resets) a function to run before controller instantiation.

~~~php
$this->request->setCallablePreConstructor(
	function () {
		// Get mock object
		$auth = $this->getDouble(
			'Ion_auth', ['logged_in' => TRUE]
		);
		// Inject mock object
		load_class_instance('ion_auth', $auth);
	}
);
~~~

##### `request->addCallablePreConstructor()`

Adds a function (callable) to run before controller instantiation.

~~~php
$this->request->addCallablePreConstructor(
	function () {
		// Get mock object
		$auth = $this->getDouble(
			'Ion_auth', ['logged_in' => TRUE]
		);
		// Inject mock object
		load_class_instance('ion_auth', $auth);
	}
);
~~~

##### `request->enableHooks()`

If you want to enable hooks, call `$this->request->enableHooks()` method. It enables only `pre_controller`, `post_controller_constructor`, `post_controller` and `display_override` hooks.

~~~php
$this->request->enableHooks();
$output = $this->request('GET', 'products/shoes/show/123');
~~~

#### `TestCase::ajaxRequest($method, $argv, $params = [])`

| param     | type         | description                                   |
|-----------|--------------|-----------------------------------------------|
|`$method`  | string       | HTTP method                                   |
|`$argv`    | array/string | controller, method [, arg1, ...] / URI string |
|`$params`  | array/string | POST params or GET params / raw_input_stream  |

`returns` (string) output strings

The same as `TestCase::request()`, but this makes an Ajax request. This adds only `$_SERVER['HTTP_X_REQUESTED_WITH']`.

~~~php
$output = $this->ajaxRequest('GET', 'api/books');
~~~

#### `TestCase::assertResponseCode($code)`

| param   | type | description      |
|---------|------|------------------|
|`$code`  | int  | HTTP status code |

Checks for a specific response code in your controller tests.

~~~php
$this->assertResponseCode(200);
~~~

#### `TestCase::assertRedirect($uri, $code = null)`

| param   | type   | description      |
|---------|--------|------------------|
|`$uri`   | string | URI to redirect  |
|`$code`  | int    | HTTP status code |

Checks if `redirect()` is called in your controller tests.

~~~php
$this->assertRedirect('auth/login');
~~~

#### `TestCase::assertResponseHeader($name, $value)`

| param   | type   | description  |
|---------|--------|--------------|
|`$name`  | string | header name  |
|`$value` | string | header value |

Checks for a specific response header in your controller tests.

~~~php
$this->assertResponseHeader(
	'Content-Type', 'application/csv; charset=utf-8'
);
~~~

**Note:** This method can only assert headers set by `$this->output->set_header()` method.

#### `TestCase::assertResponseCookie($name, $value, $allow_duplicate = false)`

| param             | type         | description                           |
|-------------------|--------------|---------------------------------------|
|`$name`            | string       | cookie name                           |
|`$value`           | string/array | cookie value / array of cookie params |
|`$allow_duplicate` | bool         | whether to allow duplicated cookies   |

Checks for a specific response cookie in your controller tests.

~~~php
$this->assertResponseCookie('cookie-name', 'cookie value');
~~~

You can also check cookie params.

~~~php
$this->assertResponseCookie(
	'cookie-name',
	[
		'value'  => 'cookie value',
		'domain' => '.example.com',
		'path'   => '/',
		'secure' => TRUE,
		'httponly' => TRUE,
	]
);
~~~

**Note:** This method can only assert cookies set by `$this->input->set_cooke()` method.

#### `TestCase::assertLogged($level, $message)`

| param     | type   | description          |
|-----------|--------|----------------------|
|`$level`   | string | log level            |
|`$message` | string | expected log message |

Checks if certain message is logged.

~~~php
$this->assertLogged('error', 'log message');
~~~

#### `TestCase::getDouble($classname, $params, $constructor_params = false)`

| param               | type        | description                                            |
|---------------------|-------------|--------------------------------------------------------|
|`$classname`         | string      | class name                                             |
|`$params`            | array       | [method_name => return_value]                          |
|                     |             | [[method_name => [return_value1, return_value2 [, ...]]] |
|`$constructor_params`| false/array | false: disable constructor / array: constructor params |

`returns` (object) PHPUnit mock object

Gets PHPUnit mock object.

~~~php
$email = $this->getMockBuilder('CI_Email')
	->disableOriginalConstructor()
	->setMethods(['send'])
	->getMock();
$email->method('send')
	->willReturn(TRUE);
~~~

You could write code above like below:

~~~php
$email = $this->getDouble('CI_Email', ['send' => TRUE]);
~~~

You can set Closure as the return value of a mocked method.

~~~php
$ret = function () {
	throw new RuntimeException('Cannot send email!');
};
$mock = $this->getDouble('CI_Email', ['send' => $ret]);
~~~

You can also set the mock itself as the return value of a mocked method with using `$this->returnSelf()`.

~~~php
$mock = $this->getDouble('CI_Email', [
	'to'      => $this->returnSelf(),
	'subject' => $this->returnSelf(),
	'send'    => TRUE,
]);
~~~

You can create mocks with consecutive calls.

~~~php
$mock = $this->getMockBuilder('CI_Input')
	->disableOriginalConstructor()
	->setMethods(['method'])
	->getMock();
$mock->expects($this->any())->method('method')
	->will($this->onConsecutiveCalls('GET', 'POST' ,'DELETE'));
~~~

You could write code above like below:

~~~php
$mock = $this->getDouble(
	'CI_Input',
	[
		['method' => ['GET', 'POST' ,'DELETE']],
	]
);
~~~

**Upgrade Note for v0.10.0**

v0.10.0 has changed the default behavior of `$this->getDouble()` and disabled original constructor. If the change causes errors, update your test code like below:

*before:*
~~~php
$validation = $this->getDouble('CI_Form_validation', ['run' => TRUE]);
~~~

↓

*after:*
~~~php
$validation = $this->getDouble('CI_Form_validation', ['run' => TRUE], TRUE);
~~~

#### `TestCase::verifyInvoked($mock, $method, $params)`

| param   | type   | description         |
|---------|--------|---------------------|
|`$mock`  | object | PHPUnit mock object |
|`$method`| string | method name         |
|`$params`| array  | arguments           |

Verifies a method was invoked at least once.

~~~php
$loader->expects($this->atLeastOnce())
	->method('view')
	->with(
		'shop_confirm', $this->anything(), TRUE
	);
~~~

You could write code above like below:

~~~php
$this->verifyInvoked(
	$loader,
	'view',
	[
		'shop_confirm', $this->anything(), TRUE
	]
);
~~~

#### `TestCase::verifyInvokedOnce($mock, $method, $params)`

| param   | type   | description         |
|---------|--------|---------------------|
|`$mock`  | object | PHPUnit mock object |
|`$method`| string | method name         |
|`$params`| array  | arguments           |

Verifies that method was invoked only once.

~~~php
$loader->expects($this->once())
	->method('view')
	->with(
		'shop_confirm', $this->anything(), TRUE
	);
~~~

You could write code above like below:

~~~php
$this->verifyInvokedOnce(
	$loader,
	'view',
	[
		'shop_confirm', $this->anything(), TRUE
	]
);
~~~

#### `TestCase::verifyInvokedMultipleTimes($mock, $method, $times, $params)`

| param   | type   | description         |
|---------|--------|---------------------|
|`$mock`  | object | PHPUnit mock object |
|`$method`| string | method name         |
|`$times` | int    | times               |
|`$params`| array  | arguments           |

Verifies that method was called exactly $times times.

~~~php
$loader->expects($this->exactly(2))
	->method('view')
	->withConsecutive(
		['shop_confirm', $this->anything(), TRUE],
		['shop_tmpl_checkout', $this->anything()]
	);
~~~

You could write code above like below:

~~~php
$this->verifyInvokedMultipleTimes(
	$loader,
	'view',
	2,
	[
		['shop_confirm', $this->anything(), TRUE],
		['shop_tmpl_checkout', $this->anything()]
	]
);
~~~

#### `TestCase::verifyNeverInvoked($mock, $method, $params)`

| param   | type   | description         |
|---------|--------|---------------------|
|`$mock`  | object | PHPUnit mock object |
|`$method`| string | method name         |
|`$params`| array  | arguments           |

Verifies that method was not called.

~~~php
$loader->expects($this->never())
	->method('view')
	->with(
		'shop_confirm', $this->anything(), TRUE
	);
~~~

You could write code above like below:

~~~php
$this->verifyNeverInvoked(
	$loader,
	'view',
	[
		'shop_confirm', $this->anything(), TRUE
	]
);
~~~

#### `TestCase::warningOff()`

Turns off WARNING and Notice in PHP error reporting.

~~~php
$this->warningOff();
$output = $this->request('GET', 'api/example/users');
$this->warningOn();
~~~

#### `TestCase::warningOn()`

Restores PHP error reporting.

~~~php
$this->warningOn();
~~~

### *class* DbTestCase

**Upgrade Note for v0.13.0**

To use this test case, you must install `application/tests/DbTestCase.php` manually.

#### `DbTestCase::seeInDatabase($table, $where)`

| param   | type   | description      |
|---------|--------|------------------|
|`$table` | string | table name       |
|`$where` | array  | where conditions |

Checks if records that match the conditions in `$where` exist in the database.

#### `DbTestCase::dontSeeInDatabase($table, $where)`

| param   | type   | description      |
|---------|--------|------------------|
|`$table` | string | table name       |
|`$where` | array  | where conditions |

Checks if records that match the conditions in `$where` do not exist in the database.

#### `DbTestCase::seeNumRecords($expected, $table, $where = [])`

| param      | type   | description      |
|------------|--------|------------------|
|`$expected` | int    | expected number  |
|`$table`    | string | table name       |
|`$where`    | array  | where conditions |

Checks if the number of rows in the database that match `$where` is equal to `$expected`.

#### `DbTestCase::hasInDatabase($table, $data)`

| param      | type   | description      |
|------------|--------|------------------|
|`$table`    | string | table name       |
|`$data`     | array  | data to insert   |

Inserts a row into to the database. This row will be removed after the test has run.

#### `DbTestCase::grabFromDatabase($table, $column, $where)`

| param      | type   | description      |
|------------|--------|------------------|
|`$table`    | string | table name       |
|`$column`   | string | column name      |
|`$where`    | array  | where conditions |

Fetches a single column from a database row with criteria matching `$where`.

### *class* UnitTestCase

**Upgrade Note for v0.16.0**

To use this test case, you must install `application/tests/UnitTestCase.php` manually.

#### `UnitTestCase::newModel($classname)`

| param       | type         | description                              |
|-------------|--------------|------------------------------------------|
|`$classname` | string       | model classname                          |

`returns` model object

Resets CodeIgniter instance and return new model instance. This method is for model unit testing.

~~~php
public function setUp(): void
{
	$this->obj = $this->newModel('Category_model');
}
~~~

#### `UnitTestCase::newLibrary($classname, $args)`

| param       | type         | description                              |
|-------------|--------------|------------------------------------------|
|`$classname` | string       | library classname                        |
|`$args`      | array        | constructor argments                     |

`returns` library object

Resets CodeIgniter instance and return new library instance. This method is for library unit testing.

~~~php
public function setUp(): void
{
	$this->obj = $this->newLibrary('Foo_library');
}
~~~

#### `UnitTestCase::newController($classname)`

| param       | type         | description                                   |
|-------------|--------------|-----------------------------------------------|
|`$classname` | string       | controller classname                          |

`returns` controller object

Resets CodeIgniter instance and return new controller instance. This method is for controller unit testing.

~~~php
$controller = $this->newController('Some_controller');
$actual = $controller->some_method();
~~~

### *class* ReflectionHelper

This class provides helper methods to access private or protected properties and methods.

But generally it is not recommended to test non-public properties or methods, so think twice before you use methods in this class.

#### ReflectionHelper::getPrivateProperty($obj, $property)

| param     | type          | description         |
|-----------|---------------|---------------------|
|`$obj`     | object/string | object / class name |
|`$property`| string        | property name       |

`returns` (mixed) property value

Gets private or protected property value.

~~~php
$obj = new SomeClass();
$private_propery = ReflectionHelper::getPrivateProperty(
	$obj,
	'private_propery',
);
~~~

#### ReflectionHelper::setPrivateProperty($obj, $property, $value)

| param     | type          | description         |
|-----------|---------------|---------------------|
|`$obj`     | object/string | object / class name |
|`$property`| string        | property name       |
|`$value`   | mixed         | value               |

Sets private or protected property value.

~~~php
$obj = new SomeClass();
ReflectionHelper::setPrivateProperty(
	$obj,
	'private_propery',
	'new value'
);
~~~

#### ReflectionHelper::getPrivateMethodInvoker($obj, $method)

| param   | type          | description         |
|---------|---------------|---------------------|
|`$obj`   | object/string | object / class name |
|`$method`| string        | method name         |

`returns` (closure) method invoker

Gets private or protected method invoker.

~~~php
$obj = new SomeClass();
$method = ReflectionHelper::getPrivateMethodInvoker(
	$obj, 'privateMethod'
);
$this->assertEquals(
	'return value of the privateMethod() method', $method()
);
~~~

### *class* MonkeyPatch

To use this class, you have to enable monkey patching. See [How to Write Tests](HowToWriteTests.md#monkey-patching).

#### `MonkeyPatch::patchFunction($function, $return_value, $class_method)`

| param         | type   | description                                    |
|---------------|--------|------------------------------------------------|
|`$function`    | string | function name to patch                         |
|`$return_value`| mixed  | return value / callback                        |
|`$class_method`| string | class::method or classname to apply this patch |

Replaces function on the fly.

If `$class_method` is present, the patch is applied to the functions only in the class method or in the class.

There are some known limitations. See [How to Write Tests](HowToWriteTests.md#patching-functions) for details.

~~~php
MonkeyPatch::patchFunction('mt_rand', 100, 'Welcome::index');
~~~

#### `MonkeyPatch::resetFunctions()`

Resets all patched functions.

This method is called on `TestCase::tearDown()` by default. So you don't have to call it normally.

#### `MonkeyPatch::patchMethod($classname, $params)`

| param       | type   | description                   |
|-------------|--------|-------------------------------|
|`$classname` | string | class name to patch           |
|`$params`    | array  | [method_name => return_value] |

Replaces method in user-defined class on the fly.

~~~php
MonkeyPatch::patchMethod(
	'Category_model',
	['get_category_list' => [(object) ['name' => 'Nothing']]]
);
~~~

#### `MonkeyPatch::resetMethods()`

Resets all patched class methods.

This method is called on `TestCase::tearDown()` by default. So you don't have to call it normally.

#### `MonkeyPatch::patchConstant($constant, $value, $class_method)`

| param         | type   | description                                    |
|---------------|--------|------------------------------------------------|
|`$constant`    | string | constant name to patch                         |
|`$value`       | mixed  | value                                          |
|`$class_method`| string | class::method or classname to apply this patch |

Replaces constant value on the fly.

If `$class_method` is present, the patch is applied to the constants only in the class method or in the class.

There are some known limitations. See [How to Write Tests](HowToWriteTests.md#patching-constants) for details.

~~~php
MonkeyPatch::patchConstant('ENVIRONMENT', 'development', 'Welcome::index');
~~~

#### `MonkeyPatch::resetConstants()`

Resets all patched constants.

This method is called on `TestCase::tearDown()` by default. So you don't have to call it normally.

#### `MonkeyPatch::verifyInvoked($class_method, $params)`

| param         | type   | description              |
|---------------|--------|--------------------------|
|`$class_method`| string | class::method / function |
|`$params`      | array  | arguments                |

Verifies a patched class method or a patched function was invoked at least once.

~~~php
MonkeyPatch::verifyInvoked(
	'Ion_auth_model::login', ['foo', 'bar']
);
~~~

#### `MonkeyPatch::verifyInvokedOnce($class_method, $params)`

| param         | type   | description              |
|---------------|--------|--------------------------|
|`$class_method`| string | class::method / function |
|`$params`      | array  | arguments                |

Verifies that patched class method or a patched function was invoked only once.

~~~php
MonkeyPatch::verifyInvokedOnce(
	'CI_Input::post', ['id']
);
~~~

#### `MonkeyPatch::verifyInvokedMultipleTimes($class_method, $times, $params)`

| param         | type   | description              |
|---------------|--------|--------------------------|
|`$class_method`| string | class::method / function |
|`$times`       | int    | times                    |
|`$params`      | array  | arguments                |

Verifies that patched method or a patched function was called exactly $times times.

~~~php
MonkeyPatch::verifyInvokedMultipleTimes(
	'CI_Input::post', 2
);
~~~

#### `MonkeyPatch::verifyNeverInvoked($class_method, $params)`

| param         | type   | description              |
|---------------|--------|--------------------------|
|`$class_method`| string | class::method / function |
|`$params`      | array  | arguments                |

Verifies that patched method or a patched function was not called.

~~~php
MonkeyPatch::verifyNeverInvoked(
	'Ion_auth_model::login', ['username', 'PHS/DL1m6OMYg']
);
~~~
