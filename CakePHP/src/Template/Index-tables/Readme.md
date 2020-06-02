# Personal Documentation
## CakePHP / "Index Tables"

### Index
- [Creating a indexTable](#Creating-a-indexTable)
- [](./)
- [](./)
- [](./)

### Creating a indexTable
```php
$this->element('BackCore.index_table', [
    'model' => 'EmployeeEmployeeCompetences',
    'options' => [
        'cols' => [
            [
                'itemFieldName' => 'employee.name',
                'label' => __('Naam'),
                'sortItemFieldName' => 'ConvertedField_name',
                'width' => 5,
            ],
            [
                'itemFieldName' => 'employee_competence.name',
                'label' => __('Competenties'),
                'width' => 8,
            ],
            [
                'format' => [
                    'format' => 'j-m-Y',
                    'type' => 'date',
                ],
                'itemFieldName' => 'expiration_date',
                'label' => __('Vervaldatum'),
                'width' => 3,
            ],
        ],
        'contain' => [
            'Employees',
            'EmployeeCompetences',
        ],
        'convertedFields' => [
            [
                'alias' => 'name',
                'itemFieldNames' => ['Employees.first_name', '' ,'Employees.last_name'],
                'type' => TableCell::CONVERTED_FIELD_TYPE_CONCAT,
            ],
        ],
        'export' => ['csv'],
        'filters' => [
            [
                'itemFieldName' => [
                    'first_name',
                    'last_name',
                ],
                'type' => TableCell::FILTER_TYPE_KEYWORDS,
            ],
            [
                'itemFieldName' => 'ConvertedField_name',
                'label' => __('Medewerker'),
                'model' => 'Employees',
                'name' => 'ConvertedField_name',
                'type' => TableCell::FILTER_TYPE_SELECT,
            ],
            [
                'itemFieldName' => 'name',
                'label' => __('Competentie'),
                'model' => 'EmployeeCompetences',
                'name' => 'name',
                'type' => TableCell::FILTER_TYPE_SELECT,
            ],
            [
                'defaultFrom' => date('Y-m-d'),
                'defaultTill' => date('Y-m-d'),
                'itemFieldName' => 'expiration_date',
                'label' => __('Periode'),
                'name' => 'period',
                'type' => TableCell::FILTER_TYPE_PERIOD,
            ],
        ],
        'width' => 'fixed',
        'itemUrl' => [
            'itemFieldName' => 'employee_id',
            'url' => [
                'action' => 'detail',
                'controller' => 'Employees',
                'options' => [
                    '#' => 'competences',
                ],
            ],
        ],
    ],
]);
```
