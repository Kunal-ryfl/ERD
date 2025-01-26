![erd](https://github.com/user-attachments/assets/e25846fb-cc0b-427f-a9df-ac927b83c40b)

- Hubs - Tenants
hubs.tenant_id is a foreign key referencing tenants.tenant_id.
Relationship: A tenant can have multiple hubs.

- Inventories - Hubs

inventories.hub_id is a foreign key referencing hubs.hub_id.
Relationship: A hub can have multiple inventory records.

- Inventories - SKUs

inventories.sku_id is a foreign key referencing skus.sku_id.
Relationship: Each inventory record is linked to a specific SKU.

- SKUs - Products

skus.product_id is a foreign key referencing products.product_id.
Relationship: A product can have multiple SKUs.

- SKUs - Tenants

skus.tenant_id is a foreign key referencing tenants.tenant_id.
Relationship: A tenant can own multiple SKUs.

- Products - Categories

products.category_id is a foreign key referencing categories.category_id.
Relationship: A category can have multiple products.

- Reviews - Products

reviews.product_id is a foreign key referencing products.product_id.
Relationship: A product can have multiple reviews.

- Reviews - Customers

reviews.customer_id is a foreign key referencing customers.customer_id.
Relationship: A customer can write multiple reviews.

- Media - SKUs

media.sku_id is a foreign key referencing skus.sku_id.
Relationship: An SKU can have multiple media assets.

- Orders - Customers

orders.customer_id is a foreign key referencing customers.customer_id.
Relationship: A customer can place multiple orders.

- Order Items - Orders

order_items.order_id is a foreign key referencing orders.order_id.
Relationship: An order can contain multiple items.

- Order Items - SKUs

order_items.sku_id is a foreign key referencing skus.sku_id.
Relationship: An SKU can appear in multiple order items.

- Payments - Orders

payments.order_id is a foreign key referencing orders.order_id.
Relationship: An order can have multiple associated payments.

- Invoices - Orders

invoices.order_id is a foreign key referencing orders.order_id.
Relationship: An order can have one invoice.

- Addresses - Polymorphic Relation
The addresses table uses type_name (to specify the entity type, e.g., customer, hub, etc.) and type_id (to specify the corresponding entity's ID).
