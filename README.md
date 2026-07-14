# ddd-node-kernel

Production-grade Domain-Driven Design building blocks for Node.js.

A framework-agnostic shared kernel providing the core abstractions needed to build modular, event-driven applications using Domain-Driven Design (DDD) and Clean Architecture.

Designed for production systems where correctness, maintainability, and explicit business modeling are more important than framework-specific conventions.

---

## Why this exists

Most DDD examples stop at Entities and Value Objects.

Production systems also need:

- Aggregate roots
- Immutable domain events
- Optimistic concurrency
- Repository contracts
- Transaction boundaries
- Transactional Outbox
- Event publishing
- Specifications
- Telemetry integration

This library provides those reusable building blocks while remaining independent of Express, NestJS, Prisma, Sequelize, TypeORM, or any specific persistence technology.

---

## Features

- Aggregate Roots
- Entities
- Value Objects
- Domain Events
- Repository Interfaces
- Unit of Work
- Event Bus
- Transaction Manager
- Specifications
- Optimistic Locking
- Transactional Outbox Support
- Framework Agnostic
- Infrastructure Agnostic
- Fully Testable

---

## Architecture

```
                Application Layer

                     Use Cases
                         │
                         ▼
                Aggregate Root
                         │
         ┌───────────────┴───────────────┐
         │                               │
      Entities                     Value Objects
         │
         ▼
    Domain Events
         │
         ▼
      Unit of Work
         │
         ▼
 Repository + Outbox
         │
         ▼
 Event Publisher (Kafka/RabbitMQ/etc.)
```

---

## Core Components

### AggregateRoot

Provides:

- identity
- optimistic versioning
- domain event recording
- event collection

```javascript
payment.recordEvent(
    new PaymentCreatedEvent(...)
);

const events = payment.pullEvents();
```

---

### Entity

Provides:

- identity equality
- version tracking
- event recording
- immutable identifiers

---

### ValueObject

Immutable value semantics.

Examples:

- Money
- Email
- Currency
- Address

---

### DomainEvent

Immutable event envelope supporting:

- event name
- aggregate id
- event version
- occurredAt
- correlationId
- causationId
- immutable payload

Example:

```javascript
new PaymentReleasedEvent({
    paymentId,
    correlationId,
    occurredAt
});
```

---

### Repository

Persistence contract.

```javascript
await repository.save(aggregate);

const payment =
    await repository.findById(id);
```

---

### UnitOfWork

Coordinates atomic commits.

```javascript
await unitOfWork.execute(async () => {

    await repository.save(payment);

    await outbox.store(
        payment.pullEvents()
    );

});
```

---

### EventBus

Publishes domain events after persistence.

Supports:

- Kafka
- RabbitMQ
- Redis Streams
- SNS/SQS
- Custom adapters

---

### Specification

Composable business rules.

```javascript
activeCustomerSpecification
    .and(hasCreditLimit)
    .or(isPremiumCustomer);
```

---

## Design Principles

- Domain-Driven Design
- Clean Architecture
- Dependency Inversion
- Rich Domain Models
- Immutable Domain Events
- Explicit Transaction Boundaries
- Optimistic Concurrency
- Event-Driven Architecture

---

## Example

```javascript
const payment = Payment.create(command);

payment.markSuccessful({
    gatewayTransactionId,
    paidAt
});

await unitOfWork.execute(async () => {

    await paymentRepository.save(payment);

    await outbox.store(
        payment.pullEvents()
    );

});
```

---

## Framework Support

Works with any Node.js framework.

- Express
- Fastify
- NestJS
- Hono
- Koa
- Custom HTTP Servers

---

## Storage Support

Compatible with

- PostgreSQL
- MySQL
- SQL Server
- MongoDB
- DynamoDB
- EventStoreDB

---

## Testing

The domain layer has no infrastructure dependencies.

Repositories, event buses, and transactions are injected through interfaces, making domain logic straightforward to unit test.

---

## Project Origin

This library was extracted from a modular Conference Management Platform after multiple bounded contexts began sharing the same domain abstractions.

The original platform contains independent modules including:

- Payments
- Registrations
- Conferences
- Notifications

Each module depends on this shared kernel for domain modeling and application contracts.

---

## License

MIT
