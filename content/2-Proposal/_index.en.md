---
title: "Proposal"
date: 2026-11-25
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Driver Drowsiness & Distraction Detection
## An Edge AI and AWS Serverless Solution for Driver Monitoring

### 1. Executive Summary

This project proposes a prototype for detecting signs of driver drowsiness and distraction using Computer Vision, Edge AI and AWS Cloud. A webcam captures facial images, which are processed locally using OpenCV and MediaPipe Face Mesh with 478 facial landmarks. Eye Aspect Ratio (EAR), Mouth Aspect Ratio (MAR), head pose and experimental gaze direction are extracted to support temporal, rule-based analysis.

Instead of continuously uploading video, the device sends only event metadata to AWS. A serverless backend validates, stores and retrieves events, while an authenticated web dashboard allows authorized users to review event history. The design aims to reduce network usage and latency, preserve privacy and minimize infrastructure management. The prototype is an awareness and research tool, not a certified driver safety system.

### 2. Problem Statement

#### Current problem

Prolonged eye closure, yawning, changes in head orientation and changes in gaze may indicate drowsiness or distraction. These signs can be difficult to notice in time. Continuous cloud-based video processing requires bandwidth and may introduce latency, increase costs and expose sensitive facial data. Unstable connectivity can also interrupt event reporting.

#### Proposed solution

The system separates local detection from cloud event management. The edge device extracts facial features and analyzes them over time. When a candidate event is detected, it generates an `event_id` and prepares metadata for local buffering and synchronization. An AWS HTTP API forwards the event to a Lambda writer, which validates the payload and uses a conditional DynamoDB write to prevent duplicate records. A separate Lambda reader serves authorized history queries. The dashboard uses Amazon Cognito for authentication and Amazon CloudFront with a private S3 origin for frontend delivery.

#### Benefits and return on investment

- Local video processing reduces reliance on network connectivity for detection and avoids uploading raw video.
- Centralized event history supports review and future analysis without maintaining a dedicated server.
- Serverless, usage-based infrastructure limits idle resource costs at prototype scale.
- Reusing a laptop and webcam, if available, avoids additional hardware expenditure.

The anticipated value is technical and educational: a reusable Edge AI pipeline, a cloud integration reference and hands-on experience with AWS services. Financial savings, accident reduction and a monetary ROI have not been measured; no payback period is claimed.

### 3. Solution Architecture

The proposed system consists of three layers: edge processing, cloud event management and an authenticated dashboard.

![System Architecture Diagram](/fcj-workshop-template/images/2-Proposal/architecture.jpg)

*Figure 1. Overall architecture of the Driver Drowsiness & Distraction Detection system using Edge AI and AWS Cloud.*

#### Data flow

1. The webcam captures facial images locally. Raw video is not sent to AWS.
2. OpenCV and MediaPipe Face Mesh extract 478 landmarks and calculate EAR, MAR, head pose and experimental gaze features.
3. Temporal analysis uses rule-based candidates and CSV-based evaluation to distinguish sustained abnormal patterns from short movements.
4. A detected event receives a unique `event_id`. The planned local event buffer retains unsynchronized metadata and retries when connectivity returns.
5. The device sends an HTTPS `POST /events` request to Amazon API Gateway (HTTP API). The ingestion route must authenticate the device or its authorized identity before deployment.
6. AWS Lambda Event Writer validates the payload and uses a DynamoDB conditional `PutItem` based on the unique event key. A repeated event must not create another record; an acknowledged duplicate should be treated as already synchronized.
7. The dashboard signs users in through Amazon Cognito and obtains JWTs. API Gateway uses a JWT authorizer for protected routes.
8. An authorized `GET /events` request invokes Lambda Event Reader, which queries DynamoDB and returns paginated event history. The backend must enforce ownership or authorized scope, not merely token validity.
9. Amazon CloudFront delivers the static dashboard from a private Amazon S3 bucket using Origin Access Control (OAC).
10. Amazon CloudWatch monitors API access logs, Lambda logs, errors, duration and DynamoDB metrics. AWS IAM, CloudTrail and AWS Budgets support permissions, auditing and spending oversight. AWS WAF is optional; if selected, its attachment point and coverage must be verified.

#### AWS services used or planned

