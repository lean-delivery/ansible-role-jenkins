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
  - `jenkins2` - configuration for jenkins
    - `user` - OS user and group used for jenkins start
       default: `jenkins`
    - `package_version` - package name for jenkins install
       default: `jenkins`
    - `release_line` - package version to be installed: stable or latest
       default: `stable`
    - `disable_gpg_check` - gpg check skipped (only for yum)
       default: `False`
    - `key_validate_certs` - validate certs during key install (only for yum)
       default: `True`
    - `context_path` - Jenkins context path
       default: `''`
    - `cli_username` - user for jenkins cli connection
       default: `admin`
    - `cli_password` - user password for jenkins cli connection
       default: `admin`
  - `jenkins2_config` - Jenkins configuration parameters: used in /etc/sysconfig/jenkins
    - `home_directory` - Directory where Jenkins store its configuration and working files (checkouts, build reports, artifacts, ...)
       default: `/var/lib/jenkins`
    - `java_cmd` - Java executable to run Jenkins
       default: `''`
    - `java_options` - Options to pass to java when running Jenkins.
       default: `"-Djava.awt.headless=true -Djenkins.install.runSetupWizard=false -Dmail.smtp.starttls.enable='true' -Dhudson.slaves.WorkspaceList=_"`
    - `http_port` - Port Jenkins is listening on.
       default: `8080`
    - `listen_address` - IP address Jenkins listens on for HTTP requests. Default is all interfaces (0.0.0.0).
       default: `''`
    - `debug_level` - Debug level for logs -- the higher the value, the more verbose. 5 is INFO.
       default: `5`
    - `enable_access_log` - Whether to enable access logging or not.
       default: `no`
    - `handler_max` - Maximum number of HTTP worker threads.
       default: `100`
    - `handler_idle` - Maximum number of idle HTTP worker threads.
       default: `20`
    - `args` - Pass arbitrary arguments to Jenkins. Full option list: java -jar jenkins.war --help
       default: `''`
  - `jenkins2_proxy` - Jenkins proxy config
    - `url` - proxy url for plugins download config
       default: `''`
    - `host` - proxy server host name
       default: `''`
    - `port` - proxy server port
       default: `0`
    - `username` - username for proxy connection
       default: `''`
    - `password` - password for proxy connection
       default: `''`
    - `no_proxy_hosts` - hosts to skip from proxying
       default: `''`
  - `jenkins2_ssh_keys` - ssh key generation config
    - `generate` - add ability to generate SSH key pairs under jenkins user
       default: `False`
    - `private_keyname` - private key name
       default: `id_rsa`
    - `size` - ssh rsa key strength size
       default: `4096`
    - `inv_slave_groupname` - slave group name in inventory file
       default: `jenkins_ssh_slaves`
  - `jenkins2_smtp` - smtp settings
    - `enabled` - to set the smtp configuration
       default: `True`
    - `sysad_email` - jenkins admin email
       default: `usermail@test.com`
    - `user` - jenkins admin email
       default: `usermail@test.com`
    - `password` - smtp user password
       default: `password`
    - `port` - smtp server port
       default: `587`
    - `host` - smtp server host name
       default: `smtp.office365.com`
  - `jenkins2_plugins` - jenkins plugins configuration
    - `timeout` - plugin installation timeout
       default: `1000`
    - `plugin_list` - list of plugins (will be merged with suggested list)
       default: `[]`
  - `jenkins2_credentials_enabled` - to add credentials
     default: `True`
  - `jenkins2_credentials` - map of credentials


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
    - role: lean-delivery.java
    - role: lean-delivery.jenkins
```

License
-------

Apache2

Author Information
------------------

DEP Infrastructure Platform
