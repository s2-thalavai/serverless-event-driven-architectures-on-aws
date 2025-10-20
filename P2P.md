# Core Event-Driven Architecture Components for Vendor & Invoice Management (AWS)

## 1. Amazon EventBridge
- Acts as the central event bus.
- Publishes events such as:
  - `VendorOnboarded`
  - `InvoiceSubmitted`
  - `InvoiceValidated`
  - `InvoiceApproved`
  - `InvoicePaid`
- Enables schema validation and cross-service routing.

## 2. Amazon SQS & SNS
- **SQS**:
  - Ensures reliable delivery with retry and DLQ support.
  - Ideal for decoupling invoice validation and compliance checks.
- **SNS**:
  - Fan-out mechanism to notify multiple consumers.
  - Useful for broadcasting `InvoicePaid` events to accounting, audit, and notification services.

## 3. AWS Lambda
- Stateless event processors.
- Sample use cases:
  - Validate invoice schema and metadata.
  - Enrich vendor profiles with external data.
  - Trigger compliance workflows.
  - Send notifications or update status in DynamoDB.

## 4. Amazon DynamoDB
- Stores:
  - Vendor profiles
  - Invoice metadata
  - Audit logs
- Features:
  - TTL for automatic expiry
  - Streams for change tracking
  - Versioning for traceability

## 5. AWS Step Functions (Optional)
- Orchestrates multi-step workflows:
  - Invoice approval chains
  - Dispute resolution
  - Conditional branching based on validation results
- Enables retry policies and state tracking.


## Invoice Lifecycle (AWS Event-Driven Architecture)

| Event              | Producer        | Consumer                        | Action                          |
|--------------------|-----------------|----------------------------------|----------------------------------|
| VendorRegistered   | Portal/API      | Lambda → DynamoDB               | Create vendor profile            |
| InvoiceSubmitted   | Vendor Portal   | Lambda → SQS                    | Validate invoice schema          |
| InvoiceValidated   | Validator Lambda| SNS → Compliance Lambda         | Trigger compliance checks        |
| InvoiceApproved    | Compliance Lambda| EventBridge → Payment Lambda    | Initiate payment                 |
| InvoicePaid        | Payment Lambda  | EventBridge → Notification Lambda| Notify vendor, update status     |

### Observability & Auditability

- **CloudWatch Logs & Metrics:** Track retries, failures, and latency per event type.

- **EventBridge Archive & Replay:** Reprocess stuck or failed events.

- **DynamoDB Streams:** Capture changes for audit trails or downstream analytics.

- **X-Ray:** Trace event flow across services for debugging and performance tuning.

### Resilience & Compliance

- Use DLQs for SQS and Lambda to capture failed events.

- Implement idempotency keys in invoice processing to avoid duplicates.

- Encrypt sensitive data at rest (DynamoDB, S3) and in transit (API Gateway, EventBridge).

- Tag events with metadata like invoiceId, vendorId, timestamp, and region for compliance and analytics.


> https://aws.amazon.com/blogs/architecture/implement-event-driven-invoice-processing-for-resilient-financial-monitoring-at-scale/

