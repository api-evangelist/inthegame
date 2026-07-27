---
name: Configure a shop product and let a viewer buy it
description: Admin creates a shop product; viewer lists products and purchases with points.
api: openapi/inthegame-openapi.yml
operations: [adminapiShopCreate, userapiShopGetproducts, userapiShopBuyitem]
---

# Configure a shop product and let a viewer buy it

## Auth
Admin creation uses the `Authorization` header token; viewer steps use `userToken` in the body.

## Steps
1. **Create the product** — `adminapiShopCreate` (`POST /adminApi/shop/create`) with product name, cost (points) and asset.
2. **Viewer lists products** — `userapiShopGetproducts` (`POST /userApi/shop/getProducts`) with `userToken`.
3. **Viewer buys** — `userapiShopBuyitem` (`POST /userApi/shop/buyItem`) with the product id and `userToken`; points are deducted.

## Rules
- Purchases are not idempotent — do not retry blindly on timeout; re-list to confirm.
- 404 = unknown product; 500 = server error. See `errors/inthegame-problem-types.yml`.
