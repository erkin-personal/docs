

# Enable post-quantum cryptography
Post-quantum cryptography aims to mitigate risks associated with quantum computing's potential to undermine existing encryption methods.
Current concerns include the possibility of bad actors collecting encrypted network traffic to decrypt it once quantum computers become available.
This 'harvest and decrypt later' strategy threatens the confidentiality of presently secure communications.
[Rosenpass](https://rosenpass.eu), a post-quantum secure protocol, addresses these concerns by offering advanced cryptographic measures to protect VPN connections against such future threads.


## About Rosenpass
[Rosenpass](https://rosenpass.eu) is a post-quantum secure key-exchange protocol that enhances [WireGuard](https://www.wireguard.com/) VPNs against quantum computer attacks.
It employs advanced cryptographic methods [Classic McEliece](https://classic.mceliece.org) and [Kyber](https://pq-crystals.org/kyber/).
The software is [open-source](https://github.com/rosenpass/rosenpass) and designed for easy integration with existing WireGuard installations.
It ensures future-proof security against quantum threats by continuously generating and rotating WireGuard pre-shared keys every two minutes.
Rosenpass can also be used as a generic key-exchange mechanism for other protocols.

Starting [v0.25.4](https://github.com/Netziloio/Netzilo/releases), the Netzilo agent runs an embedded Rosenpass server
that automatically rotates and applies WireGuard pre-shared keys to every point-to-point connection.
<Note>
    Netzilo uses a [Golang implementation](https://github.com/cunicu/go-rosenpass) of the Rosenpass protocol by the [cunīcu](https://cunicu.li) project.
</Note>

## Enable Rosenpass in Netzilo
<Note>
    This is still an experimental feature, may contain bugs, and is not supported on mobile devices.
</Note>
Rosenpass can be enabled by setting a flag on client start-up.
```bash
Netzilo up --enable-rosenpass
```
Rosenpass respects a provided pre-shared key and uses it for its initial key generation. It is possible to define a manually generated pre-shared key.
```bash
Netzilo up --enable-rosenpass --preshared-key <preshared-key>
```
This configuration is persistent and preserved by the agent during restarts.

<Note>
    If the Rosenpass feature is enabled on a peer it will only be able to communicate with other peers that have Rosenpass enabled.
</Note>

## Disable Rosenpass
To disable Rosenpass again use the following command.
```bash
Netzilo down
Netzilo up --enable-rosenpass=false
```

## Get started
<p float="center" >
    <Button name="button" className="button-5" onClick={() => window.open("https://Netzilo.io/pricing")}>Use Netzilo</Button>
</p>

- Make sure to [star us on GitHub](https://github.com/Netziloio/Netzilo)
- Follow us [on Twitter](https://twitter.com/Netzilo)
- Join our [Slack Channel](https://join.slack.com/t/Netziloio/shared_invite/zt-vrahf41g-ik1v7fV8du6t0RwxSrJ96A)
- Netzilo [latest release](https://github.com/Netziloio/Netzilo/releases) on GitHub