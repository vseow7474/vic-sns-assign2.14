# Assignment 2.14

## 1. Does SNS guarantee exactly-once delivery to subscribers?

No,**Amazon SNS** (Simple Notification Service) does not guarantee **exactly-once delivery** to subscribers. It provides **at-least-once delivery**, which means:

- A message is delivered to all subscribed endpoints at least once.
- There is a possibility of duplicate messages being delivered to subscribers.
- Subscribers should be designed to handle idempotency to avoid issues caused by duplicate messages.

## 2. What is the purpose of the Dead-letter Queue (DLQ)?

A **Dead-letter Queue (DLQ)** is a feature that captures messages that could not be successfully processed or delivered to their destination after a certain number of retries. DLQs are available for **SQS**, **SNS**, and **EventBridge** and serve these purposes:

- **Message Durability**: Prevents messages from being lost when processing fails repeatedly.
- **Debugging**: Allows developers to analyze the reasons behind processing failures by inspecting messages in the DLQ.
- **Decoupling Retry Logic**: Enables asynchronous handling of failed messages without overburdening the primary queue or topic.

## 3. How would you enable a notification to your email when messages are added to the DLQ?

To receive email notifications when messages are added to a DLQ, follow these steps:

### 1. Configure a DLQ for your SNS topic or SQS queue:

- For **SNS**, create an SQS queue to act as the DLQ and attach it to your topic.
- For **SQS**, specify a DLQ in the queue’s configuration.

### 2. Enable a CloudWatch alarm to monitor the DLQ:

- Go to the CloudWatch console and create a metric alarm for the ApproximateNumberOfMessagesVisible metric of the DLQ.
- Set a threshold (e.g., when the metric is greater than 0).

### 3. Create an SNS topic for notifications:

- Create a new SNS topic to handle alarm notifications.
- Subscribe your email address to this SNS topic.

### 4. Connect the CloudWatch alarm to the SNS topic:

- In the alarm actions, specify the SNS topic you created for notifications.

### 5. Verify the subscription:

- Check your email for a subscription confirmation message and confirm it.

**Result**: Whenever messages are added to the DLQ and the threshold condition is met, you’ll receive an email notification.

---
