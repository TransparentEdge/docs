---
description: >-
  Instructions for user authentication against the Transparent Edge Services
  API.
---

# Authentication

To authenticate against the API, we use the OAuth2 federation system. This means that in order to make requests to the API, you need to have an access token for authentication.&#x20;

First of all, we will need the values of `client_id` and `client_secret`, which can be obtained from our [dashboard](https://dashboard.transparentcdn.com/user/profile). Click on your username, to view your user profile, there you'll able to obtain the required API keys.

Take note of the values that appear, as we will need them for the following steps:

Here is an example, with fake data of the entire process of obtaining the access token (API\_TOKEN).

> client\_id: 0b58b22d51a2d68Ffdf17b&#x20;
>
> client\_secret: 0f3b38f9721211e848e39be374a4c1431386abdfe86&#x20;
>
> company\_id: 42

With this information, the first step is to make a call to the API to obtain the authorization token. This may vary depending on the programming language used, here is a basic example using cURL.

```sh
curl -XPOST -d "client_id=0b58b22d51a2d68df17b&client_secret=0f3b38f9701e848e39be374a4c1431386abdfe86&grant_type=client_credentials" https://api.transparentcdn.com/v1/oauth2/access_token/
```

This request will return a result in JSON format, similar to this:

```json
{
  "access_token": "58530ad8f1879786dbfad7Gh1a3b94c1dae1070280fdb",
  "token_type": "Bearer",
  "expires_in": 36000,
  "refresh_token": "63553322177e88fHa8Kb25b122542e35733ce2a9cc24",
  "scope": "read write"
}
```

The field `access_token` is essential to continue making requests to the API. All requests that include the header "Authorization: Bearer " will be authenticated with the user who requested the access\_token. For example, a request similar to the one shown below will return a JSON object with the user stored in our systems.

```
curl -v -H 'Authorization: Bearer 58530ad8f1879786dbfad7Gh1a3b94c1dae1070280fdb' https://api.transparentcdn.com/v1/companies/current_user/
```
