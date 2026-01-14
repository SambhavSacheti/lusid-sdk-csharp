# Errors & exceptions

## Key takeaways
- `ApiException` is the primary exception type carrying HTTP status, error content, and headers.  
  Evidence: `sdk/Lusid.Sdk/Client/ApiException.cs:L15-L62`
- `Configuration.DefaultExceptionFactory` builds `ApiException` instances from `IApiResponse` objects.  
  Evidence: `sdk/Lusid.Sdk/Client/Configuration.cs:L57-L79`
- Extension helpers extract validation/problem details and request IDs from errors/responses.  
  Evidence: `sdk/Lusid.Sdk/Extensions/ApiExceptionExtensions.cs:L20-L108` and `sdk/Lusid.Sdk/Extensions/ApiResponseExtensions.cs:L13-L48`

## Common exceptions
| Exception type | When it occurs | Handling notes | Evidence |
|---|---|---|---|
| `ApiException` | Thrown when API calls fail or `DefaultExceptionFactory` determines a non-successful response. | Inspect `ErrorCode`, `ErrorContent`, and `Headers` for details. | `sdk/Lusid.Sdk/Client/ApiException.cs:L15-L62` and `sdk/Lusid.Sdk/Client/Configuration.cs:L57-L79` |
| `ConfigurationException` | Thrown when configuration inputs are invalid or missing. | Check configuration sources and validation errors. | `sdk/Lusid.Sdk/Extensions/ConfigurationException.cs:L12-L21` and `sdk/Lusid.Sdk/Extensions/ApiConfigurationBuilder.cs:L109-L160` |
| `InvalidOperationException` (example) | Raised for invalid configuration values (e.g., null `ApiKey`/`ApiKeyPrefix`). | Ensure required configuration collections are set. | `sdk/Lusid.Sdk/Client/Configuration.cs:L406-L432` |

## Problem details helpers
### Validation errors
`ApiExceptionExtensions.IsValidationProblem` checks for HTTP 400 and can deserialize `LusidValidationProblemDetails`.  
Evidence: `sdk/Lusid.Sdk/Extensions/ApiExceptionExtensions.cs:L20-L66` and `sdk/Lusid.Sdk/Model/LusidValidationProblemDetails.cs:L26-L101`

### General problem details
`ApiExceptionExtensions.ProblemDetails` deserializes `LusidProblemDetails` if possible.  
Evidence: `sdk/Lusid.Sdk/Extensions/ApiExceptionExtensions.cs:L68-L92` and `sdk/Lusid.Sdk/Model/LusidProblemDetails.cs:L26-L95`

### Request ID extraction
`ApiExceptionExtensions.GetRequestId` extracts a request ID from the `Instance` field when present, and `ApiResponseExtensions` can read the request ID header from a response.  
Evidence: `sdk/Lusid.Sdk/Extensions/ApiExceptionExtensions.cs:L95-L108` and `sdk/Lusid.Sdk/Extensions/ApiResponseExtensions.cs:L19-L35`

## Response status semantics
`ResponseStatus` differentiates transport errors vs. HTTP status codes, with `IsSuccessful` combining both.  
Evidence: `sdk/Lusid.Sdk/Client/Response.cs:L9-L121`
