# Amazon MSK (amazon-msk)
Amazon Managed Streaming for Apache Kafka (Amazon MSK) is a fully managed service that enables you to build and run applications that use Apache Kafka to process streaming data, with the infrastructure management handled by AWS.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/amazon-msk/refs/heads/main/apis.yml)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Tags:

 - AWS, Broadcasting, Media Processing, Media

## Timestamps

- **Created:** 2026-03-16
- **Modified:** 2026-04-19

## APIs

### Amazon MSK API
Amazon Managed Streaming for Apache Kafka (Amazon MSK) is a fully managed service that enables you to build and run applications that use Apache Kafka to process streaming data, with the infrastructure management handled by AWS.

**Human URL:** [https://aws.amazon.com/msk/](https://aws.amazon.com/msk/)

#### Tags:

 - Broadcasting, Media Processing, Media

#### Properties

- [Documentation](https://docs.aws.amazon.com/msk/)
- [OpenAPI](openapi/amazon-msk-openapi-original.yml)
- [GettingStarted](https://aws.amazon.com/msk/getting-started/)
- [Pricing](https://aws.amazon.com/msk/pricing/)
- [FAQ](https://aws.amazon.com/msk/faqs/)

## Common Properties

- [Portal](https://aws.amazon.com/msk/)
- [Documentation](https://docs.aws.amazon.com/msk/)
- [TermsOfService](https://aws.amazon.com/service-terms/)
- [PrivacyPolicy](https://aws.amazon.com/privacy/)
- [Support](https://aws.amazon.com/premiumsupport/)
- [Blog](https://aws.amazon.com/blogs/media/)
- [GitHubOrganization](https://github.com/aws)
- [Console](https://console.aws.amazon.com/msk/)
- [SignUp](https://portal.aws.amazon.com/billing/signup)
- [StatusPage](https://health.aws.amazon.com/health/status)
- [Contact](https://aws.amazon.com/contact-us/)

## Features

| Name | Description |
|------|-------------|
| Fully Managed Kafka | Automatically provisions, configures, and maintains Apache Kafka clusters without operational overhead. |
| High Durability | Multi-AZ deployments with automatic replication and failover for data durability. |
| MSK Serverless | Serverless cluster mode that automatically scales capacity to match streaming demand. |
| MSK Connect | Fully managed Kafka Connect to stream data to and from databases and other services. |
| Tiered Storage | Offload older data to low-cost Amazon S3 storage while keeping recent data on brokers. |
| Schema Registry | Manage and enforce schemas for Kafka topics with AWS Glue Schema Registry integration. |

## Use Cases

| Name | Description |
|------|-------------|
| Real-Time Data Streaming | Build real-time data pipelines for clickstream analytics, log aggregation, and metrics. |
| Event Sourcing | Implement event sourcing patterns with durable, ordered Kafka topics. |
| Stream Processing | Process streaming data with Apache Flink, Spark Streaming, or custom consumers. |
| Database Change Data Capture | Stream database changes to downstream systems using Debezium and MSK Connect. |

## Integrations

| Name | Description |
|------|-------------|
| Amazon Kinesis Data Analytics | Process MSK streams with Kinesis Data Analytics for Apache Flink. |
| AWS Lambda | Trigger Lambda functions from MSK topics for serverless stream processing. |
| Amazon S3 | Use MSK Connect to sink data from Kafka topics to S3 buckets. |
| Amazon CloudWatch | Monitor cluster and broker metrics with CloudWatch dashboards and alarms. |
| AWS IAM | Authenticate Kafka clients using IAM roles for MSK cluster access control. |
| AWS Glue Schema Registry | Enforce data schemas for Kafka producers and consumers. |

## Artifacts

Machine-readable API specifications organized by format.

### OpenAPI

- [Amazon MSK OpenAPI](openapi/amazon-msk-openapi-original.yml)

### JSON Schema

- 135 schema files in [json-schema/](json-schema/)

### JSON Structure

- 135 structure files in [json-structure/](json-structure/)

### JSON-LD

- [Amazon MSK API Context](json-ld/amazon-msk-msk-api-context.jsonld)

### Examples

- 135 example files in [examples/](examples/)

## Capabilities

Naftiko capabilities organized as shared per-API definitions composed into customer-facing workflows.

### Shared Per-API Definitions

- [Amazon MSK](capabilities/shared/msk.yaml) — 36 operations for media processing

### Workflow Capabilities

| Workflow | APIs Combined | Tools | Persona |
|----------|--------------|-------|---------|
| [Amazon MSK Workflow](capabilities/amazon-msk-media-workflow.yaml) | Amazon MSK | 8 | Broadcast Engineer |

## Vocabulary

- [Amazon MSK Vocabulary](vocabulary/amazon-msk-vocabulary.yaml) — Unified taxonomy mapping resources, actions, workflows, and personas across operational (OpenAPI) and capability (Naftiko) dimensions

## Rules

- [Amazon MSK Spectral Rules](rules/amazon-msk-spectral-rules.yml) — 20 rules across 8 categories enforcing Amazon MSK API conventions

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
