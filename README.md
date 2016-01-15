# javaguirre.ghost

[![Build Status](https://travis-ci.org/javaguirre/ghost-ansible.svg?branch=master)](https://travis-ci.org/javaguirre/ghost-ansible)

Ansible role managing [ghost](https://ghost.org/).

## Dependencies

I use `debops.nodejs` role as a dependency to install node, It's an external dependency.

## Variables

List of variables and default values:

```yaml
# Roles
ghost_enabled: true
ghost_remove: false

# Ghost service
ghost_production: false
ghost_version: '0.6.3'
ghost_zip_url: "https://ghost.org/zip/ghost-latest.zip"
ghost_user: ghost
ghost_install_dir: /home/ghost

# Ghost config.js
# Production
ghost_production_url: 'http://example.com'
ghost_production_host: '127.0.0.1'
ghost_production_port: '2368'
ghost_production_mail:
  transport: 'SMTP'
  options:
    service: 'Mailgun'
    auth:
      user: ''
      pass: ''
ghost_production_database:
  client: 'sqlite3'
  connection:
    filename: "{{ ghost_install_dir }}/content/data/ghost.db"
    debug: false

# Development
ghost_development_url: 'http://example.com'
ghost_development_host: '127.0.0.1'
ghost_development_port: '2368'
ghost_development_mail:
  transport: 'SMTP'
  options:
    service: 'Mailgun'
    auth:
      user: ''
      pass: ''
ghost_development_database:
  client: 'sqlite3'
  connection:
    filename: "{{ ghost_install_dir }}/content/data/ghost-dev.db"
    debug: false
```

# Usage

You could have a requirements.yml file like this:

```yaml
---
- name: nginx
  src: https://github.com/jdauphant/ansible-role-nginx

- name: nodejs
  src: debops.nodejs

- name: ghost
  src: https://github.com/javaguirre/ghost-ansible
```

Install it:

```bash
ansible-galaxy install -r requirements.yml
```

Your main ansible would look like:

```yaml
- hosts: all
  remote_user: myuser
  become: true

  vars_files:
    - vars/nginx.yml

  vars:
    ghost_user: myuser
    ghost_install_dir: /home/myuser/ghost

    ghost_production: true
    ghost_production_url: 'http://example.com'
    ghost_production_host: '127.0.0.1'

  roles:
    - nodejs
    - ghost
    - nginx
```

`vars/nginx.yml` would be something like:

```yaml
---

nginx_events_params:
  - worker_connections 512

nginx_http_params:
  - sendfile "on"
  - tcp_nopush "on"
  - tcp_nodelay "on"
  - keepalive_timeout "65"
  - access_log "/var/log/nginx/access.log"
  - error_log "/var/log/nginx/error.log"

nginx_sites:
  ghost:
    - listen 0.0.0.0:80
    - server_name example.com

    - location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header HOST $http_host;

        proxy_pass http://127.0.0.1:2368;
        proxy_redirect off;
      }
```

All set!

You have a full example in [my blog configuration][blog].


#### License

Licensed under the MIT License. See the LICENSE file for details.

#### Feedback

[Please send me your requests/questions!](https://github.com/javaguirre/ghost-ansible/issues)


[blog]: https://github.com/javaguirre/javaguirrenet-ansible
