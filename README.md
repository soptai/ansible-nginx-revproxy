ansible-nginx-revproxy
=========

Install and configures Nginx as reverse proxy for multiple website.

|Travis|Quality|Downloads|Galaxy|Version|
|------|-------|---------|-------|-------|
|[![travis](https://img.shields.io/travis/hispanico/ansible-nginx-revproxy/master)](https://travis-ci.org/hispanico/ansible-nginx-revproxy)|[![quality](https://img.shields.io/ansible/quality/19794)](https://galaxy.ansible.com/hispanico/nginx-revproxy)|[![downloads](https://img.shields.io/ansible/role/d/19794)](https://galaxy.ansible.com/hispanico/nginx-revproxy)|[![Galaxy](https://img.shields.io/badge/galaxy-hispanico.nginx--revproxy-blue.svg)](https://galaxy.ansible.com/hispanico/nginx-revproxy/19794)|[![Version](https://img.shields.io/github/release/hispanico/ansible-nginx-revproxy.svg)](https://github.com/hispanico/ansible-nginx-revproxy/releases/)|

Requirements
------------

This role requires Ansible 2.4 or higher.

Role Variables
--------------

Default values:

```yaml
nginx_revproxy_sites:                                         # List of sites to reverse proxy
  default:                                                    # Set default site to return 444 (Connection Closed Without Response)
    ssl: false                                                # Set to True if you want to redirect http to https
    letsencrypt: false

  example.com:                                                # Domain name
    domains:                                                  # List of server_name aliases
      - example.com
      - www.example.com
    upstreams:                                                # List of Upstreams
      - { backend_address: 192.168.0.100, backend_port: 80 }
      - { backend_address: 192.168.0.101, backend_port: 8080 }
    auth:                                                     # Define this block for a single HTTP user/password, or leave undefined for unauthenticated vhosts
      login: myusername
      password: mysecretpassword
    listen: 9000                                              # Specify which port you want to listen to with clear HTTP, or leave undefined for 80
    ssl: false                                                # Set to True if you want to redirect http to https
    letsencrypt: false                                        # Set to True if you are using hispanico.letsencrypt-nginx-revproxy role

  example.org:                                                # Domain name
    domains:                                                  # List of server_name aliases
      - example.org
      - www.example.org
    upstreams:                                                # List of Upstreams
      - { backend_address: 192.168.0.200, backend_port: 80 }
      - { backend_address: 192.168.0.201, backend_port: 8080 }
    proxy_directives:                                         # List of custom directives for the proxy configuration
      - { proxy_hide_header: Content-Security-Policy }
      - { proxy_set_header: Content-Security-Policy "frame-ancestors 'none';" }
    listen: 9000                                              # Specify which port you want to listen to with clear HTTP, or leave undefined for 80
    listen_ssl: 9001                                          # Specify which port you want to listen to with HTTPS, or leave undefined for 443
    ssl: true                                                 # Set to True if you want to redirect http to https
    ssl_certificate: /etc/ssl/certs/ssl-cert-snakeoil.pem     # ssl certificate, used if letsencrypt is false
    ssl_certificate_key: /etc/ssl/private/ssl-cert-snakeoil.key # ssl certificate key, used if letsencrypt is false
    letsencrypt: false                                        # Set to True if you want use letsencrypt
    letsencrypt_email: ""                                     # Set email for letencrypt cert

nginx_revproxy_certbot_auto: true                             # Install certbot-auto

nginx_revproxy_certbot_packages:                              # Install these packages from repo, when not using certbot-auto
  - certbot
  - python3-certbot-nginx
```

Dependencies
------------

None.

Example Playbook
----------------

```yaml
  - hosts: all
    roles:
      - ansible-nginx-revproxy
    vars:
      nginx_revproxy_sites:
        default:
          ssl: false
          letsencrypt: false

        example.com:
          domains:
            - example.com
            - www.example.com
          upstreams:
            - { backend_address: 192.168.0.100, backend_port: 80 }
            - { backend_address: 192.168.0.101, backend_port: 80 }
          ssl: true
          letsencrypt: false
```

License
-------

Licensed under the GPLv3 License. See the LICENSE file for details.

Author Information
------------------

Hispanico
