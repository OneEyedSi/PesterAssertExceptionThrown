# PesterAssertExceptionThrown
A PowerShell module that can be used in Pester tests to assert that a function throws a specific 
exception.

## NOTE: THIS MODULE IS STILL UNDER DEVELOPMENT; BREAKING CHANGES MAY BE EXPECTED
The _Assert-ExceptionThrown_ function is fully tested and is working.  However, as it is still 
under development, parameters and functionality may change.  The module should stabilize by the 
end of March 2018, or once this notice is removed.

## Introduction
The AssertExceptionThrown module exports a single function, **_Assert-ExceptionThrown_**.  This can 
be used in place of the built-in Pester **_Should -Throw_** command, to test whether a function or 
command throws an exception.

The weakness of _Should -Throw_ is that it can only test that an exception with a particular 
message is thrown; it cannot test the type of exception thrown.  _Assert-ExceptionThrown_ can test 
both the type of exception thrown and its message.  It can also be negated, to test that a 
function does not throw any exception, or that it does not throw an exception of a particular type 
or with a particular message.

Like _Should -Throw_, _Assert-ExceptionThrown_ will throw an exception if a test fails.

## Getting Started
Open the Modules folder and copy the AssertExceptionThrown sub-folder, with its contents, to one 
of the locations that PowerShell recognises for modules.  The two default locations are:

1. For all users:  **%ProgramFiles%\WindowsPowerShell\Modules** 
(usually resolves to C:\Program Files\WindowsPowerShell\Modules);

2. For the current user only:  **%UserProfile%\Documents\WindowsPowerShell\Modules** 
(usually resolves to C:\Users\\{user name}\Documents\WindowsPowerShell\Modules)

Once the AssertExceptionThrown folder has been saved to a recognised Module location you should 
be able to call the _Assert-ExceptionThrown_ function without explicitly importing the module.

## Usage
Usage of _Assert-ExceptionThrown_ is similar to that for _Should -Throw_:  Wrap the function under 
test, along with any arguments, in curly braces and pipe it to _Assert-ExceptionThrown_.

### Examples

#### Test that a function taking two arguments throws an exception with a specified message
```
{ MyFunctionWithTwoArgs -Key 'header' -Value 10 } | 
    Assert-ExceptionThrown -WithMessage 'Value was of type int32, expected string'
```
The name of the function under test is MyFunctionWithTwoArgs.  The test will only pass if 
MyFunctionWithTwoArgs, with the specified arguments, throws an exception with a message 
that contains the specified text.

#### Test that a function taking no arguments throws an exception of a specific type
```
{ MyFunction } | 
    Assert-ExceptionThrown -WithTypeName System.ArgumentException
```
The test will only pass if MyFunction throws an System.ArgumentException.

#### Specify a short type name, without namespace, for the expected exception
```
{ MyFunction } | 
    Assert-ExceptionThrown -WithTypeName ArgumentException
```
The test will pass if MyFunction throws a System.ArgumentException.

#### Test that a function does not throw an exception
```
{ MyFunctionWithTwoArgs -Key 'header' -Value 'value' } | 
    Assert-ExceptionThrown -Not
```
The test will pass only if MyFunctionWithTwoArgs, with the specified arguments, does not throw 
any exception.

#### Test that a function does not throw an exception with a specified message
```
{ MyFunctionWithTwoArgs -Key 'header' -Value 10 } | 
    Assert-ExceptionThrown -Not -WithMessage 'Value was of type int32, expected string'
```
The test will fail if MyFunctionWithTwoArgs, with the specified arguments, throws an exception 
with a message that contains the specified text.  It will pass if MyFunctionWithTwoArgs does not 
throw an exception, or if it throws an exception with a different message.

#### Test that a function does not throw an exception of a specified type
```
{ MyFunction } | 
    Assert-ExceptionThrown -Not -WithTypeName ArgumentException
```
The test will fail if MyFunction throws a System.ArgumentException.  It will pass if MyFunction 
does not throw an exception, or if it throws an exception of a different type.