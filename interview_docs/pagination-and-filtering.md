# Pagination & filtering

## Key takeaways
- Resource list models include `NextPage`/`PreviousPage` fields, indicating pagination tokens are represented as strings in model DTOs.  
  Evidence: `sdk/Lusid.Sdk/Model/ResourceListOfPortfolioTradeTicket.cs:L39-L86`
- Query parameters are represented by `RequestOptions.QueryParameters` (a multimap) and converted into RestSharp query parameters.  
  Evidence: `sdk/Lusid.Sdk/Client/RequestOptions.cs:L26-L35` and `sdk/Lusid.Sdk/Client/RestSharpExtensions.cs:L67-L72`

## Pagination in models
Example pagination fields from `ResourceListOfPortfolioTradeTicket`:
- `NextPage` (string)
- `PreviousPage` (string)

Evidence: `sdk/Lusid.Sdk/Model/ResourceListOfPortfolioTradeTicket.cs:L77-L86`

## Filtering via query parameters
The SDK uses `RequestOptions.QueryParameters` to carry query string values; these are translated into RestSharp query parameters at request time.  
Evidence: `sdk/Lusid.Sdk/Client/RequestOptions.cs:L26-L35` and `sdk/Lusid.Sdk/Client/RestSharpExtensions.cs:L67-L72`

## Not found in repo
- **Explicit paging helpers or iterator utilities**: Not found in repo. Verify by searching for helpers beyond model DTOs or by inspecting generated API methods in `sdk/Lusid.Sdk/Api`. 
- **Standardized filter DSL documentation inside `sdk/`**: Not found in repo. Verify by reviewing per-endpoint docs under `sdk/docs/` or the LUSID API docs.
