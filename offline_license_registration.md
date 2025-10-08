# ReN3 Offline License Registration Process

This document provides step-by-step instructions to enable and register an **offline license** for the ReN3 ingestor service.

---

## 1. Request a License Key

Before starting the configuration, request a **license key** from the **ReN3 team**. This key uniquely identifies your deployment and is required in subsequent steps.

> **Note:** Keep the license key secure, as it is essential for registration and activation.

---

## 2. Configure Offline License Settings

Update the ingestor environment configuration with the following parameters:

```bash
enable_offline_license=true
client_device_id=xxxxx
```

> **Notes:**
> - The `client_device_id` will be **provided by the ReN3 team**.
> - Both values must be configured in the ingestor environment before performing license registration.

---

## 3. Generate License Request

Run the following API call to generate a license request, which must be sent to the ReN3 team for offline license creation.

```bash
curl --location 'http://localhost:5000/license/register' \
--header 'Content-Type: application/json' \
--data '{
  "licenseKey": "LICENSE_KEY",
  "export": true
}'
```

> **Notes:**
>
> * Replace `LICENSE_KEY` with the license key obtained from the ReN3 team.
> * The `"export": true` flag ensures the request is generated for offline validation.
> * The response will contain the license request that must be shared with the ReN3 team.

---

## 4. Obtain the License Payload

Upon receiving the license request, the **ReN3 team** will produce a **license payload**, which will be used to finalize the licensing process in your environment.

---

## 5. Import the License Payload

Once the license payload is received, import it into the system using the following API:

```bash
curl --location 'http://localhost:5000/license/import' \
--header 'Content-Type: application/json' \
--data '{
  "licenseKey": "LICENSE_KEY",
  "licensePayload": "LICENSE_PAYLOAD"
}'
```

**Response:**
A successful response confirms that the offline license has been imported and activated for use.

---