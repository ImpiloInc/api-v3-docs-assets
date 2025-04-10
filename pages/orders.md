This guide is designed to help you understand how orders can be created and linked with either an existing patient or an existing site, and the required as well as optional attributes for order creation.

## Understanding Patients and Sites

Before we dive into the order creation process, it's important to understand the entities associated with an order:

- **Patient**: A patient profile is stored within Impilo and is managed by the customer. These profiles are essential for personalizing and managing patient-related orders.

- **Site**: A site represents a customer-managed location that engages with Impilo’s services. Patients can be associated with a site for the purpose of orders and inventory management. The customer is responsible for managing the set of sites within their account, which can interact with Impilo’s services.

## Create Order Endpoint

To create an order, you'll interact with a specific endpoint designed for this purpose. This endpoint allows you to initiate and configure orders based on your requirements.

### Required Attributes

For a successful order creation, you must ensure the following required attributes are correctly provided:

- **Association**: Your order must be associated with either an existing **patient** or a **site**. It is crucial that exactly one of these is specified for an order, and only the `id` property of the patient or site is required.

- **Order Content**: Your order must include at least one of the following:
    - `orderItems`: A list of items to be ordered. You must provide at least one item with only the `id` property required.
    - `orderKits`: A list of kits to be ordered. Similar to order items, at least one kit must be specified with only the `id` property required.

### Create Patient Order Example

The following request contains the bare minimum for creating an order for an existing patient:

```bash
curl --request POST \
--url https://example.com/api/v3/order \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--data '{
  "externalOrderIds": ["xyz_123_0", "xyz_123_1", "xyz_123_2", "xyz_123_3"],
  "patient": {"id": 1234, "sex": "unknown"},
  "orderItems": [
    {"item": {"id": 1000}, "count": 10},
    {"item": {"id": 1001}, "count": 10},
    {"item": {"id": 1002}, "count": 10},
    {"item": {"id": 1003}, "count": 10}
  ],
  "orderKits": [
    {"kit": {"id": 1000}, "count": 10},
    {"kit": {"id": 1001}, "count": 10},
    {"kit": {"id": 1002}, "count": 10},
    {"kit": {"id": 1003}, "count": 10}
  ],
  "shippingOption": "standard"
}
'
```

Below is an example response containing sample data:

