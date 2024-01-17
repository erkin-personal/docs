
# Monitor system and network activity

The activity monitoring feature lets you quickly see what's happening with your network.
Whether a new machine or user joined your network or the access control policy has been modified, the activity log allows you to track the changes to your network.

## Access activity monitoring view

Activity monitoring is enabled by default for every network, and you can access it in the web UI under the [Activity tab](https://app.netbird.io/activity).
You can also use the search bar to filter events by activity type.

<p>
    <img src="/docs-static/img/how-to-guides/activity-monitoring.png" alt="activity-monitoring" width="800" className="imagewrapper"/>
</p>

<Note>
    The current version of NetBird tracks network changes that occur in the Management server. E.g., changes related to the list of peers, groups, system settings, setup keys, access control, etc.
    The future versions will support connection events that occur in NetBird agents (e.g., peer A connected to peer B).
</Note>

<Note>
    The <b>unknown</b> name or <b>unknown@unknown.com</b> e-mail address.
    In the activity event store, the system keeps the deleted user information encrypted. If the encryption key has been corrupted or lost, then the events returned by the API could show "unknown@unknown.com" for the e-mail address field and "unknown" for the name field.
    If the configuration files have been generated by the <b>configure.sh</b> script, you can find the previous encryption key in the backup files in the same folder as the script. Look for the <b>DataStoreEncryptionKey</b> field in the management.json backup file.
</Note>

## Get started
<p float="center" >
    <Button name="button" className="button-5" onClick={() => window.open("https://netbird.io/pricing")}>Use NetBird</Button>
</p>

- Make sure to [star us on GitHub](https://github.com/netbirdio/netbird)
- Follow us [on Twitter](https://twitter.com/netbird)
- Join our [Slack Channel](https://join.slack.com/t/netbirdio/shared_invite/zt-vrahf41g-ik1v7fV8du6t0RwxSrJ96A)
- NetBird [latest release](https://github.com/netbirdio/netbird/releases) on GitHub