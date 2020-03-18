# Security

Security-related documentation in Swagger is described in [Security Definitions Object](https://swagger.io/specification/v2/#securityDefinitionsObject) and [Security Requirement Object](https://swagger.io/specification/v2/#securityRequirementObject)

Standard Swagger features in combination with some extensions can be used to configure security checks about JWT tokens which are expected to be sent with a client request.
If the JWT token is valid, it is stored in a [variable](/reference/variables.md), which can then be used in the [flow](/reference/flow.md).

## The `x-flat-jwt` field

The `x-flat-jwt` field references an object with fields describing the expected JWT token:

* `key` - REQUIRED. The key to decode the JSON Web Signature (JWS). This can either be specified with a value, or by referencing a file (`file`) or an environment variable (`env`).
* `alg` - The signing algorithm the JWS is expected to be created with. This can either be specified with a value, or by referencing a file (`file`) or an environment variable (`env`). See the [`algorithm` parameter for `jwt-decode()`](/reference/functions/jwt-decode.md) for more information.
* `out-var` - The name of the variable in which the JWT is stored (must be a proper variable name, starting with `$`; default: `"$jwt"`).
* `claims` - An object with claims the JWT payload is expected to contain. The field names are the claim names, the expected claim value is specified either with a value, or by referencing a file (`file`) or an environment variable (`env`).

The token is considered valid if all of the following are true:
* the JWS can be decoded,
* the JWS has a proper JWT,
* the JWT is not expired,
* the JWT contains the expected claims, if any are configured,
* the JWT can be stored in a variable.

## JWT in `Authorization` Header

Use a [Security Scheme Object](https://swagger.io/specification/v2/#securitySchemeObject) with `type: apiKey`, `in: header` and `name: Authorization` if the JWT is expected to be sent as a bearer token in an `Authorization` header; e.g.:

```yaml
securityDefinitions:
  JWTHeaderAuth:
    type: apiKey
    in: header
    name: Authorization
    x-flat-jwt:
      key:
        file: secret.pem
      alg:
        env: FLAT_JWT_ALG
      out-var: $header_token
      claims:
        aud:
          env: FLAT_JWT_AUDIENCE
```
The code in this example defines a security scheme named `JWTHeaderAuth`.
The token is expected to be a bearer token in the `Authorization` header.
The key is read from a file named `secret.pem` relative to the swagger.yaml.
The signing algorithm is read from the `FLAT_JWT_ALG` environment variable.
The JWT will be stored in the `$header_token` variable.
The JWT payload is expected to contain an `aud` claim with a value read from the `FLAT_JWT_AUDIENCE` environment variable.

If the request does not contain an `Authorization` header with the proper bearer structure, or the token is invalid, this security scheme will fail.


## JWT in Cookie

Use a [Security Scheme Object](https://swagger.io/specification/v2/#securitySchemeObject) with `type: apiKey`, `in: header`, `name: Cookie` and `x-flat-cookiename: <name>` if the JWT is expected to be sent as a cookie value; e.g.:

```yaml
securityDefinitions:
  JWTCookieAuth:
    type: apiKey
    in: header
    name: Cookie
    x-flat-cookiename: authtoken
    x-flat-jwt:
      key:
        env: FLAT_COOKIE_SECRET
      out-var: $cookie_token
```

In this example the key is read from the `FLAT_COOKIE_SECRET` environment variable.
The JWT signing algorithm is unspecified – it defaults to `HS256`.
The JWT will be stored in `$cookie_token`.
No claims are expected.

If the request does not contain a `Cookie` header with an `authtoken` cookie, or the token is invalid, this security scheme will fail.

## Security Scheme Combinations

Swagger allows for combinations of security schemes to be applied to API endpoints.
If security schemes are specified as _alternatives_, at least one alternative must not fail.
In the following example, a `GET` request to `/foo` must satisfy at least one of the security schemes named `JWTHeaderAuth` and `JWTCookieAuth`.

```yaml
paths:
  /foo:
    get:
      security:
        - JWTHeaderAuth: []
        - JWTCookieAuth: []
```

## Applying Security Schemes

In Swagger, Security Schemes can be specified at the top level (default security) or for specific operations.

In the following example, a `GET` request to `foo` must satisfy the security scheme named `JWTHeaderAuth`.
All other requests must satisfy either the `JWTHeaderAuth` **or** `JWTCookieAuth` security schemes.

```yaml
security:
  - JWTHeaderAuth: []
  - JWTCookieAuth: []
paths:
  /foo:
    get:
      security:
        - JWTHeaderAuth: []
```

If a request does not pass the security check, it is rejected with status code `403` and error code `3206`, and the [error flow](/reference/OpenAPI/routing.md#error-flow) is run, if configured.

The security check is performed after [validation](validation.md).

## Full Example

```yaml
securityDefinitions:
  JWTHeaderAuth:
    type: apiKey
    in: header
    name: Authorization
    x-flat-jwt:
      key:
        file: secret.pem
      alg:
        env: FLAT_JWT_ALG
      out-var: $header_token
      claims:
        aud:
          env: FLAT_JWT_AUDIENCE
  JWTCookieAuth:
    type: apiKey
    in: header
    name: Cookie
    x-flat-cookiename: authtoken
    x-flat-jwt:
      key:
        env: FLAT_COOKIE_SECRET
      out-var: $cookie_token
security:                     # alternatives: token in Authorization header or authtoken cookie
  - JWTHeaderAuth: []
  - JWTCookieAuth: []
paths:
  /:                          # default security as defined at top-level
  /foo:
    get:
      security:               # token must be in Authorization header
        - JWTHeaderAuth: []
```
