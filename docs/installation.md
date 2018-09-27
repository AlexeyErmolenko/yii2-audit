---
layout: default
title: Installation and Configuration
permalink: /docs/installation/
---

# Installation and Configuration

## Download

Download using composer by running the following command:

```
$ composer require --prefer-dist bedezign/yii2-audit:"^1.0"
```

Or add a `require` line to your `composer.json`: 

```
{
    "require": {
        "bedezign/yii2-audit": "^1.0"
    }
}
```

## Migrations

Run the migrations from the `migrations` folder to create the relevant tables:  

```
$ php yii migrate --migrationPath=@bedezign/yii2/audit/migrations
```

Upgrading from pre 1.0? [Be sure to read this](../upgrading-0.1-0.2/).

## Module Configuration

Add `Audit` to your configuration array:

```php
<?php
$config = [
    'modules' => [
        'audit' => 'bedezign\yii2\audit\Audit',
    ],
];
```

See [Module Configuration](../module-configuration/) for the all configuration options and advanced usage information.

## Logging Database Changes

Add `AuditTrailBehavior` to the models you want to log:

```php
<?php
class Post extends \yii\db\ActiveRecord
{
    public function behaviors()
    {
        return [
            'bedezign\yii2\audit\AuditTrailBehavior'
        ];
    }
}
```

See [Trail Panel](../trail-panel/) for the all configuration options and advanced usage information.

You can also attach `AuditTrailBehavior` to models eg. from `vendor`

```
// attach audit trails to 3rd-party models
$trailedModels = [
    \pheme\settings\models\Setting::class,
    \Da\User\Model\User::class,
    \Da\User\Model\Profile::class
];
foreach ($trailedModels as $model) {
    \yii\base\Event::on(
        $model,
        \yii\db\ActiveRecord::EVENT_INIT,
        function ($event) {
            $event->sender->attachBehavior('audit', \bedezign\yii2\audit\AuditTrailBehavior::class);
        }
    );
}
```

## Logging Javascript

Register `JSLoggingAsset` in any of your views:

```php
<?php
\bedezign\yii2\audit\web\JSLoggingAsset::register($this);
```

See [Javascript Panel](../javascript-panel/) for the all configuration options and advanced usage information.

## Logging Errors

Add `ErrorHandler` to your configuration array:

```php
<?php
$config = [
    'components' => [
        'errorHandler' => [
            // web error handler
            'class' => '\bedezign\yii2\audit\components\web\ErrorHandler',
            // console error handler
            //'class' => '\bedezign\yii2\audit\components\console\ErrorHandler',
        ],
    ],
];
```

**Important:** Be sure to use the correct error handler! Don't simply add it to the `common` configuration, but instead add it in the frontend/web and `console` configuration separately.  

See [Error Panel](../error-panel/) for the all configuration options and advanced usage information.

## Viewing the Audit Data

Assuming you named the module "audit" you can then access the audit module through the following URL:

```
http://localhost/path/to/index.php?r=audit
```

## Where to now ?

Check out the other [Documentation](../)

