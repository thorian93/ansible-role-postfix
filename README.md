# Ansible Role: Postfix

This role configures Postfix on RHEL/CentOS, Debian/Ubuntu, Fedora and openSUSE servers.

[![Ansible Role: Postfix](https://img.shields.io/ansible/role/54168?style=flat-square)](https://galaxy.ansible.com/thorian93/ansible_role_postfix)
[![Ansible Role: Postfix](https://img.shields.io/ansible/quality/54168?style=flat-square)](https://galaxy.ansible.com/thorian93/ansible_role_postfix)
[![Ansible Role: Postfix](https://img.shields.io/ansible/role/d/54168?style=flat-square)](https://galaxy.ansible.com/thorian93/ansible_role_postfix)

## Requirements

No special requirements; note that this role requires root access, so either run it in a playbook with a global `become: yes`, or invoke the role in your playbook like:

    - hosts: foobar
      roles:
        - role: ansible-role-postfix
          become: yes

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    postfix_enabled: 'true'

Enable Postfix on boot.

    postfix_etc_aliases:
    - name: "Redirect root mails."
        line: 'root: root'
        regexp: '^root\:.*'
        insertbefore: EOF
        setype: etc_aliases_t
        state: present
        backup: 'true'

Configure aliases to forward mail to.

    postfix_myhostname: "{{ inventory_hostname }}"
    postfix_myorigin: $myhostname
    postfix_mynetworks: "127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128"
    postfix_mydestination: $mydomain, $myhostname, localhost.$mydomain, localhost
    postfix_relayhost: []
    postfix_inet_interfaces: loopback-only
    postfix_inet_protocols: ipv4
    postfix_masquerade_domains: ""
    postfix_smtpd_use_tls: 'yes'
    postfix_smtp_sasl_auth_enable: 'false'
    postfix_smtp_tls_CApath: "/etc/ssl/certs"
    postfix_smtp_use_tls: 'yes'
    postfix_local_recipient_map: ""
    postfix_smtp_sasl_security_options: ""
    postfix_os_service: ""
    postfix_generic_maps: ""

General Postfix options. Refer to their [documentation](http://www.postfix.org/) for further details.

    postfix_local_user_relay_address: ""

Relay all mail going to local users (e.g. root or cron) to another mail address.

    postfix_rewrite_sender_address: ""

Rewrite the sender address for all outgoing mail.

    postfix_smtp_sasl_user: "{{ ansible_ssh_user | default('root') }}"
    postfix_smtp_sasl_password: ""

Configure authentication for outgoing SMTP if necessary.

    postfix_send_test_mail_to: ""

Send a test mail to this address when Postfix configuration changes.

    bounce_queue_lifetime: 1h
    maximal_queue_lifetime: 1h
    maximal_backoff_time: 15m
    minimal_backoff_time: 5m
    queue_run_delay: 5m

Configure Postfix queue.

    postfix_tls_generate: 'false'
    postfix_ssl_subject: ""
    postfix_tls_cert_file: "/etc/ssl/certs/ssl-cert-snakeoil.pem"
    postfix_tls_key_file: "/etc/ssl/private/ssl-cert-snakeoil.key"

Configure Postfix TLS settings.

## Dependencies

None.

## OS Compatibility
This role ensures that it is not used against unsupported or untested operating systems by checking, if the right distribution name and major version number are present in a dedicated variable named like `<role-name>_stable_os`. You can find the variable in the role's default variable file at `defaults/main.yml`:

    role_stable_os:
      - Debian 10
      - Ubuntu 18
      - CentOS 7
      - Fedora 30

If the combination of distribution and major version number do not match the target system, the role will fail. To allow the role to work add the distribution name and major version name to that variable and you are good to go. But please test the new combination first!

Kudos to [HarryHarcourt](https://github.com/HarryHarcourt) for this idea!

## Example Playbook

    ---
    - name: "Run role."
      hosts: all
      become: yes
      vars:
        # Example configuration for authenticated outgoing SMTP
        postfix_relayhost: "[smtp.example.com]:587"
        postfix_smtp_sasl_user: myemail@example.com
        postfix_smtp_sasl_password: password
      roles:
        - ansible-role-postfix

## Contributing

Please feel free to open issues if you find any bugs, problems or if you see room for improvement. Also feel free to contact me anytime if you want to ask or discuss something.

## Disclaimer

This role is provided AS IS and I can and will not guarantee that the role works as intended, nor can I be accountable for any damage or misconfiguration done by this role. Study the role thoroughly before using it.

## License

MIT

## Author Information

This role was created in 2021 by [Thorian93](http://thorian93.de/).
