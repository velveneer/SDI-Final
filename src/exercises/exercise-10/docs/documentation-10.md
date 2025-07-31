# Using The tail -f Command

## Technical Background

System administrators often need to watch log files in real time to diagnose issues or monitor activities such as user logins. The tail -f command allows you to continuously view the end of a log file and see updates as they happen.

## Related Links

[Tail Documentation](https://man7.org/linux/man-pages/man1/tail.1.html)

## Solution
 
### Setup Server for Monitoring

1. Connect to your server:

`ssh root@<your-server-ip>`

2. Run real-time log monitoring on the authentifaction log:

`sudo tail -f /var/log/auth.log`

!!! Info
    You will now see live updates of login and authentication events.

!!! Warning
    You may need root privileges to access the logging and log files.

### Using the Monitor

1. Trigger a log event:

Log in to the same server from a different machine and oberserve the terminal wher `tail -f` is running:

```
Jul 19 17:44:23 debian sshd[1325]: Accepted publickey for root from <partner-ip> port 54822 ssh2: ED25519 SHA256:...
Jul 19 17:44:23 debian sshd[1325]: pam_unix(sshd:session): session opened for user root(uid=0) by (uid=0)
```

!!! Note
    You should see these new entries appear similar to these.

!!! Note
    Log files depend on the distribution.

2. Exit

When you logout from one of the connected machines additional output for the session should appear.

!!! Info
    Stop the monitoring by pressing `Ctrl+C`

!!! Warning
    Use `exit` to trigger logout events without problems.
    