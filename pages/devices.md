This guide is designed to help you understand how devices are managed within the Impilo platform, including how they are assigned to patients or sites, updated, disassociated, or replaced. It also covers the workflows involved in device association, timezone tracking, and status updates.

### Device Assignment and Status Updates

When an order is processed, devices are typically assigned either to a patient or a site. This assignment triggers a status update for the order, which can be observed through the Impilo webhook workflows. For more information see the [Webhook management guide](/guides/webhook-management) and the [order.Status](/resources/models#webhooktype_enumerated_value_order.statusfull) event type.

### Manual Device Assignment

Apart from automatic assignments through order processing, devices can also be manually assigned to patients. This is done using the [Associate Device endpoint](/api-reference/devices/associate-device). Note that in order to associate a device with a patient, all of the following must apply:

- The Patient must be associated with the Customer making the request
- The Device must be owned by the Customer making the request
- The Device must be associated with a non-Impilo site
- The Device must have a current status of `availableAtSite`

### Device Management Scenarios

**Multiple Devices**: There is no separate process required for ordering or assigning additional devices to a patient. Impilo’s system supports associating multiple devices with a single patient profile seamlessly.

**Device Disassociation**: It's always possible to disassociate a device from a patient, allowing for flexible management depending on the use case requirements.

**Device Replacement**: In cases where a device is defective, Impilo facilitates a smooth replacement process. The defective device is returned to an Impilo warehouse, and a new device is simultaneously dispatched to ensure minimal disruption in patient care.

### Device Association Workflow

Here’s a walkthrough of the typical device association steps in the Impilo system:

1. **Device Lookup**: Use the device lookup endpoint to find specific devices.  
```bash
curl --request GET \
--url https://example.com/api/v3/device/0 \
--header 'Accept: application/json'
```

2. **Patient or Site Lookup**: Lookup the patient or site using their respective endpoints to ensure the device is associated correctly.  
```bash
curl --request GET \
--url https://example.com/api/v3/patient/0 \
--header 'Accept: application/json'
```

3. **Device Association Request**: Finally, issue a device association request to link the device with the chosen patient or site.  
```bash
curl --request PATCH \
--url https://example.com/api/v3/device/associate-patient \
--header 'Content-Type: application/json' \
--data '{"deviceId": 1, "patientExternalIdentifier": "external-identifier"}'
```

### Updating Device Details

To update a device, the following endpoint is utilized:  


A DeviceUpdate object is used to specify the changes. This object includes an override status and an optional note. Below is an example of how to format the request body to update a device's status and include a note about the update.

For example, to block a device from submitting readings to the Impilo platform, a `decommissioned` status could be set on a specific device.

### Device Readings

**Device Timezones:**

In the Impilo system, certain devices transmit timezone information, which is included as part of the reading. Specifically, the `deviceTimeZoneOffset` and `patientTimeZoneOffset` fields are used to track the timezone offsets for the device and the patient, respectively.

- `deviceTimeZoneOffset`: This field represents the timezone offset of the device at the time the reading was taken.
- `patientTimeZoneOffset`: This field represents the timezone offset calculated based on the patient's zip code, if available.

Not all devices transmit timezone information. However, when this information is available, it is forwarded along with the reading to ensure accurate time tracking and data analysis.

### Device Status Update Workflow

#### Fetch a Device by ID

Send a request to the Device endpoint:  
```bash
curl --request GET \
--url https://example.com/api/v3/device/0 \
--header 'Accept: application/json'
```

Truncated response:

```json
{
  "id": 1,
  "currentStatus": "available"
}
```

#### Send a Device Update Request

Send a device update request to change the status of the device:  
```bash
curl --request PATCH \
--url https://example.com/api/v3/device/disable-readings \
--header 'Content-Type: application/json' \
--data '{"deviceId": 1, "deviceIdentifier": "ABC123"}'
```

#### Fetch the Updated Device by ID

Send another request to the Device endpoint and observe the change:  
```bash
curl --request GET \
--url https://example.com/api/v3/device/0 \
--header 'Accept: application/json'
```

Truncated response:

```json
{
  "id": 1,
  "currentStatus": "decommissioned"
}
```

### More

- [Device API Endpoints](/api-reference/devices)
- [Download OpenAPI YAML](javascript:window.print())
