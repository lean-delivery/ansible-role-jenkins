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
  - `jenkins2_smtp_enabled` - to set the smtp configuration
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
     default: `2000`
  - `jenkins2_plugins_list` - list of plugins (will be merged with suggested list)
     default: `[]`
# credentials configuration
  - `jenkins2_credentials_enabled` - to add credentials
     default: `True`
  - `jenkins2_credentials` -  credentials map
# pipeline libraries configuration
  - `jenkins2_pipeline_libraries_enabled` - to configure Global Pipeline Libraries
     default: `False`
  - `jenkins2_pipeline_libraries_name` - Library Name to be used in the @Library annotation
     default: `Pipeline_Libraries`
  - `jenkins2_pipeline_libraries_url` - URL of remote repository
     default: `''`
  - `jenkins2_pipeline_libraries_version` - A default version of the library to load if a script does not select another. Might be a branch name, tag, commit hash, etc., according to the SCM.
     default: 'master'
# bitbucket project configuration
  - `jenkins2_bitbucket_project_enabled` - to configure Bitbucket Team/Project
     default: `False`
  - `jenkins2_bitbucket_project_owner` - Name of the Bitbucket Team or Bitbucket User Account. It could be a Bitbucket Project also, if using Bitbucket Server. In this case (Bitbucket Server): Use the project key, not the project name. If using a user account instead of a project, add a "~" character before the username, i.e. "~joe".
     default: `Bitbucket_Project_Owner`
  - `jenkins2_bitbucket_project_repo_regexp` - A Java regular expression to restrict the repo names. Repo names that do not match the supplied regular expression will be ignored.
     default: `''`
  - `jenkins2_bitbucket_project_discover_branches_strategy` - Determines which branches are discovered. 1 - Exclude branches that are also filed as PRs, 2 - Only branches that are also filed as PRs, 3 - All branches.
     default: 1
  - `jenkins2_bitbucket_project_discover_pr_strategy` - Determines how pull requests are discovered. 1 - Merging the pull request with the current target branch revision, 2 - The current pull request revision, 3 - Both the current pull request revision and the pull request merged with the current target branch revision.
     default: 1
  - `jenkins2_bitbucket_project_scan_interval` - The maximum amount of time since the last indexing that is allowed to elapse before an indexing is triggered.
     default: 60
  - `jenkins2_bitbucket_project_autobuild_branches` - Matching branches will be triggered automatically.
     default: 'master|develop|PR-[0-9]+'
# sonarqube configuration
  - `jenkins2_sonarqube_enabled` - to add SonarQube configuration
     default: `False`
  - `jenkins2_sonarqube_name` - SonarQube Name
     default: `SonarQube`
  - `jenkins2_sonarqube_url` - SonarQube Server URL
     default: `http://localhost:9000`
  - `jenkins2_sonarqube_token` - SonarQube authentication token. Mandatory when anonymous access is disabled.
     default: `''`
  - `jenkins2_sonarqube_maven_plugin` - Version of sonar-maven-plugin. If not specified, the goal will be sonar:sonar.
     default: `''`
  - `jenkins2_sonarqube_arguments - Additional command line arguments to be passed to the SonarQube scanner. For example, -X.
     default: `''`
  - `jenkins2_sonarqube_analysis_properties - Additional analysis properties in the form of key-value pairs. For example, sonar.analysis.mode=issues.
     default: `''`
# custom files copy
  - `jenkins2_custom_files_enabled` - to copy custom files to Jenkins
     default: `False`
  - `jenkins2_custom_files` - map with files parameters
      default:
    `log_parser:
      src: "{{ role_path }}/files/pipeline_error_in_init_console"
      dest: "{{ jenkins2_home_directory }}"
      owner: "{{ jenkins2_user }}"
      group: "{{ jenkins2_user }}"
      mode: "0755"`
