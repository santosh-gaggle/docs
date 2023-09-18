# Configure Message Queues in Magento 2

The message queue topology is an essential feature in Magento Open Source, facilitating asynchronous communication between different parts of an application. It can be added to existing modules or included as part of the Magento Open Source installation.

## Overview

Configuring the message queue topology involves creating and modifying several configuration files in the `<module>/etc` directory:

- `communication.xml`: Defines common aspects of the message queue system.
- `queue_consumer.xml`: Specifies the relationship between a queue and its consumer.
- `queue_topology.xml`: Declares queues, exchanges, and defines message routing rules.
- `queue_publisher.xml`: Specifies the exchange for publishing a topic.

## Use Cases

Depending on your requirements, you may need to create and configure different combinations of these files:

- If you only want to publish to an existing queue created by a 3rd party system, you will only need the `queue_publisher.xml` file.
- If you only want to consume from an existing queue, you will only need the `queue_consumer.xml` file.
- To configure the local queue and publish to it for 3rd party systems to consume, you will need both `queue_publisher.xml` and `queue_topology.xml` files.
- When configuring the local queue and consuming messages published by a 3rd party system, you will need both `queue_topology.xml` and `queue_consumer.xml` files.

## communication.xml

The `<module>/etc/communication.xml` file defines aspects of the message queue system that all communication types have in common. This release supports AMQP and database connections.

### Sample communication.xml file

```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Communication/etc/communication.xsd">
  <topic name="synchronous.rpc.test" request="string" response="string">
    <handler name="processRpcRequest" type="Magento\TestModuleSynchronousAmqp\Model\RpcRequestHandler" method="process"/>
  </topic>
  <topic name="magento.testModuleSynchronousAmqp.api.serviceInterface.execute" schema="Magento\TestModuleSynchronousAmqp\Api\ServiceInterface::execute">
    <handler name="processRemoteRequest" type="Magento\TestModuleSynchronousAmqp\Model\RpcRequestHandler" method="process"/>
  </topic>
</config>
```


## Topic Configuration

Topic configuration in Magento 2 is flexible, allowing you to switch the transport layer for topics at deployment time. These values can be overridden in the `env.php` file. Key parameters for topic configuration include:

- **name**: A unique identifier for the topic, following a specific naming convention.
- **request**: Specifies the data type of the topic.
- **response**: Specifies the format of the response (required for synchronous topics).
- **schema**: The interface that describes the message structure.

Here's an example of a topic configuration:

```xml
<type name="Magento\Framework\Session\Storage">
    <arguments>
        <argument name="namespace" xsi:type="string">core</argument>
    </arguments>
</type>
```


Magento 2 utilizes messaging queues (MQ) for asynchronous communication. Several XML configuration files are involved in setting up and configuring messaging queues in Magento 2. Below, we detail these configuration files and their elements.

## `queue_consumer.xml`

The `queue_consumer.xml` file contains one or more consumer elements. It defines how consumers handle messages from queues. Here's an example:

```xml
<!-- app/code/YourVendor/YourModule/etc/queue_consumer.xml -->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework-message-queue:etc/consumer.xsd">
    <consumer name="basic.consumer" queue="basic.consumer.queue" handler="LoggerClass::log"/>
    <consumer name="synchronous.rpc.test" queue="synchronous.rpc.test.queue" handler="LoggerClass::log"/>
    <consumer name="rpc.test" queue="queue.for.rpc.test.unused.queue" consumerInstance="Magento\Framework\MessageQueue\BatchConsumer" connection="amqp"/>
</config>
```



# `queue_topology.xml` 

The `queue_topology.xml` file in Magento 2 defines message routing rules, declares queues, and sets up exchanges. It plays a crucial role in establishing the message flow within the system. Below is an example `queue_topology.xml` file along with explanations of its key elements:

```xml
<!-- Example queue_topology.xml file -->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework-message-queue:etc/topology.xsd">
  <!-- Exchange 1 -->
  <exchange name="magento-topic-based-exchange1" type="topic" connection="db">
    <!-- Binding Configuration -->
    <binding id="topicBasedRouting2" topic="anotherTopic" destinationType="queue" destination="topic-queue1">
      <!-- Binding Arguments -->
      <arguments>
        <!-- Not part of our use case but processed if specified -->
        <argument name="argument1" xsi:type="string">value</argument>
      </arguments>
    </binding>
    <!-- Exchange Arguments -->
    <arguments>
      <argument name="alternate-exchange" xsi:type="string">magento-log-exchange</argument>
    </arguments>
  </exchange>

  <!-- Exchange 2 -->
  <exchange name="magento-topic-based-exchange2" type="topic" connection="db">
    <!-- Binding Configuration -->
    <binding id="topicBasedRouting1" topic="#" destinationType="queue" destination="topic-queue2"/>
    <!-- Exchange Arguments -->
    <arguments>
      <argument name="alternate-exchange" xsi:type="string">magento-log-exchange</argument>
    </arguments>
  </exchange>
</config>
```



# `queue_publisher.xml`

The `queue_publisher.xml` file in Magento 2 defines the connection and exchange to use when publishing messages for specific topics. It plays a crucial role in determining how messages are sent to the message queue. Below is an example `queue_publisher.xml` file along with explanations of its key elements:

```xml
<!-- Example queue_publisher.xml file -->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework-message-queue:etc/publisher.xsd">
    <!-- Publisher 1 -->
    <publisher topic="magento.testModuleSynchronousAmqp.api.serviceInterface.execute" disabled="true" />

    <!-- Publisher 2 -->
    <publisher topic="asynchronous.test">
        <!-- Connection Configuration for Topic "asynchronous.test" -->
        <connection name="amqp" exchange="magento" disabled="false"/>
        <connection name="db" exchange="exch1" disabled="true"/>
    </publisher>
</config>
```


# For more info ---> [Configure message queues](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/message-queues/config-mq.html)