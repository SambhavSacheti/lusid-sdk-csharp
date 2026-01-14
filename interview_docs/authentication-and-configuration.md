# Authentication & configuration

## Key takeaways
- `ApiConfigurationBuilder` can build `ApiConfiguration` from a secrets file or from environment variables, and validates required inputs.  
  Evidence: `sdk/Lusid.Sdk/Extensions/ApiConfigurationBuilder.cs:L48-L163`
- `ApiFactory` chooses authentication strategy: personal access token first, otherwise client-credentials flow.  
  Evidence: `sdk/Lusid.Sdk/Extensions/ApiFactory.cs:L50-L65`
- `GlobalConfiguration` sets default headers for SDK language/version and is merged into other configurations.  
  Evidence: `sdk/Lusid.Sdk/Client/GlobalConfiguration.cs:L44-L52` and `sdk/Lusid.Sdk/Extensions/TokenProviderConfiguration.cs:L46-L75`

## Configuration objects
### `ApiConfiguration` (input configuration)
`ApiConfiguration` captures the values used by the token providers and API base URL.  
Evidence: `sdk/Lusid.Sdk/Extensions/ApiConfiguration.cs:L15-L89`

| Option | Type | Default | Where defined | Evidence |
|---|---|---|---|---|
| `TokenUrl` | `string` | Not set in code | `ApiConfiguration` | `sdk/Lusid.Sdk/Extensions/ApiConfiguration.cs:L20-L23` |
| `Username` | `string` | Not set in code | `ApiConfiguration` | `sdk/Lusid.Sdk/Extensions/ApiConfiguration.cs:L25-L28` |
| `Password` | `string` | Not set in code | `ApiConfiguration` | `sdk/Lusid.Sdk/Extensions/ApiConfiguration.cs:L30-L33` |
| `ClientId` | `string` | Not set in code | `ApiConfiguration` | `sdk/Lusid.Sdk/Extensions/ApiConfiguration.cs:L35-L38` |
| `ClientSecret` | `string` | Not set in code | `ApiConfiguration` | `sdk/Lusid.Sdk/Extensions/ApiConfiguration.cs:L40-L43` |
| `BaseUrl` (`lusidUrl`) | `string` | Not set in code | `ApiConfiguration` | `sdk/Lusid.Sdk/Extensions/ApiConfiguration.cs:L45-L49` |
| `ApplicationName` | `string` | Not set in code | `ApiConfiguration` | `sdk/Lusid.Sdk/Extensions/ApiConfiguration.cs:L63-L66` |
| `PersonalAccessToken` (`accessToken`) | `string` | Not set in code | `ApiConfiguration` | `sdk/Lusid.Sdk/Extensions/ApiConfiguration.cs:L68-L73` |
| `TimeoutMs` | `int?` | Not set in code (optional) | `ApiConfiguration` | `sdk/Lusid.Sdk/Extensions/ApiConfiguration.cs:L81-L84` |
| `RateLimitRetries` | `int?` | Not set in code (optional) | `ApiConfiguration` | `sdk/Lusid.Sdk/Extensions/ApiConfiguration.cs:L86-L89` |

### Runtime `Configuration` (client configuration)
The runtime client configuration includes base path, headers, timeouts, OAuth settings, and retry settings.  
Evidence: `sdk/Lusid.Sdk/Client/Configuration.cs:L23-L588`