# Gitlab configuration
  - `jenkins2_gitlab_enabled` - to enable gitlab config
     default: `False`
  - `gitlab_external_url` - gitlab external url
     default: `https://localhost`
  - `jenkins2_gitlab_token_cred` - gitlab token ID (from credentials: gitlabtoken in example below)
     default: `gitlab_token`

Use variable `jenkins2_credentials` to set properties of credentials
# To set AWS credentials set variables:
- aws_access_key
- aws_secret_key
# To set Gitlab connection please set:
- gitlab_master_password
- gitlab_master_username
- gitlab_master_token

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
  pipeline_libraries:
    type: 'password'
    id: 'pipeline_libraries'
    description: 'username and password for pipeline libraries'
    username: 'pipelineLibrariesUser'
    password: 'pipelineLibrariesPassword'
  sshconnection:
    type: 'key'
    keySource: 0
    key: >
      -----BEGIN RSA PRIVATE KEY-----
      MIIEpQIBAAKCAQEAp4aKtv4Tw760QLb5eG4M0nKeO8cPPf68aSbYwhSNqn4n3+Pq
      WGfP25rVmZqW6UATPdAaw4k5OahCgYCdgmEgiilj5ZJeb3pgQ1Hi4AP0+TNHBDgU
      ltY4x+oLbJmg4Kw5kjojeBoz9nNJxpBCI5FaTE9kPaAuqNaeLgK1EAC/CTwHbR4x
      6R79loSkqgltV8ex/QXf3y3D2ICS45FCoyOR+VkMZz4a2qjadc/Vvh3TtSWka+sW
      NzGJkv20TQXSNWGt6vWDMxWHI5CimfKYRQJbnkqYhULCdXVhZd86rmldR9OPDwhC
      MXDN6bsymGhYllNqk5FI7rRbJwn9Hp1Fkh4i9wIDAQABAoIBAGAUbuxCzm0eq9zN
      1Eh4vYvZRt077ob82WxpZjnxdcqJri7fVaE8cN8fb1A4dEL0h9SUzTTGN/bUhMgn
      Tq7fK71xvaSGDPlcs2cIyNj4DAD8osdNrqlP1aL6nXC8r6MLw70U8RDJ3nGDb5Wd
      WQFzNNm9Ut1xSDF3XaM/1D6IXwhY4llrhjjNvXlgyaAWCQ7byJbuoNd5NFGiGGII
      XA/3Cqb9TfssRqZ0ougBTDYSQmqev74Jc9OkRONuRnjPBX4tAj1hGl8KzBmx70bl
      mAexMQxODYUtUnAe/h+QL5aGmt6dlShwHR9Dfpn3GtSB6rtSUHm3lhg/4pDhLD3U
      rhVo8ckCgYEA3Ibsh5FYgYZGhEWb0/n1Hoa2kiav6E2WDRHwu6LAw4Y8c2dHZ5ov
      rl0aS9DqKUJqQI8LBVhZCaKbLXFZKTQBbWe6FsT5VidG9UIrYGXLT09db1Oww1PG
      s4FW6rJy1Y6VtBJxQdRz1gAse8TXED6uGzAih7q0jLkxdk/s03LbUXMCgYEAwnkS
      ONnna4obkzJ1iGNCOvT2kP2rVcx9AP/R8kN8ZAJdbHXZ57KE8h45e/R4/YOppTNt
      KSTOZwbjCOy0f1eLK9bJFZ4qm+YsNOoQFpQW9QFEqwjO76CEpdMpo+/aGxGAI+rT
      RFcEhAkz6AKlXADzLhw8/S5DvvoR1tXEKfCgd20CgYEAr1+FPpJxxh+YeJw6viqr
      qikLi9LEVYNN7vrzbOSTU2qvLD9X46YUgR99SAnODh3JDaoz435M4IK10T+w3jmD
      YRP6Qx1GBCOcJHMIt9J8CohdD6mIiu1WuW4ERwS+meKYXunDs8xWijr9JTh2p26R
      WwG+lB4Ac2DbWvFYrxdKHs0CgYEAicmCcaliYD1wIDDmOYYqTN93O2+fz6CdCPI4
      bHAIWEucqdYuWA2SSIHFtN+YQfbhYd04AKjFXRXyEkaz9G2we5Uo0BpKkj7ZH3yf
      fX/bbChD4PLSu9F9aohcvnyYigkyQ0CEA62r02k5z67gPnml0wvK4o+/DDbeINtA
      q36EE1kCgYEAnWDOCnLJKJElmJzhUhunzef6XjFHyBZ353D3VLCdoz8Aww24dMV/
      ZBoiF+26kJHPMQozRHWW1nUCaU2q2x2QgARcLvT/xTN6ipWrhjnoPU082iFdtFjV
      criSr1+d5Xyr4Ht0Cn5jb57Kt1yJSxgBo1EHGYMXR6abcTUMOyXgAkI=
      -----END RSA PRIVATE KEY-----
    ID: 'sshagent'
    username: 'hybris'
    passphrase: ''
    description: 'credentials for hybris user to connect remotely'
  cidbsysuser:
    type: 'password'
    id: 'ciDBsysUser'
    description: 'sys user for CI db'
    username: 'sys'
    password: 'syspassword'
  fqa1dbuser:
    type: 'password'
    id: 'fqa1DBuser'
    description: 'fqa1 DB user'
    username: 'fqa1user'
    password: 'fqa1password'
  dbtempuser:
    type: 'password'
    id: 'tempDBuser'
    description: 'DB user for datacut'
    username: 'DB_TEMP'
    password: 'DB_TEMP'
  gitlab_creds:
    type: 'password'
    ID: 'GIT_CREDENTIALS'
    description: 'gitlab credentials username with password'
    username: "{{ gitlab_master_username | default('admin') }}"
    password: "{{ gitlab_master_password | default('password') }}"
  jenkinshttpconnectionuser:
    type: 'password'
    ID: 'Jenkins_http_connection'
    description: 'for connection via jenkins cli; used for pipeline syntax check'
    username: '{{ jenkins2_cli_username }}'
    password: '{{ jenkins2_cli_password }}'
  gitlabusertoken:
    type: 'password'
    ID: 'GIT_CREDENTIALS_TOKEN'
    description: 'to use with GL10.2+ but could be used with earlier versions'
    username: "{{ gitlab_master_username | default('admin') }}"
    password: "{{ gitlab_master_token | default('ToKen12345') }}"
  aws_credentials:
    type: 'password'
    ID: 'AWS_CREDENTIALS'
    description: 'for operations in AWS'
    username: "{{ aws_access_key | default('AWSaccessKey') }}"
    password: "{{ aws_secret_key | default('AWSsecretKey') }}"
  gitlabtoken:
    type: 'gitlabtoken'
    ID: 'gitlab_token'
    description: 'gitlab connection with token'
  aws_ec2_credentials:
    type: 'aws_creds'
    ID: 'AWS_EC2_CREDS'
    description: 'for ec2 plugin to create ec2 for slave instances'
    access_key: "{{ aws_access_key | default('AWSaccessKey') }}"
    sec_key: "{{ aws_secret_key | default('AWSsecretKey') }}"
  bitbucket_project:
    type: 'password'
    id: 'bitbucket_project'
    description: 'username and password for bitbucket project'
    username: 'bitbucketProjectUser'
    password: 'bitbucketProjectPassword'
```
`type` has available options:
  1. `key` - if you want to configure SSH Private key
  2. `password` - if you want to configure username/password bundle
  3. `gitlabtoken` - if you want to configure gitlab token (need gitlab plugin to be installed)
  4. `aws_creds` - if you want to configure ec2 creds (need ec2 plugin to be installed)

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
