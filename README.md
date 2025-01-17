# Ansible Role: Node.js

[![CI](https://github.com/geerlingguy/ansible-role-nodejs/workflows/CI/badge.svg?event=push)](https://github.com/geerlingguy/ansible-role-nodejs/actions?query=workflow%3ACI)

Installs Node.js on RHEL/CentOS or Debian/Ubuntu.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    nodejs_version: "14.x"

The Node.js version to install. "14.x" is the default and works on most supported OSes. Other versions such as "8.x", "10.x", "13.x", etc. should work on the latest versions of Debian/Ubuntu and RHEL/CentOS.

    nodejs_install_npm_user: "{{ ansible_ssh_user }}"

The user for whom the npm packages will be installed can be set here, this defaults to `ansible_user`.

    npm_config_prefix: "/usr/local/lib/npm"

The global installation directory. This should be writeable by the `nodejs_install_npm_user`.

    npm_config_unsafe_perm: "false"

Set to true to suppress the UID/GID switching when running package scripts. If set explicitly to false, then installing as a non-root user will fail.

    nodejs_npm_global_packages: []

A list of npm packages with a `name` and (optional) `version` to be installed globally. For example:

    nodejs_npm_global_packages:
      # Install a specific version of a package.
      - name: jslint
        version: 0.9.3
      # Install the latest stable release of a package.
      - name: node-sass
      # This shorthand syntax also works (same as previous example).
      - node-sass
<!-- code block separator -->

    nodejs_package_json_path: ""

Set a path pointing to a particular `package.json` (e.g. `"/var/www/app/package.json"`). This will install all of the defined packages globally using Ansible's `npm` module.

    nodejs_generate_etc_profile: "true"
    
By default the role will create `/etc/profile.d/npm.sh` with exported variables (`PATH`, `NPM_CONFIG_PREFIX`, `NODE_PATH`). If you prefer to avoid generating that file (e.g. you want to set the variables yourself for a non-global install), set it to "false".

### System Specific Variables

Available specific variables are listed below, along with default values (see `vars/<Distribution>.yml`):

    nodejs_repositories: []

If you sign your own packages or run internal package management you can overwrite the value of this variable.
For Debian the default values are : 
```
 - "deb https://deb.nodesource.com/node_{{ nodejs_version }} {{ ansible_distribution_release }} main"
 - "deb-src https://deb.nodesource.com/node_{{ nodejs_version }} {{ ansible_distribution_release }} main"`.
``` 
For CentOS < 7 the default values are :
```
 - "http://rpm.nodesource.com/{{ nodejs_rhel_rpm_dir }}/el/{{ ansible_distribution_major_version }}/{{ ansible_architecture }}/nodesource-release-el{{ ansible_distribution_major_version }}-1.noarch.rpm"
``` 
For CentOS >= 7 the default values are :
```
 - https://rpm.nodesource.com/{{ nodejs_rhel_rpm_dir }}/el/{{ ansible_distribution_major_version }}/{{ ansible_architecture }}/nodesource-release-el{{ ansible_distribution_major_version }}-1.noarch.rpm
```

    nodejs_apt_key: 
      - url: "https://keyserver.ubuntu.com/pks/lookup?op=get&fingerprint=on&search=0x1655A0AB68576280"
        id: "68576280"

If you have Ubuntu distribution and sign your own packages or run internal package management you can overwrite the value of this variable. 

    nodejs_rpm_key: []

If you have CentOS distribution and sign your own rmp key or run internal package management you can overwrite the value of this variable.  
For CentOS < 7, the default values are : `http://rpm.nodesource.com/pub/el/NODESOURCE-GPG-SIGNING-KEY-EL`.  
For CentOS >= 7, the default values are : `https://rpm.nodesource.com/pub/el/NODESOURCE-GPG-SIGNING-KEY-EL` 

## Dependencies

None.

## Example Playbook

    - hosts: utility
      vars_files:
        - vars/main.yml
      roles:
        - geerlingguy.nodejs

*Inside `vars/main.yml`*:

    nodejs_npm_global_packages:
      - name: jslint
      - name: node-sass

## License

MIT / BSD

## Author Information

This role was created in 2014 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
