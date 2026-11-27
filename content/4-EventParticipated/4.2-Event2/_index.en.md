---
title: "Event 2 – Cloud Mastery 1"
date: 2026-11-14
weight: 2
chapter: false
draft: false
pre: "<b> 4.2. </b>"
---

# Cloud Mastery 1

## 1. Event Information

| Information | Details |
|---|---|
| Event name | Cloud Mastery 1 |
| Date | Saturday, November 14, 2026 |
| Venue | 26th Floor, Bitexco Tower, Ho Chi Minh City, Vietnam |
| Format | On-site knowledge-sharing session |
| Role | Attendee |

## 2. Event Objectives

Cloud Mastery 1 provided an opportunity to explore cloud computing through practical examples of AI application development. The event helped students understand AWS service selection, operational considerations, and the factors that contribute to trustworthy systems. It also offered an opportunity to connect with others interested in cloud computing and AI.

## 3. Key Highlights and Activities

### 3.1. The Role of Cloud in AI Applications

The **The Role of Cloud** presentation used a restaurant-chain analogy to explain how cloud infrastructure supports AI applications. The slide emphasized four considerations:

| Consideration | Meaning |
|---|---|
| Security | Protect data, control access, and reduce operational risks. |
| Scalability | Adjust resources according to processing needs and actual traffic. |
| Reliability | Aim to maintain system operation when a component fails. |
| Cost | Use resources according to demand, reduce waste, and monitor spending. |

These considerations should be evaluated together when building an application rather than focusing exclusively on model accuracy.

### 3.2. AWS Toolbox – Services for AI Development

| Service | Role introduced in the presentation |
|---|---|
| AWS Lambda | Execute code in response to events without managing dedicated servers. |
| Amazon Bedrock | Access foundation models through APIs to build AI applications. |
| Amazon Bedrock AgentCore | Provide capabilities for operating AI agents, including runtime, memory, and identity management. |

The distinct roles of these services illustrate how an AI application can combine specialized components rather than assigning all processing responsibilities to a single model.

### 3.3. Use AI Wisely

The presentation compared AI orchestration to a head chef coordinating different stations in a restaurant. A larger model can coordinate the workflow, while smaller models perform specialized tasks. Two key principles were matching the model to the task and caching repeated questions when appropriate.

This approach encourages developers to consider output quality, latency, and cost instead of defaulting to the largest model for every request.

### 3.4. Real-World Case Study: Cox Automotive

The Cox Automotive example addressed the challenge of building automated AI solutions that users can trust. The slide introduced an approach that standardizes the foundation before scaling agent deployments:

| Component | Role in the presented case study |
|---|---|
| AgentCore Runtime | A consistent execution environment supporting deployment, monitoring, and error handling. |
| AgentCore Identity | Granular, role-based permissions for individual agents. |
| Amazon Bedrock Guardrails | Input and output screening to reduce risks from inappropriate content. |
| AgentCore Memory | Context retention across interaction sessions. |
| AgentCore Observability | Interaction logging and support for troubleshooting multi-step workflows. |

The example demonstrates that AI agents require not only information-processing capabilities but also appropriate governance, security, and observability foundations.

## 4. Knowledge and Key Takeaways

- AI application design should begin with actual user requirements and operational constraints.
- Security, scalability, reliability, and cost need to be balanced when selecting a cloud architecture.
- Choosing a model according to task complexity can avoid unnecessary resource usage.
- Access control, data protection, and observability are important for building trustworthy AI applications.
- Real-world examples help connect theoretical knowledge to practical software design decisions.

## 5. Applying the Knowledge to the Internship Project

For the **Driver Drowsiness & Distraction Detection** project, the event provides the following practical connections:

| Lesson | Application to the project |
|---|---|
| Appropriate processing approach | Prioritize on-device image processing using OpenCV and MediaPipe, transmitting only necessary event metadata to the cloud. |
| Event-driven architecture | Use API Gateway and Lambda to receive and process drowsiness or distraction events. |
| Reliability | Consider buffering, retries, and event identifiers to reduce lost or duplicated data. |
| Security | Limit IAM permissions, authenticate dashboard users through Cognito, and protect personal image data. |
| Cost and monitoring | Prioritize serverless services that suit demand and monitor errors and operational metrics with CloudWatch. |

Bedrock and AgentCore broaden the perspective on AWS AI application development, but they are not required components of the current driver drowsiness detection system.

## 6. Conclusion

Cloud Mastery 1 was an opportunity to explore how AWS supports the development and operation of AI applications from an architectural perspective. Attending with a fellow student provided opportunities to exchange ideas about technology and skill development. The central lesson was to select technologies according to system objectives while balancing processing efficiency, security, reliability, and operational cost.

## 7. Event Participation Photos

![Check-in photo with a fellow participant at Cloud Mastery 1](/fcj-workshop-template/images/4-EventParticipated/Event2/check-in.png)

*Figure 1. Check-in photo with a fellow participant at Cloud Mastery 1.*

![The Role of Cloud presentation: security, scalability, reliability and cost](/fcj-workshop-template/images/4-EventParticipated/Event2/role-of-cloud.png)

*Figure 2. The Role of Cloud presentation: security, scalability, reliability and cost.*

![Use AI Wisely slide: matching models to tasks](/fcj-workshop-template/images/4-EventParticipated/Event2/use-ai-wisely.png)

*Figure 3. Use AI Wisely slide: matching models to tasks.*

![AWS Toolbox slide: Lambda, Bedrock and AgentCore](/fcj-workshop-template/images/4-EventParticipated/Event2/aws-toolbox.png)

*Figure 4. AWS Toolbox slide: Lambda, Bedrock and AgentCore.*

![Cox Automotive case study and the challenge of building trustworthy AI](/fcj-workshop-template/images/4-EventParticipated/Event2/cox-automotive.png)

*Figure 5. Cox Automotive case study and the challenge of building trustworthy AI.*

![How Cox Automotive standardizes its AI agent foundation with AgentCore](/fcj-workshop-template/images/4-EventParticipated/Event2/agentcore.png)

*Figure 6. How Cox Automotive standardizes its AI agent foundation with AgentCore.*

