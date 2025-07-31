# First Server

## Technical Background

In a software defined infrastructure environment, creating and managing virtual servers is a foundational skill.
This exercise teaches how to:

- Provision a cloud server

- Apply basic firewall rules

- Securely access the server using SSH

- Understand and verify SSH host keys to prevent security risks

## Related Links

[Hetzner Login](https://accounts.hetzner.com/login)

[Hetzner Console](https://console.hetzner.cloud/)

[ICMP](https://en.wikipedia.org/wiki/Internet_Control_Message_Protocol)

[SSH](https://en.wikipedia.org/wiki/Secure_Shell)

[How SSH Works](https://www.sectigo.com/resource-library/what-is-an-ssh-key#How%20do%20SSH%20keys%20work?)

[GUI](https://en.wikipedia.org/wiki/Graphical_user_interface)

[Debian 12](https://www.debian.org/releases/bookworm/)

[How Firewalls Work](https://www.cisco.com/site/us/en/learn/topics/security/what-is-a-firewall.html#tabs-69d6a56dd3-item-fdd67b2fb8-tab)

## Prerequsits

### Project Access

1. Login in to your [Hetzner Account](https://accounts.hetzner.com/login)
2. Open the `Hetzner Cloud Console` over the upper-right menu.
3. Select your the project with your assigned group number (e.g. g11)
4. Open the `Setting` page and go the `Members` Options
5. Check if you have the `Admin` role assigned.

### Setting Up A Firewall

1. In the `Cloud Console`, navigate to `Firewalls` → `Create Firewall`

2. Add inbound rules for:

- `Port 22`: required for secure shell access

- `ICMP`: allows ping for connectivity testing

!!! Warning
    Leave the two inbound rules port 22 and ICMP untouched. Removing port 22 access will lock you out of your server.

3. Name the firewall (e.g., basicFirewallG11)

4. Click Create Firewall

## Server Creation

1. Go to Create Server in the console.

2. Choose:
   - `Image`: Debian 12 (default)
   - `Type`: choose the cheapest available one

3. Add the created firewall 

4. Give it a name (e.g. serverG11)

5. Click `Create and Buy`

!!! Note
    You'll receive an E-Mail containing your server's IP and root password. You may reset root's password in the GUI's rescue tab.

## Accessing Server Via SSH

1. Conntect to the server via a terminal with ssh:

`ssh root@<your-server-ip`

!!! Note
    You can copy the ip address from the Hetzner Console

2. On the first connection you'll see a fingerprint prompt:

```
ssh root@95.216.187.60
The authenticity of host '95.216.187.60 (95.216.187.60)' can't be established.
ED25519 key fingerprint is SHA256:vpV7B+l9RLQ+SwTMqtkk7YbICBhyhi2OP780+WVEFMY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

3. Answer `yes` in the console to trust the server

!!! Note
    This will save the fingerprint to `~/.ssh/known_hosts` for future connections. 

!!! Important 
    This prevents man-in-the-middle attacks by ensuring you connect to the correct server.

4. After confirming the fingerprint, change the root password in the console:

```
Changing password for root:
Current password:
New password:
Retype new password:
```

## Access Server Via Hetzner Console

Hetzner offers a way to access the server over a built-in console inside the webapplication.

1. In the Hetzner GUI got to `server view`
2. Click the terminal icon `>_`
3. Log in with the credentials you received in an email

!!! Warning
    Due to keymapping issues you may need to change your keyboard layout to type the correct characters inside the built-in console.

