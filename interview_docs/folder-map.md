# Folder map (sdk/)

## Key takeaways
- `sdk/Lusid.Sdk` is the primary library containing API clients, client infrastructure, models, and extensions.  
  Evidence: `sdk/Lusid.Sdk/Api/AborApi.cs:L29-L32` and `sdk/Lusid.Sdk/Client/ApiClient.cs:L251-L256`
- Endpoint documentation is generated into `sdk/README.md` and `sdk/docs/*.md`.  
  Evidence: `sdk/README.md:L1-L9` and `sdk/docs/AborApi.md:L1-L7`

## Map of major folders/files
| Path | Responsibility | Evidence |
|---|---|---|
| `sdk/Lusid.Sdk/Api` | Generated API client interfaces and methods (example: `IAborApiSync` in `AborApi.cs`). | `sdk/Lusid.Sdk/Api/AborApi.cs:L29-L62` |
| `sdk/Lusid.Sdk/Client` | Core HTTP client infrastructure (`ApiClient`, configuration, request/response types). | `sdk/Lusid.Sdk/Client/ApiClient.cs:L251-L512`, `sdk/Lusid.Sdk/Client/Configuration.cs:L23-L151`, `sdk/Lusid.Sdk/Client/Request.cs:L20-L93`, `sdk/Lusid.Sdk/Client/Response.cs:L39-L131` |
| `sdk/Lusid.Sdk/Extensions` | Factories, configuration builders, token providers, retry policies, and helpers. | `sdk/Lusid.Sdk/Extensions/ApiFactory.cs:L20-L165`, `sdk/Lusid.Sdk/Extensions/ApiConfigurationBuilder.cs:L48-L163`, `sdk/Lusid.Sdk/Extensions/ClientCredentialsFlowTokenProvider.cs:L20-L186`, `sdk/Lusid.Sdk/Extensions/PollyApiRetryHandler.cs:L24-L299` |
| `sdk/Lusid.Sdk/Model` | Generated DTOs and schema abstractions (example: `ResourceListOfPortfolioTradeTicket`). | `sdk/Lusid.Sdk/Model/ResourceListOfPortfolioTradeTicket.cs:L23-L86` |
| `sdk/README.md` | Index of endpoint documentation and links into `sdk/docs`. | `sdk/README.md:L1-L9` |
| `sdk/docs/*.md` | Per-API endpoint documentation (example: `AborApi.md`). | `sdk/docs/AborApi.md:L1-L7` |
