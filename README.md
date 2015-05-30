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

[Please send me your requests/questions!](https://github.com/javaguirre/ghost-ansible/issues)!
