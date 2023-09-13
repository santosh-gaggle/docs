## Dependency Injection in Magento 2

Dependency injection is a technique used in Magento 2 to manage class dependencies by injecting them from the outside rather than creating them internally. This improves code maintainability and testability.

### Simple Example

Suppose you have a `Product` class that needs a `PriceCalculator` to calculate the product's price. Instead of creating a `PriceCalculator` instance inside the `Product` class, you can inject it through the constructor.

```php
// PriceCalculator.php
class PriceCalculator {
    public function calculatePrice($basePrice) {
        // Calculate price logic here
    }
}

// Product.php
class Product {
    private $priceCalculator;

    public function __construct(PriceCalculator $priceCalculator) {
        $this->priceCalculator = $priceCalculator;
    }

    public function calculateFinalPrice($basePrice) {
        // Use the injected PriceCalculator to calculate the final price
        return $this->priceCalculator->calculatePrice($basePrice);
    }
}

// Usage
$priceCalculator = new PriceCalculator();
$product = new Product($priceCalculator);
$finalPrice = $product->calculateFinalPrice(50);
```


In this example, the Product class accepts a PriceCalculator instance through its constructor. This allows you to inject different PriceCalculator implementations without modifying the Product class. It also makes it easier to test the Product class with different price calculation logic.