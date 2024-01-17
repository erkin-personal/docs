export const description =
  'This guide will get you all set up and ready to use the Netzilo API. We’ll cover how to get started and how to make your first API request.'


# Quickstart

This guide will get you all set up and ready to use the Netzilo API. We'll cover how to get started using cURL and how to make your first API request. We'll also look at where to go next to find all the information you need to take full advantage of our powerful REST API. {{ className: 'lead' }}

## Install cURL

For this guide, we'll be using cURL to make our first API request. If you don't already have cURL installed, you can download it from the [cURL website](https://curl.se/download.html).

## Get an access token

Before making your first API request, you need to create an access token to authenticate requests to the API. You can create an access token in the [Netzilo dashboard](https://app.Netzilo.io) under [Users &raquo; Me](https://app.Netzilo.io/users).
After the token was created successfully make sure to store it as we need it for the next step.

## Making your first API request

After creating your access token, you are ready to make your first call to the Netzilo API. Below, you can see how to send a GET request to the peers endpoint to get a list of all peers in your network.

<CodeGroup tag="GET" label="/api/peers">

```bash {{ title: 'cURL' }}
curl -X GET https://api.Netzilo.io/api/peers \
    -H 'Accept: application/json' \
    -H 'Authorization: Token <TOKEN>'
```

</CodeGroup>

<div className="not-prose">
  <Button
    href="/api/resources/peers"
    variant="text"
    arrow="right"
    children="Read the docs for the peers endpoint"
  />
</div>


## What's next?

Great, you're now set up with an API client and have made your first request to the API. Here are a few links that might be handy as you venture further into the Netzilo API:

- [Read how to properly authenticate against the Netzilo API](/api/guides/authentication)
- [Check out the users endpoint](/api/resources/users)
- [Learn about the different error types](/api/guides/errors)
