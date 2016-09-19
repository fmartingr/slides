<!-- $theme: gaia -->
<!-- $size: 16:9 -->

# ![Ansible logo](img/ansible_logo.png)
# Ansible 101

---

# Felipe Martin

<img src="http://cdn.fmartingr.com/avatar.png" width="400" style="float: right">

- 1987
- CTO at [Saluspot](https://www.saluspot.com)
- Dev + IT + OP + Manager...
  - aka: *Problem solver*
- [fmartingr.com](http://fmartingr.com)
- **@fmartingr** on most social.

---
<!-- page_number: true -->

# What is ansible

- A very powerful administration and orchestration tool. 
- Command line interface to easily test module usage or execute commands. 
- YML based role and task configuration to easily automate almost everything from single to multi host environments. 
- Written in python. 
- Can use it for dedicated/virtual servers or docker containers (since 2.1).

--- 

# Internals

TODO

--- 

# ROLES? TASKS?
## Ansible vocabulary

---

## Task

- The samallest in the hierarchy.
- Consists on the execution of a module.

## Role

- A group of tasks that perform the configuration/installation of a given service or related stuff. 
- For example, the installation, configuration, privilege setup of a postgresql service is a role. The update/upgrade of a server packages can be a role too. 

---

## Host

- A server or container of some sort.

## Group

- A group of hosts that have the same role.

Exaample: webservers, dbsservers, ...

---

## Inventory

- A file where all hosts are defined. 
- You can have multiple inventory files for multiple environments. (Production, staging, demo, ...)

## Playbook

- The file where the roles are segmented into groups. 
- Roles and groups can have tags defined to better filter the roles and tasks you want to execute at a given time. 

---

# What modules can I use?

- **Package management**: apt, yum, pacman, pip, bower, npm, ...
- **Files**: file, stat, acl, copy, patch, replace, ...
- **Version control**: git, bzr, hg, gitlab/github hooks
- **Databases**: postgresql_{user,db,priv,...), mysql_{...}, ...

You get the idea.

---

# Enough theory!

## Usage examples

--- 

# Command line usage

### Basic usage
``` bash
ansible (hostname) -m (module) -a (arguments)
```

#### Install git is installed on my machine

``` bash
ansible localhost -m dnf -a "name=git state=present"
```

#### apt-get update && apt-get upgrade

``` bash
ansible localhost -m dnf -a "name=* state=latest"
```

---

# Spot the difference (I)

``` bash
ansible localhost -m dnf -a "name=git state=present"
```

vs

``` bash
ansible localhost -m apt -a "name=git ensure=present"
```

---

# Spot the difference (II)

``` bash
ansbile localhost -m dnf -a "name=* state=latest"
```

vs

``` bash
ansible localhost -m apt -a "upgrade=yes" # aptitude safe-upgrade
ansible localhost -m apt -a "upgrade=full" # aptitude full-upgrade 
ansible localhost -m apt -a "upgrade=dist" # apt-get dist-upgrade
```

---

### Even if the packages are from the same category their usage may difer a lot. Remember that if something is not working you may have an argument wrong!

###### *it was ensure, state, status...?*

---

# Our fisrt playbook

---

Using the same examples as a playbook:

``` yaml
- name: Update and upgrade system
  become: true
  apt: update_cache=yes safe_upgrade=yes
- name: Ensure git is updated
  become: true
  apt: name=git ensure=latest
```

Run it with the command:
``` bash
ansible-playbook localhost playbook.yml
```

---

# Thank you!

## Q&A