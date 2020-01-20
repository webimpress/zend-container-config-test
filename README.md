# zend-container-config-test

> ## Repository abandoned 2019-12-31
>
> This repository has moved to [laminas/laminas-container-config-test](https://github.com/laminas/laminas-container-config-test).

[![Build Status](https://secure.travis-ci.org/zendframework/zend-container-config-test.svg?branch=master)](https://secure.travis-ci.org/zendframework/zend-container-config-test)
[![Coverage Status](https://coveralls.io/repos/github/zendframework/zend-container-config-test/badge.svg?branch=master)](https://coveralls.io/github/zendframework/zend-container-config-test?branch=master)

This library provides common tests for PSR-11 containers configured using a
subset of [zend-servicemanager](https://github.com/zendframework/zend-servicemanager)
[configuration](https://docs.zendframework.com/zend-servicemanager/configuring-the-service-manager/)
as [specified by Expressive](https://docs.zendframework.com/zend-expressive/v3/features/container/config/)

It guarantees delivery of the same basic functionality across multiple PSR-11
container implementations, and simplifies switching between them.

Currently we support:
- [Aura.Di](https://github.com/auraphp/Aura.Di) - via [zend-auradi-config](https://github.com/zendframework/zend-auradi-config)
- [Pimple](https://pimple.symfony.com/) - via [zend-pimple-config](https://github.com/zendframework/zend-pimple-config)
- [zend-servicemanager](https://github.com/zendframework/zend-servicemanager)

## Installation

Run the following to install this library:

```bash
$ composer require --dev zendframework/zend-container-config-test
```

## Using common tests

In your library, you will need to extend the
`Zend\ContainerConfigTest\AbstractContainerTest` class within your test suite and
implement the method `createContainer`:

```php
protected function createContainer(array $config) : ContainerInterface;
```

It should return your PSR-11-compatible container, configured using `$config`.

Then, depending on what functionality you'd like to support, you can add the
following traits into your test case:

- `Zend\ContainerConfigTest\AliasTestTrait` - to support `aliases` configuration,
- `Zend\ContainerConfigTest\DelegatorTestTrait` - to support `delegators` configuration,
- `Zend\ContainerConfigTest\FactoryTestTrait` - to support `factories` configuration,
- `Zend\ContainerConfigTest\InvokableTestTrait` - to support `invokables` configuration,
- `Zend\ContainerConfigTest\ServiceTestTrait` - to support `services` configuration,
- `Zend\ContainerConfigTest\SharedTestTrait` - to support `shared` and `shared_by_default` configuration.

To provide an Expressive-compatible container, you should extend the class
`Zend\ContainerConfigTest\AbstractExpressiveContainerConfigTest`
and implement the method `createContainer`. This class composes the following traits:

- `Zend\ContainerConfigTest\AliasTestTrait`,
- `Zend\ContainerConfigTest\DelegatorTestTrait`,
- `Zend\ContainerConfigTest\FactoryTestTrait`,
- `Zend\ContainerConfigTest\InvokableTestTrait`,
- `Zend\ContainerConfigTest\ServiceTestTrait`.

If you want also plan to support shared services, your test class should compose
the `SharedTestTrait` as well:

```php
use Zend\ContainerConfigTest\AbstractExpressiveContainerConfigTest;
use Zend\ContainerConfigTest\SharedTestTrait;

class ContainerTest extends AbstractExpressiveContainerConfigTest
{
    use SharedTestTrait;
    
    protected function createContainer(array $config) : ContainerInterface
    {
        // your container configuration
    }
}
```
