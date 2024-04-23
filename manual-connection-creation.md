---
title: Manual connection creation
layout: default
nav_order: 2
---

## Preparations

As for a widget approach you can freely setup connection using REST API.

To do that you have to use your `publicToken` as well to create `linkToken`.

Using your publicToken send initial request to API endpoint below:

```shell
POST
https://app.apipanda.net/api/v1/connection/link-token/{publicToken}
```

As response you will have next json object:

```json
{
  "linkToken": "<linkTokenString>"
}
```

Since you have `linkToken`, you're ready for next steps.

## Setup new connection

When you're already have your `linkToken` next step is about configuring your connection.
To do that you need to send following request:

```shell
POST
https://app.apipanda.net/api/v1/connection/initiate
X-LINK-TOKEN: <linkToken>
Content-Type: application/json
{
  "name": "<connection name>",
  "slug": "<slug, if applicapable>",
  "integrationType": "<integration type>"
}
```

As response you'll receive next object:

```json
{
  "linkToken": "<linkToken>",
  "authenticationProfile": {
    "authenticationType": "OAuth",
    "category": "<category>",
    "integrationType": "<integration type>",
    "authenticationUrl": "<OAuth url to proceed>"
  }
}
```

You can save `integrationType` and `category` in your data store for future connection determination with typings.

Those types have following values sets:

### IntegrationType

```
Shopify, Printify, Printful, Etsy, TikTok, Amazon
```

### Category

```
Marketplace, PrintOnDemand
```

### Relations between category and integration type

```json
{
  "Marketplace": ["Shopify", "Etsy", "TikTok", "Amazon"],
  "PrintOnDemand": ["Printify", "Printful"]
}
```


## Proceed OAuth workflow

After receiving OAuth url from target service you'll have to finish OAuth workflow via browser using given url.

After successfull bypassing OAuth process you can check it's status with following endpoint:
```shell
POST
https://app.apipanda.net/api/v1/connection/established/{linkToken}
```

Once it will be completed you'll need to exchange `publicToken` to `accessToken`

As response you'll receive next object:

```json
{
  "established": true,
  "publicToken": "<publicToken>"
}
```

## Exchange publicToken to accessToken

As a final step you need to send one following request:

```shell
POST
https://app.apipanda.net/api/v1/connection/access-token/{publicToken}
```

As response you'll receive next object:
```json
{
  "accessToken": "<accessToken>"
}
```

### Conclusion

After that you're ready to use API Panda, and how to do that you can read more in [Quick start guide](quick-start#step-5-make-a-call-universal-api)