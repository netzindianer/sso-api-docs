# SSO API Docs

[ACOMA SSO](https://www.netzindianer.net/acoma/single-sign-on-system.html) by [Netzindianer](https://www.netzindianer.net/)

- [Introduction](#introduction)
- [Users](#users)
  - [GET /api/users](#get-apiusers)
  - [POST /api/users](#post-apiusers)
  - [GET /api/users/{{id}}](#get-apiusersid)
  - [GET /api/users/me](#get-apiusersme)
  - [PATCH /api/users/{{id}}](#patch-apiusersid)
  - [DELETE /api/users/{{id}}](#delete-apiusersid)
- [Auth](#auth)
  - [POST /api/auth](#post-apiauth)
  - [PUT /api/auth/activation](#put-apiauthactivation)
  - [POST /api/auth/token](#post-apiauthtoken)
- [Attributes](#attributes)
  - [GET /api/attributes](#get-apiattributes)
  - [GET /api/users/{{id}}/attributes](#get-apiusersidattributes)
  - [PUT /api/users/{{id}}/attributes/{{name}}](#put-apiusersidattributesname)
  - [DELETE /api/users/{{id}}/attributes/{{name}}](#delete-apiusersidattributesname)
- [Optins](#optins)
  - [POST /api/users/{{id}}/optins](#post-apiusersidoptins)
  - [GET /api/users/{{id}}/optins](#get-apiusersidoptins)
  - [GET /api/users/{{id}}/optins/agreement](#get-apiusersidoptinsagreement)
- [Use cases](#use-cases)
  - [Get user by email and password](#get-user-by-email-and-password)

## Introduction

1. REST API

2. Defintions
- `{{uri}}` - SSO server address
- `{{admin_token}}` - authorization token to api on administrator level
- `{{user_token}}` - authorization token to api on user level

3. Common parameters

- `Authorization: Bearer {{token}}` - http header, `{{admin_token}}` or `{{user_token}}`
- `X-Client-Ip: {{client_ip}}` - http header, front user ip,
- `X-Client-User-Agent: {{client_user_agent}}` http header, front user agent

## Users

Users is a low level layer of users resource.
Simple CRUD operations without any logic.

### GET /api/users

List users

Request:
```http
GET {{uri}}/api/users
Authorization: Bearer {{admin_token}}
X-Client-Ip: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}
```

Request with curl:
```shell
curl\
 -H 'Authorization: Bearer {{admin_token}}'\
 -H 'X-Client-Ip: {{client_ip}}'\
 -H 'X-Client-User-Agent: {{client_user_agent}}'\
 '{{uri}}/api/users'
```

Response:
```http
HTTP/1.1 200 OK
...

{
  "success": true,
  "data": [
    {
      "id": "9503ef37-4a47-47f1-b3fc-86db963d5fb0",
      "email": "demo@netzindianer.net",
      "is_active": true,
      "activated_at": "2021-12-02T10:35:08.000000Z",
      "created_at": "2021-12-02T10:35:08.000000Z",
      "last_login_at": "2021-12-02T13:51:02.000000Z",
      "first_name": "Vorname",
      "last_name": "Nachname",
      "url": "{{uri}}/api/users/9503ef37-4a47-47f1-b3fc-86db963d5fb0"
    },
    ...
  ],
  "links": {
    "self": {
      "href": "{{uri}}/api/users",
      "method": "GET"
    },
    "detail": {
      "href": "{{uri}}/api/users/{user.id}",
      "method": "GET"
    },
    "first": {
      "href": "{{uri}}/api/users?perPage=10&sort=created_at&page=1",
      "method": "GET"
    },
    "last": {
      "href": "{{uri}}/api/users?perPage=10&sort=created_at&page=3",
      "method": "GET"
    },
    "next": {
      "href": "{{uri}}/api/users?perPage=10&sort=created_at&page=4",
      "method": "GET"
    },
    "prev": {
      "href": "{{uri}}/api/users?perPage=10&sort=created_at&page=2",
      "method": "GET"
    }
  },
  "meta": {
    "page": 3,
    "perPage": 10,
    "total": 28,
    "sort": "created_at",
    "filter": []
  }
}
```

### POST /api/users

Create user

Request:
```http
POST {{uri}}/api/users
Authorization: Bearer {{admin_token}}
X-Client-Ip: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}

{
    "email": "john.doe@example.com",
    "password": "password1!",
    "first_name": "username",
    "last_name": "usersurname",
    "active": true
}
```

Request with curl:
```shell
curl\
 -X POST\
 -H 'Authorization: Bearer {{admin_token}}'\
 -H 'X-Client-Ip: {{client_ip}}'\
 -H 'X-Client-User-Agent: {{client_user_agent}}'\
 -d '{"email": "john.doe@example.com","password": "password1!","first_name": "username","last_name": "usersurname","active": true}'\
 '{{uri}}/api/users'
```

Response:
```http
HTTP/1.1 201 Created
Location: {{uri}}/api/users/950bdad0-d4e9-4182-be78-c35faa059f73
...

{
  "success": true,
  "data": {
    "id": "950bdad0-d4e9-4182-be78-c35faa059f73"
  },
  "links": {
    "detail": {
      "href": "{{uri}}/api/users/950bdad0-d4e9-4182-be78-c35faa059f73",
      "method": "GET"
    }
  }
}
```

Response when email already in use:
```http
HTTP/1.1 400 Bad Request

{
  "success": false,
  "errors": [
    {
      "message": "E-Mail ist bereits vergeben",
      "code": 400,
      "key": "email"
    }
  ]
}
```

### GET /api/users/{{id}}

Fetch user

`{{id}}` - user id

Request:
```
GET {{uri}}/api/users/{{id}}
Authorization: Bearer {{admin_token}}
X-Client-Ip: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}
```

Request with curl:
```shell
curl\
 -H 'Authorization: Bearer {{admin_token}}'\
 -H 'X-Client-Ip: {{client_ip}}'\
 -H 'X-Client-User-Agent: {{client_user_agent}}'\
 '{{uri}}/api/users/{{id}}'
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
      "href": "{{uri}}/api/users/{{id}}",
      "method": "GET"
    },
    "update": {
      "href": "{{uri}}/api/users/{{id}}",
      "method": "PATCH"
    }
  }
}
```

### GET /api/users/me

Fetch me

`{{user_token}}` is necessary

Request:
```http
GET {{uri}}/api/users/me
Authorization: Bearer {{user_token}}
X-Client-Ip: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}
```

Request with curl:
```shell
curl\
 -H 'Authorization: Bearer {{user_token}}'\
 -H 'X-Client-Ip: {{client_ip}}'\
 -H 'X-Client-User-Agent: {{client_user_agent}}'\
 '{{uri}}/api/users/me'
```

Response:
```http
HTTP/1.1 200 OK
...

{
  "success": true,
  "data": {
    "id": "{{id}}",
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

### PATCH /api/users/{{id}}

Update user

`{{id}}` - user id

Request:
```http
PATCH {{uri}}/users/{{id}}
Authorization: Bearer {{admin_token}}
X-Client-Ip: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}

{
    "active": true
}
```

Request with curl:
```shell
curl\
 -X PATCH\
 -H 'Authorization: Bearer {{admin_token}}'\
 -H 'X-Client-Ip: {{client_ip}}'\
 -H 'X-Client-User-Agent: {{client_user_agent}}'\
 -d '{"active": true}'\
 '{{uri}}/users/{{id}}'
```

Response:
```http
HTTP/1.1 200 OK
...

{
  "success": true
}
```

### DELETE /api/users/{{id}}

Delete user

`{{id}}` - user id

Request:
```http
DELETE {{uri}}/users/{{id}}
Authorization: Bearer {{admin_token}}
X-Client-Ip: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}
```

Request with curl:
```shell
curl\
 -X DELETE\
 -H 'Authorization: Bearer {{admin_token}}'\
 -H 'X-Client-Ip: {{client_ip}}'\
 -H 'X-Client-User-Agent: {{client_user_agent}}'\
 '{{uri}}/users/{{id}}'
```

Response:
```http
HTTP/1.1 200 OK

{
  "success": true
}
```

## Auth

Authentication is a higher layer build over Users resource.
It deals with user registration, activation and login.

### POST /api/auth

Register user

Request:
```http
POST {{uri}}/api/auth
Authorization: Bearer {{admin_token}}
X-Client-Ip: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}

{
    "email": "{{email}}",
    "password": "useruser1!",
    "first_name": "John",
    "last_name": "Doe",
    "_activation_email": true,
    "_activation_optins": [
      {
        "key": "mail",
        "source": "sso:api:register"
      }
    ]
}
```

`first_name` - optional

`last_name` - optional

`_activation_email` - optional, bool, default false, when is sets to `true` then the activation mail will be sent

`_activation_optins` - optional, array, list of optins that will be recorded when activication will by finalized, more info in [Optins](#optins) section

The endpoint don't have `active` field.
Registred users are not active.

Request with curl:
```shell
curl\
 -X POST\
 -H 'Authorization: Bearer {{admin_token}}'\
 -H 'X-Client-Ip: {{client_ip}}'\
 -H 'X-Client-User-Agent: {{client_user_agent}}'\
 -d '{"email": "{{email}}","password": "useruser1!","first_name": "John","last_name": "Doe","_activation_email": true}'\
 '{{uri}}/api/auth'
```

Response:
```http
HTTP/1.1 200 OK
...

{
  "success": true,
  "data": {
    "activation": "{{activation_token}}"
  },
  "links": {
    "activation": {
      "href": "{{uri}}/api/auth/activation",
      "method": "PUT"
    }
  }
}
```

There are 3 ways to activate user:
1. The most standard way is to set `_activation_email` to `true` in this endpoint.
Then the email will be sent to user.
User will click to activation link in email and will be activated.
2. Use `{{activation_token}}` from response and call [PUT /api/auth/activation](#put-apiauthactivation)
3. Call [PATCH /api/users/{{id}}](#patch-apiusersid) and set field `active` to `true`


### PUT /api/auth/activation

Activate registred user using `{{activation_token}}`.

`{{activation_token}}` is from [POST /api/auth](#post-apiauth) response

Request:
```http
PUT {{uri}}/api/auth/activation
Authorization: Bearer {{admin_token}}
X-Client-Ip: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}

{
    "token": "{{activation_token}}"
}
```

Request with curl:
```shell
curl\
 -X PUT\
 -H 'Authorization: Bearer {{admin_token}}'\
 -H 'X-Client-Ip: {{client_ip}}'\
 -H 'X-Client-User-Agent: {{client_user_agent}}'\
 -d '{"token": "{{activation_token}}"}'\
 '{{uri}}/api/auth/activation'
```

Resepone:
```
HTTP/1.1 200 OK
...

{
  "success": true
}
```


### POST /api/auth/token

Get user token by email and password

`{{email}}` - user email

`{{password}}` - user password

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

Request with curl:
```shell
curl\
 -X POST\
 -H 'Authorization: Bearer {{admin_token}}'\
 -H 'X-Client-Ip: {{client_ip}}'\
 -H 'X-Client-User-Agent: {{client_user_agent}}'\
 -d '{"email": "{{email}}", "password": "{{password}}"}'\
 '{{uri}}/api/auth/token'
```

Response:
```http
HTTP/1.1 200 OK
...

{
  "success": true,
  "data": {
    "token": "{{user_token}}",
    "id": "{{id}}"
  }
}
```

`{{user_token}}` - authorization token to api on user level

`{{id}}` - user id

## Attributes

### GET /api/attributes

List attributes of all users

Request:
```http
GET {{uri}}/api/attributes
Authorization: Bearer {{admin_token}}
X-Client-IP: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}
```

Request with curl:
```shell
curl\
 -X GET\
 -H 'Authorization: Bearer {{admin_token}}'\
 -H 'X-Client-Ip: {{client_ip}}'\
 -H 'X-Client-User-Agent: {{client_user_agent}}'\
 '{{uri}}/api/attributes'
```

Response:
```http
HTTP/1.1 200 OK
...

{
  "success": true,
  "data": [
    {
      "name": "post_code",
      "value": "00000",
      "user.id": "16",
      "links": {
        ...
      }
    }
    ...
  ],
  "meta": {
    "page": 1,
    "perPage": 10,
    "total": 16,
    "sort": "name",
    "filter": [],
  }
}
```

### GET /api/users/{{id}}/attributes

List attributes of specific user

`{{id}}` - user id

Request:
```http
GET {{uri}}/api/users/{{id}}/attributes
Authorization: Bearer {{admin_token}}
X-Client-IP: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}
```

Request with curl:
```shell
curl\
 -X GET\
 -H 'Authorization: Bearer {{admin_token}}'\
 -H 'X-Client-Ip: {{client_ip}}'\
 -H 'X-Client-User-Agent: {{client_user_agent}}'\
 '{{uri}}/api/users/{{id}}/attributes'
```

Response:
```http
HTTP/1.1 200 OK
...

{
  "success": true,
  "data": [
    {
      "name": "post_code",
      "value": "00000",
      "user.id": "16"
    }
    ...
  ],
  "links": {
    ...
  }
}
```

### PUT /api/users/{{id}}/attributes/{{name}}

Insert or update user attribute

`{{id}}` - user id

`{{name}}` - attribute name

Request:
```http
PUT {{uri}}/api/users/{{id}}/attributes/{{name}}
Authorization: Bearer {{admin_token}}
X-Client-IP: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}

{
    "value": "00000"
}
```

- `value` - required, string, attribute value

Request curl:
```shell
curl\
 -X PUT\
 -H 'Authorization: Bearer {{admin_token}}'\
 -H 'X-Client-Ip: {{client_ip}}'\
 -H 'X-Client-User-Agent: {{client_user_agent}}'\
 -d '{"value":"00000"}'\
 '{{uri}}/api/users/{{id}}/attributes/{{name}}'
```

Response:
```http
HTTP/1.1 200 OK
...

{
  "success": true
}
```

### DELETE /api/users/{{id}}/attributes/{{name}}

Delete user attribute

- `{{id}}` user id

- `{{name}}` - attribute name

Request:
```http
DELETE {{uri}}/api/users/{id}/attributes/{name}
Authorization: Bearer {{admin_token}}
X-Client-IP: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}
```

Request curl:
```shell
curl\
 -X DELETE\
 -H 'Authorization: Bearer {{admin_token}}'\
 -H 'X-Client-Ip: {{client_ip}}'\
 -H 'X-Client-User-Agent: {{client_user_agent}}'\
 '{{uri}}/api/users/{{id}}/attributes/{{name}}'
```

Response:
```http
HTTP/1.1 200 OK
...

{
  "success": true,
}
```

## Optins

### POST /api/users/{{id}}/optins

Record new optin agreement or no agreement

`{{id}}` - user id

Request:
```json=
POST {{uri}}/api/users/{{id}}/optins
Authorization: Bearer {{admin_token}}
X-Client-Ip: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}

{
    "key": "mail",
    "agreement": "agreement",
    "agreement_type": "default",
    "source": "sso:front:register"
}
```

- `key` - required, string, optin key eg. tel, email
- `agreement` - required, string, available values `agreement`, `no_agreement`
- `agreement_type` - optional, string, available values `default`, `auto`, default value `default`
  - `default` - When user have an option to not agree for the optin. Usually, when there is a checkbox in UI for the optin and user can proceed without checkin it.
  - `auto` - When user can not have an option for disagree.
- `source` - optional, string, additional param to track optin source eg. `osc:order:2356`

Request with curl:
```shell
curl\
 -X POST\
 -H 'Authorization: Bearer {{admin_token}}'\
 -H 'X-Client-Ip: {{client_ip}}'\
 -H 'X-Client-User-Agent: {{client_user_agent}}'\
 -d '{"key":"mail","agreement":"agreement","agreement_type":"default","source":"sso:front:register"}'\
 '{{uri}}/api/users/{{id}}/optins'
```

Response:
```http
HTTP/1.1 201 Created
...

{
  "success": true,
}
```

### GET /api/users/{{id}}/optins

List optins record logs

`{{id}}` - user id

Request:
```json=
GET {{uri}}/api/users/{{id}}/optins
Authorization: Bearer {{admin_token}}
X-Client-Ip: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}
```

Request with curl:
```shell
curl\
 -H 'Authorization: Bearer {{admin_token}}'\
 -H 'X-Client-Ip: {{client_ip}}'\
 -H 'X-Client-User-Agent: {{client_user_agent}}'\
 '{{uri}}/api/users/{{id}}/optins'
```

Response:
```http
HTTP/1.1 200 OK
...

{
  "success": true,
  "data": [
    {
      "key": "mail",
      "client_ip": "127.0.0.1",
      "client_user_agent": "Mozilla/5.0 (Macintosh; I...",
      "agreement": "agreement",
      "agreement_type": "default",
      "source": "sso:api:register",
      "created_at": "2021-12-09T10:29:37.000000Z"
    },
    ...
  ],
  "links": {
    ...
  },
  "meta": {
    "page": 1,
    "perPage": 10,
    "total": 11,
    "sort": "created_at",
    "filter": []
  }
}

```

### GET /api/users/{{id}}/optins/agreement

Lists all consented optins

`{{id}}` - user id

Request:
```json=
GET {{uri}}/api/users/{{id}}/optins/log
Authorization: Bearer {{admin_token}}
X-Client-Ip: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}
```

Request with curl:
```shell
curl\
 -H 'Authorization: Bearer {{admin_token}}'\
 -H 'X-Client-Ip: {{client_ip}}'\
 -H 'X-Client-User-Agent: {{client_user_agent}}'\
 '{{uri}}/api/users/{{id}}/optins/agreement'
```

Response:
```http
HTTP/1.1 200 OK
...

{
  "success": true,
  "data": [
    {
      "key": "mail",
      "client_ip": "127.0.0.1",
      "client_user_agent": "Mozilla/5.0 (Macintosh; I...",
      "agreement": "agreement",
      "agreement_type": "default",
      "source": "sso:api:register",
      "created_at": "2021-12-09T10:29:37.000000Z"
    },
    ...
  ],
  "links": {
    "self": {
      "href": "{{uri}}/api/users/{{id}}/optins",
      "method": "GET"
    }
  }
}
```

## Use cases

### Get user by email and password

1. Use [POST /api/auth/token](#post-apiauthtoken) to get `{{user_token}}`
2. Use [GET /api/users/me](#get-apiusersme) to get user
