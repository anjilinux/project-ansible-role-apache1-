Apache
======

[![Build Status](https://travis-ci.org/jebovic/ansible-apache.svg?branch=master)](https://travis-ci.org/jebovic/ansible-apache) [![Ansible Galaxy](https://img.shields.io/badge/galaxy-jebovic.apache-blue.svg?style=flat)](https://galaxy.ansible.com/jebovic/apache)

Install and configure apache 2.4 with main macros and custom vhosts

Role Variables
--------------

```yaml
# Default apache configuration
apache_user: www-data
apache_user_group: www-data
apache_port: 80
apache_www_root: /var/www
apache_listen:
  - "*:{{ apache_port }}"
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

# Add virtualhosts to sites-available and sites-enabled directories
apache_vhosts:
  - custom_vhosts

# Add your own vhosts, simply override apache_custom_vhosts to fit your needs
apache_custom_vhosts:
  - ServerName: "{{ ansible_host }}"
    DocumentRoot: "{{ apache_www_root }}"
    macro: CommonNoFpmMacro # choose between CommonNoFpmMacro for a static website and CommonMacro for dynamic website with php
    misc: no # add your custom config here, free format

# Specific link with php fpm
php_fpm_socket_path: /var/run/php-fpm.sock
```

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
     - { role: jebovic.apache }
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
