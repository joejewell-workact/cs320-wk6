# CS320 Week 6: Automated WordPress Deployment

This project uses Ansible to deploy a complete WordPress site on an Ubuntu server running a LEMP stack (Linux, NGINX, MariaDB, PHP). It was built one role at a time, and is idempotent: a second run reports changed=0.

## Purpose

Practice building a structured Ansible project with reusable roles that install, configure, and deploy a full web application stack, while keeping secrets out of task files and making every task safe to rerun.

## The project consists of

- site.yml: main playbook; targets wordpress_servers and calls the roles in order
- validate.yml: checks services, the WordPress install, plugins, and site access
- roles/stack_install: installs NGINX, MariaDB, PHP-FPM + extensions, and WP-CLI
- roles/stack_config: creates the wordpress app account and web root, deploys a
  PHP-FPM pool, generates a self-signed TLS cert, configures NGINX for HTTP and HTTPS, and starts/enables the services
- roles/wordpress: creates the MariaDB database and user, then uses WP-CLI to download, configure, and install WordPress, set site options, and install plugins
- group_vars/wordpress_servers/vars.yml : shared settings (site, database, admin, paths, plugins)
- group_vars/wordpress_servers/vault.yml : encrypted passwords (Ansible Vault)
- requirements.yml : required Ansible collections

## Required software

Control node:
- Python 3
- Ansible (ansible-core plus the ansible.mariadb collection):

## Inventory config

ini
[wordpress_servers]
lampserver ansible_host=10.2.37.98 ansible_user=joe

[wordpress_servers:vars]
ansible_python_interpreter=/usr/bin/python3

## Secrets

No passwords appear in task files. vars.yml references vault variables and vault.yml is encrypted. The vault key lives in .vault_pass, which is excluded by .gitignore. 
## How to run

ansible wordpress_servers -m ansible.builtin.ping   
ansible-playbook site.yml --syntax-check            
ansible-playbook site.yml                          
ansible-playbook site.yml                          
ansible-playbook validate.yml                       


Then browse to https://10.2.37.98 and log in at
https://10.2.37.98/wp-admin as wpadmin.

## Testing

I built the project and tested in stages. The first-run output for each role was captured as that role was added. The idempotence test is a full site.yml run against the completed deployment.