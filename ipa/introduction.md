

# Netzilo REST API

Use the Netzilo Public API to manage users, peers, network rules and more from inside your application or scripts to automate the setup of your mesh network. {{ className: 'lead' }}

<div className="not-prose mb-16 mt-6 flex gap-3">
  <Button href="/api/guides/quickstart" arrow="right" children="Quickstart" />
</div>

## Getting started {{ anchor: false }}

To get started, it is recommended to create a [service user](/how-to/access-Netzilo-public-api#creating-a-service-user), that can later be used to communicate with the Netzilo API.
To be able to send requests to our API you need to [authenticate](/api/guides/authentication) on each request. This can be done either by Bearer token from your identity provider or by creating a [personal access token](/api/guides/authentication#using-personal-access-tokens) in the Netzilo dashboard.{{ className: 'lead' }}

<div className="not-prose">
  <Button
    href="/how-to/access-Netzilo-public-api#creating-an-access-token"
    variant="text"
    arrow="right"
    children="Get your personal access token"
  />
</div>

<Guides />

<Resources />
