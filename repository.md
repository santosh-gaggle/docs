# What is a Repository in Magento 2?

A repository in Magento 2 is a service that enables CRUD (Create, Read, Update, Delete) operations on entities or a list of entities. It's considered a service contract, and its implementation is part of the domain layer.

## Repository State

Repositories in Magento 2 should be stateless once instantiated. This means that each method call should not depend on previous calls, nor should it impact subsequent method calls. Any fields within the repository class should also be stateless.

If your repository requires functionality that involves maintaining state, such as caching, you should use the registry pattern. The CustomerRepository class is a good example that employs this pattern.

## Search Criteria

A Search Criteria is an implementation of the SearchCriteriaInterface class, allowing you to construct custom requests with various conditions. Repositories use Search Criteria to retrieve entities based on matching criteria.

## Filter

The Filter class is the smallest component of a Search Criteria. It enables you to add a custom field, value, and condition type to the criteria.


# Model
Models represent individual records or entities in the database. They contain business logic and handle data manipulation and validation.


# Resource Model
Resource models handle the interaction between models and the database. They contain methods for CRUD (Create, Read, Update, Delete) operations.


# Collection
Collections are used to retrieve multiple records from the database. They represent a set of model instances.


# Factory Class
Factory classes are responsible for creating instances of models. They provide a way to instantiate models without having to use the `new` keyword directly.


# Abstract Class
Abstract classes are base classes that can't be instantiated on their own. They provide common methods and properties that are shared among multiple classes that extend them.
