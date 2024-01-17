
# Netzilo Agent command line interface (CLI)

The Netzilo client installation adds a binary called `Netzilo` to your system. This binary runs as a daemon service to connect
your computer or server to the NetBirt network as a peer. But it can also be used as a client to control the daemon service.

This section will explore the commands available in `Netzilo`.
## Syntax
Use the following syntax to run `Netzilo` commands from your terminal window:
```shell
Netzilo [command] [subcommand] [flags]
```
* `command`: Specifies the operation that you want to perform or a top-level command: `up`, `login`, `down`, `status`, `ssh`, `version`, and `service`
* `subcommand`: Specifies the operation to be executed for a top-level command like `service`: `install`, `uninstall`, `start`, and `stop`
* `flags`:  Specifies optional flags. For example, you can use the `--setup-key` flag to specify the setup key to be used in the commands `login` and `up`

<Note>
    To see detailed command information, use the flag `--help` after each command
</Note>

## Global flags
`Netzilo` has a set of global flags that are available in every command. They specify settings that are core or shared between two or more commands, e.g. `--setup-key` is used by `login` and `up` to authenticate the client against a management service.

Below is the list of global flags:
```shell
      --admin-url string        Admin Panel URL [http|https]://[host]:[port] (default "https://app.Netzilo.io")
  -c, --config string           Netzilo config file location (default "/etc/Netzilo/config.json")
      --daemon-addr string      Daemon service address to serve CLI requests [unix|tcp]://[path|host:port] (default "unix:///var/run/Netzilo.sock")
      --log-file string         sets Netzilo log path. If console is specified the the log will be output to stdout (default "/var/log/Netzilo/client.log")
  -l, --log-level string        sets Netzilo log level (default "info")
  -m, --management-url string   Management Service URL [http|https]://[host]:[port] (default "https://api.wiretrustee.com:443")
  -p, --preshared-key string    Sets Wireguard PreSharedKey property. If set, then only peers that have the same key can communicate.
  -k, --setup-key string        Setup key obtained from the Management Service Dashboard (used to register peer)

```
### Environment Variables
Every flag of a `Netzilo` command can be passed as an environment variable. We are using the following rule for the environment variables composition:
* `PREFIX_FLAGNAME` and for flags with multiple parts: `PREFIX_FLAGNAMEPART1_FLAGNAMEPART2`
* The prefix is always **NB**
* The flag parts are separated by a dash ("-") when passing as flags and with an underscore ("_") when passing as an environment variable

For example, let's check how we can pass `--config` and `--management-url` as environment variables:
```shell
export NB_CONFIG="/opt/Netzilo/config.json"
export NB_MANAGEMENT_URL="https://api.self-hosted.com:33073"
Netzilo up
```
The `up` command would process the variables, read the configuration file on `/opt/Netzilo/config.json` and attempt to connect to the management service running at `https://api.self-hosted.com:33073`.

## Commands
### up
Single command to log in and start the Netzilo client. It can send a signal to the daemon service or run in the foreground with the flag `--foreground-mode`.

The command will check if the peer is logged in and connect to the management service. If the peer is not logged in, by default, it will attempt to initiate an SSO login flow.
#### Flags
```shell
      --dns-resolver-address string   Sets a custom address for Netzilo's local DNS resolver. If set, the agent won't attempt to discover the best ip and port to listen on. An empty string "" clears the previous configuration. E.g. --dns-resolver-address 127.0.0.1:5053 or --dns-resolver-address ""
      --external-ip-map strings       Sets external IPs maps between local addresses and interfaces.You can specify a comma-separated list with a single IP and IP/IP or IP/Interface Name. An empty string "" clears the previous configuration. E.g. --external-ip-map 12.34.56.78/10.0.0.1 or --external-ip-map 12.34.56.200,12.34.56.78/10.0.0.1,12.34.56.80/eth1 or --external-ip-map ""
  -F, --foreground-mode               start service in foreground

```
#### Usage
The minimal form of running the command is:
```shell
Netzilo up
```
If you are running on a self-hosted environment, you can pass your management url by running the following:
```shell
Netzilo up --management-url https://api.self-hosted.com:33073
```
if you want to run in the foreground, you can use "console" as the value for `--log-file` and run the command with sudo:
```shell
sudo Netzilo up --log-file console
```
<Note>
    On Windows, you may need to run the command from an elevated terminal session.
