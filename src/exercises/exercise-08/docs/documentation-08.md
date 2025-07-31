# SSH X11 Forwarding 

## Technical Background 

X11 forwarding allows you to run a graphical application on a remote host but display its window on your local desktop, all over a secure SSH connection. This is useful when the remote host is locked down (e.g., only SSH allowed, HTTP blocked) but you still need to use graphical tools, like a web browser, without exposing new network ports.

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

2. Install and start Nginx on the server:

```
apt install nginx -y
systemctl status nginx
```

3. Check that Nginx is reachable from the server itself:

`curl http://localhost`

4. In the firewall, allow only SSH (TCP port 22) and remove HTTP (TCP port 80)

5. Test that HTTP

`curl http://<server-ip>`

!!! Note
    Expected result: `connection refused or timeout`

6. Install `xauth`, which is required for managing X11 authentication:

`apt install xauth`

### Using X11 Forwarding

If your local machine is a linux system X11 is included by default. On other system check how to install the appropiate tools.

!!! Warning
    Make sure your local X11 Server is running.

1. Connect with X11 Forwarding using the `-Y` flag:

`ssh -Y root@<your-server-ip>`

!!! Info
    This enables secure X11 forwarding.

2. Install Firefox on the server:

`apt install firefox-esr`

!!! Warning
    This may take some time, so using a slightly larger server instance is recommended.

3. Run Firefox:

`firefox`

!!! Note 
    The Firefox window should appear on your local desktop, even though it runs on the remote server.

!!! Note
    If the GUI isn't showing up make sure that the X11 server is running and that your local machine has the appropiate access.

