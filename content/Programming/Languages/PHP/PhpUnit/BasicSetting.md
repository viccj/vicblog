---
title: "Basic Setting"
date: 2023-07-29T15:00:41+08:00
tags:
- php
- unittest
---


{{< toc >}}
## PHPUnit 安裝

```
composer require --dev phpunit/phpunit
```

安裝完之後就可以執行測試

```
php vendor/bin/phpunit yourTest.php
```

## 建立設定檔 phpunit.xml


### 一次執行所有test

建立一個 `phpunit.xml`檔案，並在裡面指定路徑，之後這個folder裡面所有的測試就會被執行

```php
<?xml version="1.0" encoding="UTF-8"?>

<phpunit bootstrap="bootstrap.php" colors="true">
    <testsuites>
        <testsuite name="All Tests">
            <directory>tests</directory>
        </testsuite>
    </testsuites>
</phpunit>
```

之後這個 folder 裡面所有的測試就會被執行 (測試的檔名要是Test 結尾，測試的method 要是 test 開頭)



`appTest.php`

```php
<?php


class appTest extends TestCase
{
    public function testSomething()
    {
	assertTrue(true);
    }
}
```



```bash
php vendor/bin/phpunit
```

### phpunit.xml 常用參數



#### The `bootstrap` Attribute

This attribute configures the bootstrap script that is loaded before the tests are executed. This script usually only registers the autoloader callback that is used to load the code under test.



#### The `cacheDirectory` Attribute

This attribute configures the directory in which PHPUnit caches information such as test results (see below) or the result of static code analysis that is performed for code coverage reporting.



#### The `cacheResult` Attribute

Possible values: `true` or `false` (default: `true`)

This attribute configures the caching of test results. This caching is required for ordering tests by defects or duration with the `executionOrder` attribute (see [The executionOrder Attribute](https://docs.phpunit.de/en/10.2/configuration.html#appendixes-configuration-phpunit-executionorder)).



#### The `colors` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether colors are used in PHPUnit’s output.

Setting this attribute to `true` is equivalent to using the `--colors=auto` CLI option.

Setting this attribute to `false` is equivalent to using the `--colors=never` CLI option.