</Note>
In case you need to use a setup key, use the `--setup-key` flag :
```shell
Netzilo up --setup-key AAAA-BBB-CCC-DDDDDD
```

### login
Command to authenticate the Netzilo client to a management service. If the peer is not logged in, by default, it will attempt to initiate an SSO login flow.
#### Usage
The minimal form of running the command is:
```shell
Netzilo login
```
If you are running on a self-hosted environment, you can pass your management url by running the following:
```shell
Netzilo login --management-url https://api.self-hosted.com:33073
```
In case you need to use a setup key, use the `--setup-key` flag:
```shell
Netzilo login --setup-key AAAA-BBB-CCC-DDDDDD
```
Passing a management url and a setup key:
```shell
Netzilo login --setup-key AAAA-BBB-CCC-DDDDDD --management-url https://api.self-hosted.com:33073
```

### down
Command to stop a connection with the management service and other peers in a Netzilo network. After running this command, the daemon service will enter an `Idle` state.
#### Usage
The minimal form of running the command is:
```shell
Netzilo down
```

### status
Retrieves the peer status from the daemon service.
#### Flags
```shell
  -d, --detail                    display detailed status information
      --filter-by-ips strings     filters the detailed output by a list of one or more IPs, e.g. --filter-by-ips 100.64.0.100,100.64.0.200
      --filter-by-status string   filters the detailed output by connection status(connected|disconnected), e.g. --filter-by-status connected
```
#### Usage
The minimal form of running the command is:
```shell
Netzilo status
```
This will output:
```shell
Daemon status: Connected
Management: Connected
Signal:  Connected
Netzilo IP: 100.119.62.6/16
Interface type: Kernel
Peers count: 2/3 Connected
```

