# MI Gitlab access by SSH

## Technical Background

When working with Git repositories, HTTPS access often requires re-entering a username and password or personal access token for every push and pull operation.
Using SSH keys eliminates this repetitive authentication by providing a secure, passwordless connection. Once a public key is registered in the GitLab profile, it enables encrypted communication and seamless Git operations without manually typing credentials.

## Related Links

[SSH](https://en.wikipedia.org/wiki/Secure_Shell)

[Use SSH keys to communicate with GitLab](https://docs.gitlab.com/user/ssh)

[MI GitLab](https://gitlab.mi.hdm-stuttgart.de/)

## Solution

### Adding Public SSH Key to GitLab

1. Output the content from your public key file with the command:
   
`cat ~/.ssh/id_ed25519.pub`

2. Copy the content
3. Login to your GitLab account and navigate over the GUI to:

`Profile` → `SSH Keys`

4. Paste the public key into the field and assign a descriptive title
5. Click `Add Key`

!!! Warning
    Forgetting to add the key in GitLab results in `Permission denied (publickey)` errors.

### Testing SSH Access

1. Try to clone a repository with the SSH URL:

`git clone git@gitlab.mi.hdm-stuttgart.de:<username>/<repository>.git`

!!! Info
    On the first connection, confirm the host fingerprint. If your SSH key has a passphrase, you’ll be prompted to enter it once.

!!! Warning 
    If using passphrase-protected keys, ensure an SSH agent is running to avoid repeated prompts.

2. Push changes to the remote repository:

Make a change, commit and push it:

```
git add .
git commit -m "Initial commit via SSH"
git push origin main
```
!!! Info
    The push completes without requiring a username or password because the SSH key handles authentication.

3. Pull a Change

Make a remote change and pull to your local git:

`git pull`

!!! Info
    Git synchronizes using the same SSH-secured connection.

!!! Info
    Check your SSH connection directly with `ssh -T git@gitlab.mi.hdm-stuttgart.de`

