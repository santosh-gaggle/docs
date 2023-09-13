# PHP Object-Oriented Programming (OOP) Concepts

PHP supports Object-Oriented Programming (OOP), which is a programming paradigm that revolves around the concept of objects and classes. Let's explore key OOP concepts in PHP with examples:

## 1. **Classes and Objects**

- **Class**: A class is a blueprint or template for creating objects. It defines the properties (attributes) and methods (functions) that objects of that class will have.

```php
class Car {
    // Properties
    public $brand;
    public $model;
    
    // Method
    public function startEngine() {
        return "Engine started!";
    }
}
```
- **Object**
Object: An object is an instance of a class. It represents a real-world entity and can access the properties and methods defined in the class.

```php

php
$myCar = new Car();
$myCar->brand = "Toyota";
$myCar->model = "Camry";

echo $myCar->startEngine(); // Output: Engine started!
```

# Encapsulation

Encapsulation: Encapsulation is the concept of bundling data (properties) and methods that operate on that data into a single unit (class). It restricts direct access to some of an object's components.

```php
class BankAccount {
    private $balance = 0;
    
    public function deposit($amount) {
        $this->balance += $amount;
    }
    
    public function getBalance() {
        return $this->balance;
    }
}
```
$account = new BankAccount();
$account->deposit(1000);
echo $account->getBalance(); // Output: 1000


## Inheritance
Inheritance: Inheritance allows a class to inherit properties and methods from another class. It promotes code reusability.

```php
class Animal {
    public $name;

    public function speak() {
        return "Animal speaks!";
    }
}

class Dog extends Animal {
    public function speak() {
        return "Dog barks!";
    }
}

$dog = new Dog();
echo $dog->speak(); // Output: Dog barks!
```

# Polymorphism

Polymorphism allows objects of different classes to be treated as objects of a common superclass. It enables method overriding and dynamic method calls.

```php
class Circle {
    public function area() {
        return "Calculating circle area...";
    }
}

class Rectangle {
    public function area() {
        return "Calculating rectangle area...";
    }
}

$shapes = [new Circle(), new Rectangle()];

foreach ($shapes as $shape) {
    echo $shape->area() . PHP_EOL;
}

// Output:
// Calculating circle area...
// Calculating rectangle area...

```


# Abstraction

Abstraction: Abstraction is the process of simplifying complex reality by modeling classes based on the essential properties and behaviors relevant to the problem.

```php
abstract class Shape {
    abstract public function area();
}

class Circle extends Shape {
    public function area() {
        return "Calculating circle area...";
    }
}

$circle = new Circle();
echo $circle->area(); // Output: Calculating circle area...
```

## Interface

An interface defines a contract for classes that implement it. It specifies a set of methods that must be implemented by any class that adheres to the interface.

```php
interface Logger {
    public function log($message);
}

class FileLogger implements Logger {
    public function log($message) {
        // Log message to a file
    }
}

class DatabaseLogger implements Logger {
    public function log($message) {
        // Log message to a database
    }
}
```
