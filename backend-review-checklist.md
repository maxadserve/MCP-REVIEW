# C# .NET 8 Backend Code Review Checklist

## General .NET Standards

### Code Organization & Architecture
- [ ] **Project structure** - Follow Clean Architecture or similar pattern (Domain, Application, Infrastructure, API layers)
- [ ] **Dependency direction** - Dependencies point inward (API → Application → Domain)
- [ ] **Interface segregation** - Interfaces are focused and cohesive
- [ ] **Separation of concerns** - Business logic separated from infrastructure concerns

### C# Language Features & Best Practices
- [ ] **Nullable reference types** - Enabled and properly configured (`<Nullable>enable</Nullable>`)
- [ ] **Pattern matching** - Use modern C# 12 pattern matching where appropriate
- [ ] **Records** - Use records for DTOs and immutable data structures
- [ ] **Global usings** - Organized and minimize repetition across files
- [ ] **File-scoped namespaces** - Use to reduce indentation
- [ ] **Primary constructors** - Use where appropriate for cleaner code

### Async/Await & Performance
- [ ] **Async all the way** - No async-over-sync or sync-over-async antipatterns
- [ ] **ConfigureAwait** - Used appropriately in library code
- [ ] **Cancellation tokens** - Passed through async call chains
- [ ] **Concurrent collections** - Used for thread-safe operations
- [ ] **ValueTask** - Used for hot paths where appropriate
- [ ] **IAsyncEnumerable** - Used for streaming data scenarios

### Error Handling & Logging
- [ ] **Global exception handling** - Middleware configured for unhandled exceptions
- [ ] **Custom exceptions** - Domain-specific exceptions with meaningful messages
- [ ] **Result pattern** - Consider Result<T> for expected failures
- [ ] **Structured logging** - Use ILogger with structured parameters
- [ ] **Log levels** - Appropriate use of Debug, Information, Warning, Error
- [ ] **Correlation IDs** - Track requests across services

## API Design & Web Standards

### ASP.NET Core Best Practices
- [ ] **Minimal APIs or Controllers** - Consistent approach chosen
- [ ] **Model validation** - Data annotations or FluentValidation
- [ ] **API versioning** - Implemented for backward compatibility
- [ ] **OpenAPI/Swagger** - Documented with examples and descriptions
- [ ] **Response caching** - Implemented where appropriate
- [ ] **Rate limiting** - Configured to prevent abuse

### RESTful Design
- [ ] **HTTP verbs** - Correct usage (GET, POST, PUT, PATCH, DELETE)
- [ ] **Status codes** - Appropriate HTTP status codes returned
- [ ] **Resource naming** - Consistent, plural nouns for collections
- [ ] **Pagination** - Implemented for list endpoints
- [ ] **Filtering/Sorting** - Query parameters follow conventions
- [ ] **HATEOAS** - Consider hypermedia links if applicable

### Security
- [ ] **Authentication** - JWT or cookie authentication properly configured
- [ ] **Authorization** - Role-based or policy-based access control
- [ ] **CORS** - Configured appropriately, not overly permissive
- [ ] **Input validation** - All inputs validated and sanitized
- [ ] **SQL injection** - Use parameterized queries or ORMs
- [ ] **Secrets management** - No hardcoded secrets, use IConfiguration
- [ ] **HTTPS** - Enforced in production

## Data Access & Entity Framework Core

### EF Core Best Practices
- [ ] **DbContext lifetime** - Scoped per request, not singleton
- [ ] **Async queries** - Use async methods (ToListAsync, FirstOrDefaultAsync)
- [ ] **Tracking behavior** - Use AsNoTracking() for read-only queries
- [ ] **Projection** - Select only needed columns
- [ ] **Eager loading** - Use Include() to prevent N+1 queries
- [ ] **Split queries** - Consider for complex includes
- [ ] **Raw SQL** - Use sparingly, parameterized when necessary

### Database Design
- [ ] **Migrations** - Code-first migrations tracked in source control
- [ ] **Indexes** - Appropriate indexes for query patterns
- [ ] **Constraints** - Foreign keys, unique constraints defined
- [ ] **Naming conventions** - Consistent table and column naming
- [ ] **Optimistic concurrency** - Implement where needed
- [ ] **Soft deletes** - Consider for audit requirements

## Dependency Injection & Configuration

### DI Container Usage
- [ ] **Service lifetimes** - Correct use of Transient, Scoped, Singleton
- [ ] **Interface registration** - All dependencies registered
- [ ] **Factory pattern** - Used for complex object creation
- [ ] **Options pattern** - Strongly-typed configuration
- [ ] **Service resolution** - Avoid service locator anti-pattern

### Configuration Management
- [ ] **appsettings.json** - Environment-specific settings separated
- [ ] **IOptions<T>** - Strongly-typed configuration classes
- [ ] **Configuration validation** - Validate options at startup
- [ ] **User secrets** - Used for local development
- [ ] **Environment variables** - Used for sensitive production config

## Testing & Quality

### Unit Testing
- [ ] **Test coverage** - Critical business logic covered
- [ ] **Arrange-Act-Assert** - Clear test structure
- [ ] **Mocking** - Interfaces mocked with Moq or similar
- [ ] **Test data builders** - Use builders for complex test objects
- [ ] **Parameterized tests** - Use [Theory] for multiple test cases
- [ ] **Naming convention** - MethodName_StateUnderTest_ExpectedBehavior

### Integration Testing
- [ ] **WebApplicationFactory** - Used for API integration tests
- [ ] **Test database** - In-memory or containerized database
- [ ] **Test fixtures** - Shared setup code properly managed
- [ ] **Authentication** - Test authentication scenarios
- [ ] **External services** - Mocked or stubbed appropriately

## Performance & Optimization

