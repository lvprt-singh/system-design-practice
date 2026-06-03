# API Documentation

## Get All Products
**Endpoint:** `GET /api/v1/products`
* **Description:** This call returns a list of products.
* **Required:** Domain, Username, key
* **Returns:** A list of all the products.

## Get Product by SKU
**Endpoint:** `GET /api/v1/products/{sku}`
* **Description:** This call returns the product by SKU.
* **Required:** Key
* **Returns:** sku, product name

## Get Product Stock by SKU
**Endpoint:** `GET /api/v1/products/{sku}/stock`
* **Description:** This call returns the stock by SKU.
* **Required:** key
* **Returns:** sku, product name, stock
