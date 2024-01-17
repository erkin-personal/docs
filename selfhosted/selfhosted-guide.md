# Advanced guide

Netzilo is open-source and can be self-hosted on your servers.

It relies on components developed by Netzilo Authors [Management Service](https://github.com/Netziloio/Netzilo/tree/main/management), [Management UI Dashboard](https://github.com/Netziloio/dashboard), [Signal Service](https://github.com/Netziloio/Netzilo/tree/main/signal),
a 3rd party open-source STUN/TURN service [Coturn](https://github.com/coturn/coturn), and an identity provider (IDP).

If you would like to learn more about the architecture please refer to the [architecture section](/about-Netzilo/how-Netzilo-works).

<Note>
    It might be a good idea to try Netzilo before self-hosting on your servers.
    We run Netzilo in the cloud, and it will take a few clicks to get started with our managed version. [Check it out!](https://Netzilo.io/pricing)
</Note>

If you are looking for a quick way of trying self-hosted Netzilo, please refer to [this guide](/about-Netzilo/how-Netzilo-works). Otherwise, continue here to set up
Netzilo with custom IdPs.

## Advanced self-hosting guide with a custom identity provider

### Requirements

- Virtual machine offered by any cloud provider (e.g., AWS, DigitalOcean, Hetzner, Google Cloud, Azure ...).
- Any Linux OS.
- Docker Compose installed (see [Install Docker Compose](https://docs.docker.com/compose/install/)).
- Domain name pointing to the public IP address of your server.
- Open TCP ports ```80, 443, 33073, 10000``` (Dashboard HTTP & HTTPS, Management gRCP & HTTP APIs, Signal gRPC API respectively) on your server.
- Coturn is used for relay using the STUN/TURN protocols. It requires a listening port, `UDP 3478`, and range of ports, `UDP 49152-65535`, for dynamic relay connections. These are set as defaults in setup file, but can be configured to your requirements.
- Maybe a cup of coffee or tea :)

For this tutorial we will be using domain ```demo.Netzilo.io``` which points to our Ubuntu 22.04 machine hosted at Hetzner.

### Step 1: Get the latest stable Netzilo code

```bash
#!/bin/bash
REPO="https://github.com/Netziloio/Netzilo/"
# this command will fetch the latest release e.g. v0.8.7
LATEST_TAG=$(basename $(curl -fs -o/dev/null -w %{redirect_url} ${REPO}releases/latest))
echo $LATEST_TAG

# this comman will clone the latest tag
git clone --depth 1 --branch $LATEST_TAG $REPO
```

Then switch to the infra folder that contains docker-compose file:

```bash
cd Netzilo/infrastructure_files/
```
### Step 2:  Prepare configuration files

To simplify the setup we have prepared a script to substitute required properties in the [docker-compose.yml.tmpl](https://github.com/Netziloio/Netzilo/tree/main/infrastructure_files/docker-compose.yml.tmpl) and [management.json.tmpl](https://github.com/Netziloio/Netzilo/tree/main/infrastructure_files/management.json.tmpl) files.

The [setup.env.example](https://github.com/Netziloio/Netzilo/tree/main/infrastructure_files/setup.env.example) file contains multiple properties that have to be filled. You need to copy the example file to `setup.env` before updating it.

```bash
## example file, you can copy this file to setup.env and update its values
##
# Dashboard domain. e.g. app.mydomain.com
Netzilo_DOMAIN=""
# OIDC configuration e.g., https://example.eu.auth0.com/.well-known/openid-configuration
Netzilo_AUTH_OIDC_CONFIGURATION_ENDPOINT=""
Netzilo_AUTH_AUDIENCE=""
# e.g. Netzilo-client
Netzilo_AUTH_CLIENT_ID=""
# indicates whether to use Auth0 or not: true or false
Netzilo_USE_AUTH0="false"
Netzilo_AUTH_DEVICE_AUTH_PROVIDER="none"
# enables Interactive SSO Login feature (Oauth 2.0 Device Authorization Flow)
Netzilo_AUTH_DEVICE_AUTH_CLIENT_ID=""
# e.g. hello@mydomain.com
Netzilo_LETSENCRYPT_EMAIL=""
```

- Set ```Netzilo_DOMAIN``` to your domain, e.g.  `demo.Netzilo.io`
- Configure ```Netzilo_LETSENCRYPT_EMAIL``` property.
This can be any email address. [Let's Encrypt](https://letsencrypt.org/) will create an account while generating a new certificate.

<Note>
    Let's Encrypt will notify you via this email when certificates are about to expire. Netzilo supports automatic renewal by default.
</Note>

<Note>
    If you want to setup Netzilo with your own reverse-Proxy and without using the integrated letsencrypt, follow [this step here instead](#advanced-running-Netzilo-behind-an-existing-reverse-proxy).
</Note>

### Step 3: Configure Identity Provider (IDP)

Netzilo supports generic OpenID (OIDC) protocol allowing integration with any IDP following the specification.

Netzilo's management service integrates with some of the most popular IDP APIs, allowing the service to cache and display user names and email addresses without storing sensitive data.

Pick the one that suits your needs, follow the steps, and continue with this guide:

**Self-hosted options**
- Continue with [Zitadel](/selfhosted/identity-providers#zitadel).
- Continue with [Keycloak](/selfhosted/identity-providers#keycloak).
- Continue with [Authentik](/selfhosted/identity-providers#authentik).

**Managed options**
- Continue with [Azure AD](/selfhosted/identity-providers#azure-ad).
- Continue with [Google Workspace](/selfhosted/identity-providers#google-workspace).
- Continue with [Okta](/selfhosted/identity-providers#okta).
- Continue with [Auth0](/selfhosted/identity-providers#auth0).

### Step 4: Disable single account mode (optional)

Netzilo Management service runs in a single account mode by default since version v0.10.1.
Management service was creating a separate account for each registered user before v0.10.1.
Single account mode ensures that all the users signing up for your self-hosted installation will join the same account/network.
In most cases, this is the desired behavior.

If you want to disable the single-account mode, set `--disable-single-account-mode` flag in the
[docker-compose.yml.tmpl](https://github.com/Netziloio/Netzilo/tree/main/infrastructure_files/docker-compose.yml.tmpl)
`command` section of the `management` service.

### Step 5: Run configuration script
Make sure all the required properties set in the ```setup.env``` file and run:

```bash
./configure.sh
```

This will export all the properties as environment variables and generate ```artifacts/docker-compose.yml```, ```artifacts/management.json``` and ```artifacts/turnserver.cong``` files substituting required variables.

### Step 6: Run docker compose:

```bash
cd artifacts
docker-compose up -d
```
### Step 7: Check docker logs (Optional)

```bash
cd artifacts
docker-compose logs signal
docker-compose logs management
docker-compose logs coturn
docker-compose logs dashboard
```

## Advanced: Running Netzilo behind an existing reverse-proxy

If you want to run Netzilo behind your own reverse-proxy, some additional configuration-steps have to be taken to [Step 2](#step-2--prepare-configuration-files).

<Note>
    Not all reverse-proxies are supported as Netzilo uses *gRPC* for various components.
</Note>

### Configuration for Netzilo

In `setup.env`:
- Set ```Netzilo_DOMAIN``` to your domain, e.g.  `demo.Netzilo.io`
- Set ```Netzilo_DISABLE_LETSENCRYPT=true```
- Add ```Netzilo_MGMT_API_PORT``` to your reverse-proxy TLS-port (default: 443)
- Add ```Netzilo_SIGNAL_PORT``` to your reverse-proxy TLS-port

Optional:
- Add ```TURN_MIN_PORT``` and ```TURN_MAX_PORT``` to configure the port-range used by the Turn-server

<Note>
    The `coturn`-service still needs to be directly accessible under your set-domain as it uses UDP for communication.
</Note>

Now you can continue with [Step 3](#step-3-configure-identity-provider).

### Configuration for your reverse-proxy

Depending on your port-mappings and choice of reverse-proxy, how you configure the forwards differs greatly.

The following endpoints have to be setup:

Endpoint                        | Protocol  | Target service and internal-port
------------------------------- | --------- | --------------------------------
/                               | HTTP      | dashboard:80
/signalexchange.SignalExchange/ | gRPC      | signal:80
/api                            | HTTP      | management:443
/management.ManagementService/  | gRPC      | management:443

Make sure your reverse-Proxy is setup to use the HTTP2-Protocol when forwarding.

<Note>
    You can find helpful templates with the reverse-proxy-name as suffix (e.g. `docker-compose.yml.tmpl.traefik`)
    Simply replace the file `docker-compose.yml.tmpl` with the chosen version.
</Note>

## Backup
To backup your Netzilo installation, you need to copy the configuration files, and the Management service databases.

The configuration files are located in the folder where you ran the installation script. To backup, copy the files to a backup location:
```bash
cd Netzilo/infrastructure_files/artifacts/
mkdir backup
cp docker-compose.yml turnserver.conf management.json backup/
```
To save the Management service databases, you need to stop the Management service and copy the files from the store directory using a docker compose command as follows:
```bash
docker compose stop management
docker compose cp -a management:/var/lib/Netzilo/ backup/
docker compose start management
```

## Upgrade
To upgrade Netzilo to the latest version, you need to review the [release notes](https://github.com/Netziloio/Netzilo/releases) for any breaking changes and follow the upgrade steps below:
1. Run the backup steps described in the [backup](#backup-net-bird) section.
2. Pull the latest Netzilo docker images:
   ```bash
   cd Netzilo/infrastructure_files/artifacts/
   docker compose pull
    ```
3. Restart the Netzilo containers with the new images:
   ```bash
   docker compose up -d --force-recreate
    ```

## Get in touch

Feel free to ping us on [Slack](https://join.slack.com/t/Netziloio/shared_invite/zt-vrahf41g-ik1v7fV8du6t0RwxSrJ96A) if you have any questions

- Netzilo managed version: [https://app.Netzilo.io](https://app.Netzilo.io)
- Make sure to [star us on GitHub](https://github.com/Netziloio/Netzilo)
- Follow us [on Twitter](https://twitter.com/Netzilo)
