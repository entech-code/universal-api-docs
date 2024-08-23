---
title: Release log
layout: default
nav_order: 3
---


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