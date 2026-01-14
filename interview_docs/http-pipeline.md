# HTTP pipeline

## Key takeaways
- `ApiClient` builds a `Request` by merging default headers, setting body/format, and applying timeouts.  
  Evidence: `sdk/Lusid.Sdk/Client/ApiClient.cs:L334-L397`
- Requests and responses are translated to/from RestSharp via `RestSharpExtensions`.  
  Evidence: `sdk/Lusid.Sdk/Client/RestSharpExtensions.cs:L11-L187`
- Polly-based retry policies (including rate-limit handling) are configured in `ApiFactory` and executed in `ApiClient`.  
  Evidence: `sdk/Lusid.Sdk/Extensions/ApiFactory.cs:L123-L145` and `sdk/Lusid.Sdk/Client/ApiClient.cs:L462-L575`

## Request construction
### Building the SDK `Request`
`ApiClient.NewRequest` merges default headers, fills path/query/form/file parameters, and sets the request body and format.  
Evidence: `sdk/Lusid.Sdk/Client/ApiClient.cs:L334-L393`

### Timeouts
`ApiClient` sets per-request timeout using `RequestOptions.TimeoutMs` or the configuration’s `TimeoutMs`.  
Evidence: `sdk/Lusid.Sdk/Client/ApiClient.cs:L396-L397`

### Headers
Default headers are sourced from `Configuration.DefaultHeaders` and applied to each request.  
Evidence: `sdk/Lusid.Sdk/Client/ApiClient.cs:L356-L363`

## RestSharp translation
`RestSharpExtensions.ToRestSharpRequest` maps SDK request data (headers, query parameters, body, and files) onto RestSharp’s `RestRequest`.  
Evidence: `sdk/Lusid.Sdk/Client/RestSharpExtensions.cs:L38-L93`

`RestSharpExtensions.ToSdkResponse` maps RestSharp responses back into SDK `Response<T>`, including headers, error information, and status.  
Evidence: `sdk/Lusid.Sdk/Client/RestSharpExtensions.cs:L161-L186`

## Authentication in the pipeline
When OAuth settings are present, `ApiClient` builds an `OAuthAuthenticator` and attaches it to the RestSharp client options.  
Evidence: `sdk/Lusid.Sdk/Client/ApiClient.cs:L442-L454` and `sdk/Lusid.Sdk/Client/ApiClient.cs:L220-L230`

## Retries and rate limiting
### Retry conditions
`PollyApiRetryHandler` retries on HTTP 409/503/504 and on certain network/SSL exceptions.  
Evidence: `sdk/Lusid.Sdk/Extensions/PollyApiRetryHandler.cs:L39-L89`

### Rate limit handling
`PollyApiRetryHandler` retries on HTTP 429 and uses the `Retry-After` header (if present) or exponential backoff.  
Evidence: `sdk/Lusid.Sdk/Extensions/PollyApiRetryHandler.cs:L54-L59` and `sdk/Lusid.Sdk/Extensions/PollyApiRetryHandler.cs:L282-L299`

### Wiring into `ApiClient`
`ApiFactory` sets retry policy factories when not already configured, and `ApiClient` uses those policies for execution.  
Evidence: `sdk/Lusid.Sdk/Extensions/ApiFactory.cs:L123-L145` and `sdk/Lusid.Sdk/Client/ApiClient.cs:L462-L575`

## Response handling
`ApiClient` transforms `Response<T>` into `ApiResponse<T>`, copying headers and error text.  
Evidence: `sdk/Lusid.Sdk/Client/ApiClient.cs:L408-L427`
