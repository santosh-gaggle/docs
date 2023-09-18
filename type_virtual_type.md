# Type configuration


Type configurations describe an object's lifestyle and how to instantiate it.

You can configure the type in your di.xml configuration node in the following ways:

```xml
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <virtualType name="moduleConfig" type="Magento\Core\Model\Config">
        <arguments>
            <argument name="type" xsi:type="string">system</argument>
        </arguments>
    </virtualType>
    <type name="Magento\Core\Model\App">
        <arguments>
            <argument name="config" xsi:type="object">moduleConfig</argument>
        </arguments>
    </type>
</config>
```


The preceding example declares the following types:

- **moduleConfig**: A virtual type that extends the type `Magento\Core\Model\Config`.
- **Magento\Core\Model\App**: All instances of this type receive an instance of `moduleConfig` as a dependency.

# Virtual Types in Magento 2

A `virtual type` allows you to change the arguments of a specific injectable dependency and change the behavior of a particular class. This allows you to use a customized class without affecting other classes that have a dependency on the original.

The example creates a virtual type for `Magento\Core\Model\Config` and specifies `system` as the constructor argument for type.


## Changing Namespace for `Magento\Framework\Session\Storage`

For example, the `Magento\Framework\Session\Storage` class takes a `$namespace` argument in its constructor, which defaults to the value `'default'`. You can use the type definition in your XML configuration to change the namespace to `'core'` as follows:

```xml
<type name="Magento\Framework\Session\Storage">
    <arguments>
        <argument name="namespace" xsi:type="string">core</argument>
    </arguments>
</type>
```


The above configuration would make it so that all instances of `Magento\Framework\Session\Storage` have a namespace of `core`. This is useful for altering a class's behavior without modifying its source code.

## Creating a Virtual Type

In Magento 2, you can use a virtual type to create a customized instance of a class with altered argument values. Here's how it works:

```xml

<virtualType name="Magento\Core\Model\Session\Storage" type="Magento\Framework\Session\Storage">
    <arguments>
        <argument name="namespace" xsi:type="string">core</argument>
    </arguments>
</virtualType>

```

In this example, we create a virtual type named `Magento\Core\Model\Session\Storage` based on the `Magento\Framework\Session\Storage` class. We alter the `namespace` argument to `core`. This virtual type can now be injected into other classes.


## Injecting Virtual Types

To inject a virtual type into another class, you can use a type configuration like this:

```xml

<type name="Magento\Framework\Session\Generic">
    <arguments>
        <argument name="storage" xsi:type="object">Magento\Core\Model\Session\Storage</argument>
    </arguments>
</type>

```

In this configuration, we inject the virtual type `Magento\Core\Model\Session\Storage` into `Magento\Framework\Session\Generic`. This allows `Magento\Framework\Session\Generic` to be customized without affecting other classes that also declare a dependency on `Magento\Framework\Session\Storage`.

Virtual types provide a powerful mechanism for customizing dependencies in Magento 2, making it easier to adapt and extend the behavior of classes within the framework.


# Magento2: Difference between Type and Virtual Type

- Virtual Types are the classes that are instantiated by the ObjectManager but have no physical / concrete class located in /app/code or in /generated.
- Type adjusts existing classes, whereas Virtual Types creates a new class.
- Type affect all the instances, whereas Virtual Type is equivalent of a sub class, where only the sub class has the altered argument values.
- Virtual Type must extend a concrete class type.
- To adjust the constructor parameters for an existing class, use the type declaration. These changes take effect for all class instantiations for that object in that area.
- To create an entirely new type of object that can be injected elsewhere use the virtual type declaration. You need to then inject or utilize this class in XML configuration (in di.xml or as a block).
- Virtual types are useful if the only necessary modifications to a class are the classes that are injected.
- You can’t reference the specific virtual type in a class’ constructor. Rather, your constructor should utilize the concrete class type the virtual type extends.
- You can’t write a class that inherits a virtual type.



# For more info ---> [Virtual types on abode example](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/build/di-xml-file.html)


