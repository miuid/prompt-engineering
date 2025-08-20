---
applyTo: '**'
---
# BFF Development Instructions

This file contains key patterns and practices for developing Backend-for-Frontend (BFF) applications based on the Payments.UI.BFF reference implementation.

## Project Structure

### Core Architecture
- **Controllers**: Handle HTTP requests and route to appropriate services
- **Services**: Contain business logic and orchestrate API calls
- **API Clients**: Handle communication with downstream services
- **Infrastructure**: Cross-cutting concerns (headers, logging, configuration)

### Key Directories
```
src/
├── Controllers/           # API endpoints
├── Application/Services/  # Business logic layer
├── Common/                # Helpers, extensions, and utilities
├── Configuration/         # Configuration models
├── Domain/                # Domain models, exceptions, extensions and entities
├── Infrastructure/
│   ├── ApiClients/       # Downstream API communication
│   ├── XeroHttpHeaders/  # Header management
│   └── Extensions/       # Extension methods
├── Interfaces/           # Contracts and abstractions
└── Infrastructure/       # Api clients, XeroHeaders, LaunchDarkly, Logger, and other infrastructure concerns
```

## Xero Header Handling

### Core Headers
Always handle these standard Xero headers:
- `Xero-Correlation-Id`: Request tracing
- `Xero-Tenant-Id`: Organization identification
- `Xero-User-Id`: User identification
- `Xero-Client-Id`: Client application identification
- `Xero-Payments-Origin`: Payment context (e.g., "Bills", "Payroll")
- `Xero-Payments-PartnerCode`: Partner identification

### Header Implementation Pattern

1. **Create IXeroHttpHeaders Interface**:
```csharp
public interface IXeroHttpHeaders
{
    public string? XeroClientId { get; }
    public string? XeroUserId { get; }
    public string? XeroTenantId { get; }
    public string? XeroCorrelationId { get; }
    public string? XeroPaymentsOrigin { get; }
    public string? XeroPaymentsPartnerCode { get; }
}
```

2. **Implement XeroHttpHeaders Class**:
```csharp
public class XeroHttpHeaders : IXeroHttpHeaders
{
    public XeroHttpHeaders(IHttpContextAccessor httpContextAccessor, IUserIdExtractor userIdExtractor)
    {
        // Extract headers from incoming request
        XeroCorrelationId = httpContextAccessor.HttpContext?.Request.Headers[XeroHeaderConstants.XeroCorrelationId] ?? Guid.NewGuid().ToString();
        XeroTenantId = httpContextAccessor.HttpContext?.Request.Headers[XeroHeaderConstants.XeroTenantId].ToString();
        // ... extract other headers
    }
}
```

3. **HttpClient Extension Methods**:
```csharp
public static class HttpClientExtension
{
    public static HttpClient AddOrganisationId(this HttpClient httpClient, string? organisationId)
    {
        httpClient.DefaultRequestHeaders.Remove("Xero-Tenant-Id");
        if (!string.IsNullOrEmpty(organisationId))
            httpClient.DefaultRequestHeaders.Add("Xero-Tenant-Id", organisationId);
        return httpClient;
    }

    public static HttpClient AddCorrelationId(this HttpClient httpClient, string? correlationId)
    {
        httpClient.DefaultRequestHeaders.Remove("Xero-Correlation-Id");
        if (!string.IsNullOrEmpty(correlationId))
            httpClient.DefaultRequestHeaders.Add("Xero-Correlation-Id", correlationId);
        return httpClient;
    }

    // ... other header methods
}
```

## API Client Configuration

### Configuration Pattern
1. **Create Configuration Classes**:
```csharp
public class OnboardingApiConfig
{
    public string? BaseUrl { get; set; }
}

public class AppConfiguration
{
    public OnboardingApiConfig OnboardingApi { get; set; } = new();
    public PaymentsApiConfig PaymentsApi { get; set; } = new();
    // ... other API configs
}
```

