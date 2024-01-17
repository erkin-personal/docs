---
id: using-Netzilo-with-zitadel
title: Using Netzilo with Zitadel
sidebar_position: 5
tags:
- integrations
- idp
- zitadel
- oidc
- how-to
---

This guide is a part of the [Netzilo Self-hosting Guide](/getting-started/self-hosting) and explains how to integrate 
**self-hosted** Netzilo with [Zitadel](https://zitadel.com).

:::tip managed idp
If you prefer not to self-host an Identity and Access Management solution, then you could use a managed alternative like
[Auth0](/integrations/identity-providers/self-hosted/using-Netzilo-with-auth0).
:::

### 1. Create and configure Zitadel application
In this step, we will create and configure Netzilo application in zitadel.

Create new zitadel project
- Navigate to zitadel console
- Click `Projects` at the top menu, then click `Create New Project` to create a new project
- Fill in the form with the following values and click `Continue`
  - Name: `Netzilo`

![](/img/integrations/identity-providers/self-hosted/zitadel-new-project.png)

Create new zitadel application
- Click `Projects` in the top menu and select `Netzilo` project from the list
- Click `New` in `APPLICATIONS` section to create a new application
- Fill in the form with the following values and click `Continue`
    - Name: `Netzilo`
    - TYPE OF APPLICATION: `User Agent`

![](/img/integrations/identity-providers/self-hosted/zitadel-new-application.png)

- Fill in the form with the following values and click `Continue`
    - Authentication Method: `PKCE`

![](/img/integrations/identity-providers/self-hosted/zitadel-new-application-auth.png)

- Fill in the form with the following values and click `Continue`
    - Redirect URIs: `https://<domain>/auth` and click `+`
    - Post Logout URIs: `https://<domain>/silent-auth` and click `+`

![](/img/integrations/identity-providers/self-hosted/zitadel-new-application-uri.png)

- Verify applications details and Click `Create` and then click `Close`
- Check `Refresh Token` checkbox and click `Save`

![](/img/integrations/identity-providers/self-hosted/zitadel-new-application-overview.png)

- Copy `Client ID` will be used later in the `setup.env`

### Step 2: Application Token Configuration

To configure `Netzilo` application token you need to:

- Click `Projects` in the top menu and select `Netzilo` project from the list
- Select `Netzilo` application from `APPLICATIONS` section
- Click `Token Settings` in the left menu
- Fill in the form with the following values:
  - Auth Token Type: `JWT`
  - Check `Add user roles to the access token` checkbox
- Click `Save`

![](/img/integrations/identity-providers/self-hosted/zitadel-token-settings.png)

### Step 3: Application Redirect Configuration

:::caution
This step is intended for setup running in development mode with no SSL
:::

To configure `Netzilo` application redirect you need to:

- Click `Projects` in the top menu and select `Netzilo` project from the list
- Select `Netzilo` application from `APPLICATIONS` section
- Click `Redirect Settings` in the left menu
- Fill in the form with the following values:
  - Toggle `Development Mode`
- Click `Save`

![](/img/integrations/identity-providers/self-hosted/zitadel-redirect-settings.png)

### Step 4: Create a Service User

In this step we will create a `Netzilo` service user.

- Click `Users` in the top menu
- Select `Service Users` tab
- Click `New`
- Fill in the form with the following values:
  - User Name: `Netzilo`
  - Name: `Netzilo`
  - Description: `Netzilo Service User`
  - Access Token Type: `JWT`
- Click `Create`

![](/img/integrations/identity-providers/self-hosted/zitadel-create-user.png)

In this step we will generate `ClientSecret` for the `Netzilo` service user.

- Click `Actions` in the top right corner and click `Generate Client Secret`
- Copy `ClientSecret` from the dialog will be used later to set `ClientSecret` in the `management.json`

![](/img/integrations/identity-providers/self-hosted/zitadel-service-user-secret.png)

### Step 5: Grant manage-users role to Netzilo service user

In this step we will grant `Org User Manager` role to `Netzilo` service user.

- Click `Organization` in the top menu
- Click `+` in the top right corner
- Search for `Netzilo` service user
- Check `Org User Manager` checkbox
- Click `Add`

![](/img/integrations/identity-providers/self-hosted/zitadel-service-account-role.png)


Your authority OIDC configuration will be available under:
```
https://<YOUR-ZITADEL-HOST-AND-PORT>/.well-known/openid-configuration
```
:::caution
Double-check if the endpoint returns a JSON response by calling it from your browser.
:::

- Set properties in the `setup.env` file:
```json
Netzilo_AUTH_OIDC_CONFIGURATION_ENDPOINT="https://<YOUR-ZITADEL-HOST-AND-PORT>/.well-known/openid-configuration"
Netzilo_USE_AUTH0=false
Netzilo_AUTH_CLIENT_ID="<Client ID>"
Netzilo_AUTH_AUDIENCE="<Client ID>"
Netzilo_AUTH_DEVICE_AUTH_CLIENT_ID="<Client ID>"
Netzilo_AUTH_REDIRECT_URI="/auth"
Netzilo_AUTH_SILENT_REDIRECT_URI="/silent-auth"
```

- You can now continue with the [Netzilo Self-hosting Guide](/getting-started/self-hosting#step-3-configure-identity-provider).

- Set property `IdpManagerConfig` in the `management.json` file with:
  :::caution
  The file management.json is created automatically. Please refer [here](/getting-started/self-hosting#step-5-run-configuration-script) for more information.
  :::

  ```json
  {
    "ManagerType":  "zitadel",
    "ZitadelClientCredentials": {
        "ClientID": "Netzilo",
        "ClientSecret": "<CLIENT SECRET>",
        "GrantType": "client_credentials",
        "TokenEndpoint": "https://<YOUR-ZITADEL-HOST-AND-PORT>/oauth/v2/token",
        "ManagementEndpoint": "https://<YOUR-ZITADEL-HOST-AND-PORT>/management/v1"
    }
  }
  ```