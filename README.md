Role Name
=========

This role creates and enables necessary site configurations for apache to run piwik over ssl. This role also disables default http site configuration 00-defaultof apache for the sake of hardening apache a bit.

Requirements
------------

None

Role Variables
--------------

piwik_ssl:apache:user - the user the piwik is installed for. e.g. www-data
piwik_ssl:apache:group - the group the piwik is installed for. e.g. www-data
piwik_ssl:apache:service - the service to be notified. This is apache2 in Ubuntu.
piwik_ssl:settings:locations:install - the install rootdir
piwik_ssl:settings:locations:dest - the install dir of piwik

Dependencies
------------

ansible-piwik (https://git.forgeservicelab.fi/ansible-roles/ansible-piwik) should be executed to deploy a default piwik prior to configuring it further with this role.

Example Usage
----------------

````
# requirements.yml
- src: git+ssh://gitlab@git.forgeservicelab.fi:10022/ansible-roles/ansible-piwik.git
  path: roles/
  name: ansible-piwik

- src: git+ssh://gitlab@git.forgeservicelab.fi:10022/ansible-roles/piwik-ssl.git
  path: roles/
  name: piwik-ssl
````

Install roles defined in your requirements.yml

````
$ ansible-galaxy install -r requirements.yml
````

````
# site.yml
- hosts: all
  sudo: True
  roles:
    - ansible-piwik
    - piwik-ssl
````
Run your playbook site.yml

````
$ ansible-playbook site.yml -h yourhostname
````

License
-------

MIT

Author Information
------------------

Pasi Kivikangas, pasi.kivikangas@digile.fi
