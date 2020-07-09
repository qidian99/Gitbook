---
description: Make API Requests with OAuth
---

# 用OAuth来进行API请求

{% file src="../../.gitbook/assets/make-api-requests-with-oauth-\_-drupalize.me.pdf" caption="13：用OAuth来进行API请求" %}

## Outline

* Install the Simple OAuth Drupal module, and configure it to work with a password grant flow to allow our code to exchange a username and password for an access token
* Demonstrate how to retrieve and use an OAuth access token to make authenticated requests

## Install and configure Simple OAuth module

Start by downloading and installing the [Simple OAuth module](https://www.drupal.org/project/simple_oauth).

Use the following steps to configure Simple OAuth to allow our React application to use a password grant to retrieve access and refresh tokens from Drupal. \(For detailed instructions see [Install and Configure Simple OAuth](https://drupalize.me/tutorial/install-and-configure-simple-oauth).\)

### NOTE: Don't install via tar.gz or link, use COMPOSER

![](../../.gitbook/assets/image%20%2818%29.png)

To follow along with building this app you'll need this configuration at a minimum:

* Create a Drupal role to use with OAuth; in this example we will use one named _OAUTH_.
* Give the _OAUTH_ role permissions to add, edit, and delete content.
* Add a consumer at _/admin/config/services/consumer/add_ named "react app"
* Set an app secret.
  * New Secret: `app`. Make note of the app secret that you used if it's different.
* Check the box to enable _OAUTH_ role for the scope of this client.
* Save the new configuration.
* Make note of the UUID for the client at _/admin/config/services/consumer_, e.g. `0485fbc2-6ce3-4e34-9ffe-df7833c0476c`.

![Screenshot of consumer configuration form with above settings.](https://drupalize.me/sites/default/files/tutorials/consumer-config.png)

> Create a [role](http://drupal-8-9-1.dd:8083/admin/people/roles) for every logical group of permissions you want to make available to a consumer.

### Give the _OAUTH_ role the required permissions

We want users logged in with the _OAUTH_ role to be able to add, edit, and delete _Article_ content. Make sure the role has the following permissions:

* _Article:_ Create new content
* _Article:_ Delete any content
* _Article:_ Edit any content

The following are not required, but nice to have:

* View content overview page
* View own unpublished nodes

Otherwise Drupal will return a 403 forbidden error when our application attempts to add, update, or delete content.

Not sure if you've got it configured right? Log into Drupal as the user you want to use via your app. If you can create an article as that user, you can create an article when authenticated as that user via OAuth.

### Create a user account with the _OAUTH_ role

OAuth allows us to exchange a username and password for an access token -- like how a browser can exchange a username and password for a session ID. The access token will grant you the permissions of the user for whom it was created. To do this, add a new Drupal user at _admin/people/create_ and give them the _OAUTH_ role.

## Test requests in Postman

The following are examples of retrieving and using an OAuth access token from Drupal using a password grant, and then using it to make authenticated requests. The basic process is:

* Exchange a username and password for access and refresh tokens
* Use the access token in the Authorization header of a request
* Use the refresh token to get a new access token when the current one expires

These are examples of the different requests we'll need to make via our React application.

For more detailed information about how this works, read [Get a Token for OAuth 2 Requests](https://drupalize.me/tutorial/get-token-oauth-2-requests?p=3003) and [Make an Authenticated Request Using OAuth 2](https://drupalize.me/tutorial/make-authenticated-request-using-oauth-2?p=3003).

In this example we'll use Postman to make the test requests. You can use Postman, cURL, or any other application that allows you to generate HTTP requests.

#### Get an OAuth token

Make a POST request to `http://localhost:8888/oauth/token`

In my case it's `http://drupal-8-9-1.dd:8083/oauth/token`

**Headers:**

```text
Accept: application/vnd.api+json
Content-Type: application/x-www-form-urlencoded
```

![Postman application showing headers of request for an OAuth grant.](https://drupalize.me/sites/default/files/tutorials/postman-oauth-headers.png)

**Body:**

```text
grant_type = password
client_id = 0485fbc2-6ce3-4e34-9ffe-df7833c0476c
client_secret = app
scope = oauth
username = {username of Drupal user with OAUTH role}
password = {password}
```

![Postman application showing body of request for an OAuth grant.](https://drupalize.me/sites/default/files/tutorials/postman-oauth-body.png)

You should get a response that looks like:

```javascript
{
    "token_type": "Bearer",
    "expires_in": 86400,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp0aSI6ImMwODNlYzVkMTJjNDk4M2JlMzE0MmZhMDc4NGU3NWEwNTJmMzlkNmM2YjkwNjY1YTFiZGI4OGJiOTgwOGNkZGQ3MDIwYjIzZTFhYzNmZTA0In0.eyJhdWQiOiIwNDg1ZmJjMi02Y2UzLTRlMzQtOWZmZS1kZjc4MzNjMDQ3NmMiLCJqdGkiOiJjMDgzZWM1ZDEyYzQ5ODNiZTMxNDJmYTA3ODRlNzVhMDUyZjM5ZDZjNmI5MDY2NWExYmRiODhiYjk4MDhjZGRkNzAyMGIyM2UxYWMzZmUwNCIsImlhdCI6MTUyNTk5MzAwNCwibmJmIjoxNTI1OTkzMDA0LCJleHAiOjE1MjYwNzk0MDQsInN1YiI6IjIiLCJzY29wZXMiOlsib2F1dGgiLCJhdXRoZW50aWNhdGVkIl19.O96CXDq6ZWqx5gnzB5lJXKHxqlP_Yg0zXtsY9OvNgV5GLE8FFt-gcRoylMA6uK9-VNtPUXXdrWENq_3NUup4UJqOPFEQLJJ8sZ_PNcZKTQsL4UIYokvHpZZqlSOMWNuH23SPyusTBoMOnfKTX2AT3-7xJE0J47IIbVUHIv9Rm64",
    "refresh_token": "def5020036590d2c3c1bb057ec2683e4f0600e5e1fa467fd8f26e4a7d55fe33d929501cfc806dd7f4fbe8c39798088ce62fb2832ee29110640ddcd9a2085d14fa5fe8cb319457879c92ab12aaf0dc06cbe2e50c60fd7cc2b8e581dd5eba65161c9d4bbbeeaa42f9282a56861024f5e93aed98441d055bde0698f853e920ba5857172e34ca38773be052a02470e5e1427c63b0d64cd6bcb2d887decaa1cd33d7f41db233bc039fd93a511531889df1ca69f6a88660df0a452ee3117df52fe55a6642a1952bb66835f39300bc590a528fdc3e8ba56b50be4b56d8bae0ec75daab0423c512f00efed3c4e963bcfa8fb77625515a3c92261033dc5703fd429a742541f4185400967a591c95772ef11e8aab66910da5e626f4fd17a518dcf2fcda78c1e1377af2ff00d3edd68f085534d92128c0099da3b8703d52759c7281714e6fc3cdca67710d60983fd62d2590dc85ce0badd09c31434c6d7593baa438db568238a0cf09fb9cede0ea4f858f662da79d3bb131f5ed69f3ae38b892f9e75e20dfd5287b7d6efc18a2afaecd58aa4822da5ce5d6273ec6fef414ecc6be90a"
}
```

### Need to set the OAuth key

```javascript
{
    "error": "server_error",
    "error_description": "The authorization server encountered an unexpected condition which prevented it from fulfilling the request: You need to set the OAuth2 secret and private keys.",
    "message": "The authorization server encountered an unexpected condition which prevented it from fulfilling the request: You need to set the OAuth2 secret and private keys."
}
```

![](../../.gitbook/assets/image%20%2816%29.png)

#### Change permission of the public key.

Copy the value of the access token, and **note** that it is `token_type: Bearer`.

### Need to set the halt salt to be at least 32 characters

```javascript
{
    "error": "server_error",
    "error_description": "The authorization server encountered an unexpected condition which prevented it from fulfilling the request: Hash salt must be at least 32 characters long.",
    "message": "The authorization server encountered an unexpected condition which prevented it from fulfilling the request: Hash salt must be at least 32 characters long."
}
```

### If you ever got a base\_table not exist error

{% embed url="https://www.drupal.org/project/simple\_oauth/issues/3080329" %}

Run `drush entup` for Drupal &lt; 8.7 \(since it ships with Drupal Core\).

For later Drupal versions, install the module `Devel Entity Updates`and run the same command

![](../../.gitbook/assets/image%20%289%29.png)

### And Finally

![](../../.gitbook/assets/image%20%2830%29.png)

### Use POST to create a new node

To add authentication to a request, you need to edit the header and add the token from above. Note, the token is truncated below to make it easier to read. Use the full token in your tests.

Make a POST request to `http://localhost:8888/jsonapi/node/article`

**Headers:**

```text
Accept: application/vnd.api+json
Content-Type: application/vnd.api+json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbG...
```

![Postman application with request headers filled out.](https://drupalize.me/sites/default/files/tutorials/postman-post-headers.png)

**Body:**

```javascript
{
  "data": {
    "type": "node--article",
    "attributes": {
      "title": "My New Node",
      "body": {
        "value": "Content of new node",
        "format": "plain_text"
      }
    }
  }
}
```

![Postman application with request body filled out.](https://drupalize.me/sites/default/files/tutorials/postman-post-body.png)

If this is successful, you should get a `201 Created` response.

![Postman application showing successful response.](https://drupalize.me/sites/default/files/tutorials/postman-post-response.png)

### In case "X-CSRF-Token" is missing

```javascript
{
    "jsonapi": {
        "version": "1.0",
        "meta": {
            "links": {
                "self": {
                    "href": "http://jsonapi.org/format/1.0/"
                }
            }
        }
    },
    "errors": [
        {
            "title": "Forbidden",
            "status": "403",
            "detail": "X-CSRF-Token request header is missing",
            "links": {
                "via": {
                    "href": "http://drupal-8-9-1.dd:8083/jsonapi/node/article"
                },
                "info": {
                    "href": "http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.4"
                }
            }
        }
    ]
}

```

{% embed url="https://www.drupal.org/project/drupal/issues/3055260" %}



### Use PATCH to edit a new node

Make a PATCH request to `http://localhost:8888/jsonapi/node/article/{NODE UUID}`

**Headers:**

```text
Accept: application/vnd.api+json
Content-Type: application/vnd.api+json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGci...
```

![Postman application with PATCH request headers filled out.](https://drupalize.me/sites/default/files/tutorials/postman-patch-headers.png)

**Body:**

```text
{
  "data": {
    "type": "node--article",
    "id": {NODE UUID},
    "attributes": {
      "title": "My Node, edited",
      "body": {
        "value": "A new node",
        "format": "plain_text"
      }
    }
  }
}
```

![Postman application with PATCH request body filled out.](https://drupalize.me/sites/default/files/tutorials/postman-patch-body.png)

If this is successful, you should get a `200 OK` response.

![Postman application showing response for successful PATCH request.](https://drupalize.me/sites/default/files/tutorials/postman-patch-response.png)

#### Use DELETE to delete a node

Make a DELETE request to `http://localhost:8888/jsonapi/node/article/{NODE UUID}`

**Headers:**

```text
Accept: application/vnd.api+json
Content-Type: application/vnd.api+json
Authorization: Bearer eyJ0eXAiOiJKV1Qi...
```

![Postman application with DELETE request headers filled out.](https://drupalize.me/sites/default/files/tutorials/postman-delete-headers.png)

**No Body.**

![Postman application with DELETE request body filled out.](https://drupalize.me/sites/default/files/tutorials/postman-delete-body.png)

If this is successful, you should get a `204 No Content` response with an empty body.

![Postman application showing response for successful DELETE request.](https://drupalize.me/sites/default/files/tutorials/postman-delete-response.png)

### Troubleshooting

* Use double-quotes in your JSON, not single quotes, to avoid issues with special characters
* Make sure there are no trailing commas in your JSON objects