```json
{
  "id": 0,
  "externalOrderIds": ["string"],
  "patient": {
    "id": 1,
    "externalIdentifier": "1234abcd",
    "firstName": "John",
    "lastName": "Doe",
    "dateOfBirth": "1980-01-01",
    "email": "john.doe@example.com",
    "phoneNumber": "+1234567890",
    "archived": true,
    "address": {
      "lineOne": "string",
      "lineTwo": "string",
      "lineThree": "string",
      "city": "string",
      "state": "string",
      "zipCode": "string",
      "country": "string",
      "note": "string"
    },
    "site": {
      "id": 1,
      "name": "string",
      "active": true,
      "phoneNumber": "string",
      "address": {
        "lineOne": "string",
        "lineTwo": "string",
        "lineThree": "string",
        "city": "string",
        "state": "string",
        "zipCode": "string",
        "country": "string",
        "note": "string"
      }
    },
    "enrolled": true,
    "sex": "other"
  },
  "site": {
    "id": 1,
    "name": "string",
    "active": true,
    "phoneNumber": "string",
    "address": {
      "lineOne": "string",
      "lineTwo": "string",
      "lineThree": "string",
      "city": "string",
      "state": "string",
      "zipCode": "string",
      "country": "string",
      "note": "string"
    }
  },
  "currentStatus": "availableForPickup",
  "orderItems": [
    {
      "item": {
        "id": 1,
        "manufacturer": {"id": 1, "name": "string"},
        "name": "string",
        "model": "string",
        "sku": "string",
        "orderable": true
      },
      "count": 0
    }
  ],
  "orderKits": [
    {
      "kit": {
        "id": 0,
        "name": "string",
        "archived": true,
        "kitItems": [
          {
            "item": {
              "id": 1,
              "manufacturer": {"id": 1, "name": "string"},
              "name": "string",
              "model": "string",
              "sku": "string",
              "orderable": true
            },
            "count": 0
          }
        ]
      },
      "count": 0
    }
  ],
  "orderEvents": [{"name": "string", "eventTimestamp": "string"}],
  "orderNotes": [
    {"note": "string", "createdAt": "string", "createdBy": {"id": 0, "firstName": "string", "lastName": "string", "email": "string"}}
  ],
  "trackingNumbers": [{"carrier": "string", "value": "string"}],
  "devices": [
    {
      "id": 1,
      "item": {
        "id": 1,
        "manufacturer": {"id": 1, "name": "string"},
        "name": "string",
        "model": "string",
        "sku": "string",
        "orderable": true
      },
      "currentStatus": "string",
      "used": true,
      "currentPatient": {
        "id": 1,
        "externalIdentifier": "1234abcd",
        "firstName": "John",
        "lastName": "Doe",
        "dateOfBirth": "1980-01-01",
        "email": "john.doe@example.com",
        "phoneNumber": "+1234567890",
        "archived": true,
        "address": {
          "lineOne": "string",
          "lineTwo": "string",
          "lineThree": "string",
          "city": "string",
          "state": "string",
          "zipCode": "string",
          "country": "string",
          "note": "string"
        },
        "site": {
          "id": 1,
          "name": "string",
          "active": true,
          "phoneNumber": "string",
          "address": {
            "lineOne": "string",
            "lineTwo": "string",
            "lineThree": "string",
            "city": "string",
            "state": "string",
            "zipCode": "string",
            "country": "string",
            "note": "string"
          }
        },
        "enrolled": true,
        "sex": "other"
      },
      "site": {
        "id": 1,
        "name": "string",
        "active": true,
        "phoneNumber": "string",
        "address": {
          "lineOne": "string",
          "lineTwo": "string",
          "lineThree": "string",
          "city": "string",
          "state": "string",
          "zipCode": "string",
          "country": "string",
          "note": "string"
        }
      },
      "lastHealthCheck": "string",
      "deviceIdentifiers": [{"type": "string", "value": "string"}],
      "deviceEvents": [{"type": "string", "eventTimestamp": "string"}],
      "externalIdentifier": "string",
      "disabledReadings": true
    }
  ],
  "packedKits": [
    {
      "kit": {
        "id": 0,
        "name": "string",
        "archived": true,
        "kitItems": [
          {
            "item": {
              "id": 1,
              "manufacturer": {"id": 1, "name": "string"},
              "name": "string",
              "model": "string",
              "sku": "string",
              "orderable": true
            },
            "count": 0
          }
        ]
      },
      "devices": [
        {
          "id": 1,
          "item": {
            "id": 1,
            "manufacturer": {"id": 1, "name": "string"},
            "name": "string",
            "model": "string",
            "sku": "string",
            "orderable": true
          },
          "currentStatus": "string",
          "used": true,
          "currentPatient": {
            "id": 1,
            "externalIdentifier": "1234abcd",
            "firstName": "John",
            "lastName": "Doe",
            "dateOfBirth": "1980-01-01",
            "email": "john.doe@example.com",
            "phoneNumber": "+1234567890",
            "archived": true,
            "address": {
              "lineOne": "string",
              "lineTwo": "string",
              "lineThree": "string",
              "city": "string",
              "state": "string",
              "zipCode": "string",
              "country": "string",
              "note": "string"
            },
            "site": {
              "id": 1,
              "name": "string",
              "active": true,
              "phoneNumber": "string",
              "address": {
                "lineOne": "string",
                "lineTwo": "string",
                "lineThree": "string",
                "city": "string",
                "state": "string",
                "zipCode": "string",
                "country": "string",
                "note": "string"
              }
            },
            "enrolled": true,
            "sex": "other"
          },
          "site": {
            "id": 1,
            "name": "string",
            "active": true,
            "phoneNumber": "string",
            "address": {
              "lineOne": "string",
              "lineTwo": "string",
              "lineThree": "string",
              "city": "string",
              "state": "string",
              "zipCode": "string",
              "country": "string",
              "note": "string"
            }
          },
          "lastHealthCheck": "string",
          "deviceIdentifiers": [{"type": "string", "value": "string"}],
          "deviceEvents": [{"type": "string", "eventTimestamp": "string"}],
          "externalIdentifier": "string",
          "disabledReadings": true
        }
      ]
    }
  ],
  "shippingOption": "standard"
}

```

