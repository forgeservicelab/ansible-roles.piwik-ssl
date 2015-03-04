Role Name
=========

This role creates and enables necessary site configurations for apache to run piwik over ssl. This role also disables default http site configuration 00-default of apache for the sake of hardening apache a bit. Note! Ubuntu's self signed certificates are used by default and should be replaced with properly signed certificates.

Requirements
------------

In order to be able to have a proper ssl capable Piwik site configuration for Apache, you must provide information about the proper server certificate locations in the target system.

Define the server certificate locations in ./roles/piwik-ssl/defaults/main.yml

Role Variables
--------------

./defaults/main.yml

/* the server certificate locations */
sslcert: '/etc/ssl/certs/ssl-cert-snakeoil.pem'
sslkey: '/etc/ssl/private/ssl-cert-snakeoil.key'
#sslchain: '/etc/ssl/forgeservicelab.fi.crt.chain'

/* misc apache related settings */
piwik_ssl:apache:user - the user the piwik is installed for. e.g. www-data
piwik_ssl:apache:group - the group the piwik is installed for. e.g. www-data
piwik_ssl:apache:service - the service to be notified. This is apache2 in Ubuntu.
piwik_ssl:settings:locations:dest - the install dir of piwik
ip_range - defines the IP range where the web access to piwik is allowed from

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
Run your playbook site.yml. Extra variable IP is used to configure apache to allow web browser access to piwik from the defined IP address"

````
$ ansible-playbook -i inventory site.yml --extra-vars "ip_range=IP"
````

License
-------

MIT

Author Information
------------------

Pasi Kivikangas, pasi.kivikangas@digile.fi
