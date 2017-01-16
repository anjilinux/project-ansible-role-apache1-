Apache
======

[![Build Status](https://travis-ci.org/jebovic/ansible-apache.svg?branch=master)](https://travis-ci.org/jebovic/ansible-apache) [![Ansible Galaxy](https://img.shields.io/badge/galaxy-jebovic.apache-blue.svg?style=flat)](https://galaxy.ansible.com/jebovic/apache)

Install and configure apache 2.4 with main macros and custom vhosts

This role is a part of my [OPS project](https://github.com/jebovic/ops), follow this link to see it in action. OPS provides a lot of stuff, like a vagrant file for development VMs, playbooks for roles orchestration, inventory files, examples for roles configuration, ansible configuration file, and many more.

Compatibility
-------------

Tested and approved on :

* Debian jessie (8+)
* Ubuntu Trusty (14.04 LTS)
* Ubuntu Xenial (16.04 LTS)

Role Variables
--------------

```yaml
# Apache install configuration
apache_apt_packages:
    - apache2
    - apache2-utils

# Default apache configuration
apache_user: www-data
apache_user_group: www-data
apache_port: 80
apache_www_root: /var/www
apache_listen:
  - "*:{{ apache_port }}"
  - "*:443"
apache_default_servername: "{{ ansible_fqdn }}"

# see macros name in templates/macros directory
apache_macros:
  - common-nofpm
  - common
  - commonREST
  - php-fpm
  - ssl-with-chain
  - ssl

# list modules you want to load in apache2.conf
apache_modules:
  - proxy.load
  - proxy_fcgi.load
  - proxy_http.load
  - proxy_ftp.load
  - proxy_connect.load
  - macro.load
  - rewrite.load
  - headers.load
  - ssl.load

# Add virtualhosts to sites-available and sites-enabled directories
apache_vhosts:
  - custom_vhosts

# Add your own vhosts, simply override apache_custom_vhosts to fit your needs
apache_custom_vhosts:
  - ServerName: "{{ ansible_host }}"
    DocumentRoot: "{{ apache_www_root }}"
    macro: CommonNoFpmMacro # choose between false, CommonNoFpmMacro for a static website and CommonMacro for dynamic website with php, if false, use 'misc' key for writing your vhost
    misc: no # add your custom config here, free format

# Specific link with php fpm
php_fpm_socket_path: /var/run/php-fpm.sock

```

Example : Playbook
------------------

```yaml
- hosts: servers
  roles:
     - { role: jebovic.apache }
```

Example : config
----------------

```yaml
# Apache only listen on port 443
apache_port: 443
# Add your vhost, see templates/macros for more informations
apache_custom_vhosts:
  - ServerName: www.example.com
    DocumentRoot: /var/www/example.com
    macro: no
    misc: |
           ServerAlias example.com
           RewriteEngine On
           RewriteRule (.*) index.php [P,QSA,L]
           Use sslMacro /path/to/cert /path/to/key
```

Tags
----

* apache_config : only update config and restart service


License
-------

MIT

Author Information
------------------

Jérémy Baumgarth https://github.com/jebovic
