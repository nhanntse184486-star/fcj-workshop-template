---
title: "Blog 3"
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

## Amazon EventBridge Scheduler: Automating Recurring Tasks with AWS Lambda

### 1. Why use a scheduling service?

Applications often need periodic tasks such as report generation, data checks or notifications. Keeping a server running solely to wait for a task adds operational work. EventBridge Scheduler provides AWS-managed scheduling that invokes configured targets.

### 2. Schedule types

| Type | Example | Purpose |
|---|---|---|
| Rate-based | `rate(30 minutes)` | Repeat at regular intervals |
| Cron-based | `cron(0 8 * * ? *)` | Run daily at 08:00 in the configured time zone |
| One-time | `at(2026-12-01T08:00:00)` | Run once at a specified time |

Scheduler supports time zones for cron and one-time schedules. Its scheduling precision is at the minute level, not suitable for second-level real-time control.

### 3. Components and reliability

Reference flow: `Schedule → Execution role → Lambda → CloudWatch Logs`.

The execution role authorizes Scheduler to invoke a specific target. A retry policy defines retry limits and the maximum age of undelivered events. An Amazon SQS dead-letter queue (DLQ) can capture events that could not be delivered after the retry conditions are exhausted. A flexible time window is useful when invocation can occur within an interval rather than at a fixed minute.

Scheduler provides at-least-once delivery, so business logic should be idempotent when duplicate executions could cause side effects.

### 4. Hands-on guide: invoke Lambda every 30 minutes

Create a Lambda function with a supported Python runtime and a simple handler:

```python
import json
from datetime import datetime, timezone

def lambda_handler(event, context):
    result = {
        "task": "scheduled-check",
        "timestamp": datetime.now(timezone.utc).isoformat()
    }
    print(json.dumps(result))
    return result
```

1. In the Lambda console, create `scheduled-check` and grant CloudWatch Logs permissions.
2. Open EventBridge Scheduler, select Create schedule and name it `check-every-30-minutes`.
3. Select a recurring rate-based schedule, enter `rate(30 minutes)` and turn the flexible time window off if a fixed schedule is required.
4. Select AWS Lambda as the target, choose the function and configure an execution role with `lambda:InvokeFunction` permission for that function.
5. Configure a suitable retry policy, optionally add an SQS DLQ, enable the schedule and save.
6. Inspect the Lambda logs in CloudWatch after invocation. Delete the schedule, function and dedicated lab resources when no longer required.

### 5. Validation scenarios

| Scenario | Expected result |
|---|---|
| Enabled schedule and valid permissions | Target receives scheduled invocations |
| Missing Lambda invocation permission | Scheduler records a delivery error |
| Retries exhausted with a configured DLQ | Undelivered event reaches the DLQ if authorized |
| Duplicate Lambda event | Business logic avoids duplicate side effects |

Function execution failures after an asynchronous request has been accepted by Lambda require separate monitoring and handling at the Lambda layer.

### 6. Connection to the internship project

Scheduler could aggregate daily drowsiness and distraction event counts or generate periodic reports from DynamoDB metadata. This is a possible extension for Driver Drowsiness & Distraction Detection, not a replacement for immediate edge alerts or a required component of the current architecture.

### 7. Conclusion

EventBridge Scheduler is suitable for background tasks with clear timing requirements. Restricting permissions, handling duplicates, monitoring failures and deleting unused schedules improve operational reliability.

### References

- [EventBridge Scheduler](https://docs.aws.amazon.com/scheduler/latest/UserGuide/what-is-scheduler.html)
- [Schedule types](https://docs.aws.amazon.com/scheduler/latest/UserGuide/schedule-types.html)
- [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
- [Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html)
