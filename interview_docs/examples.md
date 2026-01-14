# Examples

## Key takeaways
- The README includes a full example using `ApiFactoryBuilder` and `AborApi.AddDiaryEntry`.  
  Evidence: `README.md:L149-L205`
- Generated API interfaces provide `WithHttpInfo` variants that return `ApiResponse<T>`, enabling header access helpers.  
  Evidence: `sdk/Lusid.Sdk/Api/AborApi.cs:L49-L62` and `sdk/Lusid.Sdk/Extensions/ApiResponseExtensions.cs:L13-L35`
- `ApiFactoryBuilder` can build an `IApiFactory` from a URL and token provider (e.g., `PersonalAccessTokenProvider`).  
  Evidence: `sdk/Lusid.Sdk/Extensions/ApiFactoryBuilder.cs:L24-L37` and `sdk/Lusid.Sdk/Extensions/PersonalAccessTokenProvider.cs:L14-L52`

## Example 1: Basic secrets file + Abor API call (from repo README)
```csharp
var secretsFilename = "secrets.json";
// var apiInstance = ApiFactoryBuilder.Build(secretsFilename, opts: opts).Api<AborApi>();
var apiInstance = ApiFactoryBuilder.Build(secretsFilename).Api<AborApi>();
var scope = "scope_example";
var code = "code_example";
var diaryEntryRequest = new DiaryEntryRequest();

DiaryEntry result = apiInstance.AddDiaryEntry(scope, code, diaryEntryRequest);
```
Evidence: `README.md:L164-L199`

## Example 2: Configuration overrides with `ConfigurationOptions` (from repo README)
```csharp
// uncomment the below to use configuration overrides
// var opts = new ConfigurationOptions();
// opts.TimeoutMs = 30_000;

// var apiInstance = ApiFactoryBuilder.Build(secretsFilename, opts: opts).Api<AborApi>();
```
Evidence: `README.md:L180-L187` and `sdk/Lusid.Sdk/Extensions/ConfigurationOptions.cs:L20-L36`

## Example 3: Accessing response headers with `WithHttpInfo` (pseudo-code)
```csharp
// Pseudo-code: replace with valid request parameters
var response = apiInstance.AddDiaryEntryWithHttpInfo(scope, code, diaryEntryRequest);
string requestId = response.GetRequestId();
```
Evidence: `sdk/Lusid.Sdk/Api/AborApi.cs:L49-L62` and `sdk/Lusid.Sdk/Extensions/ApiResponseExtensions.cs:L28-L35`

## Example 4: Build via URL + Personal Access Token provider (pseudo-code)
```csharp
var tokenProvider = new PersonalAccessTokenProvider("<personal-access-token>");
var factory = ApiFactoryBuilder.Build("https://<your-domain>.lusid.com/api", tokenProvider);
var aborApi = factory.Api<AborApi>();
```
Evidence: `sdk/Lusid.Sdk/Extensions/ApiFactoryBuilder.cs:L24-L37` and `sdk/Lusid.Sdk/Extensions/PersonalAccessTokenProvider.cs:L21-L52`
