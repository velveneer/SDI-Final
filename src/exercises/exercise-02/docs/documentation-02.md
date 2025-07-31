# Server Recreation

## Technical Background

When a server is deleted and recreated while using the same IP address, its unique SSH host key changes. SSH clients detect this mismatch and block the connection to prevent potential man-in-the-middle attacks. Understanding why this warning occurs and how to resolve it safely is essential for managing dynamic cloud environments where servers are frequently replaced or re-provisioned.

## Related Links

[SSH](https://en.wikipedia.org/wiki/Secure_Shell)

[Hetzner Login](https://accounts.hetzner.com/login)

[Hetzner Console](https://console.hetzner.cloud/)

## Solution

### Deletion and recreation of server

1. Delete the current running server with the Hetzner Cloud Console
2. When prompted choose to keep the IPv4 and IPv6 address unassagnied to be reused
3. Create a new server and use the unassigned IP addresses 

### Connecting via SSH

When you try to connect to the new server via SSH:

`ssh root@<your-server-ip>`

You will get an output in the console containing:

```
~$ ssh root@<your-server-ip>
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ED25519 key sent by the remote host is
SHA256:Dg9+vHl/JYIEgYto5AjlwnttzI4yUcgre0c6hXXKkaQ.
Please contact your system administrator.
Add correct host key in /home/user/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /home/user/.ssh/known_hosts:288
Host key for <your-server-ip> has changed and you have requested strict checking.
```

### What does that mean and why does it happen?

SSH saves server host keys in ~/.ssh/known_hosts. When connecting again, it compares the saved fingerprint with the server’s current fingerprint. If the key differs (because the VM is new), SSH assumes a potential MITM attack and blocks the connection. This behavior is by design to protect against impersonation attacks.

### How to fix the Host Key Conflict

!!! Warning
    Only do this if you are certain that the server was intentionally recreated and the IP address was reused for the same purpose.

You can either remove the old key with the command:

`ssh-keygen -R <your-server-ip>`

!!! Note
    This deletes the old entry. When you reconnect, SSH will ask you to trust the new fingerprint.

Or manually edit your Known Host file:

1. Open `~/.ssh/known_hosts` in an editor.

2. Remove the line referencing `<your-server-ip>`

3. Reconnect and accept the new fingerprint

!!! Warning
    Do not copy keys between servers. Each server must have its own unique host key.
    Always confirm why the host key changed and verify the fingerprint after resolving the conflict.