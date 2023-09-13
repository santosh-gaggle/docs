# Injectable and Newable/Non-Injectable Objects in Magento

In Magento, objects can be categorized into two main types based on their usage and lifecycle: **Injectable** and **Newable/Non-Injectable** objects. Understanding the difference between these two types is essential for efficient object management.

## Injectable Objects

**What are Injectable Objects?** Injectable objects are like pre-made, reusable tools that you can use in your code. They are created and managed by Magento's object manager using configuration from the `di.xml` file.

**Example:** Think of them like a toolbox with various tools like a screwdriver, a wrench, and a hammer. You can use these tools (injectable objects) in your code wherever you need them.

**Result:** When you ask for an injectable object, Magento provides you with a single, consistent instance of that object throughout your application. It's like always getting the same screwdriver from the toolbox.

**For instance:** Consider a `ProductDetailsFetcher` object in Magento. It's an injectable object that retrieves details of a product. You can use it throughout your code because it's reusable, and Magento manages it.

## Newable/Non-Injectable Objects

**What are Newable/Non-Injectable Objects?** These objects are like one-time use tools. Every time you need them, you create a brand new instance of that tool (object).

**Example:** Imagine you have a special tool that can only be used once because it needs specific input each time. You can't use the same tool for different tasks. These are newable/non-injectable objects.

**Result:** When you ask for a newable object, Magento creates a fresh instance of that object for you each time. It's like borrowing a tool from a friend and returning it after you're done because you can't use the same tool for a different task.

**For instance:** Continuing with the `Product` object in Magento:

- **Injectable:** You might have an injectable object like `ProductDetailsFetcher` that retrieves details of a product. You can use it throughout your code because it's reusable, and Magento manages it.

- **Newable/Non-Injectable:** But when you need a specific product, like `Product #123`, you can't inject it directly because it's unique. Instead, you ask for a "factory" that can create a new `Product` object with the specific ID you need. This way, you get a fresh `Product` object every time you request it.

Here's how you might request an injectable and a newable object in code:

```php
// Asking for an injectable object (pre-made tool)
$detailsFetcher = $objectManager->get(ProductDetailsFetcher::class);
$productDetails = $detailsFetcher->getDetails(123);

// Asking for a newable object (creating a new tool instance)
$productFactory = $objectManager->create(ProductFactory::class);
$product123 = $productFactory->create()->load(123);
```

In the code above:

`ProductDetailsFetcher` is an injectable object, and you can use it directly.
`ProductFactory` is a factory for creating newable `Product` objects, and you need to specify which product you want by providing an ID (in this case, 123).
This distinction is crucial because it helps Magento manage objects efficiently, ensuring that injectable objects are reused when possible, while newable objects are created anew each time they're needed.