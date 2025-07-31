# ssh-agent

## Technical Background

Using SSH keys with passphrases significantly increases security by protecting private keys at rest. However, entering the passphrase every time you connect to a remote server quickly becomes tedious, especially when managing multiple servers.
The ssh-agent service solves this problem by storing unlocked keys securely in memory during a session, so you only enter the passphrase once. This approach maintains security while improving usability, particularly for frequent administrative tasks.

On most modern Linux distributions the ssh-agent is installed as a part of the OpenSSH package. Check before if you're using a minimal distribution and install it in case it's not included.

## Related Links

[ssh-keygen](https://linux.die.net/man/1/ssh-keygen)

[ssh-passphrase](https://www.ssh.com/academy/ssh/passphrase)

[ssh-agent](https://linux.die.net/man/1/ssh-agent)

[KeepassXC SSH Agent Integration](https://keepassxc.org/docs/#faq-ssh-agent-how)

## Solution 

### Starting the SSH Agent

Begin by launching the agent in your shell:

`eval "$(ssh-agent -s)"`

If successful, it returns a process ID like:

`Agent pid 5678`

### Setting up the Agent

1. Load your private key into the agent:

`ssh-add ~/.ssh/id_ed25519`

2. If the key is protected by a passphrase, you will be prompted once:

```
Enter passphrase for /home/user/.ssh/id_ed25519:
Identity added: /home/user/.ssh/id_ed25519
```

!!! Info
    After this, the agent keeps the unlocked key available for the rest of your session.

3. Confirm that the agent is active:

`printenv | grep SSH_AUTH_SOCK`

!!! Note
    Expected Output: `SSH_AUTH_SOCK=/run/user/1000/keyring/ssh`

!!! Warning
    The agent must be started for every new shell session unless automated.

!!! Warning
    Forgetting to run `ssh-add` will still prompt for the passphrase.

