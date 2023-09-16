# Create a cache type

A cache type enables you to specify what is cached and enables merchants to clear that cache type using the Cache Management page in the Admin.
The tag scope provides a mechanism for a cache type.

# Important Question

 - When you will create any new cache type then you need to extends `Magento\Framework\Cache\Frontend\Decorator\TagScope` class
 - To check your custom cache is enabled or disable so you need to call `Magento\Framework\App\Cache\StateInterface` 
    **exp**: 

    ```php

        private $isAttributeCacheEnabled;

            public function __construct(
                Magento\Framework\App\Cache\StateInterface $state
            ) {

                $this->state = $state;
            }

            private function isEnabled()
            {
                if (null === $this->isAttributeCacheEnabled) {
                    $this->isAttributeCacheEnabled = $this->state->isEnabled(Type::TYPE_IDENTIFIER);
                }
                return $this->isAttributeCacheEnabled;
            }

    ```


### Declare a new cache type in the `<module_dir>/etc/cache.xml` file with the following attributes:

```xml

<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Cache/etc/cache.xsd">
    <type name="%cache_type_id%" translate="label,description" instance="VendorName\ModuleName\Model\Cache\Type\CacheType">
        <label>Cache Type Label</label>
        <description>Cache Type Description</description>
    </type>
</config>


```

## Cache type model

```php

<?php
/**
 * Copyright © Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */

namespace VendorName\ModuleName\Model\Cache\Type;

use Magento\Framework\App\Cache\Type\FrontendPool;
use Magento\Framework\Cache\Frontend\Decorator\TagScope;

/**
 * System / Cache Management / Cache type "Your Cache Type Label"
 */
class CacheType extends TagScope
{
    /**
     * Cache type code unique among all cache types
     */
    const TYPE_IDENTIFIER = '%cache_type_id%';

    /**
     * The tag name that limits the cache cleaning scope within a particular tag
     */
    const CACHE_TAG = '%CACHE_TYPE_TAG%';

    /**
     * @param FrontendPool $cacheFrontendPool
     */
    public function __construct(FrontendPool $cacheFrontendPool)
    {
        parent::__construct(
            $cacheFrontendPool->get(self::TYPE_IDENTIFIER),
            self::CACHE_TAG
        );
    }
}


```

### Store data in a custom cache type

To store serialized data in a custom cache, follow these steps:

Pass the argument to the constructor `Magento\Framework\App\CacheInterface $cache` of a required class (Repository, Model, Block, etc).

```php

/**
 * @param CacheInterface $cache
 * @param SerializerInterface $serializer
 */
public function __construct(CacheInterface $cache, SerializerInterface $serializer)
{
    $this->cache = $cache;
    $this->serializer = $serializer;
}

/**
 * @param CacheInterface $cache
 * @param SerializerInterface $serializer
 */
public function __construct(CacheInterface $cache, SerializerInterface $serializer)
{
    $this->cache = $cache;
    $this->serializer = $serializer;
}
```

### Store data in the cache.

```php

$cacheKey  = \VendorName\ModuleName\Model\Cache\Type\CacheType::TYPE_IDENTIFIER;
$cacheTag  = \VendorName\ModuleName\Model\Cache\Type\CacheType::CACHE_TAG;

$storeData = $this->cache->save(
    $this->serializer->serialize($cacheData),
    $cacheKey,
    [$cacheTag],
    86400
);

$cacheKey  = \VendorName\ModuleName\Model\Cache\Type\CacheType::TYPE_IDENTIFIER;
$cacheTag  = \VendorName\ModuleName\Model\Cache\Type\CacheType::CACHE_TAG;

$storeData = $this->cache->save(
    $this->serializer->serialize($cacheData),
    $cacheKey,
    [$cacheTag],
    86400
);
```

### Retrieve data from custom cache type

Retrieve data from the cache with:

```php
$cacheKey  = \VendorName\ModuleName\Model\Cache\Type\CacheType::TYPE_IDENTIFIER;

$data = $this->serializer->unserialize($this->cache->load($cacheKey));

$cacheKey  = \VendorName\ModuleName\Model\Cache\Type\CacheType::TYPE_IDENTIFIER;

$data = $this->serializer->unserialize($this->cache->load($cacheKey));

```

### Invalidate custom cache type

To invalidate a custom cache type, follow these steps:

Pass the argument to the constructor `Magento\Framework\App\Cache\TypeListInterface $typeList` of a required class (Repository, Model, Block, etc).

```php

/**
 * @param TypeListInterface $typeList
 */
public function __construct(TypeListInterface $typeList)
{
    $this->typeList = $typeList;
}
```

### Invalidate the cache.

```php
$cacheKey  = \VendorName\ModuleName\Model\Cache\Type\CacheType::TYPE_IDENTIFIER;

$this->typeList->invalidate($cacheKey);

$cacheKey  = \VendorName\ModuleName\Model\Cache\Type\CacheType::TYPE_IDENTIFIER;

$this->typeList->invalidate($cacheKey);
```


### Flush custom cache type

The custom cache type can be flushed in the following ways:

- Go to System -> Cache Management and flush the custom cache type
- Programmatically, using the TypeList.

```php
$cacheKey  = \VendorName\ModuleName\Model\Cache\Type\CacheType::TYPE_IDENTIFIER;

$this->typeList->cleanType($cacheKey);

$cacheKey  = \VendorName\ModuleName\Model\Cache\Type\CacheType::TYPE_IDENTIFIER;

$this->typeList->cleanType($cacheKey);
```
<div style="text-align: center;font-size: 30px;color: red;">
    # Why we need to create the custom caching
</div>


In Magento, you may need to create custom caching when you want to improve the performance and responsiveness of your store by storing and serving frequently accessed or computationally expensive data more efficiently. Custom caching is useful in various scenarios, including:

- **Dynamic Content**: When your store displays dynamic content that doesn't change frequently but is resource-intensive to generate. Examples include product catalogs, pricing information, and customer data.

- **Third-Party Integrations**: If your store relies on external APIs or services for data, you can cache the responses to reduce latency and minimize the impact of service outages or slow responses.

- **Personalization**: For personalized content, like product recommendations or user-specific data, caching can reduce the processing required for each user request while still providing a personalized experience.

- **Complex Calculations**: If your store performs complex calculations or data processing, you can cache the results to avoid repeating the calculations for each request.

- **Database Queries**: Magento uses a database to store a significant amount of data. Caching frequently queried data or query results can reduce the load on the database server and improve page load times.

- **Expensive Queries**: When you have custom reports or data retrieval processes that involve costly database queries, caching can significantly speed up these operations.

- **Content Blocks**: Caching can be applied to specific content blocks on your site, such as menus, banners, or promotional blocks, to reduce the load on the server.

- **Custom Modules**: If you've developed custom modules that perform resource-intensive operations, you can implement caching within those modules to improve their performance.

- **High-Traffic Sites**: On high-traffic sites, caching helps reduce the load on your server and database, allowing your store to handle a larger number of concurrent users without performance degradation.

- **API Responses**: If your store serves data via APIs, caching API responses can reduce the response time for API requests and improve the overall experience for integrators and consumers.

When implementing custom caching in Magento, it's essential to consider cache invalidation strategies to ensure that cached data remains up-to-date when the underlying data changes. Additionally, Magento provides a flexible caching framework that allows you to define different cache types and strategies for different types of data, ensuring that you can fine-tune caching to your specific needs.

Keep in mind that while caching can significantly improve performance, it should be used judiciously and managed effectively to avoid serving outdated or incorrect data to users.