If you want to see more details about the peer connections, you can use the `--detail` or `-d` flag:
```shell
Netzilo status -d
```
This will output:
```shell
Peers detail:
 Peer:
  Netzilo IP: 100.119.85.4
  Public key: 2lI3F+fDUWh58g5oRN+y7lPHpNcEVWhiDv/wr1/jiF8=
  Status: Disconnected
  -- detail --
  Connection type: -
  Direct: false
  ICE candidate (Local/Remote): -/-
  Last connection update: 2022-07-07 12:21:31

 Peer:
  Netzilo IP: 100.119.201.225
  Public key: +jkH8cs/Fo83qdB6dWG16+kAQmGTKYoBYSAdLtSOV10=
  Status: Connected
  -- detail --
  Connection type: P2P
  Direct: true
  ICE candidate (Local/Remote): host/host
  Last connection update: 2022-07-07 12:21:32

 Peer:
  Netzilo IP: 100.119.230.104
  Public key: R7olj0S8jiYMLfOWK+wDto+j3pE4vR54tLGrEQKgBSw=
  Status: Connected
  -- detail --
  Connection type: P2P
  Direct: true
  ICE candidate (Local/Remote): host/host
  Last connection update: 2022-07-07 12:21:33

Daemon status: Connected
Management: Connected to https://api.Netzilo.io:33073
Signal:  Connected to https://signal2.wiretrustee.com:10000
Netzilo IP: 100.119.62.6/16
Interface type: Kernel
Peers count: 2/3 Connected
```
To filter the peers' output by connection status, you can use the `--filter-by-status` flag with either "connected" or "disconnected" as value:
```shell
Netzilo status -d --filter-by-status connected
```
This will output:
```shell
Peers detail:
 Peer:
  Netzilo IP: 100.119.201.225
  Public key: +jkH8cs/Fo83qdB6dWG16+kAQmGTKYoBYSAdLtSOV10=
  Status: Connected
  -- detail --
  Connection type: P2P
  Direct: true
  ICE candidate (Local/Remote): host/host
  Last connection update: 2022-07-07 12:21:32

 Peer:
  Netzilo IP: 100.119.230.104
  Public key: R7olj0S8jiYMLfOWK+wDto+j3pE4vR54tLGrEQKgBSw=
  Status: Connected
  -- detail --
  Connection type: P2P
  Direct: true
  ICE candidate (Local/Remote): host/host
  Last connection update: 2022-07-07 12:21:33

Daemon status: Connected
Management: Connected to https://api.Netzilo.io:33073
Signal:  Connected to https://signal2.wiretrustee.com:10000
Netzilo IP: 100.119.62.6/16
Interface type: Kernel
Peers count: 2/3 Connected
```
To filter the peers' output by peer IP addresses, you can use the `--filter-by-ips` flag with one or more IPs separated by a comma as a value:
```shell
Netzilo status -d --filter-by-ips 100.119.201.225
```
This will output:
```shell
Peers detail:
 Peer:
  Netzilo IP: 100.119.201.225
  Public key: +jkH8cs/Fo83qdB6dWG16+kAQmGTKYoBYSAdLtSOV10=
  Status: Connected
  -- detail --
  Connection type: P2P
  Direct: true
  ICE candidate (Local/Remote): host/host
  Last connection update: 2022-07-07 12:21:32

Daemon status: Connected
Management: Connected to https://api.Netzilo.io:33073
Signal:  Connected to https://signal2.wiretrustee.com:10000
Netzilo IP: 100.119.62.6/16
Interface type: Kernel
Peers count: 2/3 Connected
```
You can combine both filters and get the peers that are both connected and with specific IPs:
```shell
Netzilo status -d --filter-by-status connected --filter-by-ips 100.119.85.4,100.119.230.104
```
This will output:
```shell
Peers detail:

 Peer:
  Netzilo IP: 100.119.230.104
  Public key: R7olj0S8jiYMLfOWK+wDto+j3pE4vR54tLGrEQKgBSw=
  Status: Connected
  -- detail --
  Connection type: P2P
  Direct: true
  ICE candidate (Local/Remote): host/host
  Last connection update: 2022-07-07 12:21:33

Daemon status: Connected
Management: Connected to https://api.Netzilo.io:33073
Signal:  Connected to https://signal2.wiretrustee.com:10000
Netzilo IP: 100.119.62.6/16
Interface type: Kernel
Peers count: 2/3 Connected
```
<Note>
    The peer with IP `100.119.85.4` wasn't returned because it was not connected
</Note>

### ssh
Command to connect using ssh to a remote peer in your Netzilo network.

You should run the ssh command with elevated permissions.
#### Flags
```shell
  -p, --port int   Sets remote SSH port. Defaults to 44338 (default 44338)
```
#### Arguments
The ssh command accepts one argument, `user@host`; this argument indicates the remote host to connect:
* `user`: indicates the remote user to login
* `host`: indicates the remote peer host IP address
#### Usage
The minimal form of running the command is:
```shell
sudo Netzilo ssh user@100.119.230.104
```
If you the remote peer agent is running the ssh service on a different port, you can use the `--port` or `-p` flag:
```shell
sudo Netzilo ssh -p 3434 user@100.119.230.104
```

### version
Outputs the `Netzilo` command version.
#### Usage
The minimal form of running the command is:
```shell
Netzilo version
```
This will output:
```shell
0.8.2
```

### service
The service command is a top-level command with subcommands to perform operations related to the daemon service.

You should run the service command with elevated permissions.

### service install
The install installs the daemon service on the system.
#### Usage
The minimal form of running the command is:
```shell
sudo Netzilo service install
```
You can use the global flags to configure the daemon service. For instance, you can set a debug log level with the flag `--log-level`
```shell
sudo Netzilo service install --log-level debug
```
You can set a custom configuration path with the flag `--config`
```shell
sudo Netzilo service install --config /opt/Netzilo/config.json
```

### service uninstall
The uninstall uninstalls the daemon service from the system.
#### Usage
The minimal form of running the command is:
```shell
sudo Netzilo service uninstall
```

### service start
Starts the daemon service
#### Usage
The minimal form of running the command is:
```shell
sudo Netzilo service start
```

### service stop
Stops the daemon service
#### Usage
The minimal form of running the command is:
```shell
sudo Netzilo service stop
```
