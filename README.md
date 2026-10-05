# Add to Cart Pop-up - Magento 2 Hyvä

## Overview

The task is assigned to implement a pop-up after a product is 
added to the cart from the product detail page (PDP). The solution is
AJAX-based, and it implemented using Hyva, Alpine.js and TailwindCSS.

After successfully adding a product to the cart, the pop-up opens and
displays the product name and image, and provides options to continue 
shopping or navigate to the shopping cart 

## Implementation

### Popup

The pop-up is implemented in:

`app/design/frontend/Custom/HyvaTheme/Magento_Catalog/templates/product/view/add-to-cart-popup.phtml`

The template uses Magento's `Product\View` block to access the current
product and its image.

Alpine.js `open` property is used to manage the open and close 
state of the pop-up. The pop-up can be closed by:

- Clicking "Continue shopping"
- Clicking the overlay
- Pressing `Escape`

### Add to Cart

The PDP add-to-cart form is replaced by Alpine.js and submitted using 
`fetch()` instead of a normal form submission

This way, by using AJAX request, the page is not refreshed and the 
pop-up can appear immediately after adding a product without being
destroyed of refreshing the page.

If Magento returns a `backUrl`, the implementation is designed to 
follow the default Magento redirect behavior. instead of displaying 
the pop-up.

If the AJAX request fails, the implementation falls back to Magento's
normal form submission, page is refreshed and the product is added.

### Mini Cart

The initial AJAX implementation did not update the mini cart immediately
after adding a product, requiring a page refresh.

The first solution considered was using `location.reload()`, but this
would unnecessarily refresh the page. Instead, Hyvä's section data
functionality was used to reload the customer section data after a
successful add-to-cart request.

The `reload-customer-section-data` event is dispatched to keep the mini
cart synchronized with the newly added product.

Reference:
[Hyvä section data documentation](https://docs.hyva.io/hyva-themes/writing-code/working-with-sectiondata.html#sectiondata-in-a-nutshell)

### Styling

TailwindCSS for styling and responsive behavior.

On smaller screens, the pop-up is displayed on the most of the available 
screen width, while at larger screens has a fixed maximum width. Buttons 
are displayed on a row for larger screens, and it changes to column on 
a smaller screens.  

