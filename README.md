# Ansible role SSH

Manage the SSH client and server configuration.

[![Build Status](https://travis-ci.org/Caseraw/ansible_role_ssh.svg?branch=master)](https://travis-ci.org/Caseraw/ansible_role_ssh) [<img src="https://img.shields.io/ansible/role/47612">](https://galaxy.ansible.com/caseraw/ansible_role_ssh) [<img src="https://img.shields.io/ansible/role/d/47612">](https://galaxy.ansible.com/caseraw/ansible_role_ssh) [<img src="https://img.shields.io/ansible/quality/47612">](https://galaxy.ansible.com/caseraw/ansible_role_ssh)

- [Ansible role SSH](#ansible-role-ssh)
  - [License](#license)
  - [Author Information](#author-information)
  - [Requirements](#requirements)
  - [Dependencies](#dependencies)
  - [Compatibility](#compatibility)
  - [Role Variables](#role-variables)
  - [Example Playbook](#example-playbook)
  - [Useful shell commands](#useful-shell-commands)
  - [Additional documentation resources](#additional-documentation-resources)
  - [Testing with Molecule](#testing-with-molecule)
  - [CI/CD with Travis CI](#cicd-with-travis-ci)
  - [Useful links](#useful-links)

## License

MIT / BSD

## Author Information

- Made and maintained by: [Kasra Amirsarvari](https://www.linkedin.com/in/caseraw)
- Ansible Galaxy community author: <https://galaxy.ansible.com/caseraw>
- Dockerhub community user: <https://hub.docker.com/u/caseraw>

## Requirements

- Ensure privileged permissions are set for the user executing this role to:
  - Edit files such as, `/etc/ssh/ssh_config` and `/etc/ssh/sshd_config`.
  - Manage `systemd` services for `sshd`.
- Ensure network traffic to and or from the remote host for SSH connectivity.

## Dependencies

N/A

## Compatibility

Compatible with the following list of operating systems:

- CentOS 7
- CentOS 8
- RHEL 7.x
- RHEL 8.x

## Role Variables

| Variable name | Description |
|---------------|-------------|
| role_ssh_required_packages | Require packages to install SSH client and server. |
| role_ssh_client_config_list | Combined list of other lists that start with the name `role_ssh_client_config_list_`. Each list contains the client configuration parameters. |
| role_ssh_server_config | SSH server configuration parameters. |

## Example Playbook

```yaml
---
- name: Manage the SSH client and server configuration
  become: True
  gather_facts: True
  tasks:
    - import_role:
        name: ansible_role_ssh
      vars:
        role_ssh_client_config_list_default:
          - path: /etc/ssh/ssh_config
            state: present
            entries:
              - entry: Include /etc/ssh/ssh_config.d/*.conf
                entry_parameters: []
          - path: /etc/ssh/ssh_config.d/99_defaults.conf
            state: present
            entries:
              - entry: Match final all
                entry_parameters:
                  - GSSAPIAuthentication yes
                  - ForwardX11Trusted yes
                  - SendEnv LANG LC_CTYPE LC_NUMERIC LC_TIME LC_COLLATE LC_MONETARY LC_MESSAGES
                  - SendEnv LC_PAPER LC_NAME LC_ADDRESS LC_TELEPHONE LC_MEASUREMENT
                  - SendEnv LC_IDENTIFICATION LC_ALL LANGUAGE
                  - SendEnv XMODIFIERS
        role_ssh_server_config:
          path: /etc/ssh/sshd_config
          parameters:
            - HostKey /etc/ssh/ssh_host_rsa_key
            - HostKey /etc/ssh/ssh_host_ecdsa_key
            - HostKey /etc/ssh/ssh_host_ed25519_key
            - MACs hmac-sha2-512,hmac-sha2-256
            - Ciphers aes192-ctr,aes256-ctr
            - SyslogFacility AUTHPRIV
            - LogLevel INFO
            - LoginGraceTime 60
            - PermitRootLogin no
            - MaxAuthTries 4
            - PubkeyAuthentication yes
            - AuthorizedKeysFile .ssh/authorized_keys
            - PasswordAuthentication no
            - GSSAPIAuthentication yes
            - GSSAPICleanupCredentials yes
            - UsePAM yes
            - X11Forwarding yes
            - UsePrivilegeSeparation sandbox
            - ClientAliveInterval 1800
            - ClientAliveCountMax 0
            - Banner none
            - AllowTcpForwarding no
            - AllowAgentForwarding no
            - TCPKeepAlive no
            - AcceptEnv LANG LC_CTYPE LC_NUMERIC LC_TIME LC_COLLATE LC_MONETARY LC_MESSAGES
            - AcceptEnv LC_PAPER LC_NAME LC_ADDRESS LC_TELEPHONE LC_MEASUREMENT
            - AcceptEnv LC_IDENTIFICATION LC_ALL LANGUAGE
            - AcceptEnv XMODIFIERS
            - Subsystem sftp /usr/libexec/openssh/sftp-server
            - DenyUsers bin

...
```

## Useful shell commands

N/A

## Additional documentation resources

- <https://www.openssh.com/manual.html>
- <https://www.ssh.com/ssh/config>
- <https://www.ssh.com/ssh/sshd_config>

## Testing with Molecule

This role is locally tested with the use of [Molecule](https://molecule.readthedocs.io/en/latest/), the configuration is located at: [molecule/default](molecule/default).  
The Molecule tests are run (using the [docker driver](https://molecule.readthedocs.io/en/latest/configuration.html#docker)) on [Dockerhub images](https://hub.docker.com/u/caseraw) built for this purpose:

- [CentOS](https://hub.docker.com/r/caseraw/ansible-molecule-centos)
- [Fedora](https://hub.docker.com/r/caseraw/ansible-molecule-fedora)

## CI/CD with Travis CI

This role uses [Travis CI](https://travis-ci.org/) to run online tests with the use of [Molecule](https://molecule.readthedocs.io/en/latest/) and pushes notifications to import the role into [Ansible Galaxy](https://galaxy.ansible.com/) once the tests are successful. The Travis CI configuration is located at the root of the Ansible role [.travis.yml](.travis.yml)

## Useful links

- GitHub repository: <https://github.com/Caseraw/ansible_role_ssh>
- Travis CI build status: <https://travis-ci.org/Caseraw/ansible_role_ssh>
- Ansible Galaxy role: <https://galaxy.ansible.com/caseraw/ansible_role_ssh>