This guide was designed to assist you in understanding the utilization of webhooks for real-time data receipt from the Impilo platform. Webhooks serve as a powerful automation tool that allows your systems to be integrated with Impilo. This allows immediate data retrieval as events are triggered within the platform.

### Understanding Webhooks

Webhooks are HTTP callbacks defined by the user. They are designed to be triggered by specific events on the Impilo platform. When an event is triggered, Impilo sends an HTTP POST request to the webhook's configured URL. This request contains details related to the event, enabling your system to react in kind.

### Setting Up Webhooks

Webhook setup requires you to supply Impilo with a URL endpoint capable of handling POST requests. This is where the webhook payloads will be delivered.

### Creating a Webhook

Creation of a webhook involves interaction with the "Create Webhook" endpoint. This requires you to indicate the types of events you are interested in monitoring and to provide a URL to handle event notification delivery.

```bash
curl --request POST \
--url https://example.com/api/v3/webhook \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--data '{
  "type" : "order.status",
  "url" : "https://impilo.health"
}'
```

### Webhook Types

You can refer to the API documentation for the list of supported webhook types.

### Consuming a Webhook

When your application receives a webhook request from Impilo, inspect the `type` attribute to identify the event trigger. The first part of the event type will reveal the payload type, e.g., patient created, order status updated, etc.

```json
{
    "id": 57638473,
    "type": "patient.created",
    "payload": {}
}
```

### Security

To verify that a webhook was sent by Impilo, you can configure a webhook secret that Impilo will send on each notification. The webhook secret is passed in a header with the name `Impilo-Webhook-Secret`. You can manage your webhook secret via API or in your account portal.

Webhook secret request:

```bash
curl --request POST \
--url https://example.com/api/v3/webhook-secret \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--data '{
  "secret" : "string"
}'
```

Response:
```json
{
  "secret" : "string"
}
```

### Webhook Retries

### Intervals

In situations where a webhook delivery attempt to your endpoint fails, the Impilo platform automatically retries sending the webhook notification according to a defined schedule:

- First retry: 15 minutes after the initial failed delivery
- Second retry: 30 minutes after the first retry
- Third retry: 60 minutes after the second retry

### Monitoring Webhook Attempts and Retries

To monitor and troubleshoot webhook deliveries, including retries, the Webhook Log endpoint provides comprehensive logs. You can query these logs with several filterable parameters to better understand the delivery status and manage your webhook activities effectively. Details about these parameters are available in the [Webhook Log section of the API documentation](/api-reference/webhooks/list-webhook-logs).

Webhook logs request:
```bash
curl --request GET \
--url https://example.com/api/v3/webhook/logs \
--header 'Accept: application/json'
```

Response:
```json
{
  "content" : [ {
    "id" : 1,
    "webhookId" : 1,
    "webhookType" : "order.status",
    "webhookUrl" : "http://webhookserver.com",
    "payload" : "{'id':1234}",
    "createTimestamp" : "2023-08-22T14:15:30.345Z",
    "lastResponseStatus" : 200,
    "retryAttempts" : 3,
    "lastRetryTimestamp" : "2023-08-22T14:15:30.345Z"
  } ],
  "page" : 0,
  "size" : 0,
  "total" : 0,
  "first" : true,
  "last" : true
}
```

### Walkthroughs

To facilitate user understanding on the use of webhooks in real-world scenarios, this guide includes various case studies on potential webhook applications. Real code examples and implementations are provided to give a practical understanding of webhook implementation.

> **Note:** In all subsequent examples, `https://example.com/webhook` is used as a placeholder for your webhook URLs.

### Patient Reading Events

The `reading.*` webhook types can be utilized to receive patient reading data via webhook. Depending on the type of reading data you wish to receive, you can use one of the following for the `type` parameter:

| Name                      |
|---------------------------|
| `reading.bloodGlucose`    |
| `reading.bloodOxygen`     |
| `reading.bloodPressure`   |
| `reading.ecg`             |
| `reading.temperature`     |
| `reading.weight`          |

To get notifications on new weight readings, you can create a device webhook through the webhook endpoint.

```bash
POST /api/v3/webhook
curl -X POST http://app.impiloplatform.com/api/v3/webhook \
-H "Content-Type: application/json" \
-d '{"type":"reading.weight","url":"https://example.com/webhook"}'
```

Going forward, the recorded weight readings of Impilo will be forwarded to `https://example.com/webhook` in the following format:

```json
{
    "id": 57638473,
    "type": "reading.weight",
    "payload": {
        "id": 1,
        "readingTimestamp": "readingTimestamp",
        "weight": 1.1,
        "manual": true,
        "patient": {},
        "item": {},
        "device": {},
        "site": {}
    }
}
```

For pulling readings and more information about weight readings, see the weight reading section of the API Reference.

### Webhook Testing

### Using Webhook Tests

The Impilo API provides several endpoints for testing different types of webhook events. These endpoints allow you to simulate real-world events and observe how your system handles the incoming webhook payloads. This is particularly useful for debugging and ensuring that your webhook handlers are working correctly.

First, create a webhook using the above instructions. Then, fire a test event for the desired event type.

#### Test Blood Pressure Webhook

For example, to trigger a test blood pressure webhook, use the following endpoint:  

```bash
curl --request POST \
--url https://example.com/api/v3/test/blood-pressure-webhook \
--header 'Content-Type: application/json' \
--data '{
  "patientId" : 1,
  "systolic" : 120,
  "diastolic" : 80,
  "heartRate" : 100
}'
```

More endpoints are available in the [tests section](/api-reference/tests) of the API reference.

### More

- [Webhook API Reference](/api-reference/webhooks/create-webhook)
- [Weight Reading API Reference](/api-reference/weight-reading)
