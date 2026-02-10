# Rule: Clean Architecture

Layer dependency and import rules for the foundation library.

## Layer Dependencies

- **Domain** (`core/domain/`) has zero external dependencies. It defines entities, aggregate roots, value objects, and domain events. Never import from application or infrastructure.
- **Application** (`core/application/`) depends only on Domain. It defines abstract repository ports and query interfaces. Never import from infrastructure.
- **Infrastructure** (`core/infrastructure/`) implements Application ports. It depends on both Domain and Application. Contains persistence adapters (TypeORM, InMemory), messaging, and config providers.
- **Modules** (`modules/`) provide NestJS-integrated wrappers (logger, config, messaging, queue, knex, healthcheck, graphql). They may depend on core layers.
- **NestJS** (`nestjs/`) contains HTTP-specific concerns: middleware, interceptors, filters, and Swagger setup.
- **Testing** (`testing/`) provides stubs and fakes. It may import from any layer.

## Folder Structure

```
src/
  core/
    domain/         # Entity, AggregateRoot, ValueObject, UniqueEntityId, DomainEvent
    application/    # Repository (abstract), QueryBuilder, ReadProjection
    infrastructure/ # Config, Messaging, Persistence adapters
    errors/         # DomainError, EntityNotFoundError, CommandFailureError, etc.
    types/          # Shared type definitions
  modules/          # NestJS modules (logger, config, messaging, queue, knex, healthcheck, graphql)
  nestjs/           # HTTP filters, interceptors, middleware, Swagger
  validation/       # Custom class-validator decorators
  utils/            # Pure utility functions (array, string, date, hash, csv, etc.)
  testing/          # Stubs (CommandBus, EventBus, QueryBus) and FakeLogger
```

## Import Direction Rules

- Imports must flow inward: `infrastructure -> application -> domain`.
- `domain/` must never import from `application/`, `infrastructure/`, or `modules/`.
- `application/` must never import from `infrastructure/` or `modules/`.
- `utils/` and `validation/` are leaf modules with no dependency on core layers.
- `testing/` may import from any layer (it exists outside the dependency hierarchy).
- Circular imports between layers are strictly forbidden.
