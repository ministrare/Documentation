### Exmaples 2

**Dropdown Filter**
```php
    [
        'permission' => $this->Authorization->isAuthorized('store', 'admin'),
        'itemFieldName' => 'store_id',
        'label' => __('Filiaal'),
        'model' => 'Stores',
        'name' => 'store_id',
        'type' => TableCell::FILTER_TYPE_SELECT,
    ],
```

**Datum en uur Picker**
```php
<?php
    echo $this->element('FormManager.date_time', [
    'entity' => $arrestee,
    'label' => __('Datum & uur uit cel'),
    'name' => 'release_time',
    ]);
?>
```

**Set id of indexTable for URL**
```php
'itemUrl' => [
    'itemFieldName' => 'id',
    'url' => [
        'action' => 'indexInternalProduct',
        'controller' => 'StockProducts',
        'plugin' => 'BackStock',
        'options' => [
            'type' => 'id',
        ],
    ],
],
```