### Create Site Order Example

The following request contains the bare minimum for creating an order for an existing site:

```bash
curl -X POST /api/v3/order \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'


```

`POST /api/v3/order`

This endpoint is for creating orders linked to either a patient or a site, with order content specified by item or kit IDs and an association with a patient or site ID. Optional attributes offer further customization, such as tracking through external order IDs.\n\nFor a comprehensive guide on utilizing this endpoint, including examples, required and optional attributes, and additional functionalities like listing and fetching orders, visit the [Order Management Guide](/guides/order-management).

> Body parameter

```json
{
  "externalOrderIds": [
    "xyz_123_0",
    "xyz_123_1",
    "xyz_123_2",
    "xyz_123_3"
  ],
  "patient": {
    "id": 1234,
    "sex": "unknown"
  },
  "orderItems": [
    {
      "item": {
        "id": 1000
      },
      "count": 10
    },
    {
      "item": {
        "id": 1001
      },
      "count": 10
    },
    {
      "item": {
        "id": 1002
      },
      "count": 10
    },
    {
      "item": {
        "id": 1003
      },
      "count": 10
    }
  ],
  "orderKits": [
    {
      "kit": {
        "id": 1000
      },
      "count": 10
    },
    {
      "kit": {
        "id": 1001
      },
      "count": 10
    },
    {
      "kit": {
        "id": 1002
      },
      "count": 10
    },
    {
      "kit": {
        "id": 1003
      },
      "count": 10
    }
  ],
  "shippingOption": "standard"
}
```

Below is an example response containing sample data:

`POST /api/v3/order` (response)

### Optional Attributes

In addition to the required attributes, the following optional attributes can be specified to further customize your order:

- **External Order IDs** (`externalOrderIds`): An array of strings allowing the client to associate one or more of their own order IDs with the Impilo order record. This is particularly useful for tracking and management purposes on the client’s end.

## Listing Orders

This functionality allows you to retrieve a list of orders made within the system. It supports pagination to help manage and navigate through large sets of orders effectively. This endpoint is particularly useful for overviewing all orders and analyzing order trends or statuses within a specific timeframe or criteria.

```bash
curl --request GET \
--url https://example.com/api/v3/order \
--header 'Accept: application/json'
```

