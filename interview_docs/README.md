# Interview Docs for LUSID C# SDK

## Key takeaways
- The SDK is packaged as `Lusid.Sdk` and described as an OpenAPI-generated library, so the docs here focus on generated client behavior and configuration surfaces defined in code.  
  Evidence: `sdk/Lusid.Sdk/Lusid.Sdk.csproj:L11-L16`
- Endpoint documentation is already present in `sdk/README.md` and per-API markdown files under `sdk/docs/`, and these interview docs reference those sources rather than duplicating them.  
  Evidence: `sdk/README.md:L1-L9` and `sdk/docs/AborApi.md:L1-L7`

## sdk/ top-level inventory (short)
- `Lusid.Sdk/` (library source directory).  
  Evidence: `sdk/Lusid.Sdk/Client/ApiClient.cs:L251-L256`
- `Lusid.Sdk.sln` (solution file).  
  Evidence: `sdk/Lusid.Sdk.sln:L1-L6`
- `README.md` (endpoint index).  
  Evidence: `sdk/README.md:L1-L9`
- `docs/` (per-API docs).  
  Evidence: `sdk/docs/AborApi.md:L1-L7`

## What this documentation set contains
- Architecture overview: [architecture-overview.md](architecture-overview.md)
- Folder responsibilities map: [folder-map.md](folder-map.md)
- Authentication & configuration: [authentication-and-configuration.md](authentication-and-configuration.md)
- HTTP pipeline: [http-pipeline.md](http-pipeline.md)
- Models & serialization: [models-and-serialization.md](models-and-serialization.md)
- Errors & exceptions: [errors-and-exceptions.md](errors-and-exceptions.md)
- Pagination & filtering: [pagination-and-filtering.md](pagination-and-filtering.md)
- Examples: [examples.md](examples.md)
- How to add a new endpoint/client: [how-to-add-a-new-endpoint-or-client.md](how-to-add-a-new-endpoint-or-client.md)
- Mind map: [mindmap.md](mindmap.md)
- Glossary: [glossary.md](glossary.md)
