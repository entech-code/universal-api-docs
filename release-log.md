---
title: Release log
layout: default
nav_order: 3
---

### Release 0.23

### Planned: Tue Sep 3rd, 2024

* [Breaking] Errors response format changed to be more explicit:
    * Before change:
    
            HTTP 400 Reponse
        
        ```json
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

  * After change:
        
        HTTP 400 Response

    ```json
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
* Optimized /connection/established endpoint
* Fixed response HTTP code for validation errors
    * Before change:

            HTTP 200 OK
    
    * After change:

            HTTP 400 BadRequest
* Required Etsy location field:
    * location field is getting to be required per each `product.variant` which is set to `product`
    * in case of not set location response will contain validation error

            HTTP 400 Response

        ```json
        [
            {
                "code": 68753035282,
                "message": "Product variant inventory locations are required",
                "property_name": "Locations"
            }
        ]
        ```

### Release 0.22

### Deployed:  Mon Aug 3, 2024
 * [Breaking change] Changed response for endpoint /api/print-on-demand/v1/images:
    * Before change:
    
            HTTP 200 Reponse
        
        ```json
    [
            {
                "id": "string",
                "url": "string"
            }
    ]
        ```
    * After change:
    
            HTTP 200 Reponse

        ```json
        {
            "images": [
                {
                    "id": "string",
                    "url": "string"
                }
            ]
        }
        ```