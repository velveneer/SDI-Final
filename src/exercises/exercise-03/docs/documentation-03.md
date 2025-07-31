# Server Security Improvement

## Technical Background

Default cloud servers are often configured for convenience rather than security. Common issues include password-based SSH access, open firewall rules, and outdated software packages. This exercise demonstrates how to improve server security by:

- Using public key authentication instead of passwords
- Limiting network access with a minimal firewall configuration
- Keeping the system up to date
- Deploying a basic web service (NGINX) under these security constraints

## Related Links

[SSH](https://en.wikipedia.org/wiki/Secure_Shell)

[Hetzner Login](https://accounts.hetzner.com/login)

[Hetzner Console](https://console.hetzner.cloud/)

[ssh-keygen](https://linux.die.net/man/1/ssh-keygen)

[ssh-passphrase](https://www.ssh.com/academy/ssh/passphrase)

[scp](https://linux.die.net/man/1/scp)

[NGINX](https://docs.nginx.com/nginx/admin-guide/web-server)

[wget](https://linux.die.net/man/1/wget)

[HTTP](https://www.w3.org/Protocols/)

[How Firewalls Work](https://www.cisco.com/site/us/en/learn/topics/security/what-is-a-firewall.html#tabs-69d6a56dd3-item-fdd67b2fb8-tab)

## Solution

### Creating a Firewall

In the Hetzner Cloud Console:

1. Go to `Firewalls` → `Create Firewall`

2. Add one inbound rule:
    - `Protocol`: ICMP
    - `Action`: Allow

3. Save the firewall, for example as `icmp-only-fw`

### Uploading and Using a Public SSH Key

1. Navigate to `Security` → `SSH Keys`

2. Upload your public SSH key and mark it as `default`

!!! Info
    This ensures passwordless and more secure authentication.

### Creating a Server Using Firewall and SSH Key

1. Deploy a new server using the same `Image` and `Type` as before

2. Select the `icmp-only-fw` firewall and your uploaded SSH key

3. After deployment, note the server’s public IP

### Testing ICMP and SSH Connectivity

Run this command in your local terminal:

`ping <ypur-server-ip>`

!!! Info
    The server responds to ICMP packets, confirming that the firewall rule works.

Try to connect to your server via SSH:

`ssh root@<your-server-ip>`

!!! Note
    Your connection should time out because port 22 is not allowed by the firewall

### Allowing SSH 

1. In the Hetzner Cloud Console go to `Firewalls`
2. Edit your created rule and add a new inbound rule:
    - `Protocol`: TCP
    - `Port`: 22
    - `Action`: Allow
3. Retry the SSH Connection:

```
ssh root@<server-ip>
hostname
```

!!! Note
    This time your login should succed. This should not require a password because the SSH key was used.

### Updating and Rebooting the Server

Run these commands in a terminal on your server:

```
apt update
apt upgrade -y
aptitude -y upgrade
reboot
```

!!! Info
    This ensures all packages and the kernel are up to date.

### Installing NGINX on the Server

To install the webser NGINX run this commands:

`apt install nginx -y`

To verify that the installation was successful use:

`systemctl status nginx`

!!! Note
    The output should show NGINX running and enabled.

### Local HTTP Test

1. On the server run:

`wget -O - http://<your-server-ip>`

!!! Note
    You should see the NGINX welcome page HTML returned.

2. Open `http://<your-server-ip>` in your local machine browser

!!! Note
    This should fail because port 80 is still blocked by the firewall

### Allowing HTTP Access

1. In the Hetzner Cloud Console go to `Firewalls`
2. Edit your created rule and add a new inbound rule:
   - `Protocol`: TCP
   - `Port`: 80
   - `Action`: Allow
3. Retry to visit the site in your browser from your local machine.

!!! Note
    Now the NGINX welcome page should load successfully.

