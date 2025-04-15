This guide is designed to help you understand how patients can be created, updated, and managed, and how to utilize webhooks and external identifiers for effective patient tracking.

### Understanding Patients

Before diving into patient management, it's important to understand the primary entity associated with patient operations:

- **Patient**: A patient profile is stored within Impilo and is managed by the customer. These profiles are essential for personalizing and managing patient-related activities and services.

### Creating a Patient

To create a patient, you'll interact with a specific endpoint designed for this purpose. This endpoint allows you to initiate and configure patient profiles based on your requirements.

### Required Attributes

For a successful patient creation, you must ensure the following required attributes are correctly provided:

- **Patient Information**: Basic information about the patient such as `name`, `dateOfBirth`, and `gender` must be included.

### Create Patient Example

The following request contains the bare minimum for creating a patient:


```bash
curl --request POST \
--url https://example.com/api/v3/patient \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--data '{
  "id" : 456,
  "externalIdentifier" : "external-identifier-123",
  "firstName" : "John",
  "lastName" : "Doe",
  "dateOfBirth" : "1987-09-13",
  "email" : "email@example.com",
  "phoneNumber" : "111-222-3333",
  "address" : {
    "lineOne" : "1234 Market Street",
    "lineTwo" : "Suite 110",
    "city" : "Philadelphia",
    "state" : "PA",
    "zipCode" : "19137",
    "country" : "USA"
  },
  "sex" : "unknown"
}'
```

### Updating a Patient

To update a patient, you'll use a specific endpoint that allows modifying existing patient profiles. This can be useful for updating patient information as required.

### Update Patient Example

The following request contains the minimum required for updating a patient:

```bash
curl --request PUT \
--url https://example.com/api/v3/patient/0 \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--data '{
  "externalIdentifier" : "1234abcd",
  "firstName" : "John",
  "lastName" : "Doe",
  "dateOfBirth" : "1980-01-01",
  "email" : "john.doe@example.com",
  "phoneNumber" : "+1234567890",
  "archived" : true,
  "address" : {
    "lineOne" : "string",
    "lineTwo" : "string",
    "lineThree" : "string",
    "city" : "string",
    "state" : "string",
    "zipCode" : "string",
    "country" : "string",
    "note" : "string"
  },
  "site" : {
    "name" : "string",
    "active" : true,
    "phoneNumber" : "string",
    "address" : {
      "lineOne" : "string",
      "lineTwo" : "string",
      "lineThree" : "string",
      "city" : "string",
      "state" : "string",
      "zipCode" : "string",
      "country" : "string",
      "note" : "string"
    }
  },
  "enrolled" : true,
  "sex" : "other"
}'
```

### Using Webhooks for Patient Updates

Impilo supports webhooks to notify your system when a patient is updated. This allows you to keep your system in sync with any changes in patient data. For more information, see the [webhook management guide](/guides/webhook-management).

### Setting Up a Webhook

To set up a webhook, register a URL endpoint in your system that can receive POST requests from Impilo.

```bash
POST /api/v3/webhook
curl -X POST http://app.impiloplatform.com/api/v3/webhook \
-H "Content-Type: application/json" \
-d '{"type":"patient.updated","url":"https://example.com/webhook"}'
```

### Webhook Example

When a patient is updated, a POST request will be sent to your registered webhook URL with the following payload:

```http
POST /your-webhook-url
Content-Type: application/json

{
  "event": "patient.updated",
  "data": {
    "id": "12345",
    "name": "John Doe",
    "dateOfBirth": "1990-01-01",
    "gender": "male",
    "updatedAt": "2024-05-21T16:00:00Z"
  }
}
```

### Using External Identifiers

To effectively track individual patients using customer-specific identifiers, the external identifiers field can be used. This can be particularly useful for ensuring uniqueness and consistency in a way that matches external systems.

### Example of Generating an External Identifier

For example purposes, a customer system may use email addresses and phone numbers as a way to track patients. The external identifier field could hold a hash of these two values:

#### Java

```java
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;

public class HashGenerator {

    public static void main(String[] args) {
        String email = "example@example.com";
        String phone = "(123) 456-7890";
        String hash = generateMD5Hash(email, phone);
        System.out.println("MD5 Hash: " + hash);
    }

    public static String generateMD5Hash(String email, String phone) {
        try {
            phone = phone.replaceAll("\\D", "");
            String input = email.toLowerCase() + phone;
            MessageDigest md = MessageDigest.getInstance("MD5");
            byte[] hashBytes = md.digest(input.getBytes());
            StringBuilder hexString = new StringBuilder();
            for (byte b : hashBytes) {
                hexString.append(String.format("%02x", b));
            }
            return hexString.toString();
        } catch (NoSuchAlgorithmException e) {
            throw new RuntimeException(e);
        }
    }
}
```

#### Python

```python
import re
import hashlib

def generate_md5_hash(email, phone):
    phone = re.sub(r'\D', '', phone)
    input_string = email.lower() + phone
    hash_object = hashlib.md5(input_string.encode())
    return hash_object.hexdigest()

email = "example@example.com"
phone = "(123) 456-7890"
hash_value = generate_md5_hash(email, phone)
print(f"MD5 Hash: {hash_value}")
```

#### JavaScript

```javascript
const crypto = require('crypto');

function generateMD5Hash(email, phone) {
    phone = phone.replace(/\D/g, '');
    const input = email.toLowerCase() + phone;
    return crypto.createHash('md5').update(input).digest('hex');
}

const email = "example@example.com";
const phone = "(123) 456-7890";
const hash = generateMD5Hash(email, phone);
console.log(`MD5 Hash: ${hash}`);
```

### Using the Hash as an Identifier

You can use the generated hash as an external identifier in your patient profiles to uniquely identify and track patients within Impilo.

### More

- [Create Patient API Reference](/api-reference/patients/create-patient)
