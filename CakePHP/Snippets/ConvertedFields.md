### Examples 3
```php
public function addConvertedFieldProductLocations($convertedField, Query $query, &$convertedFields)
{
    $distinctLocations = TableRegistry::get('pick_order_products')->find()
        ->select(['locations' => 'distinct(stock_products.location_id)'])
        ->epilog('
            join products on ((products.id = pick_order_products.product_id or products.parent_id = pick_order_products.product_id) and products.deleted is null)
            join stock_products on 
                  (stock_products.product_id = products.id 
                     AND 
                     (
                        (stock_products.package_type_id = pick_order_products.package_type_id 
                        OR 
                        ( stock_products.package_type_id is null AND pick_order_products.package_type_id is null)                               
                     )   
                        OR (products.parent_id is not null and stock_products.package_type_id is null)
                         )                     
                    and stock_products.amount > 0 )
              where 
                pick_order_products.pick_order_id = PickOrders.id 
                AND(
                  pick_order_products.amount > pick_order_products.amount_picked 
                  OR pick_order_products.amount_picked is null
                )');
    $locations = TableRegistry::get('Locations')->find()
        ->select(['product_locations' => "GROUP_CONCAT(distinct(building) SEPARATOR ', ')"])
        ->where([
            'OR' => [
                'id in' => $distinctLocations,
            ]
        ]);
    $convertedFields['ConvertedField_' . $convertedField['alias']] = $locations;
    $query->select(['ConvertedField_' . $convertedField['alias'] => $locations]);
}

```