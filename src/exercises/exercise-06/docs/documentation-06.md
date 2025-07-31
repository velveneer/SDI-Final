# SSH Host Hopping

## Technical Background

In many infrastructures, some systems are located in restricted networks and can only be accessed through an intermediate “jump host”. Copying private SSH keys onto each host is considered insecure because it increases the risk of key compromise. A safer approach is SSH agent forwarding, which allows you to reuse your local SSH credentials when hopping through one or more intermediate hosts, without ever transferring your private key.

## Related Links

[SSH](https://en.wikipedia.org/wiki/Secure_Shell)
[SSH jump host](https://wiki.gentoo.org/wiki/SSH_jump_host)
[SSH Agent Forwarding Guide](https://developer.mozilla.org/en-US/docs/Glossary/SSH_Agent_Forwarding)
[OpenSSH Manual: `ForwardAgent`](https://man.openbsd.org/ssh_config#ForwardAgent)

## Solution 

### SSH Configuration for Agent Forwarding

1. On your local workstation, enable agent forwarding for the intermediate host (Host A).

Edit `~/.ssh/config`:

```
Host hostA
    HostName <hostA-address>
    User root
    ForwardAgent yes

Host hostB
    HostName <hostB-address>
    User root

```

!!! Info
    This ensures your SSH agent is forwarded when connecting to Host A.

2. Before connecting, start the SSH agent and load your private key:

```
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

!!! Note
    You only enter the passphrase once.

### Using Host Hopping

1. Connect to Host A

From your local machine:

`ssh hostA`

!!! Info
    You are now inside Host A while your local SSH agent is available to it.

2. Connecting from Host A to Host B

From within Host A:

`ssh hostB`

!!! Info
    This works even though Host B doesn’t have your private key, because the authentication is forwarded from your local workstation.

3. Closing all connections:

Exit Host B and then Host A:

```
exit
exit
```

4. Direct Login to Host B

Try to connect directly from your workstation to Host B:

`ssh hostB`

!!! Warning
    This fails with: `Permission denied (publickey).` Host B only allows access through Host A.

5. Reverse Hop from B to A:

If you log in to Host B (through Host A) and attempt to connect back to Host A:

`hostA`

You will again see:

`Permission denied (publickey).`

!!! Info 
    This demonstrates that agent forwarding is one-directional. It allows the initial remote host to use your local agent, but doesn’t cascade to subsequent reverse connections.

