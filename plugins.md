# Magento 2 Plugins: Extending Functionality

In Magento 2, **plugins**, also known as interceptors, are a powerful way to customize and extend the behavior of the application without modifying the core code. They allow you to inject your custom code before, after, or around the execution of a Magento 2 class method. This concept is similar to the aspect-oriented programming (AOP) technique.

## Key Details About Magento 2 Plugins

### Purpose of Plugins

1. **Extensibility**: They enable you to extend the behavior of a class method without modifying its source code, making your customizations modular and upgrade-safe.

2. **Functionality Enhancement**: You can use plugins to add, modify, or remove parameters and return values of methods, effectively enhancing their functionality.

3. **Observation**: Plugins allow you to observe and react to specific events or method calls in the system, making them a valuable tool for event-driven development.

### Plugin Types

There are three types of plugins in Magento 2, each with a specific purpose:

1. **Before Plugin**: Executes before the original method. You can modify method parameters and the returned result.

2. **After Plugin**: Executes after the original method. You can modify the returned result but not method parameters.

3. **Around Plugin**: Wraps around the original method, allowing you to control whether the original method is executed and what it returns. You can modify both parameters and return values.

### Plugin Limitations

While plugins are versatile, they have some limitations:

- Plugins cannot be used on:
  - Final methods
  - Final classes
  - Non-public methods
  - Class methods (such as static methods)
  - __construct and __destruct
  - Virtual types
  - Objects that are instantiated before Magento\Framework\Interception is bootstrapped
  - Objects that implement Magento\Framework\ObjectManager\NoninterceptableInterface
  

- Plugins may affect performance, especially when multiple plugins are involved in the same method call. Careful consideration should be given to performance optimizations.

### Plugin Execution Order

Plugins are executed in a specific order determined by their sequence declarations in the `di.xml` file. You can control this order by specifying the `before`, `after`, or `around` attribute in the XML configuration.

### Plugin Example

Here's an example of how to create a simple plugin in Magento 2:

### Before Plugin

```php
// Plugin class
class MyPlugin
{
    public function beforeExecute(\Magento\Catalog\Controller\Product\View $subject, $param1, $param2)
    {
        // Code to execute before the original method
        // Modify $param1 and $param2
        $param1 = 'Modified ' . $param1;
        $param2 = 'Modified ' . $param2;
        return [$param1, $param2];
    }
}
```
### After Plugin

```php
// Plugin class
class MyPlugin
{
    
    public function afterExecute(\Magento\Catalog\Controller\Product\View $subject, $result)
    {
        // Code to execute after the original method
        // You can modify the returned result if needed
        return $result . ' Modified';
    }

}
```
### Around Plugin

```php
// Plugin class
class MyPlugin
{

    public function aroundExecute(\Magento\Catalog\Controller\Product\View $subject, $proceed, $param1, $param2)
    {
        // Perform actions before the original method
        // You can choose whether to call the original method
        $result = $proceed($param1, $param2);
        // Perform actions after the original method
        return $result;
    }
}
```

# Declaring a Plugin

To declare a plugin in Magento 2, you need to add an entry in the `di.xml` configuration file specifying the plugin's type, class, and methods to intercept.

```xml
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <type name="Magento\Catalog\Controller\Product\View">
        <plugin name="myPlugin" type="Vendor\Module\Plugin\MyPlugin" sortOrder="10" disabled="false"/>
    </type>
</config>
```


In this example, the plugin `MyPlugin` is associated with the `View` method of the `Magento\Catalog\Controller\Product\View` class.

Magento 2 plugins are a fundamental part of the customization and extension capabilities of the platform, allowing developers to tailor the behavior of the system to specific requirements without modifying the core code.