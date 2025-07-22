---
title: Bulk Uploader
summary: Upload multiple images or files at once to populate DataObjects managed by a GridField
icon: upload
---

# Bulk uploader

A component for uploading images and/or files in bulk into [`DataObject`](api:SilverStripe\ORM\DataObject) managed by the [`GridField`](api:SilverStripe\Forms\GridField\GridField).

## Usage 1

Simplest usage, add the component to your [`GridFieldConfig`](api:SilverStripe\Forms\GridField\GridFieldConfig) as below. The component will find the first [`Image`](api:SilverStripe\Assets\Image) or [`File`](api:SilverStripe\Assets\File) has_one relation to use on the managed `DataObject`.

```php
use Colymba\BulkUpload\BulkUploader;

$config->addComponent(new BulkUploader());
```

## Usage 2

You can specify which `Image` or `File` field to use and a specific `DataObject` class name to use.

- `$fileRelationName (string, optional)`: The name of the `Image` or `File` `has_one` field to use (If your relation is set as `'MyImage' => Image::class`, the parameter should be `'MyImage'`)
- `$recordClassName (string, optional)`: The class name of the `DataObject` to create (Useful if for example your `GridField` holds `DataObject`s of different classes, like when used with the `GridFieldAddNewMultiClass` component.)

```php
use Colymba\BulkUpload\BulkUploader;

$config->addComponent(new BulkUploader($fileRelationName, $recordClassName));
```

## Configuration

### Component configuration

The component's option can be configurated through the `setConfig` functions like this:

```php
use Colymba\BulkUpload\BulkUploader;

$config->getComponentByType(BulkUploader::class)->setConfig($reference, $value);
```

The available configuration options are:

- `fileRelationName`: sets the name of the `Image` or `File` has_one field to use (i.e. 'MyImage')

### UploadField configuration

The underlying [`UploadField`](api:SilverStripe\AssetAdmin\Forms\UploadField) can be configured via [`BulkUploader::setUfSetup()`](api:Colymba\BulkUpload\BulkUploader::setUfSetup()) which is used to pass function calls on to the `UploadField` itself

For example, to set the upload folder, which is set by calling `setFolderName` on the `UploadField`, you would use the following:

```php
use Colymba\BulkUpload\BulkUploader;

$config->getComponentByType(BulkUploader::class)
    ->setUfSetup('setFolderName', 'myFolder')
```

Please see [`UploadField`](api:SilverStripe\AssetAdmin\Forms\UploadField) and the [`Upload`](api:SilverStripe\Assets\Upload) for more info.

## Bulk editing

To get a quick edit shortcut to all the newly upload files, please also add the [`BulkManager`](api:Colymba\BulkManager\BulkManager) component to your `GridFieldConfig`.
