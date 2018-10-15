Jenkins 2 installation
=========
[![License](https://img.shields.io/badge/license-Apache-green.svg?style=flat)](https://raw.githubusercontent.com/lean-delivery/ansible-role-jenkins/master/LICENSE)
[![Build Status](https://travis-ci.org/lean-delivery/ansible-role-jenkins.svg?branch=master)](https://travis-ci.org/lean-delivery/ansible-role-jenkins)
## Summary

This role:
  - installs jenkins2 on Ununtu, Centos7, RHEL7
  - make minimal configuration (e.g. smtp config, plugins install)

Role tasks
------------
  - Install Jenkins
  - Install plugins
  - Configure SMTP settings
  - Add credentials
  - Generate ssh keys and copy them to slave servers

Requirements
------------

 - Minimal Version of the ansible for installation: 2.5
 - **Java 8** [![Build Status](https://travis-ci.org/lean-delivery/ansible-role-java.svg?branch=master)](https://travis-ci.org/lean-delivery/ansible-role-java)
 - **Supported OS**:
   - CentOS
     - 7
   - RHEL
     - 7
   - Ubuntu

## Role Variables
--------------
# configuration for jenkins
  - `jenkins2_user` - OS user and group used for jenkins start
     default: `jenkins`
  - `jenkins2_package_version` - package name for jenkins install
     default: `jenkins`
  - `jenkins2_release_line` - package version to be installed: stable or latest
     default: `stable`
  - `jenkins2_disable_gpg_check` - gpg check skipped (only for yum)
     default: `False`
  - `jenkins2_key_validate_certs` - validate certs during key install (only for yum)
     default: `True`
  - `jenkins2_context_path` - Jenkins context path
     default: `''`
  - `jenkins2_cli_username` - user for jenkins cli connection
     default: `admin`
  - `jenkins2_cli_password` - user password for jenkins cli connection
     default: `admin`
  - `jenkins2_home_directory` - Directory where Jenkins store its configuration and working files (checkouts, build reports, artifacts, ...)
     default: `/var/lib/jenkins`
# Jenkins configuration parameters: used in /etc/sysconfig/jenkins or in /etc/default/jenkins
  - `jenkins2_config_java_cmd` - Java executable to run Jenkins
     default: `''`
  - `jenkins2_config_java_options` - Options to pass to java when running Jenkins.
     default: `"-Djava.awt.headless=true -Djenkins.install.runSetupWizard=false -Dmail.smtp.starttls.enable='true' -Dhudson.slaves.WorkspaceList=_"`
  - `jenkins2_config_http_port` - Port Jenkins is listening on.
     default: `8080`
  - `jenkins2_config_listen_address` - IP address Jenkins listens on for HTTP requests. Default is all interfaces (0.0.0.0).
     default: `''`
  - `jenkins2_config_debug_level` - Debug level for logs -- the higher the value, the more verbose. 5 is INFO.
     default: `5`
  - `jenkins2_config_enable_access_log` - Whether to enable access logging or not.
     default: `no`
  - `jenkins2_config_handler_max` - Maximum number of HTTP worker threads.
     default: `100`
  - `jenkins2_config_handler_idle` - Maximum number of idle HTTP worker threads.
     default: `20`
  - `jenkins2_config_args` - Pass arbitrary arguments to Jenkins. Full option list: java -jar jenkins.war --help
     default: `''`
# Jenkins proxy config
  - `jenkins2_proxy_url` - proxy url for plugins download config
     default: `''`
  - `jenkins2_proxy_host` - proxy server host name
     default: `''`
  - `jenkins2_proxy_port` - proxy server port
     default: `0`
  - `jenkins2_proxy_username` - username for proxy connection
     default: `''`
  - `jenkins2_proxy_password` - password for proxy connection
     default: `''`
  - `jenkins2_proxy_no_proxy_hosts` - hosts to skip from proxying
     default: `''`
# ssh key generation config
  - `jenkins2_ssh_keys_generate` - add ability to generate SSH key pairs under jenkins user
     default: `False`
  - `jenkins2_ssh_keys_private_keyname` - private key name
     default: `id_rsa`
  - `jenkins2_ssh_keys_size` - ssh rsa key strength size
     default: `4096`
  - `jenkins2_ssh_keys_inv_slave_groupname` - slave group name in inventory file
     default: `jenkins_ssh_slaves`
# smtp settings
  - `jenkins2_smtp_enabledenabled` - to set the smtp configuration
     default: `True`
  - `jenkins2_smtp_sysad_email` - jenkins admin email
     default: `usermail@test.com`
  - `jenkins2_smtp_user` - jenkins admin email
     default: `usermail@test.com`
  - `jenkins2_smtp_password` - smtp user password
     default: `password`
  - `jenkins2_smtp_port` - smtp server port
     default: `587`
  - `jenkins2_smtp_host` - smtp server host name
     default: `smtp.office365.com`
# jenkins plugins configuration
  - `jenkins2_plugins_timeout` - plugin installation timeout
     default: `1000`
  - `jenkins2_plugins_list` - list of plugins (will be merged with suggested list)
     default: `[]`
# credentials configuration
  - `jenkins2_credentials_enabled` - to add credentials
     default: `True`
  - `jenkins2_credentials` -  credentials map


Use variable `jenkins2_credentials` to set properties of credentials
```yml
jenkins2_credentials_enabled: True # Set False to disable credentials configuration
jenkins2_credentials:
  slave:
    type: 'password'
    id: 'ci_slave'
    description: 'credentials for jenkins slave'
    username: 'user'
    password: 'password'
  jira:
    type: 'password'
    id: 'jirauser'
    description: 'username and password for jira'
    username: 'jiraUser'
    password: 'JiraUserPassword'
```
`type` has 2 available options:
  1. `key` - if you want to configure SSH Private key
  2. `password` - if you want to configure username/password bundle

`keySource` specifies method of private key providing:
  * `0` - DirectEntryPrivateKeySource. _If this value is set you need to place your private key to variable `key` in plain text._
  * `1` - FileOnMasterPrivateKeySource. _If this value is set you need to place absolute path to your private key to variable `key`._
  * `2` - UsersPrivateKeySource. _If this value is set Jenkins will use default private keys from `~/.ssh`._
=======
* jenkins2_ssh_keys_slave_hosts - dict that describes slave hosts with remote ssh users, ex:

  jenkins2_ssh_keys_slave_hosts:
    - host: test-jenkins-s1
      users:
        - user1
        - user2

  vars:
    jenkins2_ssh_keys_slave_hosts:
      - {host: 'test-jenkins-s1', users: ['user1', 'user2']}

NOTE: - Please use <users> reserved word for vars way
      - Please use <jenkins_ssh_user> reserved word for inventory way

Example Inventory
----------------
[jenkins]
server.example.com

[jenkins_ssh_slaves]
slave1.example.com
slave2.example.com

Example Playbook
----------------

```yml
- name: Install and Configure Jenkins
  hosts: jenkins
  vars:
    jenkins2_ssh_keys_generate: True
    jenkins2_ssh_keys_slave_hosts:
      - {host: 'slave1.example.com', users: ['root', 'jenkins']}
      - {host: 'slave2.example.com', users: ['root', 'jenkins']}
  roles:
    - role: lean_delivery.java
    - role: lean_delivery.jenkins
```

License
-------

Apache

Author Information
------------------

authors:
  - Lean Delivery Team <team@lean-delivery.com>