2. **API Client Implementation**:
```csharp
public class OnboardingApiHttpClient : IOnboardingApiHttpClient
{
    private readonly HttpClient _httpClient;
    private readonly IXeroHttpHeaders _headers;
    private readonly IOptions<IdentityConfiguration> _configuration;

    public OnboardingApiHttpClient(HttpClient httpClient, IXeroHttpHeaders headers, IOptions<IdentityConfiguration> configuration)
    {
        _httpClient = httpClient;
        _headers = headers;
        _configuration = configuration;
        AddDefaultHeaders();
    }

    private void AddDefaultHeaders()
    {
        _httpClient
            .AddOrganisationId(_headers.XeroTenantId)
            .AddUserId(_headers.XeroUserId)
            .AddCorrelationId(_headers.XeroCorrelationId)
            .AddPaymentsOrigin(_headers.XeroPaymentsOrigin)
            .AddClientId(_configuration.Value.ClientId)
            .AddPaymentsPartnerCode(_headers.XeroPaymentsPartnerCode);
    }
}
```

3. **Dependency Injection Setup**:
```csharp
// In Startup.cs or Program.cs
services.AddHttpClient<IOnboardingApiHttpClient, OnboardingApiHttpClient>(client =>
{
    client.BaseAddress = new Uri(appConfiguration.OnboardingApi.BaseUrl);
});
```

## Controller Patterns

### Standard Controller Structure
```csharp
[Route("v1/")]
[ApiController]
[Authorize]
public class OnboardingController(IOnboardingService onboardingService) : ControllerBase
{
    [ProducesResponseType(StatusCodes.Status200OK, Type = typeof(OnboardingSummaryResponseDto))]
    [HttpGet("{orgId}/onboarding-summary")]
    public async Task<OnboardingSummaryResponseDto?> GetOnboardingSummary([FromRoute] Guid orgId) =>
        await onboardingService.GetOnboardingSummaryAsync(orgId);
}
```

### Key Controller Conventions
- Use primary constructor pattern: `Controller(IService service)`
- Always include `[ProducesResponseType]` attributes
- Use route parameters with `[FromRoute]`
- Keep controllers thin - delegate to services
- Use async/await patterns consistently

## Service Layer Patterns

### Service Implementation
```csharp
public class OnboardingService(ILogger<OnboardingService> logger, IOnboardingApiHandler onboardingApiHandler) : IOnboardingService
{
    public async Task<OnboardingSummaryResponseDto?> GetOnboardingSummaryAsync(Guid orgId)
    {
        var onboardingSummary = await onboardingApiHandler.GetOnboardingSummaryAsync(orgId);

        logger.LogInformation("Successfully fetched onboarding summary");

        if (onboardingSummary == null)
            return null;

        // Transform domain model to DTO
        return new OnboardingSummaryResponseDto()
        {
            Status = onboardingSummary.Status,
            IsConnectionAdmin = onboardingSummary.IsConnectionAdmin,
            // ... map other properties
        };
    }
}
```

### Service Conventions
- Use constructor injection with primary constructor
- Always inject and use `ILogger<T>`
- Handle null responses gracefully
- Transform domain models to DTOs
- Log successful operations and errors

## HttpClient Usage Patterns

### Generic HTTP Methods
```csharp
public async Task<TResult?> Get<TResult>(string endpoint) where TResult : class?, new()
{
    var response = await _httpClient.GetAsync(endpoint);

    if (response.StatusCode == HttpStatusCode.NotFound)
        return null;

    response.EnsureSuccessStatusCode();

    var responseContent = await response.Content.ReadAsStringAsync();
    return JsonConvert.DeserializeObject<TResult>(responseContent);
}

public async Task<TResult?> Post<TResult, TRequest>(string endpoint, TRequest request)
    where TResult : class?, new()
{
    var json = JsonConvert.SerializeObject(request);
    var content = new StringContent(json, Encoding.UTF8, "application/json");

    var response = await _httpClient.PostAsync(endpoint, content);
    response.EnsureSuccessStatusCode();

    var responseContent = await response.Content.ReadAsStringAsync();
    return JsonConvert.DeserializeObject<TResult>(responseContent);
}
```

