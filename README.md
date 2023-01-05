# cesnet.msmtp
Ansible role cesnet.msmtp for configuring a simple MTA for forwarding emails to a mail relay

Installs [msmtp](https://marlam.de/msmtp/msmtp.html) Mail Transfer Agent on a Debian system.

Also installs `bsd-mailx` package providing `/usr/bin/mail` command and sets it as the used alternative,
because `mailutils` implementation does not handle local names correctly when used together with msmtp.

Role Variables
--------------

- **msmtp_listen_interface** - IP address to listen on, default is `127.0.0.1`. Use `0.0.0.0` for all, but
  then set firewall to disable incoming connections from outside. Only a single IPv4 or IPv6 address 
  can be used.
- **msmtp_aliases_default** - email address to be used in /etc/aliases as default, default is `root`
- **msmtp_config** - content of /etc/msmtprc file specifying connection. 
  See [msmtp documentation](https://marlam.de/msmtp/msmtp.html) for options.

Example Playbook
----------------
```yaml
- hosts: all
  roles:
    - role: cesnet.msmtp
      vars:
        msmtp_listen_interface: 0.0.0.0
        msmtp_aliases_default: john@example.org
        msmtp_config: |
          from %U@%H
          host relay.muni.cz
          port 465
          tls on
          tls_starttls off
          auth on
          user john@IS.MUNI.CZ
          password s3cr3t
```
