# Indexers in Magento 2

In Magento 2, indexers are a fundamental component designed to enhance the performance and efficiency of the e-commerce platform. Indexers optimize data retrieval by precomputing and storing data in an indexed format, enabling faster and more efficient queries. Let's explore the key aspects of indexers in Magento 2.

## What is an Indexer?

An indexer in Magento 2 is responsible for:

- Creating and maintaining various types of indexes, such as product, category, and customer indexes.
- Monitoring changes in the underlying data (e.g., product additions or updates) and updating the corresponding indexes.
- Improving performance by allowing Magento to retrieve data quickly without extensive database queries.

## Types of Indexes

Magento 2 includes different types of indexes, each optimized for specific data and queries. Common index types include:

- **Flat Indexes:** Used for product and category data, optimizing data retrieval for the catalog.
- **Search Indexes:** Improve product search performance by indexing search-related data.

## Indexing Process

The indexing process in Magento 2 involves:

- Continuous monitoring of data changes.
- Batch processing to update indexes in a controlled manner.
- Storing information about the status of each indexer, including the last update time, in the `indexer_state` table.

## Custom Indexers

Developers have the flexibility to create custom indexers tailored to specific data types or custom modules. This allows for further optimization and customization of the indexing process.

## Managing Indexers

You can manage indexers through the Magento Admin Panel or by using command-line interface (CLI) commands, such as:

- Reindexing: `bin/magento indexer:reindex`
- Checking indexer status: `bin/magento indexer:status`

Indexers are essential for maintaining optimal performance in your Magento 2 e-commerce store, ensuring speedy data retrieval and a seamless customer experience.

For more detailed information and configuration options, refer to the official Magento 2 documentation.


# Magento 2 Database Tables `indexer_state` Table and `mview_state` Table

In Magento 2, there are two important database tables:

## `indexer_state` Table

The `indexer_state` table is used to store information about the current state of indexers in Magento 2. It tracks:

- Indexer status
- Last execution timestamp
- Other metadata related to indexing

This table is crucial for managing the indexing process, which enhances the performance of your store.

## `mview_state` Table

The `mview_state` table is related to Materialized Views in Magento 2, used for performance optimization. It stores information about:

- Materialized View status
- Last refresh timestamp
- Metadata related to Materialized Views

Magento uses this table to manage Materialized Views, ensuring they are up to date with source data.

These tables play vital roles in maintaining the efficiency of your Magento 2 store. When interacting with them or troubleshooting indexing issues, it's essential to follow best practices to avoid impacting store functionality.

Keep in mind that direct modifications to these tables should be avoided. Magento provides command-line tools and configuration settings for managing indexing and Materialized View states.




# Using Application Lock Mode for Reindex Processes

Starting with Magento 2.4.3, you can enable the `use_application_lock` mode for reindexing. This can be done through the use of environment variables or by configuring the `app/etc/env.php` file as follows:

```php

return [
    'indexer' => [
        'use_application_lock' => true
    ]
];

```

Enabling this mode is beneficial in case of a failure during the reindexing of a certain indexer. It provides a more accurate status of the indexer, which can be obtained from the indexer grid in the Admin or through the CLI using the following command:

```bash
bin/magento indexer:status
```

With `use_application_lock` mode enabled, you'll have better visibility into the status of your indexers, making it easier to identify and address any issues during the reindexing process.


### How the application implements indexing

# Indexing types

Each index can perform the following types of reindex operations:

- Full reindex, which means rebuilding all the indexing-related database tables

    Full reindexing can be caused by a variety of things, including creating a new web store or new customer group.

    You can optionally fully reindex at any time using the command line.

- Partial reindex, which means rebuilding the database tables only for the things that changed (like changing a single product attribute or price)

The type of reindex performed in each particular case depends on the type of changes made in the dictionary or in the system. This dependency is specific for each indexer.

The following figure shows the logic for partial reindexing.


![Indexer Process](_media/index_indexers_flow.webp ':size=500x1000')


# Create custom indexers

External Link of Adobe :- [Create custom indexers](https://developer.adobe.com/commerce/php/development/components/indexing/custom-indexer/)



