# Creating a Custom Shipping Method in Magento 2

Creating a custom shipping method in Magento 2 involves several steps. This process allows you to add a unique shipping method to your e-commerce store. Here's an overview of the steps involved:

1. **Create a Custom Module:**

   First, you need to create a custom module for your shipping method. This module should include the necessary files and configurations for your custom shipping method.

2. **Define the Shipping Carrier:**

   In your custom module, define the shipping carrier by extending the `Magento\Shipping\Model\Carrier\AbstractCarrier` class. You'll need to implement the required methods, including `collectRates` and `proccessAdditionalValidation`.

3. **Configure the Shipping Method:**

   Configure your custom shipping method by specifying its title, method name, and other details in the module's configuration XML.

4. **Implement the Rate Calculation:**

   In the `collectRates` method of your shipping carrier class, implement the logic for calculating shipping rates based on factors such as destination, weight, dimensions, and any custom conditions.

5. **Manage Shipping Rates:**

   You can define the shipping rates for your custom method using various conditions and pricing rules. These rates should be returned as a result of the `collectRates` method.

6. **Define Admin Configuration:**

   Create a configuration section in the admin panel where you can enable or disable your custom shipping method and configure its settings.

7. **Create a Shipping Model:**

   Implement a shipping model that contains the logic for retrieving and displaying shipping rates to customers during the checkout process.

8. **Implement Frontend Integration:**

   Update your frontend templates to display the custom shipping method and its rates to customers during the checkout process.

9. **Testing:**

   Thoroughly test your custom shipping method to ensure it calculates rates correctly, displays them to customers, and integrates seamlessly with the checkout process.

10. **Documentation:**

    Create documentation for your custom shipping method to explain its functionality, configuration options, and any specific requirements for use.

11. **Deployment:**

    Deploy your custom module to your Magento 2 store and configure it as needed through the admin panel.

12. **Maintenance:**

    Regularly maintain and update your custom shipping method as needed to ensure compatibility with Magento updates and to address any issues that may arise.

Creating a custom shipping method in Magento 2 is a complex task that requires a good understanding of Magento's architecture and development practices. It's essential to follow Magento's coding standards and best practices to create a robust and reliable shipping method for your online store.


# Detailed info ---> [Custom Shipping Method](https://devdocs.magento.com/guides/v2.3/howdoi/checkout/checkout-add-custom-carrier.html)

