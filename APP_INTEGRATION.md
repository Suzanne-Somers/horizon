# Suzanne Somers App Integration with Horizon Theme

This document explains how the Suzanne Somers custom Shopify app functionality has been integrated into the Horizon theme.

## 📦 Integrated Features

### 1. **Cart Gift Automation** (`snippets/app-extensions/cart-gift.liquid`)
- Automatically adds free gifts to cart based on rules
- Supports cart value thresholds and product triggers
- Handles free samples separately from regular gifts
- **Metafield**: `shop.metafields.cart-gift.rules` and `shop.metafields.free-samples.rules`

### 2. **BOGO Deals** (`snippets/app-extensions/bogo.liquid`)
- Automatically marks items for BOGO discounts
- Works with the bogo-discount Shopify Function
- Marks cheapest items as free (best practice)
- **Metafield**: `shop.metafields.bogo.campaigns`

### 3. **Order Limits** (`snippets/app-extensions/order-limits.liquid`)
- Prevents customers from exceeding product quantity limits
- Shows error messages when limits are exceeded
- Excludes gift items from limit calculations
- **Metafield**: `shop.metafields.order-limit.rules`

### 4. **Free Shipping Bar** (`snippets/app-extensions/free-shipping-bar.liquid`)
- Dynamic progress bar showing progress toward free shipping
- Updates in real-time as cart changes
- Uses Horizon theme CSS variables for styling
- **Metafields**: 
  - `shop.metafields.shipping.free_threshold` (default: 50)
  - `shop.metafields.shipping.message` (default: "Add {amount} more for free shipping!")
  - `shop.metafields.shipping.success_message` (default: "You qualify for free shipping!")

## 🔧 Integration Details

### Theme Layout Integration
All app snippets are included in `layout/theme.liquid` right after the header group:

```liquid
{% comment %} Suzanne Somers App Extensions {% endcomment %}
{% render 'app-extensions/free-shipping-bar' %}
{% render 'app-extensions/cart-gift' %}
{% render 'app-extensions/bogo' %}
{% render 'app-extensions/order-limits' %}
```

### Event System Compatibility
The scripts listen for multiple cart events to ensure compatibility:
- `cart:updated`
- `cart:change`
- `cart:refresh`
- `CartAddEvent` (Horizon's custom event)

They also intercept `fetch` calls to `/cart/add`, `/cart/change`, and `/cart/update` endpoints.

### Horizon Theme Compatibility
- Uses Horizon's CSS variables (`--color-*`, `--page-width`, etc.)
- Compatible with Horizon's cart drawer component
- Triggers cart drawer refresh when cart changes
- Styling matches Horizon's design system

## 🎨 Styling

The free shipping bar uses Horizon's CSS variables:
- `rgb(var(--color-background))` for background
- `rgb(var(--color-border))` for borders
- `rgb(var(--color-button))` for progress bar
- `rgb(var(--color-success))` for qualified state
- `rgb(var(--color-error))` for error messages

## 📝 Configuration

All configuration is done through Shopify metafields, managed via the Suzanne Somers app admin interface:

1. **Free Gifts**: Configure in app → Promos → Free Gifts
2. **BOGO Campaigns**: Configure in app → Promos → BOGO
3. **Order Limits**: Configure in app → Settings → Order Limits
4. **Free Shipping**: Configure via metafields (can be added to app settings)

## ✅ Testing Checklist

- [ ] Free gifts add automatically when cart threshold is met
- [ ] Free gifts remove when cart value drops below threshold
- [ ] BOGO items are marked correctly in cart
- [ ] Order limits prevent exceeding maximum quantities
- [ ] Free shipping bar updates dynamically
- [ ] Cart drawer refreshes when items are added/removed
- [ ] All functionality works on mobile devices
- [ ] No JavaScript errors in browser console

## 🚀 Deployment

1. Upload the Horizon theme to your Shopify store
2. Ensure the Suzanne Somers app is installed and configured
3. Verify metafields are populated with rules/campaigns
4. Test all functionality in a staging environment
5. Deploy to production

## 📞 Support

For issues or questions about the app functionality, refer to the main app documentation or contact support through the Suzanne Somers app admin interface.