Response:
```json
{
  "content": [
    {
      "id": 0,
      "externalOrderIds": ["string"],
      "patient": {
        "id": 1,
        "externalIdentifier": "1234abcd",
        "firstName": "John",
        "lastName": "Doe",
        "dateOfBirth": "1980-01-01",
        "email": "john.doe@example.com",
        "phoneNumber": "+1234567890",
        "archived": true,
        "address": {
          "lineOne": "string",
          "lineTwo": "string",
          "lineThree": "string",
          "city": "string",
          "state": "string",
          "zipCode": "string",
          "country": "string",
          "note": "string"
        },
        "site": {
          "id": 1,
          "name": "string",
          "active": true,
          "phoneNumber": "string",
          "address": {
            "lineOne": "string",
            "lineTwo": "string",
            "lineThree": "string",
            "city": "string",
            "state": "string",
            "zipCode": "string",
            "country": "string",
            "note": "string"
          }
        },
        "enrolled": true,
        "sex": "other"
      },
      "site": {
        "id": 1,
        "name": "string",
        "active": true,
        "phoneNumber": "string",
        "address": {
          "lineOne": "string",
          "lineTwo": "string",
          "lineThree": "string",
          "city": "string",
          "state": "string",
          "zipCode": "string",
          "country": "string",
          "note": "string"
        }
      },
      "currentStatus": "availableForPickup",
      "orderItems": [
        {
          "item": {
            "id": 1,
            "manufacturer": {"id": 1, "name": "string"},
            "name": "string",
            "model": "string",
            "sku": "string",
            "orderable": true
          },
          "count": 0
        }
      ],
      "orderKits": [
        {
          "kit": {
            "id": 0,
            "name": "string",
            "archived": true,
            "kitItems": [
              {
                "item": {
                  "id": 1,
                  "manufacturer": {"id": 1, "name": "string"},
                  "name": "string",
                  "model": "string",
                  "sku": "string",
                  "orderable": true
                },
                "count": 0
              }
            ]
          },
          "count": 0
        }
      ],
      "orderEvents": [{"name": "string", "eventTimestamp": "string"}],
      "orderNotes": [
        {"note": "string", "createdAt": "string", "createdBy": {"id": 0, "firstName": "string", "lastName": "string", "email": "string"}}
      ],
      "trackingNumbers": [{"carrier": "string", "value": "string"}],
      "devices": [
        {
          "id": 1,
          "item": {
            "id": 1,
            "manufacturer": {"id": 1, "name": "string"},
            "name": "string",
            "model": "string",
            "sku": "string",
            "orderable": true
          },
          "currentStatus": "string",
          "used": true,
          "currentPatient": {
            "id": 1,
            "externalIdentifier": "1234abcd",
            "firstName": "John",
            "lastName": "Doe",
            "dateOfBirth": "1980-01-01",
            "email": "john.doe@example.com",
            "phoneNumber": "+1234567890",
            "archived": true,
            "address": {
              "lineOne": "string",
              "lineTwo": "string",
              "lineThree": "string",
              "city": "string",
              "state": "string",
              "zipCode": "string",
              "country": "string",
              "note": "string"
            },
            "site": {
              "id": 1,
              "name": "string",
              "active": true,
              "phoneNumber": "string",
              "address": {
                "lineOne": "string",
                "lineTwo": "string",
                "lineThree": "string",
                "city": "string",
                "state": "string",
                "zipCode": "string",
                "country": "string",
                "note": "string"
              }
            },
            "enrolled": true,
            "sex": "other"
          },
          "site": {
            "id": 1,
            "name": "string",
            "active": true,
            "phoneNumber": "string",
            "address": {
              "lineOne": "string",
              "lineTwo": "string",
              "lineThree": "string",
              "city": "string",
              "state": "string",
              "zipCode": "string",
              "country": "string",
              "note": "string"
            }
          },
          "lastHealthCheck": "string",
          "deviceIdentifiers": [{"type": "string", "value": "string"}],
          "deviceEvents": [{"type": "string", "eventTimestamp": "string"}],
          "externalIdentifier": "string",
          "disabledReadings": true
        }
      ],
      "packedKits": [
        {
          "kit": {
            "id": 0,
            "name": "string",
            "archived": true,
            "kitItems": [
              {
                "item": {
                  "id": 1,
                  "manufacturer": {"id": 1, "name": "string"},
                  "name": "string",
                  "model": "string",
                  "sku": "string",
                  "orderable": true
                },
                "count": 0
              }
            ]
          },
          "devices": [
            {
              "id": 1,
              "item": {
                "id": 1,
                "manufacturer": {"id": 1, "name": "string"},
                "name": "string",
                "model": "string",
                "sku": "string",
                "orderable": true
              },
              "currentStatus": "string",
              "used": true,
              "currentPatient": {
                "id": 1,
                "externalIdentifier": "1234abcd",
                "firstName": "John",
                "lastName": "Doe",
                "dateOfBirth": "1980-01-01",
                "email": "john.doe@example.com",
                "phoneNumber": "+1234567890",
                "archived": true,
                "address": {
                  "lineOne": "string",
                  "lineTwo": "string",
                  "lineThree": "string",
                  "city": "string",
                  "state": "string",
                  "zipCode": "string",
                  "country": "string",
                  "note": "string"
                },
                "site": {
                  "id": 1,
                  "name": "string",
                  "active": true,
                  "phoneNumber": "string",
                  "address": {
                    "lineOne": "string",
                    "lineTwo": "string",
                    "lineThree": "string",
                    "city": "string",
                    "state": "string",
                    "zipCode": "string",
                    "country": "string",
                    "note": "string"
                  }
                },
                "enrolled": true,
                "sex": "other"
              },
              "site": {
                "id": 1,
                "name": "string",
                "active": true,
                "phoneNumber": "string",
                "address": {
                  "lineOne": "string",
                  "lineTwo": "string",
                  "lineThree": "string",
                  "city": "string",
                  "state": "string",
                  "zipCode": "string",
                  "country": "string",
                  "note": "string"
                }
              },
              "lastHealthCheck": "string",
              "deviceIdentifiers": [{"type": "string", "value": "string"}],
              "deviceEvents": [{"type": "string", "eventTimestamp": "string"}],
              "externalIdentifier": "string",
              "disabledReadings": true
            }
          ]
        }
      ],
      "shippingOption": "standard"
    }
  ],
  "page": 0,
  "size": 0,
  "total": 0,
  "first": true,
  "last": true
}

```

