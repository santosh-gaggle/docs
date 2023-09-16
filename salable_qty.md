# Salable Quantity in Magento 2 with MSI

In Magento 2, the concept of "Salable Quantity" is a crucial aspect of Multi-Source Inventory (MSI) and is used to manage and represent the available quantity of a product for sale across multiple sources (warehouses, stores, etc.). Let's dive into the details of how Salable Quantity works in Magento 2 with an example:

## Definition
Salable Quantity represents the quantity of a product that is available for sale, considering various factors such as stock from different sources, reservations for orders, and other constraints.

## Example
Suppose you have a product called "Widget X" available in your Magento 2 store, and you have set up two sources:

- Default Source: This is the primary source for the product, such as your main warehouse.
- Secondary Source: This could be another warehouse or a physical store.

### Let's go through the steps of how Salable Quantity works:

#### Initial Stock
- Default Source: 100 units of Widget X.
- Secondary Source: 50 units of Widget X.

#### Salable Quantity Calculation
Initially, both sources have stock, so the total salable quantity is the sum of their stocks: 100 + 50 = 150 units.

#### Order Placement
A customer places an order for 20 units of Widget X.
Magento's MSI system reserves 20 units from the available sources. Assuming the default source is selected first due to its higher priority, it reserves 20 units from the Default Source.

#### Salable Quantity Update
After the reservation, the available quantity is updated:
- Default Source: 100 units - 20 units (reserved) = 80 units.
- Secondary Source: 50 units (unchanged).

#### Salable Quantity Replenishment
Now, suppose you receive a shipment of 30 units of Widget X in your Secondary Source (warehouse or store).
The available quantity in the Secondary Source becomes 50 units (initial stock) + 30 units (replenishment) = 80 units.

#### Salable Quantity Recalculation
Magento continuously recalculates the salable quantity based on available stock and reservations.
The Salable Quantity becomes:
- Default Source: 80 units (unchanged).
- Secondary Source: 80 units (available stock) - 20 units (reserved) = 60 units.

#### Multiple Orders
If more orders are placed, Magento will continue to allocate stock from sources based on their priority until one source runs out. It will then move to the next source in priority.

#### Out of Stock
If the Default Source runs out of stock, and there are still reservations, Magento will automatically allocate stock from the Secondary Source to fulfill the remaining orders, maintaining the salable quantity.

This process ensures that your store can efficiently manage inventory across multiple sources and provide accurate information to customers regarding product availability.

Keep in mind that the actual implementation and behavior may vary depending on your specific configuration, extensions, and customizations in Magento 2. The MSI system provides flexibility to handle complex inventory scenarios effectively.
