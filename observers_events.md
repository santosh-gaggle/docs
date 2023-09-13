# Events and Observers

**Events and Observers** are key tools in Adobe Commerce and Magento Open Source for extending the functionality of the platform. They use a publish-subscribe pattern, allowing you to react to specific events or even create your own custom events.

## Events

### What Are Events?

Think of **Events** as signals or notifications that something important has happened within the application. For example, when a customer places an order, an event is triggered to let the system know.

### How Do Events Work?

1. **Event Dispatch**: Events are dispatched (triggered) when specific actions occur in Magento. For instance, when an order is placed, an "order placed" event is dispatched.

2. **Observer Registration**: Developers can register observers to listen for specific events. Observers are like listeners waiting for a particular event to happen.

3. **Event Handling**: When the observed event occurs, all registered observers associated with that event are called to perform specific actions or responses.

### Creating New Events

You can create custom events by specifying a unique event name in your code. These custom events are configured in your module's `events.xml` file, where you define which observers should react to that event.

## Observers

### What Are Observers?

Think of **Observers** as special classes that keep an eye on events. They are like detectives waiting for specific events to occur.

### How Do Observers Work?

1. **Observer Creation**: To create an observer, you define a class that implements the `Magento\Framework\Event\ObserverInterface` interface. This class contains the code that should execute when a particular event occurs.

2. **Observer Registration**: Observers are registered in your module's `events.xml` file. You specify which event they should watch and define their behavior when that event takes place.

3. **Responding to Events**: When the observed event is triggered, Magento calls the observer's `execute` function, allowing it to perform its predefined actions.

### Subscribing to Events

Observers are associated with events in your `events.xml` file. You declare the observer's name, class, and the event it should respond to.

### Disabling Observers

If you want to temporarily stop an observer from running, you can disable it in the `events.xml` file. This is useful when you need to modify the observer's logic without removing it completely.

In summary, Events are notifications of important occurrences, and Observers are the code blocks that respond to those events. Together, they allow you to extend and customize Magento's functionality without altering its core code.
