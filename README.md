# libertempo

Ansible role to install/update and configure libertempo on RHEL/CentOS based distrib.

Role Variables
--------------
These are settable variables for this role and are pre-defined into `defaults/main.yml` with default values.
You might need to override them according to your need. Most of them **are not suitable** for a production environment.

Base variables
```yaml
libertempo_version: # Defines app release version
libertempo_archive: # Defines app archive full name
libertempo_dl_url: # Defines download url to retrieve and install app
```

Variables related to PHP-fpm configuration
```yaml
libertempo_php_fpm_includedir: # Defines php-fpm pools config directory
libertempo_php_fpm_pool: # Defines app pool config absolute path
libertempo_php_fpm_user: # Defines which user php-fpm will run the app from
libertempo_php_fpm_group: # Defines which group php-fpm will run the app from
libertempo_php_fpm_owner: # Defines the owner of php-fpm socket
libertempo_php_fpm_group: # Defines the group of php-fpm socket
libertempo_php_fpm_mode: # Defines the mode of php-fpm socket (mostly if using file-based socket)
libertempo_php_fpm_whitelist: # Defines a list of IP/hostname to allow to talk to php-fpm
libertempo_php_fpm_listen: # Defines php-fpm socket method (file-based, host:port, etc)
```

Variable related to database setup
```yaml
libertempo_dbhost: # Defines database server hostname to conntect to
libertempo_dbuser: # Defines database user
libertempo_dbpass: # Defines database password
libertempo_dbname: # Defines database name to connect to
```

Variables related to SMTP setup
```yaml
libertempo_smtp_host: # Defines SMTP server hostname to connect to
libertempo_smtp_port: # Defines SMTP server port
libertempo_smtp_auth: # Defines SMTP authentication method (TLS, SSL). Leave it blank for none.
libertempo_smtp_user: # Defines SMTP user (if authentication is set)
libertempo_smtp_pass: # Defines SMTP password (if authentication is set)

libertempo_error_report: # Defines if app has to send out any catched SQL error
libertempo_error_report_email: # Defines recipient to send notification out
```
 
Dependencies
------------
* geerlingguy.repo-remi
When variable `libertempo_install_php` is true.

* jdauphant.nginx
When variable `libertempo_install_webserver` is true.

* geerlingguy.php
when variable `libertempo_install_php` is true.

* geerlingguy.mysql
When variable `libertempo_install_dbserver` is true.

Note that if you manage to use different roles than those above, consider setting those variable to `False` and make sure to run and deploy them first.

Also, you may replace nginx web server by apache if this is what your infrastructure is running for. This role doesn't come with a pre-defined web configuration. It's set through NGINX variables provided by this role (see `tests/test.yml` for more details).

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
    - hosts: servers
      roles:
         - {
            role: laxathom.libertempo
            libertempo_version: "1.10.0"
        }
```

Testing
---------
* Host requirements
    * docker engine. Make sure it's installed and running.

Set up ansible env
```bash
printf '[defaults]\nroles_path=../\nhost_key_checking = False' > ansible.cfg
```

Install tests requirement
```bash
% ansible-galaxy install -r tests/requirements.yml -p tests/roles
```
then run the playbook for deployment test
```bash
% sudo ansible-playbook -vv -i tests/inventory tests/test.yml
```
Once done, you should be able to hit the deployed & running application at http://localhost:8080/
