---
title: Quick start
layout: default
nav_order: 1
---

## Quick start guide

This guide walks you through the step-by-step process to set it up on your HTML page, initialize the required script, and make authenticated calls to the API Panda.

## Step 1: Add Script Link

Insert the following script link at the end of the body tag in your HTML page. This link references the required JavaScript file for the API Panda.

```html
<script src="https://app.apipanda.net/api/v1/connection/widget-plugin"></script>
```

## Step 2: Initialize the Script

Upon page load, initialize the script using the following snippet. This snippet also demonstrates how to set up a callback function to handle the token received from the API Panda, which is essential for authenticated communication with third-party services.

```javascript
$(document).ready(function () {
  /*
        Receives API Panda token details
        @param {tokenDetails} represents accumulated information about token
        @param {tokenDetails.token} serves to authorize requests to particular integration
        @param {tokenDetails.token} serves to determine concrete integration type for token
        @param {tokenDetails.category} category helps to determine which type of api is acceptable for current token
        @example tokenDetails.category = { 'Marketplace', 'PrintOnDemand' }
        @example tokenDetails.integrationType = { 'Shopify', 'Etsy', 'Printify', 'Printful' }
    */
  function onTokenReceived(tokenDetails) {
    localStorage["uapi_token"] = tokenDetails.token;
    localStorage["uapi_category"] = tokenDetails.category;
  }

  var widgetInitialization = function (publicToken) {
    $.ajax({
      url: `https://app.apipanda.net/api/v1/connection/link-token/${publicToken}`,
      method: "POST",
      contentType: "application/json; charset=utf-8",
      dataType: "json",
      success: function (data, status) {
        var plugin = new window.WidgetPlugin({
          linkToken: linkToken,
          onTokenReceived: onTokenReceived,
        });

        plugin.init();
      },
    });
  };
});
```

## Step 3: Add Button to HTML

Add a button to your HTML which will trigger the API Panda widget when clicked.

```html
<a id="open-widget" href="#" class="btn btn-primary">Open widget</a>
```

## Step 4: Add Button Handler

Implement a click event handler for the button to show the API Panda widget as shown below.

```javascript
var btn = $("#open-widget");

btn.click(function (e) {
  e.preventDefault();
  plugin.showWidget();
});
```

## Step 5: Make a call API Panda

Once authenticated, you can make calls to the API Panda to manage your third-party services. Utilize the token saved earlier to authorize your requests.

### Marketplace

The example below demonstrates the creation of product assets in a third-party platform where the user has been authenticated.

```javascript
var title = $("#title").val();
var description = $("#description").val();
var imageUrl = $("#image-url").val();

var data = {
  title: title,
  description: description,
  categoryId: categoryId,
  images: [
    {
      url: imageUrl,
    },
  ],
};

var uapiToken = localStorage["uapi_token"];
$.ajax({
  url: `https://app.apipanda.net/api/marketplace/v1/products`,
  data: JSON.stringify(data),
  method: "POST",
  contentType: "application/json; charset=utf-8",
  dataType: "json",
  headers: {
    "X-ACCOUNT-TOKEN": uapiToken,
  },
  success: function (data, status) {
    alert(`Saved ${data.title} successfully`);
  },-
});
```

### PrintOnDemand

The example below demonstrates the uploading images in a third-party platform where the user has been authenticated.

```javascript
var data = {
  images: [],
};

$(".image-url").each(function () {
  var imageUrl = $(this).val();

  if (imageUrl) {
    data.images.push({
      url: imageUrl,
    });
  }
});

$.ajax({
  url: `https://app.apipanda.net/api/print-on-demand/v1/images`,
  data: JSON.stringify(data),
  method: "POST",
  contentType: "application/json; charset=utf-8",
  dataType: "json",
  headers: {
    "X-ACCOUNT-TOKEN": $("#token").text(),
  },
  success: function (data, status) {
    alert(`Saved ${data.length} images successfully`);
  },
});
```

That's it! You have now successfully set up and utilized the API Panda on your website.
