# Personal Documentation
## CakePHP / "Table association"

### Index
- [Contain model and foreign key with invoices](#contain-model-and-foreign-key-with-invoices)
- [](./)
- [](./)
- [](./)

### Contain model and foreign key with invoices
```php
$this->belongsTo('Orders', [
   'conditions' => [
       'model' => 'Orders',
   ],
   'foreignKey' => 'foreign_key',
]);
```