---
title: Release log
layout: default
nav_order: 3
---

# 23 Aug 2024

## Breaking changes

## All endpoints

HTTP 400 Reponse

Before change:
```
[
    {
      "code": 0,
      "message": "string",
      "property_name": "string",
      "context": {
        "additionalProp1": "string",
        "additionalProp2": "string",
        "additionalProp3": "string"
      }
    }
]
```

After change:
```
{
    "errors": [
        {
            "code": 0,
            "message": "string",
            "property_name": "string",
            "context": {
            "additionalProp1": "string",
            "additionalProp2": "string",
            "additionalProp3": "string"
            }
        }
    ]
}
```

# 8 Aug 2024

## Breaking changes

## Print On Demand APIs

### /api/print-on-demand/v1/images

HTTP 200 Response

Before change:
```
[
    {
        "id": "string",
        "url": "string"
    }
]
```

After change:
```
{
    "images": [
        {
            "id": "string",
            "url": "string"
        }
    ]
}
```