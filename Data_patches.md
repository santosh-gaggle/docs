# Magento 2 Data Patches

In Magento 2, data patches are used to apply data modifications or updates to the database. These patches are primarily used when you need to make changes to existing data, such as adding, modifying, or deleting records in database tables. Data patches are a part of Magento's declarative schema and data schema approach, which allows you to manage database schema and data changes through XML files and PHP scripts.

## How Data Patches Work

### Creation of Data Patches:

- Data patches are typically created in custom modules or extensions.
- Each data patch is associated with a specific module.

### XML Declaration:

- Data patches are declared in XML files within the module's `etc/` directory.
- In the XML file, you specify the target version of Magento to which the patch should be applied.

### PHP Implementation:

- Data patches are implemented as PHP classes within the module.
- These classes must implement the `\Magento\DataPatch\DataPatchInterface` interface.
- The PHP class contains the logic for the data modification or update.

### Patch Execution:

- When Magento is upgraded or when you apply updates, it checks the XML declarations to determine which data patches need to be executed based on the module's target version.
- Magento executes the data patches in the specified order.

### Database Operations:

- Inside the data patch class, you can use Magento's database abstraction layer to perform various database operations, such as inserting, updating, or deleting records.

### Versioning:

- Data patches are versioned, allowing you to manage changes and ensure that each patch is only executed once.

## Example Data Patch

Here's a simplified example of a data patch:

```php
<?php
namespace Vendor\Module\Setup\Patch\Data;

use Magento\Framework\Setup\Patch\DataPatchInterface;
use Magento\Framework\Setup\Patch\PatchVersionInterface;
use Magento\Framework\Setup\Patch\PatchRevertableInterface;
use Magento\Framework\Setup\ModuleDataSetupInterface;

class AddSampleData implements
    DataPatchInterface,
    PatchVersionInterface,
    PatchRevertableInterface
{
    private $moduleDataSetup;

    public function __construct(ModuleDataSetupInterface $moduleDataSetup)
    {
        $this->moduleDataSetup = $moduleDataSetup;
    }

    public static function getVersion()
    {
        return '1.0.0'; // Patch version
    }

    public function apply()
    {
        $this->moduleDataSetup->startSetup();

        // Perform database operations here, e.g., insert data

        $this->moduleDataSetup->endSetup();
    }

    public function revert()
    {
        $this->moduleDataSetup->startSetup();

        // Revert database operations here, e.g., delete data

        $this->moduleDataSetup->endSetup();
    }

    public function getAliases()
    {
        return [];
    }

    public static function getDependencies()
    {
        return [];
    }
}
```



In this example, the AddSampleData data patch class inserts data into the database when applied and reverts the changes when reverted.

Data patches in Magento 2 provide a structured way to manage and apply data modifications to your store's database as part of the upgrade or update process, ensuring data consistency and version control.


Detailed Info:- [Data and schema patches](https://developer.adobe.com/commerce/php/development/components/declarative-schema/patches/)