### Caching Strategies
- [ ] **Memory cache** - IMemoryCache for frequently accessed data
- [ ] **Distributed cache** - Redis for scaled environments
- [ ] **Response caching** - HTTP caching headers configured
- [ ] **Cache invalidation** - Strategy for keeping cache fresh

### Performance Considerations
- [ ] **Database queries** - Optimized, avoid N+1 problems
- [ ] **Bulk operations** - Use bulk insert/update when appropriate
- [ ] **Background tasks** - IHostedService or BackgroundService
- [ ] **Connection pooling** - Configured appropriately
- [ ] **Memory usage** - Avoid large object allocations in hot paths

## Common .NET Antipatterns to Avoid

### Code Smells
- [ ] **God classes** - Classes doing too much
- [ ] **Anemic domain model** - Domain objects with only properties
- [ ] **Service locator** - Avoid in favor of constructor injection
- [ ] **Tight coupling** - Depend on abstractions, not concretions
- [ ] **Premature optimization** - Profile before optimizing

### Async Antipatterns
- [ ] **Async void** - Only use for event handlers
- [ ] **Task.Run abuse** - Don't use for I/O-bound operations
- [ ] **Blocking async code** - Never use .Result or .Wait()
- [ ] **Missing ConfigureAwait** - Important in library code
- [ ] **Fire and forget** - Properly handle background tasks

### EF Core Pitfalls
- [ ] **Lazy loading** - Avoid, use eager loading instead
- [ ] **Multiple contexts** - Can cause tracking issues
- [ ] **Raw SQL injection** - Always parameterize queries
- [ ] **Over-fetching** - Select only needed columns
- [ ] **Sync over async** - Use async EF methods

## Modern .NET 8 Features

### Performance Improvements
- [ ] **Native AOT** - Consider for microservices
- [ ] **Frozen collections** - Use for read-only reference data
- [ ] **SearchValues** - Use for efficient string searching
- [ ] **Time abstraction** - Use TimeProvider for testability

### New Language Features
- [ ] **Primary constructors** - Use for simple initialization
- [ ] **Collection expressions** - Simplified collection initialization
- [ ] **Alias any type** - Use for complex generic types
- [ ] **Interceptors** - Consider for AOP scenarios

### API Improvements
- [ ] **Identity API endpoints** - Use built-in identity endpoints
- [ ] **Keyed services** - Use for multiple implementations
- [ ] **IExceptionHandler** - Global exception handling
- [ ] **Native resilience** - Built-in retry and circuit breaker

## Example Patterns

### Clean Architecture Service
```csharp
public interface IOrderService
{
    Task<Result<OrderDto>> CreateOrderAsync(CreateOrderCommand command, CancellationToken cancellationToken);
}

public class OrderService : IOrderService
{
    private readonly IOrderRepository _orderRepository;
    private readonly IUnitOfWork _unitOfWork;
    private readonly ILogger<OrderService> _logger;

    public OrderService(
        IOrderRepository orderRepository,
        IUnitOfWork unitOfWork,
        ILogger<OrderService> logger)
    {
        _orderRepository = orderRepository;
        _unitOfWork = unitOfWork;
        _logger = logger;
    }

    public async Task<Result<OrderDto>> CreateOrderAsync(
        CreateOrderCommand command, 
        CancellationToken cancellationToken)
    {
        try
        {
            var order = Order.Create(command.CustomerId, command.Items);
            
            await _orderRepository.AddAsync(order, cancellationToken);
            await _unitOfWork.SaveChangesAsync(cancellationToken);
            
            _logger.LogInformation("Order {OrderId} created for customer {CustomerId}", 
                order.Id, command.CustomerId);
            
            return Result<OrderDto>.Success(order.ToDto());
        }
        catch (DomainException ex)
        {
            _logger.LogWarning(ex, "Domain validation failed");
            return Result<OrderDto>.Failure(ex.Message);
        }
    }
}
```

### Minimal API Endpoint
```csharp
app.MapPost("/api/orders", async (
    CreateOrderRequest request,
    IOrderService orderService,
    IValidator<CreateOrderRequest> validator,
    CancellationToken cancellationToken) =>
{
    var validationResult = await validator.ValidateAsync(request, cancellationToken);
    if (!validationResult.IsValid)
        return Results.ValidationProblem(validationResult.ToDictionary());

    var command = request.ToCommand();
    var result = await orderService.CreateOrderAsync(command, cancellationToken);

    return result.IsSuccess
        ? Results.Created($"/api/orders/{result.Value.Id}", result.Value)
        : Results.BadRequest(result.Error);
})
.RequireAuthorization()
.WithName("CreateOrder")
.WithOpenApi()
.Produces<OrderDto>(StatusCodes.Status201Created)
.Produces<ValidationProblemDetails>(StatusCodes.Status400BadRequest);
```

### Global Exception Handler
```csharp
public class GlobalExceptionHandler : IExceptionHandler
{
    private readonly ILogger<GlobalExceptionHandler> _logger;

    public GlobalExceptionHandler(ILogger<GlobalExceptionHandler> logger)
    {
        _logger = logger;
    }

    public async ValueTask<bool> TryHandleAsync(
        HttpContext httpContext,
        Exception exception,
        CancellationToken cancellationToken)
    {
        _logger.LogError(exception, "Exception occurred: {Message}", exception.Message);

        var problemDetails = new ProblemDetails
        {
            Status = StatusCodes.Status500InternalServerError,
            Title = "Server error",
            Type = "https://datatracker.ietf.org/doc/html/rfc7231#section-6.6.1"
        };

        httpContext.Response.StatusCode = problemDetails.Status.Value;

        await httpContext.Response.WriteAsJsonAsync(problemDetails, cancellationToken);

        return true;
    }
}
```