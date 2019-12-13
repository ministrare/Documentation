# Personal Documentation
## CakePHP

### Folders Index 
- [Bin/cake](./Bin-cake/)
- [Composer](./Composer/)
- [IndexTable](./Index-tables/)
- [Finders](./Finders/)
- [Nieuw Project](./New-project/)


### To Classify
- [Create a Cron on Combell](#create-a-cron-on-combell)
- [Contain model and foreign key with invoices](#contain-model-and-foreign-key-with-invoices)
- [Print_r < pre >](#print_r--pre-)
- [Run Packages](#run-packages)


### Create a Cron on Combell
Open Mremote and check in the root folder if project has any crons. \
To create a cron:
```
cat .crontab
nano .crontab
```
To save:
```
Ctrl - O 
```
To close:
```
Ctrl - x
```
Always test your Cron!!

### Contain model and foreign key with invoices
```php
$this->belongsTo('Orders', [
   'conditions' => [
       'model' => 'Orders',
   ],
   'foreignKey' => 'foreign_key',
]);
```

### Print_r < pre >
```php 
print_r($shoppingCart);die;
print_r($shoppingCart['products'][$productId][$productIndex]);die;
```

### Run Packages
```
php subsites/packages.digitaltalents.be/satis/bin/satis build 
subsites/packages.digitaltalents.be/satis/satis.json 
subsites/packages.digitaltalents.be/satis/web
```

### To be able to Load images inside the Editor
```
bin/cake plugin assets symlink
bin/cake FormManager.Editor CreateFileManagerConfig
```