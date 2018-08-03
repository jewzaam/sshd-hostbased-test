# Overview
Create 2 EC2 instances that allow tcp/22 from your laptop and from each other.  Easiest way is allowing 0.0.0.0/0 but that's up to you.  This test is relying on public access and public IP + DNS.

## Requirements
* Nobody can SSH to the host as root.
* Users can SSH to the host as their user with a password.
* Privileged clients can SSH to the host as a specific user with publickey.

## Assumptions
* SSH to the test host and client you have as `root` requires an ssh key without a passphrase
* You do not have anything of importance on the test host and client!  (Total loss is OK)

# Setup
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

# Outcome
At the end of this setup the following logins will work:
- SSH from any client to the host as standard users (not root, not privileged user)
- SSH from privileged client to the host as privileged user with privatekey

## Note
For hostbased to work you have to have users on host and client that have the same uid in addition to the host configuration.  May also require the same name, but haven't tested that.

# References
* https://arc.liv.ac.uk/SGE/howto/hostbased-ssh.html
* https://docs.oracle.com/cd/E53394_01/html/E54793/sshuser-12.html
* https://lists.mindrot.org/pipermail/openssh-unix-dev/2015-January/033321.html
* http://www.ep.ph.bham.ac.uk/general/support/sshtips.html
