
![logo](https://docsify.js.org/_media/icon.svg ':size=50x100')
# What is Composer?

Composer is a dependency management tool for PHP. It allows you to declare the libraries your project depends on and manages their installation.


## Getting Started with Composer

To start using Composer, you need to have it installed on your system. You can download it from [getcomposer.org](https://getcomposer.org/). Once installed, you can create a `composer.json` file in your project directory to define your dependencies.

Here's a basic example of a `composer.json` file:

```json

{
    "require": {
        "monolog/monolog": "2.0.*"
    }
}

```


# Composer Install VS Composer Update

## Composer Installation

When using Composer, you'll typically run the `composer install` command in your terminal. Here's what happens during this process:

1. **Checking composer.lock:** Composer first checks if the `composer.lock` file exists. If it does, it installs the dependencies listed in this file. This ensures you get the exact versions used in your project.

2. **Using composer.json:** If there is no `composer.lock` file (perhaps in a fresh project), Composer reads the `composer.json` file and installs the specified dependencies. It then creates the `composer.lock` file to record the installed versions.

### When to Use `composer install`

`composer install` is primarily used in production to ensure that your project uses the exact dependencies and versions used during development. However, it can also be used during development.

## Composer Update

The `composer update` command is used to update your project's dependencies. Here's how it works:

1. **Processing composer.json:** Composer processes the `composer.json` file, updating dependencies as specified.

2. **Updating composer.lock:** After updating dependencies, Composer creates or updates the `composer.lock` file to reflect the new dependency versions.

### When to Use `composer update`

`composer update` should primarily be used in the development phase of your project when you want to update dependencies. Be cautious about using it in a production environment, as it can potentially introduce changes that may not be suitable for production use.

Remember to consider the potential impact on your project when deciding whether to run `composer install` or `composer update` based on your development or production needs. 😎



### Is it Possible to install any package into the custom directory instead of the vendor

`Yes` 


```json

{
    "require": {
        "monolog/monolog": "2.0.*"
    },
    "config": {
        "vendor-dir": "/var/www/html/magento/mycustom"
    }
}

```

### Also if you will run the `composer require monolog/monolog` into the terminal

- if the composer file will not present then it will create the composer.json file and install the dependencies and create the `composer.lock` file

- But by this way the dependency will install into the default `vendor` folder , if still you want to install the package into custom folder then create a `composer.json` file  and add this line only

```json

{
    "config": {
        "vendor-dir": "/var/www/html/magento/mycustom"
    }
}

```

- after run this command `composer require monolog/monolog`  the  `composer.json` file will look like this 


```json

{
       "config": {
        "vendor-dir": "/var/www/html/magento/mycustom"
    },
       "require": {
              "monolog/monolog": "^2.9"
       }
}

```