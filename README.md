[![Build Status](https://travis-ci.org/magicalyak/minishift-nginx.svg?branch=master)](https://travis-ci.org/magicalyak/minishift-nginx)

# minishift-nginx

Uses existing [minishift](https://github.com/minishift/minishift) install and configures NGINX to be the router.

Performs the following tasks:

- Installs NGINX or NGINX Plus openshift router

## Supported platforms

- Debian
- Red Hat
- OSX

## Prerequisites

- [Ansible 2.4+](https://docs.ansible.com)
- minishift installed (ideally use my [minishift-up](https://galaxy.ansible.com/magicalyak/minishift_up) role)
- copy nginx-repo.crt and nginx-repo.key to the files directory
- run the following command prior ```eval $(minishift docker-env)```

## Known Issues

none

## Defaults

**nginx_plus:** no

> Enable NGINX Plus.  If you select yes (you rock!!!) then make sure you drop the nginx-repo.crt and nginx-repo.key in the `/files` directory

## Example Playbook

Below is a sample playbook that includes all of the default parameters. You'll find this exact example in the root of the project tree.

```yaml
---
- name: Install NGINX router for minishift
  hosts: localhost
  connection: local
  gather_facts: yes
  roles:
    - role: magicalyak.minishift_nginx
      nginx_plus: yes
```

After you install the role, copy *minishift-nginx.yml* to your project directory. For example:

```sh
# Install the role
$ ansible-galaxy install magicalyak.minishift_nginx

# Copy nginx-repo.crt and nginx-repo-key files
$ mkdir ${ANSIBLE_ROLES_PATH}/magicalyak.minishift_nginx/files
$ cp nginx-repo.{crt,key} ${ANSIBLE_ROLES_PATH}/magicalyak.minishift_nginx/files

# Copy the playbook from your roles path to the current working directory
$ cp ${ANSIBLE_ROLES_PATH}/magicalyak.minishift_nginx/minishift-nginx.yml .

# Create a localhost inventory file
$ echo "localhost">./inventory

# Set the minishift docker variables
$ eval $(minishift docker-env)

# Run the playbook
$ ansible-playbook -i inventory --ask-become-pass minishift-nginx.yml
```

## Uninstall

To revert the changes make sure you don't delete the default-router-backup.yml file in the $HOME directory and run the following:

```sh
# Delete the NGINX Router
$ oc delete service/router dc/router clusterrolebinding/router-router-role serviceaccount/router

# Restore existing router
$ oc create -f $HOME/default-router-backup.yml
```

## Dependencies

[minishift-up](https://galaxy.ansible.com/magicalyak/minishift_up)

## License

Apache v2

## Author

[@magicalyak](https://github.com/magicalyak)