| Service | Role |
| --- | --- |
| Amazon API Gateway | HTTP API for event ingestion and history retrieval; JWT authorization for protected routes |
| AWS Lambda | Separate Event Writer and Event Reader functions |
| Amazon DynamoDB | Event metadata storage, conditional writes and history queries |
| Amazon Cognito | User registration, sign-in and JWT issuance |
| Amazon S3 | Private storage for static dashboard files |
| Amazon CloudFront | HTTPS frontend delivery with OAC |
| Amazon CloudWatch | Logs, operational metrics and alarms |
| AWS IAM | Least-privilege permissions for AWS resources |
| AWS CloudTrail | Auditing AWS account activity |
| AWS Budgets | Budget tracking and spending alerts |
| AWS WAF (optional) | Additional web protection, subject to compatibility and budget |

#### Component and data design

The edge pipeline runs independently of cloud connectivity. The proposed event schema includes `event_id`, `device_id`, `event_type`, UTC timestamp, duration and relevant derived features. Final field names, detection thresholds and retention periods will be defined during implementation. A proposed DynamoDB design uses `event_id` as a unique primary key and an authorized device/user scope plus timestamp as an index for history queries; the final index and access model must be validated against the actual query patterns.

The writer should reject malformed payloads, verify the identity and permitted device scope, and avoid storing raw images or video. The reader should restrict queries to the caller's authorized scope. JWTs, credentials and sensitive payloads must not be exposed in source code or logs. The S3 bucket will block public access, with CloudFront OAC granted only the required origin access. Event metadata may still be sensitive, so access and retention need to be controlled.

### 4. Technical Implementation

#### Implementation phases

1. **Edge AI and feature extraction:** configure webcam capture, OpenCV and MediaPipe; calculate EAR, MAR and head pose; evaluate gaze as an experimental feature.
2. **Temporal analysis and reliability:** analyze CSV feature data, calibrate rules and event durations, implement a persistent local event buffer and bounded retry with backoff; preserve event IDs across retries.
3. **Serverless backend:** configure DynamoDB, Lambda Writer/Reader, API Gateway routes, request validation and conditional writes; verify authorized device ingestion and history retrieval.
4. **Dashboard and security:** configure Cognito, JWT authorization and backend access checks; host the static frontend in private S3 and deliver through CloudFront OAC; configure IAM and monitoring.
5. **Validation and workshop:** test detection, synchronization, duplicate delivery, access control and cloud integration; estimate costs, prepare demonstration evidence and document cleanup steps.

#### Technical requirements

| Layer | Requirements |
| --- | --- |
| Edge device | Computer with webcam, Python, OpenCV, MediaPipe and local storage |
| Analysis | EAR, MAR, head pose, experimental gaze, CSV evaluation and temporal rules |
| Cloud | AWS account, HTTP API, Lambda, DynamoDB, Cognito, S3 and CloudFront |
| Operations | IAM permissions, CloudWatch logging and alarms, CloudTrail and Budgets |
| Testing | Controlled lighting and head angles, sample labeled events, network interruption, duplicate requests and unauthorized requests |

#### Current implementation status

According to the project self-assessment, the webcam-based edge pipeline and facial feature extraction have been developed. Gaze direction remains experimental and temporal rule-based analysis requires refinement. The AWS architecture has been designed; end-to-end cloud deployment and dashboard functionality have not yet been confirmed. The local buffer, retry mechanism, accuracy evaluation and network recovery tests remain to be completed. These are planned deliverables, not claims of completed validation.

### 5. Timeline & Milestones

The following is a proposed schedule aligned with the existing Worklog dates and the intended submission at the end of Week 10. Activities should be recorded as completed only when actually performed.

| Week | Dates (2026) | Planned activities and milestones |
| --- | --- | --- |
| 1–5 | 05 Oct – 06 Nov | AWS foundations, networking, compute, storage, serverless and AI/ML study |
| 6 | 09–13 Nov | Analyze the problem, define scope, research Edge AI and design the architecture |
| 7 | 16–20 Nov | Develop facial feature extraction and evaluate EAR, MAR, head pose and experimental gaze |
| 8 | 23–27 Nov | Refine temporal analysis, local buffering and retry; implement the event ingestion backend |
| 9 | 30 Nov – 04 Dec | Implement protected history access, dashboard delivery, IAM and monitoring |
| 10 | 07–11 Dec | Test the prototype, finalize costs, document the workshop, prepare demo and submit the report |

Target submission: **11 December 2026**, subject to FCAJ approval of the submission schedule and required internship duration.

### 6. Budget Estimation

#### Planning assumptions

