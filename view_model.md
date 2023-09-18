# View Models in Magento 2

**View models** in Magento 2 are a way to offload functionality and business logic from block classes into separate classes, making it easier to maintain, test, and reuse code. They serve as an abstraction layer between the block and the template in the presentation layer.


> In Magento 2, a view model is a PHP class that acts as an intermediary between a block and its corresponding template in the presentation layer. View models are used to enhance the separation of concerns within the code and improve the maintainability and reusability of Magento components.


## When to Use View Models

Use view models when you need to inject functionality into template files, and your code doesn't need to be compatible with older versions of Magento. View models are available in Magento 2.2 onwards and are recommended over using helpers in templates.

## Creating View Models

To create a view model, follow these steps:

1. Pass the view model class as an argument to a template's block in the page layout configuration file. Here's an example:

   ```xml
   <block name="examplecorp.new.viewmodel" template="ExampleCorp_Catalog::example.phtml">
       <arguments>
           <argument name="view_model" xsi:type="object">ExampleCorp\Catalog\ViewModel\MyNewViewModel</argument>
       </arguments>
   </block>
   ```

Ensure that the view model class implements the \Magento\Framework\View\Element\Block\ArgumentInterface interface.

```php
namespace ExampleCorp\Catalog\ViewModel;

class MyNewViewModel implements \Magento\Framework\View\Element\Block\ArgumentInterface
{
    public function getTitle()
    {
      return 'Hello World';
    }
}
```

### In your template, access the public methods of the view model class like this:

```php

<?php
/** @var $viewModel \ExampleCorp\Catalog\ViewModel\MyNewViewModel */
$viewModel = $block->getViewModel();
?>
<h1><?= $block->escapeHtml($viewModel->getTitle()); ?></h1>
```

## Why Create View Models?

1. **Separation of Concerns**: View models help maintain a clear separation of concerns in the codebase. They separate the business logic and data preparation from the presentation layer, ensuring that each component has a distinct responsibility.

2. **Code Reusability**: View models can be reused across multiple templates or blocks. When multiple blocks or templates require similar data processing or transformation, a view model can centralize this logic for reuse, reducing code duplication.

3. **Maintainability**: By encapsulating data-related operations in view models, it becomes easier to maintain and update the code. Changes to data retrieval or manipulation logic can be made in the view model without affecting the template or block.

4. **Testability**: View models are highly testable since they contain business logic that can be unit-tested independently. This ensures that the logic behaves as expected, improving the overall quality of the code.

## Use Cases of View Models:

1. **Data Formatting**: View models are commonly used to format and prepare data for presentation. For example, converting timestamps into human-readable dates or currencies into localized formats.

2. **Data Retrieval**: They can fetch data from various sources, such as databases or external APIs, and prepare it for rendering in the template. This includes querying the database for product information or fetching customer data.

3. **Conditional Logic**: View models can implement conditional logic to determine which data to display or how to display it based on specific conditions or user preferences.

4. **Data Transformation**: They can transform raw data into a structured format suitable for rendering. For instance, converting a collection of database records into an array of objects that can be easily iterated in a template.

## Simple Use Example:

Let's say you have a product detail page in your e-commerce store. You need to display the product name, price, and description. You create a view model for this page.

In the view model:

- You retrieve the product information from the database.
- You format the price to include the currency symbol and decimal places.
- You prepare the description by stripping any HTML tags and limiting the character count.
- You make decisions like whether to display a discount badge based on the product's pricing.

In the template, you simply render the data provided by the view model without worrying about how the data was fetched, formatted, or processed. This separation of concerns and encapsulation of logic makes your code more organized and easier to maintain.

In summary, view models in Magento 2 are essential for maintaining clean, maintainable, and reusable code. They separate data-related logic from presentation, making your codebase more structured and easier to work with, while also improving testability and code quality.
