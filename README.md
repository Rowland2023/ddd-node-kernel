# ddd-node-kernel

Production-grade Domain-Driven Design building blocks for Node.js.

Battle-tested patterns for transaction boundaries, event sourcing, and observability in high-throughput services. Zero dependencies on specific ORMs or frameworks.

## **Why this exists**

Most DDD examples stop at entities and value objects. This kernel handles the hard parts: concurrency, outbox, and tracing across async boundaries. Drop it into any Node.js service and focus on business logic.

## **Core Components**

| Component | Responsibility | Key Features |
| --- | --- | --- |
| **`IRepository`** | Aggregate persistence port | `save()`, `getById()`, `findByIdForUpdate()` with optimistic locking contract |
| **`UnitOfWork`** | Transaction boundary + outbox | Atomic commit of domain changes + events. Prevents dual-write failures |
| **`EventBus`** | Domain event publishing | `publishBatch()` for outbox pattern. At-least-once delivery semantics |
| **`TransactionManager`** | DB transaction abstraction | Vendor-agnostic `execute()` for wrapping UoW. Works with Postgres, MySQL |
| **`TelemetryUseCaseDecorator`** | Cross-cutting observability | Auto-injects logging, metrics, tracing via AsyncLocalStorage. Zero boilerplate |
| **`AggregateRoot`** | Domain event collection | `pullDomainEvents()`, optimistic `version` field |
| **`Specification`** | Composable business rules | `isSatisfiedBy()`, `and()`, `or()`, `not()`. Translatable to SQL |
| **`ILogger`** | Structured logging port | `child()` for request context. Pino/Winston adapters included |

## **Production Features**

1. **Outbox Pattern**: Domain events written to DB in same transaction as state change. Background publisher ensures eventual delivery to Kafka/RabbitMQ. No lost events on crash.
2. **Optimistic Locking**: `aggregate.version` checked on save. Prevents lost updates under concurrency. Throws `OptimisticLockError` for retry.
3. **Handless Observability**: `AsyncLocalStorage` middleware propagates `correlationId` + `logger` automatically. Every log/metrics/trace correlated without passing params.
4. **Framework Agnostic**: No Express/Fastify/NestJS imports. Use with any HTTP layer. Ports/adapters only.
5. **Test Friendly**: All infra mocked via ports. Unit test domain logic in milliseconds.

## **Quick Start**

### **1. Install**
```bash
npm install ddd-node-kernel
