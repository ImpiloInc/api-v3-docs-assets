This guide outlines the process for authenticating requests to the Impilo API, which is necessary for accessing any of the available endpoints. Authentication ensures that API interactions are authorized and secure.

#### Introduction to Authentication

Accessing the Impilo API requires an API key, which serves as the method for authenticating requests. This section will cover how to obtain and use your API key effectively.

#### Acquiring and Utilizing Your API Key

1. **API Key Management:** The API key is provided through your account portal on the Impilo platform. This key is essential for authenticating requests to the API.

2. **Authenticating Requests:** To authenticate a request, include the API key in the request header named '**Impilo-API-Key**'. The following cURL command demonstrates how to send an authenticated request:

    ```bash
    curl https://app.impiloplatform.com/api/v3/patient \
      -H "Impilo-API-Key: your-api-key-here"
    ```

3. **API Key Security:** Upon creation, the API key is displayed fully only once. It's crucial to copy and securely store this key immediately. The portal provides visibility into the API key's prefix and expiration date. If necessary, an API key can be deactivated through the portal.

#### Best Practices

Ensure the secure handling of your API key to maintain the integrity of your API interactions. Regularly review and manage your API keys through the account portal to prevent unauthorized access.
