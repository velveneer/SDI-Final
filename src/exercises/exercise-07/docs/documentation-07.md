# SSH Port Forwarding

## Technical Background

Firewalls are often configured to block access to all services except essential ones like SSH, reducing the attack surface of a server.
When you need temporary access to an internal service (like a web server running on port 80) without opening it publicly, SSH port forwarding provides a secure workaround. With a single SSH connection, you can tunnel traffic from a local port to a remote port, effectively “borrowing” your SSH access to securely reach otherwise blocked services.

## Related Links

[SSH](https://en.wikipedia.org/wiki/Secure_Shell)

[SSH jump host](https://wiki.gentoo.org/wiki/SSH_jump_host)

[SSH Agent Forwarding Guide](https://developer.mozilla.org/en-US/docs/Glossary/SSH_Agent_Forwarding)

[OpenSSH Manual: `ForwardAgent`](https://man.openbsd.org/ssh_config#ForwardAgent)

[Nginx](https://docs.Nginx.com/Nginx/admin-guide/web-server)

[HTTP](https://www.w3.org/Protocols/)

## Solution

### Server Setup

1. Deploy a server with the same configuration as in `Exercise 3`

2. Install and start Nginx:

```
apt install Nginx -y
systemctl status Nginx
```

3. Verify that the webserver works by visiting

`http://<your-server-ip>`

!!! Note
    You should see the Nginx welcome page.

### SSH Only Firewall

In the Hetzner Cloud Console:

1. Go to `Firewalls` → `Create Firewall`

2. Edit to only keep one inbound TCP rule:
    - `Protocol`: TCP
    - `Port`: 22

3. Remove HTTP (Port 80) access

!!! Warning
    Tunnel effect doesn't work if Port 80 rule was not removed

Test that HTTP is now bloacked:

`curl http://<server-ip>`

!!! Note
    Expected Output: `curl: (7) Failed to connect to <server-ip> port 80: Connection refused`

### Forward Remote Port to Local Port

1. Use SSH port forwarding from your local machine:

`ssh -L 2000:localhost:80 root@<your-server-ip>`

!!! Note 
    `2000` -> local port on your workstation

!!! Note 
    `localhost:80` -> port 80 on the remote server (from its own perspective)

2. Keep this SSH session running while testing.

### Accessing Nginx

Opem a browser in your local machine and navigate to:

`http://localhost:2000`

!!! Info
    Even though port 80 is blocked externally, you now see the Nginx welcome page via the secure SSH tunnel.