## Fetching a Single Order

For detailed analysis or management of a specific order, the system provides an endpoint to fetch the details of a single order using its unique identifier.

```bash
curl --request GET \
--url https://example.com/api/v3/order/0 \
--header 'Accept: application/json'
```

Response:

```json
{
  "id": 0,
  "externalOrderIds": ["string"],
  "patient": {
    "id": 1,
    "externalIdentifier": "1234abcd",
    "firstName": "John",
    "lastName": "Doe",
    "dateOfBirth": "1980-01-01",
    "email": "john.doe@example.com",
    "phoneNumber": "+1234567890",
    "archived": true,
    "address": {
      "lineOne": "string",
      "lineTwo": "string",
      "lineThree": "string",
      "city": "string",
      "state": "string",
      "zipCode": "string",
      "country": "string",
      "note": "string"
    },
    "site": {
      "id": 1,
      "name": "string",
      "active": true,
      "phoneNumber": "string",
      "address": {
        "lineOne": "string",
        "lineTwo": "string",
        "lineThree": "string",
        "city": "string",
        "state": "string",
        "zipCode": "string",
        "country": "string",
        "note": "string"
      }
    },
    "enrolled": true,
    "sex": "other"
  },
  "site": {
    "id": 1,
    "name": "string",
    "active": true,
    "phoneNumber": "string",
    "address": {
      "lineOne": "string",
      "lineTwo": "string",
      "lineThree": "string",
      "city": "string",
      "state": "string",
      "zipCode": "string",
      "country": "string",
      "note": "string"
    }
  },
  "currentStatus": "availableForPickup",
  "orderItems": [
    {
      "item": {
        "id": 1,
        "manufacturer": {"id": 1, "name": "string"},
        "name": "string",
        "model": "string",
        "sku": "string",
        "orderable": true
      },
      "count": 0
    }
  ],
  "orderKits": [
    {
      "kit": {
        "id": 0,
        "name": "string",
        "archived": true,
        "kitItems": [
          {
            "item": {
              "id": 1,
              "manufacturer": {"id": 1, "name": "string"},
              "name": "string",
              "model": "string",
              "sku": "string",
              "orderable": true
            },
            "count": 0
          }
        ]
      },
      "count": 0
    }
  ],
  "orderEvents": [{"name": "string", "eventTimestamp": "string"}],
  "orderNotes": [
    {"note": "string", "createdAt": "string", "createdBy": {"id": 0, "firstName": "string", "lastName": "string", "email": "string"}}
  ],
  "trackingNumbers": [{"carrier": "string", "value": "string"}],
  "devices": [
    {
      "id": 1,
      "item": {
        "id": 1,
        "manufacturer": {"id": 1, "name": "string"},
        "name": "string",
        "model": "string",
        "sku": "string",
        "orderable": true
      },
      "currentStatus": "string",
      "used": true,
      "currentPatient": {
        "id": 1,
        "externalIdentifier": "1234abcd",
        "firstName": "John",
        "lastName": "Doe",
        "dateOfBirth": "1980-01-01",
        "email": "john.doe@example.com",
        "phoneNumber": "+1234567890",
        "archived": true,
        "address": {
          "lineOne": "string",
          "lineTwo": "string",
          "lineThree": "string",
          "city": "string",
          "state": "string",
          "zipCode": "string",
          "country": "string",
          "note": "string"
        },
        "site": {
          "id": 1,
          "name": "string",
          "active": true,
          "phoneNumber": "string",
          "address": {
            "lineOne": "string",
            "lineTwo": "string",
            "lineThree": "string",
            "city": "string",
            "state": "string",
            "zipCode": "string",
            "country": "string",
            "note": "string"
          }
        },
        "enrolled": true,
        "sex": "other"
      },
      "site": {
        "id": 1,
        "name": "string",
        "active": true,
        "phoneNumber": "string",
        "address": {
          "lineOne": "string",
          "lineTwo": "string",
          "lineThree": "string",
          "city": "string",
          "state": "string",
          "zipCode": "string",
          "country": "string",
          "note": "string"
        }
      },
      "lastHealthCheck": "string",
      "deviceIdentifiers": [{"type": "string", "value": "string"}],
      "deviceEvents": [{"type": "string", "eventTimestamp": "string"}],
      "externalIdentifier": "string",
      "disabledReadings": true
    }
  ],
  "packedKits": [
    {
      "kit": {
        "id": 0,
        "name": "string",
        "archived": true,
        "kitItems": [
          {
            "item": {
              "id": 1,
              "manufacturer": {"id": 1, "name": "string"},
              "name": "string",
              "model": "string",
              "sku": "string",
              "orderable": true
            },
            "count": 0
          }
        ]
      },
      "devices": [
        {
          "id": 1,
          "item": {
            "id": 1,
            "manufacturer": {"id": 1, "name": "string"},
            "name": "string",
            "model": "string",
            "sku": "string",
            "orderable": true
          },
          "currentStatus": "string",
          "used": true,
          "currentPatient": {
            "id": 1,
            "externalIdentifier": "1234abcd",
            "firstName": "John",
            "lastName": "Doe",
            "dateOfBirth": "1980-01-01",
            "email": "john.doe@example.com",
            "phoneNumber": "+1234567890",
            "archived": true,
            "address": {
              "lineOne": "string",
              "lineTwo": "string",
              "lineThree": "string",
              "city": "string",
              "state": "string",
              "zipCode": "string",
              "country": "string",
              "note": "string"
            },
            "site": {
              "id": 1,
              "name": "string",
              "active": true,
              "phoneNumber": "string",
              "address": {
                "lineOne": "string",
                "lineTwo": "string",
                "lineThree": "string",
                "city": "string",
                "state": "string",
                "zipCode": "string",
                "country": "string",
                "note": "string"
              }
            },
            "enrolled": true,
            "sex": "other"
          },
          "site": {
            "id": 1,
            "name": "string",
            "active": true,
            "phoneNumber": "string",
            "address": {
              "lineOne": "string",
              "lineTwo": "string",
              "lineThree": "string",
              "city": "string",
              "state": "string",
              "zipCode": "string",
              "country": "string",
              "note": "string"
            }
          },
          "lastHealthCheck": "string",
          "deviceIdentifiers": [{"type": "string", "value": "string"}],
          "deviceEvents": [{"type": "string", "eventTimestamp": "string"}],
          "externalIdentifier": "string",
          "disabledReadings": true
        }
      ]
    }
  ],
  "shippingOption": "standard"
}

```

## More

- [Create Order API Reference](/api-reference/orders/create-order)
- [Download OpenAPI YAML](javascript:window.print())
