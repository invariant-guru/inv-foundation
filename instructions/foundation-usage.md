# Foundation Library Usage

How to use `@invariant--labs/foundation` in NestJS projects.

## Creating Entities

Extend `Entity` with a props type. The `id` is auto-generated if not provided.

```typescript
type UserProps = EntityProps & { name: string; email: string };

class User extends Entity<UserProps> {
  get name() { return this.props.name; }
  get email() { return this.props.email; }
}
```

## Creating Aggregate Roots

Extend `AggregateRoot` for entities that emit domain events. Call `mergeContext(eventPublisher)` in handlers.

```typescript
type OrderProps = EntityProps & { total: number };

class Order extends AggregateRoot<OrderProps> {
  place() { this.apply(new OrderPlacedEvent(this.id)); }
}
```

## Creating Value Objects

Extend `ValueObject` and implement `validate()`. Props are frozen on construction.

```typescript
class Email extends ValueObject<{ value: string }> {
  validate() {
    if (!this.props.value.includes("@")) throw new Error("Invalid email");
  }
}
```

## Using Repositories

The `Repository` abstract class defines: `findOne`, `findOneOrFail`, `findByIds`, `save`, `saveAll`, `exists`, `count`.

**InMemory (testing):**
```typescript
class UserInMemoryRepository extends InMemoryRepository<User> {}
```

**TypeORM (production):** Use `EntityTypeormRepository` or `AggregateTypeormRepository`. Implement `toDomain` and `toOrm` mapping methods.

## Error Handling

```typescript
throw new EntityNotFoundError("User", userId);
throw new DomainError("Order total must be positive");
```

Available: `DomainError`, `EntityNotFoundError`, `CommandFailureError`, `CommandFilterError`, `QueryFailureError`, `TooManyResultsError`, `DetailedError`.

## Logger Setup

```typescript
@Module({ imports: [LoggerModule.forRoot({ service: "my-app" })] })
export class AppModule {}
```

## Testing with Stubs

```typescript
const logger = new FakeLogger();
logger.log("test message");
expect(logger.logs).toHaveLength(1);
logger.clear();

const commandBus = new CommandBusStub();
const eventBus = new EventBusStub();
const queryBus = new QueryBusStub();
```

## Checklist

- [ ] Entities extend `Entity<Props>` with `EntityProps`.
- [ ] Aggregates extend `AggregateRoot<Props>` and call `mergeContext` in handlers.
- [ ] Value objects implement `validate()`.
- [ ] Repository ports are abstract classes in the application layer.
- [ ] Infrastructure implements repository ports (TypeORM or InMemory).
- [ ] Errors use typed foundation error classes.
- [ ] Tests use `FakeLogger` and bus stubs.
