# Models & serialization

## Key takeaways
- The SDK uses Newtonsoft.Json for serialization and includes an OpenAPI-specific JSON codec.  
  Evidence: `sdk/Lusid.Sdk/Lusid.Sdk.csproj:L25-L29` and `sdk/Lusid.Sdk/Client/ApiClient.cs:L18-L76`
- OpenAPI `oneOf`/`anyOf` schemas are represented by `AbstractOpenAPISchema`, with custom serializer settings.  
  Evidence: `sdk/Lusid.Sdk/Model/AbstractOpenAPISchema.cs:L14-L51`
- Date handling includes an OpenAPI date converter (`OpenAPIDateConverter`) and a `DateTimeOrCutLabel` type with custom JSON conversion.  
  Evidence: `sdk/Lusid.Sdk/Client/OpenAPIDateConverter.cs:L11-L24` and `sdk/Lusid.Sdk/Extensions/DateTimeOrCutLabel.cs:L16-L157`

## Model organization
Models are generated into `Lusid.Sdk.Model` and use `DataContract`/`DataMember` attributes (example: `ResourceListOfPortfolioTradeTicket`).  
Evidence: `sdk/Lusid.Sdk/Model/ResourceListOfPortfolioTradeTicket.cs:L23-L86`

## JSON serialization pipeline
### Core JSON codec
`CustomJsonCodec` uses Newtonsoft.Json to serialize/deserialize models and handles `AbstractOpenAPISchema` specially.  
Evidence: `sdk/Lusid.Sdk/Client/ApiClient.cs:L29-L148`

### oneOf/anyOf support
`ApiClient` invokes `FromJson` for `AbstractOpenAPISchema` response types to deserialize `oneOf`/`anyOf` responses.  
Evidence: `sdk/Lusid.Sdk/Client/ApiClient.cs:L482-L489`

### Date formatting for parameters
`ClientUtils.ParameterToString` formats `DateTime`/`DateTimeOffset` with `Configuration.DateTimeFormat`.  
Evidence: `sdk/Lusid.Sdk/Client/ClientUtils.cs:L78-L99`

## Custom converters
| Converter | Purpose | Evidence |
|---|---|---|
| `OpenAPIDateConverter` | Formats OpenAPI `date` fields as `yyyy-MM-dd`. | `sdk/Lusid.Sdk/Client/OpenAPIDateConverter.cs:L11-L24` |
| `CutLabelJsonConverter` (via `DateTimeOrCutLabel`) | Serializes/reads a date or cut label string representation. | `sdk/Lusid.Sdk/Extensions/DateTimeOrCutLabel.cs:L16-L157` |
| `PropertyBasedConverter` | Populates properties marked with `DataMember` (used for problem details). | `sdk/Lusid.Sdk/Extensions/PropertyBasedConverter.cs:L19-L79` |
