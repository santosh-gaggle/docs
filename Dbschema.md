# Magento 2 db_schema.xml

In Magento 2, the `db_schema.xml` file is a fundamental component of the declarative database schema approach. It plays a crucial role in defining the structure of database tables, columns, indexes, constraints, and other database-related elements. This approach simplifies database management by allowing you to declare the desired database structure using XML files.

## Purpose of `db_schema.xml`

1. **Database Structure Definition:** `db_schema.xml` is used to define the structure of database tables, columns, indexes, constraints, and related attributes.

2. **Declarative Approach:** Magento 2 introduced the declarative schema approach, replacing SQL scripts with XML files to manage database changes. This approach enhances version control and simplifies database management.

## Contents of `db_schema.xml`

The `db_schema.xml` file is typically located in a module's `etc/` directory and can contain the following main elements:

# Example `db_schema.xml`:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<schema xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Setup/Declaration/Schema/etc/schema.xsd">
    <table name="your_custom_table" resource="default" engine="innodb" comment="Custom Table">
        <column xsi:type="int" name="entity_id" padding="10" unsigned="true" nullable="false" identity="true" comment="Entity ID"/>
        <column xsi:type="varchar" name="name" nullable="false" length="255" comment="Name"/>
        <constraint xsi:type="primary" referenceId="PRIMARY">
            <column name="entity_id"/>
        </constraint>
        <index referenceId="IDX_NAME" indexType="btree">
            <column name="name"/>
        </index>
    </table>
</schema>

```
