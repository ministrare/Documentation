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