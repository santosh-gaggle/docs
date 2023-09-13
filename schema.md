![logo](https://docsify.js.org/_media/icon.svg ':size=50x100')

# Module Lifecycle and Executable Classes

This topic describes a module's lifecycle and how to create classes that execute code when your module is initialized, upgraded, or uninstalled. These executable classes can perform tasks that set up the database, update data, and clean up data.

> **Note:** Theme and language package extensions do not need initialization or uninstallation tasks because they do not install database schemas or update data.


# Schema Initialization Stages

The schema initialization stages are the first set of processes that Adobe Commerce and Magento Open Source run when your module is installed, re-installed, or upgraded.

## Schema Installation

During the initial installation of your module, the application executes the schema installation class. This class is responsible for setting up the database schema and performing any necessary tasks for your module's functionality.

If the `schema_version` for your module is found in the `setup_module` table, the application skips this stage and proceeds to the schema upgrade stage.

> **Note:** The schema installation stage is executed when your module is initially installed.

- **Class name:** `InstallSchema`
- **Interface:** `InstallSchemaInterface`
- **Method:** `install()`


## Example: InstallSchema.php

```php

<?php
namespace VendorName\ModuleName\Setup;

use Magento\Framework\Setup\InstallSchemaInterface;

class InstallSchema implements InstallSchemaInterface
{
    /**
     * @inheritdoc
     */
    public function install(SchemaSetupInterface $setup, ModuleContextInterface $context)
    {
        //Install schema logic
    }
}

```

## Schema Upgrade

The schema upgrade stage comes into play when your module is re-installed or upgraded. If changes to the database schema are required due to module updates, the application executes the schema upgrade class. This class handles tasks such as updating data structures and ensuring compatibility with the new module version.

> **Note:** The schema upgrade stage is executed when your module undergoes re-installation or upgrading.


- **Class name:** `UpgradeSchema`
- **Interface:** `UpgradeSchemaInterface`
- **Method:** `upgrade()`

## Example: UpgradeSchema.php

```php

<?php
/**
 * Copyright © Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */

namespace VendorName\ModuleName\Setup;

use Magento\Framework\Setup\UpgradeSchemaInterface;

class UpgradeSchema implements UpgradeSchemaInterface
{
    /**
     * @inheritdoc
     */
    public function upgrade(SchemaSetupInterface $setup, ModuleContextInterface $context)
    {
        //Upgrade schema logic
    }
}
```


# Recurring Schema Event

The application executes your module's recurring schema event class after every schema installation or upgrade stage. This class makes final modifications to the database schema after it has been installed or updated.


- **Class name:** `Recurring`
- **Interface:** `InstallSchemaInterface`
- **Method:** `install()`

## Example: Recurring.php

```php
<?php
/**
 * Copyright © Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */

namespace VendorName\ModuleName\Setup;

use Magento\Framework\Setup\InstallSchemaInterface;

class Recurring implements InstallSchemaInterface
{
    /**
     * @inheritdoc
     */
    public function install(SchemaSetupInterface $setup, ModuleContextInterface $context)
    {
        //Recurring schema event logic
    }
}

```


# Data Initialization

After the schema initialization processes are completed, the application proceeds to your module's data initialization stages.

## Data Installation

The application executes the data installation class during your module's initial install unless an existing version entry is found in the database. The purpose of this class is to populate the database with initial data.

- **Class name:** `InstallData`
- **Interface:** `InstallDataInterface`
- **Method:** `install()`

## Example: InstallData.php
```php

<?php
/**
 * Copyright © Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */

namespace VendorName\ModuleName\Setup;

use Magento\Framework\Setup\InstallDataInterface;

class InstallData implements InstallDataInterface
{
    /**
     * {@inheritdoc}
     */
    public function install(ModuleDataSetupInterface $setup, ModuleContextInterface $context)
    {
        // Data install logic
    }
}

```



# Data Upgrade

The application executes the data upgrade class when it detects an earlier version in the `data_version` field for the module in the `setup_module` table. The purpose of this class is to fix corrupted data or populate a new data field after a schema change.


- **Class name:** `UpgradeData`
- **Interface:** `UpgradeDataInterface`
- **Method:** `upgrade()`

## Example: UpgradeData.php

```php
<?php
/**
 * Copyright © Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */

namespace VendorName\ModuleName\Setup;

use Magento\Framework\Setup\UpgradeDataInterface;

class UpgradeData implements UpgradeDataInterface
{
    /**
     * @inheritdoc
     */
    public function upgrade(ModuleDataSetupInterface $setup, ModuleContextInterface $context)
    {
        // Data upgrade logic
    }
}

```


# Recurring Data Event

The application executes your module's recurring data event class after every data installation or upgrade stage. This class makes final modifications to the database store after data has been installed or updated.


- **Class name:** `RecurringData`
- **Interface:** `InstallDataInterface`
- **Method:** `install()`

## Example: RecurringData.php

```php
<?php
/**
 * Copyright © Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */

namespace VendorName\ModuleName\Setup;

use Magento\Framework\Setup\InstallDataInterface;

class RecurringData implements InstallDataInterface
{
    /**
     * @inheritdoc
     */
    public function install(ModuleDataSetupInterface $setup, ModuleContextInterface $context)
    {
        // Recurring data event logic
    }
}

```




## Module Version

You can use the `ModuleContextInterface` to retrieve the current version of a module and execute logic based on that version.

### Example: User Module's UpgradeData.php

```php
/**
 * Copyright © Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */

namespace Magento\User\Setup;

use Magento\Framework\Encryption\Encryptor;
use Magento\Framework\Setup\ModuleContextInterface;
use Magento\Framework\Setup\ModuleDataSetupInterface;
use Magento\Framework\Setup\UpgradeDataInterface;

class UpgradeData implements UpgradeDataInterface
{
    /**
     * @inheritdoc
     */
    public function upgrade(ModuleDataSetupInterface $setup, ModuleContextInterface $context)
    {
        $setup->startSetup();
        if (version_compare($context->getVersion(), '2.0.1', '<')) {
            $this->upgradeHash($setup);
        }
        $setup->endSetup();
    }

    // ... Other code related to the upgrade logic
}

```

## Uninstall Event

The uninstall event is executed when your module is uninstalled using the following command:

```shell
bin/magento module:uninstall --remove-data <module_name>
```


> During the uninstallation phase, your module should remove all traces of its existence in the database. This can involve actions such as dropping tables, deleting data, or restoring data.

### Class Name: Uninstall

- Interface: UninstallInterface
- Method: uninstall()

### Example: Uninstall.php

Here's an example of an `Uninstall` class that implements the `UninstallInterface` and defines the `uninstall()` method where you can place your module-specific uninstallation logic.

```php
<?php
/**
 * Copyright © Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */

namespace VendorName\ModuleName\Setup;

use Magento\Framework\Setup\UninstallInterface;

class Uninstall implements UninstallInterface
{
    /**
     * @inheritdoc
     */
    public function uninstall(SchemaSetupInterface $setup, ModuleContextInterface $context)
    {
        // Uninstall logic
    }
}


```
> **Note:** Disabled modules can still execute their uninstall event. However, it's important to note that when a module is disabled, its module-specific configurations, such as dependency injection and event/observer configurations, will not be available. This can lead to problems during the uninstallation process. To avoid this situation, it's recommended not to include dependencies in your uninstall event class.



`Scenario`: Imagine you have a module called "CustomShipping" that adds custom shipping options to your e-commerce platform. This module has a dependency on the "PaymentGateway" module, which handles payment processing. Both modules have an uninstall event to clean up their respective database tables.


`In this example`, if you disable the "PaymentGateway" module and then try to uninstall the "CustomShipping" module, you might encounter issues because "CustomShipping" relies on the presence of "PaymentGateway." The "PaymentGateway" module's configurations are not available when it's disabled, which could result in unexpected behavior during the uninstallation of "CustomShipping."

To avoid such problems, it's best practice not to include dependencies or rely on configurations from other modules in your uninstall event class. This ensures a smoother uninstallation process and minimizes the risk of conflicts or errors when handling disabled modules.