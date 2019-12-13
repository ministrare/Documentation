# Personal Documentation
## CakePHP / "Detail Item"

### Index
- [Creating a detailItem](#Creating-a-detailItem)
- [](./)
- [](./)
- [](./)

### Creating a detailItem
```php
$this->_detailItem('Employees', $id, 'employee', [
    'getItemContain' => [
           'Users',
           'EmployeeEmployeeCompetences',
           'EmployeePrices',
       ],
       'saveSubItems' => [
           'employee_employee_competences' => [
               'model' => 'EmployeeEmployeeCompetences',
           ],
           'employee_prices' => [
               'model' => 'EmployeePrices',
           ]
       ],
   ]);

```
When creating a detail item, the element needs to know what model to cast to a index table.
This will be defined by the first property inside the function ``'Employees' ``, the second property will define the ID shown in the URL (index of table) ``$id``.
The last parameter will be the name used inside the view in order to access this model ``employee``.

The first next thing we come across is the ``getItemContain``.
This is used to collect all the data from the associated models/tables.
This way we dont have to call this information from inside the controller. 

In order to save these models, we need to add the ``'saveSubItems'``.
This will tell the controller to store all the associated models on update as well.
