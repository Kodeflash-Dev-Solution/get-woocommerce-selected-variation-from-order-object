# get-woocommerce-selected-variation-from-order-object
```php
// Get an instance of the WC_Order object from an Order ID
 $order = wc_get_order( $order_id ); 

// Loop though order "line items"
foreach( $order->get_items() as $item_id => $item ){
  $product_id   = $item->get_product_id(); //Get the product ID
  $quantity     = $item->get_quantity(); //Get the product QTY
  $product_name = $item->get_name(); //Get the product NAME

  // Get an instance of the WC_Product object (can be a product variation  too)
  $product      = $item->get_product();

   // Get the product description (works for product variation too)
  $description  = $product->get_description();

  // Only for product variation
  if( $product->is_type('variation') ){

    // Get the variation attributes
    $variation_attributes = $product->get_variation_attributes();

    // Loop through each selected attributes
    foreach($variation_attributes as $attribute_taxonomy => $term_slug ){

      // Get product attribute name or taxonomy
      $taxonomy = str_replace('attribute_', '', $attribute_taxonomy );
      // The label name from the product attribute
      $attribute_name = wc_attribute_label( $taxonomy, $product );

      // The term name (or value) from this attribute
      if( taxonomy_exists($taxonomy) ) {

          $attribute_value = get_term_by( 'slug', $term_slug, $taxonomy )->name;

      } else {

          $attribute_value = 
          $term_slug; // For custom product attributes

      }
    }
  }
}

// OR

$order_id = XXX;

$order = wc_get_order( $order_id ); //returns WC_Order if valid order 
$items = $order->get_items();   //returns an array of WC_Order_item or a child class (i.e. WC_Order_Item_Product)

foreach( $items as $item ) {

  //returns the type (i.e. variable or simple)
  $type = $item->get_type();

  //the product id, always, no matter what type of product
  $product_id = $item->get_product_id();

  //a default
  $variation_id = false;

  //check if this is a variation using is_type
  if( $item->is_type('variable') ) {
      $variation_id = $item->get_variation_id();
  }
}
```