The following assumptions are illustrative for a small prototype, not measured usage: one edge device, up to five test users, 10,000 event submissions and 10,000 history requests per month, 20,000 Lambda invocations at 128 MB and 200 ms average duration, 1 GB of DynamoDB data including indexes, 0.1 GB of S3 storage, 10,000 CloudFront HTTPS requests, 1 GB of frontend transfer, 0.5 GB of CloudWatch log ingestion and two standard alarms. Additional retries, index operations and history query pagination must be counted when estimating actual usage.

#### Infrastructure budget

| Service / cost category | Estimated monthly usage | Proposed budget allocation (USD/month) |
| --- | --- | ---: |
| API Gateway and Lambda | 20,000 API requests and invocations | 0.50 |
| DynamoDB | On-demand reads/writes and 1 GB storage including indexes | 1.00 |
| Cognito | Up to five test users; confirm selected tier | 0.50 |
| S3 and CloudFront | Static hosting, requests and 1 GB transfer | 1.00 |
| CloudWatch | Logs, storage and two alarms | 1.00 |
| CloudTrail, Budgets and miscellaneous charges | Basic account auditing and budget notifications | 0.50 |
| Contingency | Additional requests or data transfer | 0.50 |
| **Total planning budget** | | **5.00** |

This is a proposed spending allocation, **not an AWS Pricing Calculator quotation or an actual bill**. At the same monthly budget, a twelve-month planning ceiling would be USD 60; the workshop is only a short prototype. Hardware cost is assumed to be zero additional expenditure if the existing computer and webcam are reused. Paid WAF, SMS/MFA charges, optional backups, taxes and additional traffic are excluded and must be estimated separately if required.

Before submission, select the actual AWS Region, confirm service tiers and workload, create an estimate using the [AWS Pricing Calculator](https://calculator.aws/) and add its actual share link or exported PDF. Use [AWS pricing](https://aws.amazon.com/pricing/) to verify applicable rates and free-tier eligibility. Set budget alerts (for example, at 50%, 80% and 100% of the proposed USD 5 budget), limit log retention and remove unused resources. AWS Budgets alerts do not automatically cap spending.

### 7. Risk Assessment

#### Risk matrix and mitigation

| Risk | Likelihood | Impact | Proposed mitigation |
| --- | --- | --- | --- |
| Poor lighting, occlusion or unusual head angles | Medium | High | Evaluate under controlled variations; flag unreliable face tracking |
| False positive or missed detection | Medium | High | Calibrate thresholds, use temporal rules and measure performance on labeled samples |
| Unstable network | Medium | Medium | Persistent local buffering and retry with backoff |
| Duplicate event submissions | Medium | Medium | Stable event IDs and conditional DynamoDB writes |
| Unauthorized access or sensitive data exposure | Medium | High | Authenticated ingestion, JWT, ownership checks, IAM, private S3 and minimal metadata |
| Unexpected AWS charges | Low–Medium | Medium | Usage monitoring, budget alerts, limited retention and cleanup |
| Schedule or integration delays | Medium | Medium | Prioritize edge detection, ingestion, authorized history and demo; defer optional features |

#### Contingency plans

When the network is unavailable, local detection should continue and unsynchronized metadata should remain buffered until connectivity returns. If a feature is unreliable, report the limitation and restrict its use to experimentation. If an optional cloud component cannot be completed within the schedule, document its status and demonstrate the verified core path. Tests should be conducted in a safe, stationary environment, not by intentionally inducing distraction or drowsiness while driving.

### 8. Expected Outcomes

#### Technical deliverables and evaluation

- A local computer vision prototype that extracts facial features and identifies candidate events using temporal rules.
- A metadata-only event synchronization workflow with a local buffer, retry and duplicate handling.
- An authenticated AWS dashboard displaying event history through a serverless backend.
- Documented tests covering controlled facial movements, lighting/head angles, network recovery, duplicate requests and authorization. Report observed precision/recall or false-positive and missed-event counts if labeled data is sufficient; do not claim accuracy without measurements.
- Architecture diagram, bilingual workshop instructions, cost estimate, monitoring evidence, demo and resource cleanup instructions.

#### Long-term value

The project provides a reusable foundation for further Edge AI research, improvements to temporal analysis and scalable event-driven applications. Gaze analysis remains an experimental extension until validated. The prototype is intended to support awareness and research and must not be relied upon as a substitute for safe driving practices or a validated automotive safety product.
