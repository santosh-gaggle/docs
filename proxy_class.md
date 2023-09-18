# Magento 2 Proxy

A proxy in Magento 2 is a placeholder class that represents another class, allowing for deferred or lazy loading of that class. Proxies are used to enhance performance and efficiency by loading the actual class only when necessary, reducing resource consumption.

## Example

Let's consider an example where you have a heavy or resource-intensive class called `ExpensiveClass`. This class is not needed immediately when your application starts but may be required at some point during its execution.

Without a proxy, `ExpensiveClass` would be instantiated as soon as the application starts, consuming resources even if it's not immediately used. However, by using a proxy, you can defer the instantiation until the moment it's needed.

Here's how you can use a proxy in Magento 2:

1. Define the proxy in your constructor:

   ```php
   protected $expensiveClass;
   
   public function __construct(
       \Vendor\Module\Model\ExpensiveClassFactory $expensiveClassFactory
   ) {
       $this->expensiveClass = $expensiveClassFactory->create();
   }
   ```

When you need to use ExpensiveClass, you can do so like this:

```php
$this->expensiveClass->someMethod();

```

With this setup, ExpensiveClass is only instantiated when you call a method or access a property on the $expensiveClass object. Until then, a lightweight proxy is used in its place.

# Benefits

### The use of proxies in Magento 2 provides several benefits:

- **Lazy Loading**: Actual classes are loaded only when they are required, improving performance.
- **Resource Optimization**: Resource-intensive classes are not loaded prematurely, conserving memory and CPU resources.
- **Efficiency**: The application becomes more efficient by deferring the creation of objects until they are needed.

 `Proxies` are a valuable tool in Magento 2 for optimizing system performance while maintaining flexibility in class instantiation.