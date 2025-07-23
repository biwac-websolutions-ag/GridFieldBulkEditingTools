---
title: Bulk Manager
summary: Perform actions on multiple records, such as unlinking, deleting and bulk editing, directly from a GridField
icon: edit
---

# Bulk manager

Perform actions on multiple records straight from the [`GridField`](api:SilverStripe\Forms\GridField\GridField). Comes with *unlink*, *delete* and bulk *editing*. You can also easily create/add your own.

## Usage

Simply add component to your [`GridFieldConfig`](api:SilverStripe\Forms\GridField\GridFieldConfig)

```php
use Colymba\BulkManager\BulkManager;

$config->addComponent(new BulkManager());
```

## Configuration

The component's options can be configurated individually or in bulk through the 'config' functions like this:

```php
use Colymba\BulkManager\BulkManager;

$config->getComponentByType(BulkManager::class)->setConfig($reference, $value);
```

### `$config` overview

The available configuration options are:

- `editableFields`: array of string referencing specific CMS fields available for editing

## Custom actions

You can remove or add individual action or replace them all via [`BulkManager::addBulkAction()`](api:Colymba\BulkManager\BulkManager::addBulkAction()) and [`BulkManager::removeBulkAction()`](api:Colymba\BulkManager\BulkManager::removeBulkAction()).

### Adding a custom action

To add a custom bulk action to the list use:

```php
use App\BulkActions\MyBulkAction;
use Colymba\BulkManager\BulkManager;

$config
    ->getComponentByType(BulkManager::class)
    ->addBulkAction(MyBulkAction::class)
```

#### Custom action handler

When creating your own bulk action [`RequestHandler`](api:SilverStripe\Control\RequestHandler), you should extend [`Handler`](api:Colymba\BulkManager\BulkAction\Handler) which will expose 2 useful functions [`Handler::getRecordIDList()`](api:Colymba\BulkManager\BulkAction\Handler::getRecordIDList()) and [`Handler::getRecords()`](api:Colymba\BulkManager\BulkAction\Handler::getRecords()) returning either an array with the selected records IDs or a `DataList` of the selected records.

Make sure to define the handler's [`RequestHandler.url_segment`](api:SilverStripe\Control\RequestHandler->url_segment), from which the handler will be called and its relating [`RequestHandler.allowed_actions`](api:SilverStripe\Control\RequestHandler->allowed_actions) and [`RequestHandler.url_handlers`](api:SilverStripe\Control\RequestHandler->url_handlers). See `Handler`, [`DeleteHandler`](api:Colymba\BulkManager\BulkAction\DeleteHandler) and [`UnlinkHandler`](api:Colymba\BulkManager\BulkAction\UnlinkHandler) for examples.

#### Front-end config

Bulk action handler's front-end configuration is set via class properties `label`, `icon`, `buttonClasses`, `xhr` and `destructive`. See `Handler`, `DeleteHandler` and `UnlinkHandler` for reference and examples.
