# Production-Grade DDD Building Blocks for Node.js

Zero-boilerplate foundation for building scalable, maintainable Domain-Driven Design applications in Node.js/TypeScript.

Built for production: includes outbox pattern, optimistic locking, and first-class observability.

### **What’s Inside**

| Module | Purpose | Why It Matters |
| --- | --- | --- |
| **AggregateRoot** | Base class for domain aggregates with event tracking | Enforces invariants + generates domain events |
| **DomainEvent** | Versioned, traceable event objects | Safe evolution, distributed tracing |
| **ValueObject** | Immutable value equality with deep freeze | Prevents accidental mutation bugs |
| **Specification** | Composable business rules + DB query translation | Reuse rules across domain + infrastructure |
| **IRepository** | Aggregate-only persistence port | No anemic models, enforces transaction scope |
| **UnitOfWork** | Transaction boundary + outbox integration | Atomic writes + guaranteed event delivery |
| **EventBus** | At-least-once async event publishing | Decouples domains without data loss |
| **ILogger** | Structured logging port with `child()` context | CorrelationId on every line |
| **Observability Decorator** | Wraps use cases with logging/metrics/tracing | Zero duplication, 100% coverage |

### **Production Features**

1. **Transactional Outbox**: Events are saved in the same DB transaction as aggregate changes. Background publisher guarantees delivery to Kafka/RabbitMQ. No dual-write failures.
2. **Optimistic Locking**: `version` field on aggregates prevents lost updates in multi-instance deployments. `save()` throws on version mismatch.
3. **Observability Built-In**: `TelemetryUseCaseDecorator` automatically adds structured logs, duration metrics, and trace spans to every use case. Uses `AsyncLocalStorage` so correlationId flows without manual passing.
4. **Dependency Inversion**: All infra depends on domain ports. Swap Pino → Winston, Postgres → Mongo, Datadog → Prometheus with zero business code changes.
5. **Battle-Tested Patterns**: Handles idempotency, race conditions, graceful shutdown, and DLQ routing out of the box.

### **Quick Start**

```bash
npm install
npm test # 100% coverage on domain kernel
