- Query 1: Nearest Hub with Available Stock for Customer 1 and SKU 1 
```sql
SELECT 
    h.hub_id,
    h.hub_name,
    ST_Distance(
        ST_SetSRID(ST_MakePoint(c.customer_longitude, c.customer_latitude), 4326),
        ST_SetSRID(ST_MakePoint(h.hub_longitude, h.hub_latitude), 4326)
    ) AS distance
FROM 
    Hubs h
JOIN 
    Inventories i ON h.hub_id = i.hub_id
JOIN 
    Customers c ON c.customer_id = 1 -- Customer 1
WHERE 
    i.sku_id = 1 -- SKU 1
    AND i.quantity > 0
ORDER BY 
    distance
LIMIT 1;
```
Query 2: Top 10 Customers by Total Value of Orders, Grouped by Tenant
```sql
SELECT 
    t.tenant_id,
    c.customer_id,
    c.customer_name,
    SUM(o.order_value) AS total_order_value
FROM 
    Customers c
JOIN 
    Orders o ON c.customer_id = o.customer_id
JOIN 
    Tenants t ON c.tenant_id = t.tenant_id
GROUP BY 
    t.tenant_id, c.customer_id, c.customer_name
ORDER BY 
    total_order_value DESC
LIMIT 10;
```
Query 3: Customers with More Than 3 Unique SKUs
```sql
SELECT 
    c.customer_id,
    c.customer_name,
    COUNT(DISTINCT oi.sku_id) AS unique_sku_count
FROM 
    Customers c
JOIN 
    Orders o ON c.customer_id = o.customer_id
JOIN 
    OrderItems oi ON o.order_id = oi.order_id
GROUP BY 
    c.customer_id, c.customer_name
HAVING 
    COUNT(DISTINCT oi.sku_id) > 3;
```
Order Routing:
Question: Write a pseudocode to assign orders to hubs based on stock availability and customer proximity. If stock is unavailable in the nearest hub, check the next nearest hub.
```
FUNCTION assign_order_to_hub(customer_id, sku_id, quantity):
    // Step 1: Get customer location
    customer_location = GET_CUSTOMER_LOCATION(customer_id)
    
    // Step 2: Get all hubs with the required SKU in stock
    hubs_with_stock = GET_HUBS_WITH_STOCK(sku_id, quantity)
    
    // Step 3: If no hubs have enough stock, return an error
    IF hubs_with_stock IS EMPTY:
        RETURN "Error: No hubs have sufficient stock for this SKU."
    
    // Step 4: Sort hubs by proximity to the customer
    sorted_hubs = SORT_HUBS_BY_DISTANCE(hubs_with_stock, customer_location)
    
    // Step 5: Iterate through sorted hubs and assign the order to the nearest hub with stock
    FOR hub IN sorted_hubs:
        IF hub.quantity >= quantity:
            // Assign the order to this hub
            ASSIGN_ORDER_TO_HUB(customer_id, sku_id, quantity, hub.hub_id)
            RETURN "Order assigned to hub: " + hub.hub_name
        ELSE:
            // If stock is insufficient, move to the next nearest hub
            CONTINUE
    
    // Step 6: If no hub has enough stock, return an error
    RETURN "Error: No hubs have sufficient stock for this order."
END FUNCTION
```
