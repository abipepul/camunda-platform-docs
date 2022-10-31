---
id: rabbitmq
title: RabbitMQ Service Connector
sidebar_label: RabbitMQ Connector
description: Send messages to RabbitMQ from your BPMN process.
---

The **RabbitMQ Connector** allows you to connect your BPMN service with [RabbitMQ](https://www.rabbitmq.com/).

## Prerequisites

To use the **RabbitMQ Connector**, you need to have an installed RabbitMQ server and credentials to use it.
It is highly recommended not to expose your secret credentials as plain text but rather use Camunda secrets. See an [appendix entry](#how-do-i-store-secrets-for-my-connector) to learn more.
Make sure that you write correct exchange name and routing key, because **RabbitMQ Connector** can't throw exception if it is wrong. 


## Create a RabbitMQ Connector task

To use the **RabbitMQ Connector** in your process, either change the type of existing task by clicking on it and using the wrench-shaped **Change type** context menu icon, or create a new Connector task by using the **Append Connector** context menu. Follow our [guide on using Connectors](../use-connectors.md) to learn more.

## Make your RabbitMQ Connector for sending messages executable

To make the **RabbitMQ Connector** executable, choose needed connection type in **Authentication** section and fill out the mandatory fields highlighted in red in the properties panel:

![connectors-rabbitmq-red-properties](../img/connectors-rabbitmq-red-properties.png)

:::note
All the mandatory and non-mandatory fields will be covered in the upcoming sections. Depending on the authentication selection you choose, different amount of fields are required. We will also cover this in the next section.
:::

## Authentication

You can choose among the available RabbitMQ Connectors according to your authentication requirements.
First of all, you need have a user in your RabbitMQ instance with necessary permissions. See more at the [RabbitMQ access control specification](https://www.rabbitmq.com/access-control.html).
In next step choose a required type of connection:

### URI type connection 

1. Click **URI** connection type in **Authentication** section
2. Set **URI** to `URI`. It must contain RabbitMQ username, password, host name, port number, and virtual host. Example: `amqp://userName:password@serverHost:port/virtualHost`, follow the [RabbitMQ URI specification](https://www.rabbitmq.com/uri-spec.html) to learn more.
![connectors-rabbitmq-uri-fields](../img/connectors-rabbitmq-uri-fields.png)

### Credentials type connection

1. Click **Username/Password** connection type in **Authentication** section
2. Set **Password** to `Password`.
![connectors-rabbitmq-credentials-fields](../img/connectors-rabbitmq-credentials-fields.png)

## Routing Data 

In **Routing** section you need set routing data (free feel use json format and FEEL expression format) :

```json
{
  "exchange": "<your-rabbitmq-exchange-name>",
  "routingKey": "<your-rabbitmq-routing-key-name>",
  "virtualHost": "<your-rabbitmq-virtual-host-name>",
  "hostName": "<your-rabbitmq-host-name-or-ip-address>",
  "port": "<your-rabbitmq-port-number>"
}
```

For **URI** type connection required fields : `exchange` and `routingKey`.
For **Credentials** type connection all fields is required. 

![connectors-rabbitmq-routing](../img/connectors-rabbitmq-routing.png)


## Message

1. Set your message payload to **Message** section. Message can be text and JSON format.
2. (Optional) Set message properties (JSON format or FEEL expression format) to **Properties** section. Follow [RabbitMQ documentation](https://www.rabbitmq.com/publishers.html#message-properties) for learn more about message properties.

![connectors-rabbitmq-message-with-properties](../img/connectors-rabbitmq-message-with-properties.png)


## RabbitMQ Connector response

The **RabbitMQ Connector** returns the `Success` result.
The response contains a `messageId` variable.

You can use an output mapping to map the response:

1. Use **Result Variable** to store the response in a process variable. For example, `myResultVariable`.
2. Use **Result Expression** to map fields from the response into process variables. For example:

```
= {
  "myResultVariable": response.statusResult
}
```

## Appendix & FAQ

### How do I store Secrets for my Connector?

It is highly recommended storing your secret credentials as Camunda secrets. Follow our documentation on [managing secrets](../../../components/console/manage-clusters/manage-secrets.md) to learn more.
