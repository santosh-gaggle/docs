# SOLID Principles

The SOLID principles are a set of five design principles for writing maintainable and scalable software in object-oriented programming. Let's go through each principle in detail with examples:


# Advantages of following these principles:

- Avoid Duplicate code
- Easy to maintain
- Easy to understand
- Flexible software
- Reduce Complexity

## 1. Single Responsibility Principle (SRP)

**Definition**: A class should have only one reason to change, meaning it should have only one responsibility.

**Example**:

```php
class UserManager {
    public function registerUser($user) {
        // Register user logic
    }

    public function updateUser($user) {
        // Update user logic
    }
}
```

In this example, the `UserManager` class handles both user registration and user updating. If there are changes to either of these processes, the class needs to be modified. To follow SRP, we can split it into two classes:

```php
class UserRegistration {
    public function registerUser($user) {
        // Register user logic
    }
}

class UserUpdater {
    public function updateUser($user) {
        // Update user logic
    }
}
```

Now, each class has a single responsibility.


# 2. Open-Closed Principle (OCP)

**Definition**: Software entities (classes, modules, functions) should be open for extension but closed for modification.

**Example**:

```php
class Shape {
    public function area() {
        // Calculate area logic
    }
}

class Circle extends Shape {
    public function area() {
        // Calculate circle area logic
    }
}

class Rectangle extends Shape {
    public function area() {
        // Calculate rectangle area logic
    }
}

```

In this example, if you want to add a new shape (e.g., Triangle), you need to modify the `Shape` class by adding a new method. To follow OCP, use an interface or an abstract class:

```php

interface Shape {
    public function area();
}

class Circle implements Shape {
    public function area() {
        // Calculate circle area logic
    }
}

class Rectangle implements Shape {
    public function area() {
        // Calculate rectangle area logic
    }
}

class Triangle implements Shape {
    public function area() {
        // Calculate triangle area logic
    }
}


```

Now, you can add new shapes without modifying existing code.

### 3. Liskov Substitution Principle (LSP)

**Definition:** Subtypes must be substitutable for their base types without altering the correctness of the program.

**Example:**

```php
class Bird {
    public function fly() {
        // Fly logic
    }
}

class Ostrich extends Bird {
    public function fly() {
        // Ostriches cannot fly, so this method does nothing
    }
}
```
In this example, the `Ostrich` class violates LSP because it's not substitutable for the `Bird` class in all contexts. To fix it:

```php

interface Bird {
    public function fly();
}

class Sparrow implements Bird {
    public function fly() {
        // Fly logic for sparrows
    }
}

class Ostrich implements Bird {
    public function fly() {
        // Ostriches cannot fly, so this method does nothing
    }
}

```

Now, `Ostrich` adheres to LSP as it implements the `Bird` interface correctly.


# 4. Interface Segregation Principle (ISP)

**Definition:** Clients should not be forced to depend on interfaces they do not use.

**Example:**

```php
interface Worker {
    public function work();
    public function eat();
}

class Engineer implements Worker {
    public function work() {
        // Engineer's work logic
    }

    public function eat() {
        // Engineer's eat logic
    }
}
```

In this example, the `Engineer` class is forced to implement both `work` and `eat` methods, even if it doesn't need the `eat` method. To follow ISP, split the interface:

```php

interface Workable {
    public function work();
}

interface Eatable {
    public function eat();
}

class Engineer implements Workable, Eatable {
    public function work() {
        // Engineer's work logic
    }

    public function eat() {
        // Engineer's eat logic
    }
}

```
Now, classes can implement only the interfaces they need.


## 5. Dependency Inversion Principle (DIP)

**Definition:** High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details; details should depend on abstractions.

**Example:**

```php
class LightBulb {
    public function turnOn() {
        // Turn on logic
    }
}

class Switch {
    private $bulb;

    public function __construct(LightBulb $bulb) {
        $this->bulb = $bulb;
    }

    public function operate() {
        // Operate logic
        $this->bulb->turnOn();
    }
}

```
In this example, `Switch` depends on `LightBulb`, violating DIP. To follow DIP, use abstractions (interfaces or abstract classes):

```php

interface Switchable {
    public function turnOn();
}

class LightBulb implements Switchable {
    public function turnOn() {
        // Turn on logic
    }
}

class Switch {
    private $device;

    public function __construct(Switchable $device) {
        $this->device = $device;
    }

    public function operate() {
        // Operate logic
        $this->device->turnOn();
    }
}

```

Now, both `LightBulb` and `Switch` depend on abstractions, adhering to DIP.

These are the SOLID principles in PHP with examples. Following these principles helps in creating maintainable, extensible, and robust software systems.