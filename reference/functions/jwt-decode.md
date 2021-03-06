# `jwt-decode()`

```
OXN-node-set jwt-decode(string jws, string key [, string algorithm])
```

validates and unpacks the content of a _JSON web signature_ ([JWS](https://tools.ietf.org/html/rfc7515)) as [OXN](/reference/templating/oxn.md).


## Parameters

`jws` is expected to be a _compact serialization_.

For the HMAC based algorithms, `key` is a _Base64URL_ encoded symmetric key for the digital signature. For the `RSASSA` based algorithms, use the PEM encoded *public* key (without the `BEGIN` and `END` lines and without any line breaks). The key must not be password protected.

Currently, signatures created with `HS256`, `HS385`, `HS512`, `RS256`, `RS384` or `RS512` can be validated.

The additional `algorithm` parameter specifies the signing algorithm the signature is expected to be created with. Valid values are `HS256`, `HS385`, `HS512`, `HS` (any supported HMAC based algorithm), `RS256`, `RS384`, `RS512`, and `RS` (any supported RSASSA based algorithm). When using RSASSA based signing, the `algorithm` parameter is mandatory.

If validation and decoding is successful, you get the original web token as OXN encoded node-set.

If the decoding fails, you get an empty node-set.

Reasons for a decoding failure can be:

* a corrupt JWS,
* an expired time-to-live (JWT `exp` claim),
* an unknown signature algorithm,
* a corrupt JWT (no JSON).

## Example

In this example, the JWS is read from the `token`-Cookie in the request: `Cookie: token=eyJ….….…`.
The Base64URL-encoded key `$key` is read from the JSON file `jwt-key.json`: `{"k":"c2VjcmV0"}`.
The unpacked web token is stored in `$jwt`, which provides easy access to its content:

```xml
<eval out="$key">json-doc('jwt-key.json')/k</eval>
<eval out="$jwt">jwt-decode($request/cookies/token, $key)</eval>

<template>
{
  "user":   {{ $jwt/user }},
  "expiry": {{ $jwt/exp }}
}
</template>
```

## See also

* [`jwt-encode()`](jwt-encode.md) (reference)
* [Encoding and Decoding JWT](/cookbook/jwt.md) (cookbook)
* [Protecting Access using JWT Tokens](/cookbook/x-flat-jwt.md) (cookbook)
