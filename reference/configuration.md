# Configuration

## Swagger/OpenAPI

Most settings for routing, validation and CORS can be set in `swagger.yaml`. See [OpenAPI Integrations](OpenAPI/README.md) for detailed information.


## Dynamic Configuration

If you need to define configuration settings dynamically, you can use the `conf/config.xml` file.

It supports the same settings as in `swagger.yaml`:

```xml
<config>
  <flat>
    <definition src="swagger.yaml"/>
    <validation request="true" response="report-only"/>
    <cors allowed-origins="http://localhost:9000" allow-credentials="true"/>
  </flat>
</config>
```

The difference is, that you may use _Dynamic Attribute Values_ and if-clauses as in the [flow](flow.md):

```xml
<config>
  <flat>
    <validation request="true" if="$server/role = 'dev'"/>
    <validation request="report-only" if="$server/role = 'prod'"/>
  </flat>
</config>
```

## LDAP TLS Configuration

If you use the [`ldap-lookup()`](/reference/functions/ldap-lookup.md) or
[`ldap-query()`](/reference/functions/ldap-query.md) function and connect to the LDAP server via TLS
(`ldaps://...` URL), you may have to provide the corresponding CA certificate using the following config setting in your config file:

```xml
<config>
  <flat>
    <ldap cacert-src="path/to/ca-certificate.cer"/>
  </flat>
</config>
```

The path is resolved relative to the config.xml file.

## LDAP Timeout

LDAP requests via [`ldap-lookup()`](/reference/functions/ldap-lookup.md) or
[`ldap-query()`](/reference/functions/ldap-query.md) use `FLAT_MAX_TIMEOUT` as the default timeout.
If you want to set a lower timeout for LDAP requests, use the setting below in your config file:

```xml
<config>
  <flat>
    <ldap timeout="3"/>
  </flat>
</config>
```
