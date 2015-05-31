# javaguirre.ghost

Ansible role which manage [ghost](https://ghost.org/)

## Dependencies

You could use Stouts.nginx for nginx configuration

```bash
ansible-galaxy install Stouts.nginx
```

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

```yaml
- hosts: all

  roles:
    - javaguirre.ghost
    - Stouts.nginx

  vars:
    ghost_port: 2368
```


#### License

Licensed under the MIT License. See the LICENSE file for details.

#### Feedback

[Please send me your requests/questions!](https://github.com/javaguirre/ghost-ansible/issues)