### Error Handling
- Always handle `HttpStatusCode.NotFound` as null return
- Use `EnsureSuccessStatusCode()` for other error cases
- Log errors appropriately with correlation IDs

## Testing Patterns

### Unit Test Structure
```csharp
public class OnboardingServiceTests
{
    private readonly Mock<IOnboardingApiHandler> _mockApiHandler = new();
    private readonly Mock<ILogger<OnboardingService>> _mockLogger = new();

    [Fact]
    public async Task GivenGetOnboardingSummary_WhenApiHandlerReturnsValue_ThenReturnsResponse()
    {
        // Arrange
        var expectedResponse = new OnboardingSummaryResponse { /* ... */ };
        _mockApiHandler.Setup(x => x.GetOnboardingSummaryAsync(It.IsAny<Guid>()))
            .ReturnsAsync(expectedResponse);

        var service = new OnboardingService(_mockLogger.Object, _mockApiHandler.Object);

        // Act
        var result = await service.GetOnboardingSummaryAsync(Guid.Empty);

        // Assert
        result.Should().BeEquivalentTo(expectedResponse);
    }
}
```

### HttpClient Extension Tests
```csharp
[Fact]
public void AddPaymentsPartnerCode_WhenHeaderExists_ShouldRemoveAndAddNewHeader()
{
    // Arrange
    var httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Xero-Payments-PartnerCode", "existing-code");

    // Act
    var result = httpClient.AddPaymentsPartnerCode("new-code");

    // Assert
    Assert.Equal("new-code", httpClient.DefaultRequestHeaders.GetValues("Xero-Payments-PartnerCode").First());
    Assert.Single(httpClient.DefaultRequestHeaders.GetValues("Xero-Payments-PartnerCode"));
}
```

## Configuration Management

### Startup Configuration
```csharp
public void ConfigureServices(WebApplicationBuilder webApplicationBuilder)
{
    var appConfiguration = webApplicationBuilder.Configuration.Get<AppConfiguration>();

    services.AddControllers()
        .AddJsonOptions(options =>
        {
            options.JsonSerializerOptions.PropertyNamingPolicy = JsonNamingPolicy.CamelCase;
            options.JsonSerializerOptions.Converters.Add(new JsonStringEnumConverter());
        });

    services.AddAuthentication()
        .AddSecretHeader("SecretHeaderAuthentication", options => { /* ... */ });

    services.AddApiClients(appConfiguration);
    services.AddSwaggerConfiguration();
}
```

### Health Checks
```csharp
webApplication.MapHealthChecks("/healthcheck", new HealthCheckOptions
{
    Predicate = healthCheck => healthCheck.Tags.Contains("live"),
    ResponseWriter = HealthCheckResponseWriters.WriteJsonResponse
});

webApplication.MapHealthChecks("/ready", new HealthCheckOptions
{
    Predicate = healthCheck => healthCheck.Tags.Contains("ready")
});
```

## Middleware Patterns

### Custom Middleware
```csharp
public static class MiddlewareExtensions
{
    public static void UseInboundXeroCorrelationIdMiddleware(this IApplicationBuilder builder)
    {
        builder.UseMiddleware<InboundXeroCorrelationIdMiddleware>();
    }

    public static void UseRequestLogging(this IApplicationBuilder builder)
    {
        builder.UseMiddleware<RequestLoggingMiddleware>();
    }
}
```

## Key Conventions

### Naming Conventions
- Controllers: `{Domain}Controller`
- Services: `{Domain}Service`
- API Clients: `{Service}ApiHttpClient`
- Interfaces: `I{ClassName}`
- DTOs: `{Purpose}ResponseDto` or `{Purpose}RequestDto`

### Code Structure
- Use primary constructors for dependency injection
- Implement interfaces for all services and clients
- Use `async/await` consistently
- Handle null responses gracefully
- Always include comprehensive logging
- Use FluentAssertions for test assertions

### Error Handling
- Return `null` for not found scenarios
- Use `HttpResponseMessage` for complex responses
- Log errors with correlation IDs
- Use appropriate HTTP status codes

This instruction file should be used as a reference for developing BFF applications that follow the established patterns and practices in your organization.