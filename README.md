Jenkins 2 installation
=========
[![License](https://img.shields.io/badge/license-Apache-green.svg?style=flat)](https://raw.githubusercontent.com/lean-delivery/ansible-role-jenkins/master/LICENSE)
[![Build Status](https://travis-ci.org/lean-delivery/ansible-role-jenkins.svg?branch=master)](https://travis-ci.org/lean-delivery/ansible-role-jenkins)
[![Galaxy](https://img.shields.io/badge/galaxy-lean_delivery.jenkins-blue.svg)](https://galaxy.ansible.com/lean_delivery/jenkins)
![Ansible](https://img.shields.io/ansible/role/d/29895.svg)
![Ansible](https://img.shields.io/badge/dynamic/json.svg?label=min_ansible_version&url=https%3A%2F%2Fgalaxy.ansible.com%2Fapi%2Fv1%2Froles%2F29895%2F&query=$.min_ansible_version)
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
     default: `false`
  - `jenkins2_key_validate_certs` - validate certs during key install (only for yum)   
     default: `true`
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
# Enable HTTPS
  - `jenkins2_https_enabled` - Use or not use secure connection.   
     default: `true`
  - `jenkins2_http_disabled` - Disable insecure connection.   
     default: `false`
  - `jenkins2_config_https_port` - Jenkins port for secure connection (https).   
     default: `8443`
  - `jenkins2_local_keystore` - If true - to search for keystore on ansible host on {{ jenkins2_local_keystore_path }}. If false - to check keystore on remote host.    
     default: `false`
  - `jenkins2_local_keystore_path` - Path to local keystore file (in order not to create self-signed)  
     default: `{{ role_path }}/files/{{ jenkins2_ssl_key_store_name }}`
  - `jenkins2_ssl_configure` - Configure or not ssl connection for jenkins.   
     default: `true`
  - `jenkins2_ssl_key_store_name` - Keystore file name. If file with such name exists in role folder/files - it will be used as keystore.  
     default: `jenkins2-ssl.keystore.jks`
  - `jenkins2_ssl_key_size` - Certificate key size.   
     default: `4096`
  - `jenkins2_ssl_key_store_password` - Keystore password.   
     default: `123456`
  - `jenkins2_ssl_key_store` - Full path to keystore file.   
     default: '{{ jenkins2_home_directory }}/{{ jenkins2_ssl_key_store_name }}'
  - `jenkins2_ssl_certificate_provider` - Only for Linux os. https://docs.ansible.com/ansible/latest/openssl_certificate_module.html.   
     default: `selfsigned`
# Certificate
  - `jenkins2_ca_domain` - Certificate domain name.   
     default: `example.com`
  - `jenkins2_local_pkey_file_name` - Private cert name.   
     default: `{{ ansible_hostname }}.ca-pkey.pem`
  - `jenkins2_local_cert_file_name` - Public cert name.   
     default: `{{ ansible_hostname }}.ca-cert.pem`
# Jenkins proxy config
  - `jenkins2_proxy_url` - Proxy url for plugins download config   
     default: `''`
  - `jenkins2_proxy_host` - Proxy server host name   
     default: `''`
  - `jenkins2_proxy_port` - Proxy server port   
     default: `0`
  - `jenkins2_proxy_username` - Username for proxy connection   
     default: `''`
  - `jenkins2_proxy_password` - Password for proxy connection   
     default: `''`
  - `jenkins2_proxy_no_proxy_hosts` - Hosts to skip from proxying   
     default: `''`
# ssh key generation config
  - `jenkins2_ssh_keys_generate` - add ability to generate SSH key pairs under jenkins user   
     default: `false`
  - `jenkins2_ssh_keys_private_keyname` - private key name   
     default: `id_rsa`
  - `jenkins2_ssh_keys_size` - ssh rsa key strength size   
     default: `4096`
  - `jenkins2_ssh_keys_inv_slave_groupname` - slave group name in inventory file   
     default: `jenkins_ssh_slaves`
# smtp settings
  - `jenkins2_smtp_enabled` - to set the smtp configuration   
     default: `true`
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
     default: `true`
  - `jenkins2_credentials` - credentials map
# pipeline libraries configuration
  - `jenkins2_pipeline_libraries_enabled` - to configure Global Pipeline Libraries   
     default: `false`
  - `jenkins2_pipeline_libraries_name` - Library Name to be used in the @Library annotation   
     default: `Pipeline_Libraries`
  - `jenkins2_pipeline_libraries_url` - URL of remote repository   
     default: `''`
  - `jenkins2_pipeline_libraries_version` - A default version of the library to load if a script does not select another. Might be a branch name, tag, commit hash, etc., according to the SCM.   
     default: `'master'`
# bitbucket project configuration
  - `jenkins2_bitbucket_project_enabled` - to configure Bitbucket Team/Project   
     default: `false`
  - `jenkins2_bitbucket_project_owner` - Name of the Bitbucket Team or Bitbucket User Account. It could be a Bitbucket Project also, if using Bitbucket Server. In this case (Bitbucket Server): Use the project key, not the project name. If using a user account instead of a project, add a "~" character before the username, i.e. "~joe".   
     default: `Bitbucket_Project_Owner`
  - `jenkins2_bitbucket_project_repo_regexp` - A Java regular expression to restrict the repo names. Repo names that do not match the supplied regular expression will be ignored.   
     default: `''`   
  - `jenkins2_bitbucket_project_discover_branches_strategy` - Determines which branches are discovered. 1 - Exclude branches that are also filed as PRs, 2 - Only branches that are also filed as PRs, 3 - All branches.   
     default: `1`
  - `jenkins2_bitbucket_project_discover_pr_strategy` - Determines how pull requests are discovered. 1 - Merging the pull request with the current target branch revision, 2 - The current pull request revision, 3 - Both the current pull request revision and the pull request merged with the current target branch revision.   
     default: `1`
  - `jenkins2_bitbucket_project_scan_interval` - The maximum amount of time since the last indexing that is allowed to elapse before an indexing is triggered.
     default: `60`
  - `jenkins2_bitbucket_project_autobuild_branches` - Matching branches will be triggered automatically.   
     default: `'master|develop|PR-[0-9]+'`
# sonarqube configuration
  - `jenkins2_sonarqube_enabled` - to add SonarQube configuration   
     default: `false`
  - `jenkins2_sonarqube_name` - SonarQube Name   
     default: `SonarQube`
  - `jenkins2_sonarqube_url` - SonarQube Server URL   
     default: `http://localhost:9000`
  - `jenkins2_sonarqube_token` - SonarQube authentication token. Mandatory when anonymous access is disabled.   
     default: `''`
  - `jenkins2_sonarqube_maven_plugin` - Version of sonar-maven-plugin. If not specified, the goal will be sonar:sonar.   
     default: `''`
  - `jenkins2_sonarqube_arguments` - Additional command line arguments to be passed to the SonarQube scanner. For example, -X.   
     default: `''`
  - `jenkins2_sonarqube_analysis_properties` - Additional analysis properties in the form of key-value pairs. For example, sonar.analysis.mode=issues.   
     default: `''`
# custom files copy
  - `jenkins2_custom_files_enabled` - to copy custom files to Jenkins   
     default: `false`
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
     default: `false`
  - `gitlab_external_url` - gitlab external url   
     default: `https://localhost`
  - `jenkins2_gitlab_token_cred` - gitlab token ID (from credentials: gitlabtoken in example below)   
     default: `gitlab_token`
# Jira configuration
  - `jenkins2_jira_enabled` - to enable jira config   
     default: `false`
  - `jenkins2_jira_url` - jira url   
     default: `https://jira.example.com`
  - `jenkins2_jira_alternative_url` - jira alternative url   
     default: `https://jira.example.com`
  - `jenkins2_jira_use_http_auth` - This option forces Jenkins to connect to JIRA using HTTP Basic Authentication, instead of logging in over RPC   
     default: `true`
  - `jenkins2_jira_support_wiki_notation` - if true JIRA supports Wiki notations in comments. When true, Jenkins will post comments that take advantage of the Wiki notation. If false, Jenkins will only post plain-text comments   
     default: `true`
  - `jenkins2_jira_record_scm_changes` - If true, scm changes will be recorded in JIRA : link to the scm repository browser and paths changes   
     default: `false`
  - `jenkins2_jira_disable_changelog_annotations` - Disable creating JIRA hyperlinks in the changeset   
     default: `false`
  - `jenkins2_jira_issue_pattern` - You can define your own pattern to search for JIRA issue ids in the SCM logs   
     default: `PATTERN.*`
  - `jenkins2_jira_update_relevant_issues` - If false, issues will be only updated if the build is SUCCESSful or UNSTABLE. If true, related JIRA issues will be always updated, regardless of the build result   
     default: `true`
  - `jenkins2_jira_credentials_id` - Jenkins credentials ID for jira   
     default: `jirauser`
  - `jenkins2_jira_connection_timeout` - Connection timeout for JIRA REST API calls (in seconds)   
     default: `10`
  - `jenkins2_jira_read_timeout` - Read timeout for JIRA REST API calls (in seconds)   
     default: `30`
  - `jenkins2_jira_thread_executor_size` - Size of the Thread Pool Executor to query Jira   
     default: `10`
  - `jenkins2_jira_visible_for_group` - Enter the name of the JIRA group that has permission to view the comment, leave the field empty to make the comment available to all JIRA users   
     default: ``
  - `jenkins2_jira_visible_for_project_role` - Enter the name of the JIRA project role that has permission to view the comment, leave the field empty to make the comment available to all JIRA users   
     default: ``
  - `jenkins2_jira_add_timestamps_for_comments` - If true, SCM change entries date and time will be recorded in JIRA   
     default: `true`
  - `jenkins2_jira_timestamp_format` - See javadoc for SimpleDateFormat for help. If not set, DateFormat.SHORT for the current locale will be used.    
     default: `EEE, d MMM yyyy HH:mm:ss Z`
	 
# Enable Permissive Script Security Plugin
  - `jenkins2_permissive_script_enabled` - Use or not use permissive script security.   
     default: `True`

# Seed job configuration
### More info about DSL https://jenkinsci.github.io/job-dsl-plugin/ https://github.com/jenkinsci/job-dsl-plugin
- `jenkins2_seed_job_enable` - enable jenkins seed job   
   default: `false`
- `jenkins2_seed_job_template` - DSL template file name without .j2. ***Do not forget to setup all needed variables for template*** For default template need following variables: gitlab_external_url, gitlab_project_group, gitlab_project_name, ci_test_jenkins_slave_label, feature_verification_jenkins_slave_label   
   default: `dsl_hybris_create_job.groovy`
- `jenkins2_seed_job_ignore_existing` - to ignore existing jobs and do not rewrite them   
   default: `"true"`
- `jenkins2_seed_job_name` - Jenkins seed job name   
   default: `"seed_job"`


Use variable `jenkins2_credentials` to set properties of credentials
# To set AWS credentials set variables:
- aws_access_key
- aws_secret_key
# To set Gitlab connection please set:
- gitlab_master_password
- gitlab_master_username
- gitlab_master_token

```yml
jenkins2_credentials_enabled: true # Set false to disable credentials configuration
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
    id: 'sshagent'
    username: 'hybris'
    passphrase: ''
    description: 'credentials for hybris user to connect remotely'
  cidbsysuser:
    type: 'password'
    id: 'ciDBsysUser'
    description: 'sys user for CI db'
    username: 'root'
    password: 'Qwerty_123'
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
    id: 'GIT_CREDENTIALS'
    description: 'gitlab credentials username with password'
    username: "{{ gitlab_master_username | default('admin') }}"
    password: "{{ gitlab_master_password | default('password') }}"
  jenkinshttpconnectionuser:
    type: 'password'
    id: 'Jenkins_http_connection'
    description: 'for connection via jenkins cli; used for pipeline syntax check'
    username: '{{ jenkins2_cli_username }}'
    password: '{{ jenkins2_cli_password }}'
  gitlabusertoken:
    type: 'password'
    id: 'GIT_CREDENTIALS_TOKEN'
    description: 'to use with GL10.2+ but could be used with earlier versions'
    username: "{{ gitlab_master_username | default('admin') }}"
    password: "{{ gitlab_master_token | default('ToKen12345') }}"
  aws_credentials:
    type: 'password'
    id: 'AWS_CREDENTIALS'
    description: 'for operations in AWS'
    username: "{{ aws_access_key | default('AWSaccessKey') }}"
    password: "{{ aws_secret_key | default('AWSsecretKey') }}"
  gitlabtoken:
    type: 'gitlabtoken'
    id: 'gitlab_token'
    description: 'gitlab connection with token'
  aws_ec2_credentials:
    type: 'aws_creds'
    id: 'AWS_EC2_CREDS'
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

# EC2 plugin configuration
- `jenkins2_ec2_enable` - enable jenkins ec2 plugin configure https://wiki.jenkins.io/display/JENKINS/Amazon+EC2+Plugin   
   default: `false`
```yml
jenkins2_ec2_cloud_parameters:
  cloud_name: "AWS_cloud"
  credentials_id: "AWS_EC2_CREDS"
  instance_cap_str: "2"
  private_key: |
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
  region: "us-east-1"
  use_instance_profile_for_credentials: "false"
  role_arn: ""
  role_session_name: ""

jenkins2_ec2_ami_list:
  ci_ami:
    ami: "ami-AAAAAAAA"
    associatePublicIp: "false"
    connectBySSHProcess: "true"
    connectUsingPublicIp: "false"
    customDeviceMapping: ""
    deleteRootOnTermination: "true"
    description: "Jenkins slave for CI"
    ebsOptimized: "false"
    iamInstanceProfile: ""
    idleTerminationMinutes: "5"
    initScript: ""
    instanceCapStr: "5"
    jvmopts: ""
    labelString: "ci_slaves"
    launchTimeoutStr: ""
    numExecutors: "1"
    remoteAdmin: "ec2-user"
    remoteFS: "/opt/jenkins"
    securityGroups: "sg-11111111"
    stopOnTerminate: "false"
    subnetId: "subnet-SSSSSSSS"
    tags:
      - {name: "Name", value: "CI Slave"}
      - {name: "Name2", value: "CI2 Slave"}
    tmpDir: ""
    type: "t2.medium"
    useDedicatedTenancy: "false"
    useEphemeralDevices: "true"
    usePrivateDnsName: "true"
    userData: ""
    zone: "us-east-1a,us-east-1b"
    monitoring: "false"
    SpotConfiguration:
      enabled: true
      spotMaxBidPrice: "0.001"
  fv_ami:
    ami: "ami-BBBBBBBB"
    associatePublicIp: "false"
    connectBySSHProcess: "true"
    connectUsingPublicIp: "false"
    customDeviceMapping: ""
    deleteRootOnTermination: "true"
    description: "Jenkins slave for FV"
    ebsOptimized: "false"
    iamInstanceProfile: ""
    idleTerminationMinutes: "5"
    initScript: ""
    instanceCapStr: "5"
    jvmopts: ""
    labelString: "ci_slaves"
    launchTimeoutStr: ""
    numExecutors: "1"
    remoteAdmin: "ec2-user"
    remoteFS: "/opt/jenkins"
    securityGroups: "sg-11111111"
    stopOnTerminate: "false"
    subnetId: "subnet-SSSSSSSS"
    tags:
      - {name: "Name", value: "FV Slave"}
    tmpDir: ""
    type: "t2.medium"
    useDedicatedTenancy: "false"
    useEphemeralDevices: "true"
    usePrivateDnsName: "true"
    userData: ""
    zone: "us-east-1a,us-east-1b"
    monitoring: "false"
    SpotConfiguration:
      enabled: false
    t2Unlimited: "false"
```

# Security and Authorization configuration
- `jenkins2_security_enable`: enable Jenkins security.   
   default: `true`
- `jenkins2_security_realm`: Select type of Jenkins security. Avaible: `basic`, `ldap`, `active-directory`
   default: `"basic"`

  ### Default `active-directory` plugin configuration
  ```yaml
  jenkins2_active_directory_domain_name: "domain.com"
  jenkins2_active_directory_domain_controller: "server.domain.com:3268"
  jenkins2_active_directory_site: "null"
  jenkins2_active_directory_bind_name: "user@domain.com"
  jenkins2_active_directory_bind_pass: "password"
  jenkins2_active_directory_start_tls: "true"
  jenkins2_active_directory_remove_irrelevant_groups: "false"
  ```

  ### Default `LDAP` plugin configuration
  ```yaml
  jenkins2_ldap_server: "ldap.server.com:3268"
  jenkins2_ldap_root_dn: "dc=server,dc=com"
  jenkins2_ldap_user_search_base: ""
  jenkins2_ldap_user_search: "mail={0}"
  jenkins2_ldap_group_search_base: ""
  jenkins2_ldap_group_search_filter: ""
  jenkins2_ldap_group_membership_filter: "null"
  jenkins2_ldap_manager_dn: "CN=User,OU=Auto Users,OU=Service,OU=Managed by Enterprise Admins,DC=server,DC=com"
  jenkins2_ldap_manager_password: "Pa$$word"
  jenkins2_ldap_inhibit_infer_root_dn: "true"
  jenkins2_ldap_disable_mail_address_resolver: "false"
  jenkins2_ldap_display_name_attribute_name: "displayname"
  jenkins2_ldap_mail_address_attribute_name: "mail"
  ```

- `jenkins2_authorization_strategy_configure`: enable authorization strategy configuration   
   default: `true`
- `jenkins2_authorization_strategy`: type of authorization strategy. Available: `logged-in`, `matrix-based`   
   default: `"logged-in"`
- `jenkins2_authorization_strategy_logged_in_allow_anonymous_read`: allow anonymous read access   
   default: `"false"`

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
    jenkins2_ssh_keys_generate: true
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
