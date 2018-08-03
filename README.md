Create 2 EC2 instances that allow tcp/22 from your laptop and from each other.  Easiest way is allowing 0.0.0.0/0 but that's up to you.  This test is relying on public access and public IP + DNS.

To run the setup you need:
- host IP address
- privileged client IP address
- EC2 identity file (pem) path
- a username for the privileged user

The setup is not idempotent, but is easy to run again.

Because you'll be changing sshd configuration it is *highly* recommended you ssh to the host and stay connected.  If you need to re-configure things this will be required.

To run the setup:
```
./host-client-setup <host IP address> <client IP address> <path to EC2 identity file> <privileged username>
```

To revert setup enough so you can run it again, on the host remove the configuration that was added to `/etc/ssh/sshd_config` and `systemctl restart sshd`.

At the end of this setup the following logins will work:
- SSH from any client to the host as standard users (not root, not privileged user)
- SSH from privileged client to the host as privileged user with privatekey

