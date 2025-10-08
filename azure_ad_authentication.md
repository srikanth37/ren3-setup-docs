# Azure AD Authentication Configuration

To enable Azure Active Directory (Azure AD) Single Sign-On (SSO) in ReN3, configure the following flags in your server service:

| Flag Name              | Type    | Description                                                                 |
|------------------------|---------|-----------------------------------------------------------------------------|
| `include_azureadsso`   | boolean | Enable Azure AD Single Sign-On authentication. Set to `true` to activate.   |
| `azuread_clientid`     | string  | The Client ID of your Azure AD registered application.                      |
| `azuread_tenantid`     | string  | The Tenant ID of your Azure AD directory.                                   |
| `azuread_clientsecret` | string  | The Client Secret generated for your Azure AD application.                  |

**Example Configuration:**
```
include_azureadsso=true
azuread_clientid=<your-client-id>
azuread_tenantid=<your-tenant-id>
azuread_clientsecret=<your-client-secret>
```

> **Note:**  
> - The redirect URI must match your portal URL followed by `/signin`.
> - Store the client secret securely and do not share it publicly.

## How to Register an Azure AD Application

Follow these steps to obtain the required values for the configuration above:

1. **Log in to Azure Portal**
   - Visit [Azure Portal](https://portal.azure.com) and sign in.

2. **Register a New Application**
   - Go to **Azure Active Directory** > **App registrations**.
   - Click **New registration**.
   - Enter a name (e.g., "ReN3 Portal").
   - Set the **Redirect URI** to: `<ren3portalurl>/signin`  
     Example: `https://ren3.ai/signin`
   - Click **Register**.

3. **Obtain Required Keys**
   - On the app's overview page:
     - **Client ID**: Copy the "Application (client) ID".
     - **Tenant ID**: Copy the "Directory (tenant) ID".
   - Go to **Certificates & secrets**:
     - Click **New client secret**.
     - Add a description and expiry.
     - Click **Add** and copy the generated secret value.

For more details, see [Azure AD App Registration Documentation](https://learn.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app).






