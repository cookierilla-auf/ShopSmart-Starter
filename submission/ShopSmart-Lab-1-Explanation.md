# ShopSmart Laboratory 1 Explanation

## Product data flow

The frontend calls `api.getProducts()` inside `ProductsPage`. The resolved API response supplies `data.products`, which is stored in the `products` React state variable. `ProductsPage` filters that array into `visibleProducts`, maps the matching objects into `ProductCard` components, and passes each product through the `product` prop. Each card then renders the product name, description, price, stock, and cart action.

## JavaScript and React concepts

| Concept | Code location or name | What it does |
| --- | --- | --- |
| Variable | `products` in `ProductsPage` | Stores the product array returned by the API and causes the page to rerender when it changes. |
| Array method | `products.filter(...)` | Creates `visibleProducts` by applying the search and category conditions. |
| Function | `calculateCartItemCount(cart)` | Adds every cart item's quantity and returns zero for an empty cart. |
| Condition | The stock `if / else if / else` branches in `ProductCard` | Selects the out-of-stock, low-stock, or regular-stock message. |
| Event handler | `onClick={() => onAddToCart(product)}` | Responds to the Add to cart button and sends the selected product to the cart action. |

## React events and state

Clicking Add to cart runs the product card's click handler, which calls the `addToCart` action supplied by `CartContext`. That action uses the functional form of `setCart` to derive the next cart from the current cart. React then rerenders the provider, recalculates `itemCount` and `total`, and updates consumers such as the navigation and cart page.

The search input follows the same event-to-state pattern. Its `onChange` handler updates `search`, which rerenders `ProductsPage`. The new normalized query is applied to the product name and description, while the selected category remains a required condition.

## Implemented improvements

Product search now finds a trimmed, case-insensitive query in either the product name or description. The category filter is still combined with the search result, so selecting a category continues to narrow the catalog.

Product cards now display `Out of stock` for zero stock, `Only N left` for stock from 1 through 8, and `N in stock` for stock of 9 or more. The existing unavailable styling and disabled Add to cart behavior remain unchanged.

Cart item counting is now implemented once in `calculateCartItemCount`. `CartContext` exposes the calculated value as `itemCount`, and both the navigation and order summary read that shared value. The price total remains a separate calculation based on price multiplied by quantity.

## Verification evidence

- `01-description-search.png` shows a description-only search result.
- `02-low-stock-label.png` shows the low-stock message.
- `03-cart-item-count.png` shows synchronized navigation and order-summary counts after increasing quantity.
- `npm-run-check.txt` records the successful lint, test, and production-build checks.
