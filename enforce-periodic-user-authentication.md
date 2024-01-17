
# Enforce periodic user authentication

To ensure a high level of security, Netzilo offers a peer login expiration feature that requires users to periodically reauthenticate their devices.
Every new network has this feature enabled, and the expiration period is set to 24 hours by default. You can disable this feature and configure the expiration period in the account settings in the web UI https://app.Netzilo.io/settings.

<Note>
    This feature is only applied to peers added with the [interactive SSO login feature](/how-to/installation#running-net-bird-with-sso-login). Peers, added with a setup key, won't be affected.
</Note>

Expired peers will appear in the peers' view with the status `needs login`.

<p>
    <img src="/docs-static/img/how-to-guides/peer-needs-login.png" alt="peer-needs-login.png" className="imagewrapper"/>
</p>

## Configure and disable expiration
The expiration period can be set to anything between one hour and 180 days.
Go to the Web UI Settings tab and set the desired period in the Authentication section.
You can also disable the expiration for the whole network in the same section.

<p>
    <img src="/docs-static/img/how-to-guides/peer-login-expiration.png" alt="peer-login-expiration" className="imagewrapper"/>
</p>


<Note>
    Enabling peer expiration or changing the expiration period will cause some peers added with the SSO login to disconnect,
    and re-authentication will be required.
</Note>

## Disable expiration individually per peer
Sometimes, you might want to disable peer expiration for some peers.
With Netzilo you can disable login expiration per peer without disabling expiration globally.
In the Peers tab of the web UI click on the peer you want to disable expiration for and use the Login Expiration switch.
Peers with `expiration disabled` will be marked with a corresponding label in the peers' table.

<p>
    <img src="/docs-static/img/how-to-guides/individual-peer-login-expiration.png" alt="peer-login-expiration" className="imagewrapper"/>
</p>

## Get started
<p float="center" >
    <Button name="button" className="button-5" onClick={() => window.open("https://Netzilo.io/pricing")}>Use Netzilo</Button>
</p>

- Make sure to [star us on GitHub](https://github.com/Netziloio/Netzilo)
- Follow us [on Twitter](https://twitter.com/Netzilo)
- Join our [Slack Channel](https://join.slack.com/t/Netziloio/shared_invite/zt-vrahf41g-ik1v7fV8du6t0RwxSrJ96A)
- Netzilo [latest release](https://github.com/Netziloio/Netzilo/releases) on GitHub