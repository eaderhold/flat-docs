# OpenAPI / Swagger Integration

FLAT uses the well-known [OpenAPI 2.0 definition](https://swagger.io/docs/specification/2-0/basic-structure/) aka "Swagger" to describe its API. Many developers know this format and have worked with it before, primarily to document their APIs.

This section covers all FLAT features that integrate with Swagger.

By default, FLAT tries to read its API definition from the `swagger.yaml` file in the project's top-level directory. You can configure a different path (or even choose one dynamically) in [`conf/config.xml`](/reference/configuration.md).

FLAT uses [Swagger Extensions](https://swagger.io/docs/specification/2-0/swagger-extensions/) prefixed with the `x-flat-` namespace to [augment](differences.md#x-flat--extensions) the `swagger.yaml`.


## [Routing](routing.md)

## [Validation](validation.md)

## [Security Check](security.md)

## [Mocking](mocking.md)

## [CORS](cors.md)

## [Upstream APIs](upstream.md)

## [Differences from Swagger](differences.md)
