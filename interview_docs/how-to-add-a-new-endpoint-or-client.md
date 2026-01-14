# How to add a new endpoint or client (evidence-based)

## Key takeaways
- `ApiFactory` discovers API clients by searching for classes that implement `IApiAccessor`, so new API clients must implement that interface to be auto-registered.  
  Evidence: `sdk/Lusid.Sdk/Extensions/ApiFactory.cs:L34-L36`
- Generated API interfaces include both standard and `WithHttpInfo` methods returning `ApiResponse<T>`, so new endpoints should follow this pattern.  
  Evidence: `sdk/Lusid.Sdk/Api/AborApi.cs:L34-L62`
- `ApiFactory` injects a custom `ExceptionFactory` into each API instance.  
  Evidence: `sdk/Lusid.Sdk/Extensions/ApiFactory.cs:L147-L156`

## Step-by-step (based on code patterns)
1. **Ensure the new API client implements `IApiAccessor`.**  
   Evidence: `sdk/Lusid.Sdk/Client/IApiAccessor.cs:L12-L32`
2. **Expose both `Foo` and `FooWithHttpInfo` methods for each endpoint.**  
   Evidence: `sdk/Lusid.Sdk/Api/AborApi.cs:L34-L62`
3. **Include `ConfigurationOptions? opts = null` in method signatures for per-request overrides (timeout/retries).**  
   Evidence: `sdk/Lusid.Sdk/Api/AborApi.cs:L44-L62`
4. **Let `ApiFactory` instantiate the client; avoid manual registration.**  
   Evidence: `sdk/Lusid.Sdk/Extensions/ApiFactory.cs:L147-L165`

## Regeneration or codegen steps
- **Not found in repo.** Verify by checking for OpenAPI generator configs (e.g., `.openapi-generator` files), build scripts, or CI pipelines that run code generation. The code files themselves include an OpenAPI generator header, indicating auto-generation.  
  Evidence: `sdk/Lusid.Sdk/Client/ApiClient.cs:L1-L5` and `sdk/Lusid.Sdk/Api/AborApi.cs:L1-L5`

## Testing expectations
- **Not found in repo.** Verify by searching for test projects or CI workflows (e.g., `.github/workflows`). The `Lusid.Sdk` project exposes `InternalsVisibleTo` for `Finbourne.Sdk.Extensions.Tests`, which suggests tests exist in a separate repo or package.  
  Evidence: `sdk/Lusid.Sdk/Lusid.Sdk.csproj:L32-L34`
