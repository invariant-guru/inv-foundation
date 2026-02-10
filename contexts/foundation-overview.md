# Context: Foundation Library Overview

The `@invariant--labs/foundation` library provides base building blocks for DDD and Clean Architecture in NestJS applications.

## Core Domain Primitives

- **Entity** -- Base class with `UniqueEntityId`, equality by identity, and `getProps()`.
- **AggregateRoot** -- Extends NestJS CQRS `AggregateRoot` with domain event support and `mergeContext(eventPublisher)`.
- **ValueObject** -- Immutable object with structural equality via `shallowEqual`. Subclasses implement `validate()`.
- **UniqueEntityId** -- Wraps a UUID string identifier.
- **DomainEvent** -- Base interface for domain events dispatched by aggregates.

## Application Layer

- **Repository** (abstract) -- Defines `findOne`, `findOneOrFail`, `findByIds`, `save`, `saveAll`, `exists`, `count`.
- **QueryBuilder** and **ReadProjection** -- Interfaces for read-side query patterns.

## Infrastructure

- **Persistence** -- TypeORM repositories (`AggregateTypeormRepository`, `EntityTypeormRepository`) and `InMemoryRepository` for both write-side and general use.
- **Messaging** -- Abstract `MessagingService` with RabbitMQ and fake implementations.
- **Config** -- `ConfigProvider` and `ConfigService` abstractions.

## NestJS Modules

| Module       | Purpose                                      |
|--------------|----------------------------------------------|
| `logger`     | Structured logging with pluggable transports (console-color, console-json, fake). |
| `config`     | NestJS config module wrapper.                |
| `messaging`  | Messaging module with RabbitMQ support.      |
| `queue`      | Queue management with RabbitMQ.              |
| `knex`       | Knex.js integration module.                  |
| `healthcheck`| Health check DTO.                            |
| `graphql`    | Paginated response object type for GraphQL.  |

## HTTP Layer (`nestjs/`)

- **Filters** -- `RpcExceptionsFilter`, `TooManyResultsFilter`.
- **Interceptors** -- `LogAppCtxInterceptor`, `SerializeOutputInterceptor`.
- **Middleware** -- `HttpLoggerMiddleware`, `LoggerMiddleware`.
- **Swagger** -- Swagger setup utility.

## Error Handling

Typed errors: `DomainError`, `EntityNotFoundError`, `CommandFailureError`, `CommandFilterError`, `QueryFailureError`, `TooManyResultsError`, `DetailedError`, `FakeImplementationError`.

## Utilities

Pure functions for arrays, strings, dates, base64, CSV, XML, files, hashing, pagination, and type helpers.

## Validation

Custom `class-validator` decorators: `IsIsoDate`, `IsLessThan`, `IsMoreThan`, `IsTimeString`, `EnsureArray`, and variants.

## Testing

- **Stubs** -- `CommandBusStub`, `EventBusStub`, `QueryBusStub`, `EventPublisherStub`, generic `Stub`.
- **FakeLogger** -- In-memory logger capturing logs for assertions.
