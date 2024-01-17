
## Quickstart Guide

Step-by-step video guide on YouTube:

<div className="videowrapper">
    <iframe src="https://www.youtube.com/embed/HYlhvr_eu2U" allow="fullscreen;"></iframe>
</div>
<br/>
This guide describes how to quickly get started with Netzilo and create a secure private network with two connected machines.

One machine is a Linux laptop, and the other one a EC2 node running on AWS.
Both machines are running Linux but Netzilo also works on Windows, MacOS nad popular mobile platforms like Android and iOS.

1. Sign-up at [https://app.Netzilo.io/](https://app.Netzilo.io/)

You can use your Google, GitHub or Microsoft account.

<p>
    <img src="/docs-static/img/getting-started/auth.png" alt="login-to-Netzilo" className="imagewrapper" />
</p>

2. After a successful login you will be redirected to the ```Peers``` screen which is empty because you don't have any peers yet.

The `Add peer` window should automatically pop up, but if it doesn't, click ```Add new peer``` to add a new machine.

<p>
    <img src="/docs-static/img/getting-started/empty-peers.png" alt="login-to-Netzilo" className="imagewrapper"/>
</p>

3. Choose your machine operating system (in our case it is ```Linux```) and proceed with the installation steps.

<p>
    <img src="/docs-static/img/getting-started/add-peer.png" alt="login-to-Netzilo" className="imagewrapper"/>
</p>

4. If you installed Netzilo Desktop UI you can use it to connect to the network instead of running `Netzilo up` command. Look for `Netzilo` in your application list, run it, and click `Connect`.
>

<p>
    <img src="/docs-static/img/getting-started/systray.png" alt="login-to-Netzilo" className="imagewrapper"/>
</p>
5. At this point a browser window pops up starting a device registration process. Click confirm and follow the steps if required.

<p>
    <img src="/docs-static/img/getting-started/device-confirmation.png" alt="login-to-Netzilo" className="imagewrapper"/>
</p>

6. On the EC2 node repeat the installation steps and run `Netzilo up` command.

```bash
sudo Netzilo up
   ```
7. Copy the verification URL from the terminal output and paste it in your browser. Repeat step #5

<p>
    <img src="/docs-static/img/getting-started/Netzilo-up.png" alt="login-to-Netzilo" className="imagewrapper"/>
</p>

8. Return to ```Peers``` and you should notice 2 new machines with status ```online```

<p>
    <img src="/docs-static/img/getting-started/peers.png" alt="login-to-Netzilo" className="imagewrapper"/>
</p>

9. To test the connection you could try pinging devices:

On your laptop:
```bash
ping 100.64.0.2
   ```

On the EC2 node:
 ```bash
ping 100.64.0.1
   ```
10. Done! You now have a secure peer-to-peer private network configured.

<br/>

- Make sure to [star us on GitHub](https://github.com/Netziloio/Netzilo)
- Follow us [on Twitter](https://twitter.com/Netzilo)
- Join our [Slack Channel](https://join.slack.com/t/Netziloio/shared_invite/zt-vrahf41g-ik1v7fV8du6t0RwxSrJ96A)
- Netzilo release page on GitHub: [releases](https://github.com/Netziloio/Netzilo/releases/latest)

## Installation

### Linux

**APT/Debian**
1. Add the repository:

 ```bash
 sudo apt-get update
 sudo apt-get install ca-certificates curl gnupg -y
 curl -sSL https://pkgs.Netzilo.io/debian/public.key | sudo gpg --dearmor --output /usr/share/keyrings/Netzilo-archive-keyring.gpg
 echo 'deb [signed-by=/usr/share/keyrings/Netzilo-archive-keyring.gpg] https://pkgs.Netzilo.io/debian stable main' | sudo tee /etc/apt/sources.list.d/Netzilo.list
```
2. Update APT's cache

```bash
 sudo apt-get update
```
3. Install the package

```bash
 # for CLI only
 sudo apt-get install Netzilo
 # for GUI package
 sudo apt-get install Netzilo-ui
```

**RPM/Red hat**

1. Add the repository:
```bash
 cat <<EOF | sudo tee /etc/yum.repos.d/Netzilo.repo
 [Netzilo]
 name=Netzilo
 baseurl=https://pkgs.Netzilo.io/yum/
 enabled=1
 gpgcheck=0
 gpgkey=https://pkgs.Netzilo.io/yum/repodata/repomd.xml.key
 repo_gpgcheck=1
 EOF
```
2. Install the package
```bash
 # for CLI only
 sudo yum install Netzilo
 # for GUI package
 sudo yum install Netzilo-ui
```

**Fedora**

1. Create the repository file:
```bash
 cat <<EOF | sudo tee /etc/yum.repos.d/Netzilo.repo
 [Netzilo]
 name=Netzilo
 baseurl=https://pkgs.Netzilo.io/yum/
 enabled=1
 gpgcheck=0
 gpgkey=https://pkgs.Netzilo.io/yum/repodata/repomd.xml.key
 repo_gpgcheck=1
 EOF
```
2. Import the file
```bash
 sudo dnf config-manager --add-repo /etc/yum.repos.d/Netzilo.repo
```
3. Install the package
```bash
 # for CLI only
 sudo dnf install Netzilo
 # for GUI package
 sudo dnf install Netzilo-ui
```


**NixOS 22.11+/unstable**

1. Edit your [`configuration.nix`](https://nixos.org/manual/nixos/stable/index.html#sec-changing-config)

```nix
 { config, pkgs, ... }:
 {
   services.Netzilo.enable = true; # for Netzilo service & CLI
   environment.systemPackages = [ pkgs.Netzilo-ui ]; # for GUI
 }
```
2. Build and apply new configuration

```bash
 sudo nixos-rebuild switch
```

### macOS
**Homebrew install**
1. Download and install homebrew at https://brew.sh/
2. If Netzilo was previously installed with homebrew, you will need to run:
```bash
# Stop and uninstall daemon service:
sudo Netzilo service stop
sudo Netzilo service uninstall
# unlik the app
brew unlink Netzilo
```
> Netzilo will copy any existing configuration from the Netzilo's default configuration paths to the new Netzilo's default location

3. Install the client
```bash
  # for CLI only
  brew install Netziloio/tap/Netzilo
  # for GUI package
  brew install --cask Netziloio/tap/Netzilo-ui
```
4. If you installed CLI only, you need to install and start the client daemon service:
 ```bash
  sudo Netzilo service install
  sudo Netzilo service start
```

### Windows
1. Checkout Netzilo [releases](https://github.com/Netziloio/Netzilo/releases/latest)
2. Download the latest Windows release installer ```Netzilo_installer_<VERSION>_windows_amd64.exe``` (**Switch VERSION to the latest**):
3. Proceed with the installation steps
4. This will install the UI client in the C:\\Program Files\\Netzilo and add the daemon service
5. After installing, you can follow the steps from [Running Netzilo with SSO Login](#Running-Netzilo-with-SSO-Login) steps.
> To uninstall the client and service, you can use Add/Remove programs

⚠️ In case of any issues with the connection on Windows check the firewall settings. With default Windows 11 firewall setup there could be connectivity issue related to egress traffic.

Recommended way is to add Netzilo in firewall settings:

1. Go to "Control panel".
2. Select "Windows Defender Firewall".
3. Select "Advanced settings".
4. Select "Outbound Rules" -> "New rule".
5. In the new rule select "Program" and click "Next".
6. Point to the Netzilo installation exe file (usually in `C:\Program Files\Netzilo\Netzilo.exe`) and click "Next".
7. Select "Allow the connection" and click "Next".
8. Select the network in which rule should be applied (Domain, Private, Public) according to your needs and click "Next".
9. Provide rule name (e.g. "Netzilo Egress Traffic") and click "Finish".
10. Disconnect and connect to Netzilo.


### Binary Install
**Installation from binary (CLI only)**

1. Checkout Netzilo [releases](https://github.com/Netziloio/Netzilo/releases/latest)
2. Download the latest release:
```bash
  curl -L -o ./Netzilo_<VERSION>.tar.gz https://github.com/Netziloio/Netzilo/releases/download/v<VERSION>/Netzilo_<VERSION>_<OS>_<Arch>.tar.gz
```

<Note>

    You need to replace some variables from the URL above:

    - Replace **VERSION** with the latest released verion.
    - Replace **OS** with "linux", "darwin" for MacOS or "windows"
    - Replace **Arch** with your target system CPU archtecture

</Note>

3. Decompress
```bash
  tar xcf ./Netzilo_<VERSION>.tar.gz
  sudo mv Netzilo /usr/bin/Netzilo
  sudo chown root:root /usr/bin/Netzilo
  sudo chmod +x /usr/bin/Netzilo
```
After that you may need to add /usr/bin in your PATH environment variable:
````bash
  export PATH=$PATH:/usr/bin
````
4. Install and run the service
```bash
  sudo Netzilo service install
  sudo Netzilo service start
```

### Running Netzilo with SSO Login
#### Desktop UI Application
If you installed the Desktop UI client, you can launch it and click on Connect.
> It will open your browser, and you will be prompt for email and password. Follow the instructions.

<p>
    <img src="/docs-static/img/getting-started/Netzilo-sso-login-ui.gif" alt="high-level-dia" className="imagewrapper"/>
</p>

#### CLI
Alternatively, you could use command line. Simply run
   ```bash
  Netzilo up
   ```
> It will open your browser, and you will be prompt for email and password. Follow the instructions.

<p>
    <img src="/docs-static/img/getting-started/Netzilo-sso-login-cmd.gif" alt="high-level-dia" className="imagewrapper"/>
</p>

Check connection status:
```bash
  Netzilo status
```

### Running Netzilo with a Setup Key
In case you are activating a server peer, you can use a [setup key](/how-to/register-machines-using-setup-keys) as described in the steps below.
> This is especially helpful when you are running multiple server instances with infrastructure-as-code tools like ansible and terraform.

1. Login to the Management Service. You need to have a `setup key` in hand (see [setup keys](/how-to/register-machines-using-setup-keys)).

For all systems:
```bash
  Netzilo up --setup-key <SETUP KEY>
```

For **Docker**, you can run with the following command:
```bash
docker run --network host --privileged --rm -d -e NB_SETUP_KEY=<SETUP KEY> -v Netzilo-client:/etc/Netzilo Netziloio/Netzilo:<TAG>
```
> TAG > 0.6.0 version

Alternatively, if you are hosting your own Management Service provide `--management-url` property pointing to your Management Service:
```bash
  Netzilo up --setup-key <SETUP KEY> --management-url http://localhost:33073
```

> You could also omit the `--setup-key` property. In this case, the tool will prompt for the key.

2. Check connection status:
```bash
  Netzilo status
```

3. Check your IP:

On **macOS** :
````bash
  sudo ifconfig utun100
````
On **Linux**:
```bash
  ip addr show wt0
     ```
On **Windows**:
```bash
  netsh interface ip show config name="wt0"
```

### Running Netzilo in Docker

Set the ```NB_SETUP_KEY``` environment variable and run the command.

<Note>
    You can pass other settings as environment variables. See [environment variables](/how-to/cli#environment-variables) for details.
</Note>

Netzilo makes use of eBPF and raw sockets, therefore to guarantee the client software functionality, we recommend adding the flags `--cap-add=SYS_ADMIN` and `--cap-add=SYS_RESOURCE` for docker clients.
The experience may vary depending on the docker daemon, operating system, or kernel version.

```bash
docker run --rm --name PEER_NAME --hostname PEER_NAME --cap-add=NET_ADMIN --cap-add=SYS_ADMIN --cap-add=SYS_RESOURCE -d -e NB_SETUP_KEY=<SETUP KEY> -v Netzilo-client:/etc/Netzilo Netziloio/Netzilo:latest
```

See [Docker example](/how-to/examples#net-bird-client-in-docker) for details.

### Troubleshooting
1. If you are using self-hosted version and haven't specified `--management-url`, the client app will use the default URL
which is ```https://api.wiretrustee.com:33073```.

2. If you have specified a wrong `--management-url` (e.g., just by mistake when self-hosting)
to override it you can do the following:

```bash
Netzilo down
Netzilo up --management-url https://<CORRECT HOST:PORT>/
```

To override it see the solution #1 above.