| Option | Type | Default | Where defined | Evidence |
|---|---|---|---|---|
| `BasePath` | `string` | `https://fbn-prd.lusid.com/api` | `Configuration` constructor | `sdk/Lusid.Sdk/Client/Configuration.cs:L128-L132` |
| `TimeoutMs` | `int` | `DefaultTimeoutMs` (1,800,000 ms) | `Configuration` constructor | `sdk/Lusid.Sdk/Client/Configuration.cs:L30-L33` and `sdk/Lusid.Sdk/Client/Configuration.cs:L149-L150` |
| `RateLimitRetries` | `int` | `DefaultRateLimitRetries` (2) | `Configuration` constructor | `sdk/Lusid.Sdk/Client/Configuration.cs:L35-L38` and `sdk/Lusid.Sdk/Client/Configuration.cs:L149-L150` |
| `DefaultHeaders` | `IDictionary<string,string>` | empty dictionary | `Configuration` constructor | `sdk/Lusid.Sdk/Client/Configuration.cs:L133-L135` |
| `UserAgent` | `string` | `OpenAPI-Generator/2.0.0/csharp` (URL encoded) | `Configuration` constructor | `sdk/Lusid.Sdk/Client/Configuration.cs:L130-L132` |
| `OAuthTokenUrl`/`OAuthClientId`/`OAuthClientSecret`/`OAuthFlow` | `string`/`string`/`string`/`OAuthFlow?` | Not set in code | `Configuration` properties | `sdk/Lusid.Sdk/Client/Configuration.cs:L306-L328` |
| `TempFolderPath` | `string` | OS temp path | `Configuration` property | `sdk/Lusid.Sdk/Client/Configuration.cs:L330-L361` |

### Configuration overrides (`ConfigurationOptions`)
`ConfigurationOptions` provides optional per-request/ApiFactory overrides for timeout and rate-limit retries.  
Evidence: `sdk/Lusid.Sdk/Extensions/ConfigurationOptions.cs:L5-L36`

| Option | Type | Default | Where defined | Evidence |
|---|---|---|---|---|
| `TimeoutMs` | `int?` | `null` (unless set) | `ConfigurationOptions` | `sdk/Lusid.Sdk/Extensions/ConfigurationOptions.cs:L20-L27` |
| `RateLimitRetries` | `int?` | `null` (unless set) | `ConfigurationOptions` | `sdk/Lusid.Sdk/Extensions/ConfigurationOptions.cs:L30-L36` |

## Configuration sources
### Secrets file (e.g., `secrets.json`)
`ApiConfigurationBuilder` reads a JSON file, binding the `api` section to `ApiConfiguration`.  
Evidence: `sdk/Lusid.Sdk/Extensions/ApiConfigurationBuilder.cs:L192-L205`

### Environment variables
`ApiConfigurationBuilder` supports environment variables like `FBN_TOKEN_URL`, `FBN_LUSID_URL`, and `FBN_ACCESS_TOKEN`.  
Evidence: `sdk/Lusid.Sdk/Extensions/ApiConfigurationBuilder.cs:L22-L33` and `sdk/Lusid.Sdk/Extensions/ApiConfigurationBuilder.cs:L117-L134`

### Base URL fallback logic
If `BaseUrl` is blank, `ApiConfigurationBuilder` falls back to `SnakeCaseBaseUrl` or `LowerCaseBaseUrl`.  
Evidence: `sdk/Lusid.Sdk/Extensions/ApiConfigurationBuilder.cs:L245-L250`

## Authentication flows
### Personal Access Token (PAT)
`ApiFactory` prefers the `PersonalAccessToken`/`AccessTokenOldName` settings and uses `PersonalAccessTokenProvider` to supply the bearer token.  
Evidence: `sdk/Lusid.Sdk/Extensions/ApiFactory.cs:L50-L58` and `sdk/Lusid.Sdk/Extensions/PersonalAccessTokenProvider.cs:L14-L52`

### Client credentials flow (username/password + client id/secret)
When PAT is not supplied, `ApiFactory` falls back to `ClientCredentialsFlowTokenProvider`, which requests tokens from the configured `TokenUrl`.  
Evidence: `sdk/Lusid.Sdk/Extensions/ApiFactory.cs:L60-L65` and `sdk/Lusid.Sdk/Extensions/ClientCredentialsFlowTokenProvider.cs:L123-L186`

### OAuth flow (RestSharp authenticator)
If OAuth fields are populated on `Configuration`, `ApiClient` constructs an `OAuthAuthenticator` and attaches it to the RestSharp client.  
Evidence: `sdk/Lusid.Sdk/Client/ApiClient.cs:L442-L454`

## Global defaults
The global configuration instance adds SDK language/version headers by default.  
Evidence: `sdk/Lusid.Sdk/Client/GlobalConfiguration.cs:L44-L52`
