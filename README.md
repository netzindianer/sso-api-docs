# SSO API Docs

REST API

- `{{uri}}` - SSO server address
- `{{admin_token}}` - authorization token to api on administrator level
- `{{user_token}}` - authorization token to api on user level


## Common parameters

- `Authorization: Bearer {{token}}` - http header, `{{admin_token}}` or `{{user_token}}`
- `X-Client-Ip: {{client_ip}}` - http header, front user ip,
- `X-Client-User-Agent: {{client_user_agent}}` http header, front user agent

## Actions

### /auth/token - get user token

Request:
```http
POST {{uri}}/api/auth/token
Authorization: Bearer {{admin_token}}
X-Client-Ip: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}

{
    "email": "{{email}}",
    "password": "{{password}}"
}

```

Request with Curl:
```shell
curl \
 -X POST \
 -H 'Authorization: Bearer {{admin_token}}'\
 -d '{"email": "{{email}}","password": "{{password}}"}'\
 '{{uri}}/api/auth/token'
```

Response:
```http
HTTP/1.1 200 OK
...

{
  "success": true,
  "data": {
    "token": "{{user_token}}"
  }
}
```

### /users/me get user data

Request:
```http
GET {{uri}}/api/users/me
Authorization: Bearer {{user_token}}
```

Request with Curl:
```shell
curl \
 -H 'Authorization: Bearer {{user_token}}'\
 '{{uri}}/api/users/me'
```

Response:
```http
HTTP/1.1 200 OK
...

{
  "success": true,
  "data": {
    "id": "{{user_id}}",
    "email": "demo@netzindianer.net",
    "is_active": true,
    "activated_at": "2021-12-02T10:35:08.000000Z",
    "created_at": "2021-12-02T10:35:08.000000Z",
    "last_login_at": "2021-12-02T11:24:24.000000Z",
    "first_name": "Vorname",
    "last_name": "Nachname"
  },
  "links": {
    "self": {
      "href": "{{uri}}/api/users/me",
      "method": "GET"
    },
    "update": {
      "href": "{{uri}}/api/users/me",
      "method": "PATCH"
    }
  }
}
```