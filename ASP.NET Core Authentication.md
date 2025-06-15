Authentication is the process of determining the user identity. This process is performed by `IAuthenticationService` and it's used by the authentication `middleware` to determine if the request is authenticated or not.

The set of authentication handler and its configuration it's called Authentication Scheme. One application may have one or more authentication schemes. When only one is specified, then it becomes the default authentication handler for all requests. Otherwise, if there're more than one authentication handler, you need to specify which one will be used to extract the `ClaimsPrincipal` for that user. If none is specified, the default will be used.

## Authentication Handler
An Authentication Handler implements the interface `IAuthenticationHandler`. It implements the behavior of the scheme.