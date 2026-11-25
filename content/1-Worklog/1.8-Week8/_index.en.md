---
title: "Week 8 Worklog"
date: 2026-11-09
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

* Finalize state-analysis flow and event metadata format.
* Integrate HTTP API with Lambda Event Writer for metadata ingestion.
* Integrate Lambda Event Reader to retrieve event history.

### Weekly work activities:

| No. | Task | Reference Material |
| --- | --- | --- |
| 1 | Finalize state-analysis flow and event metadata format. | Proposal – Driver Drowsiness & Distraction Detection |
| 2 | Work on event IDs, local event buffering and retries. | Proposal – Driver Drowsiness & Distraction Detection |
| 3 | Integrate HTTP API with Lambda Event Writer for metadata ingestion. | <https://docs.aws.amazon.com/lambda/> |
| 4 | Integrate DynamoDB and conditional PutItem for duplicate-event prevention. | <https://docs.aws.amazon.com/amazondynamodb/> |
| 5 | Integrate Lambda Event Reader to retrieve event history. | <https://docs.aws.amazon.com/amazondynamodb/> |

### Achievements:

* Completed the integration between the Edge AI pipeline and the AWS backend for event metadata transmission and processing.
* The backend architecture uses API Gateway HTTP API, Lambda Event Writer, Lambda Event Reader and DynamoDB for ingestion, storage and event-history queries.
* Designed event identification, conditional writes and synchronization mechanisms to address duplicate events and network interruptions.
