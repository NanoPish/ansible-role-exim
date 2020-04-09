# Ansible Role: Exim

[![Build Status](https://travis-ci.org/geerlingguy/ansible-role-exim.svg?branch=master)](https://travis-ci.org/geerlingguy/ansible-role-exim)

Installs Exim (a Mail Transfer Agent) on RedHat/CentOS or Debian/Ubuntu.

The original from geerlingguy/ansible-role-exim aims at using smtp to send mail directy (config type: internet)
This fork can be configured for multiple modes.

I use it for configuring exim4 as an smtp satellite that sends mails through an smtp relay, like gmail or online.net

It supports TLS, generating exim certificate, configuring smtp relay and credentials, adding localmacros, changing most of dc_* configs

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    exim_dc_eximconfig_configtype: internet

(Debian/Ubuntu only) Main configuration type. Should be 'internet' for public mail sending, or 'local' if mail should only be delivered locally. See Exim documentation for other options.

    exim_dc_localdelivery: mail_spool

(Debian/Ubuntu only) Default transport for local mail delivery. Defaults to `mail_spool` if unset.

    exim_primary_hostname: ""

Force a primary server hostname for Exim. Usually you don't need to set this, but if Exim can't reliably determine the FQDN of your server, you can set this and it will ensure Exim uses the correct hostname.

## Dependencies

None.

## Example Playbook

    - hosts: servers
      roles:
        - ansible-role-exim:
	  import_role:
            name: exim
          vars:
            exim_dc_eximconfig_configtype: 'satellite'
            exim_dc_local_interfaces: '127.0.0.1 ; ::1'
            exim_dc_readhost: '{{ inventory_hostname }}'
            exim_dc_minimaldns: 'false'
            exim_dc_smarthost: 'smtp.online.net::25'
            exim_dc_hide_mailname: 'true'
            exim_dc_localdelivery: 'mail_spool'
            exim_primary_hostname: '{{ inventory_hostname }}'
            exim_generate_certificate: true
            exim_certificate_hostname: "{{ inventory_hostname }}"
            exim_use_TLS: true
            exim_use_TLS_ports: '25'
            exim_require_protocol: 'smtp'
            exim_smarthosts_tls: '*'
            exim_smtp_passwd_clients: ['*.smtp.host.net:john@doe.online.net:password']
            exim_sender_rewrite: 'reporting-mailbox'
	

## License

MIT / BSD

## Author Information

This role was created in 2015 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
This role was modified in 2020 by Paul Belloc
