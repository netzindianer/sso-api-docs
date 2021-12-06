# SSO API Docs

- [Introduction](#introduction)
- [Resources](#resources)
  - [Users](#users)
    - [GET /api/users](#get-apiusers)
    - [POST /api/users](#post-apiusers)
    - [GET /api/users/{{id}}](#get-apiusersid)
    - [GET /api/users/me](#get-apiusersme)
    - [PATCH /api/users/{{id}}](#patch-apiusersid)
    - [DELETE /api/users/{{id}}](#delete-apiusersid)
  - [Auth](#auth)
    - [POST /api/auth/token](#post-apiauthtoken)
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

## Resources

### Users

#### GET /api/users

List users

Request:
```http
GET {{uri}}/api/users
Authorization: Bearer {{admin_token}}
X-Client-Ip: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}
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
    "predicate": []
  }
}
```




#### POST /api/users

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

#### GET /api/users/{{id}}

Fetch user

`{{id}}` - user id

Request:
```
GET {{uri}}/api/users/{{id}}
Authorization: Bearer {{admin_token}}
X-Client-Ip: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}
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



#### GET /api/users/me

Fetch me

- `{{user_token}}` is necessary

Request:
```http
GET {{uri}}/api/users/me
Authorization: Bearer {{user_token}}
X-Client-Ip: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}
```

Request with curl:
```shell
curl \
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



#### PATCH /api/users/{{id}}

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

Response:
```http
HTTP/1.1 200 OK
...

{
  "success": true
}
```

#### DELETE /api/users/{{id}}

Delete user

`{{id}}` - user id

Request:
```http
DELETE {{uri}}/users/{{id}}
Authorization: Bearer {{admin_token}}
X-Client-Ip: {{client_ip}}
X-Client-User-Agent: {{client_user_agent}}
```

Response:
```http
HTTP/1.1 200 OK

{
  "success": true
}
```



### Auth

#### POST /api/auth/token

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
curl \
 -X POST \
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
    "token": "{{user_token}}"
  }
}
```




## Use cases

### Get user by email and password

1. Use [POST /api/auth/token](#post-api-auth-token) to get `{{user_token}}`
2. Use [GET /api/users/me](#get-api-users-me) to get user