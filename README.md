# Ansible Vault Tutorial

In this tutorial, I try to explain how to structure one Ansible environment with:

* [Ansible Vault](http://docs.ansible.com/ansible/2.4/vault.html) support
* master key is stored in [secret file](secrets/ansible-vault-password.txt.gpg) encrypted with [GPG](https://en.wikipedia.org/wiki/GNU_Privacy_Guard)
* Many users support: master key can is encrypted with several GPG Keys

## Prerequisite

* [Ansible](https://en.wikipedia.org/wiki/Ansible_(software))

```
$ brew install ansible gnupg
```

Optional, for test only:

* [Virtualbox](https://en.wikipedia.org/wiki/VirtualBox) and [vagrant](https://en.wikipedia.org/wiki/Vagrant_(software))

```
$ brew install python3
$ brew cask install vagrant virtualbox
$ pip3 install paramiko
```


## Create or edit secret file

If secret file not exist:

```
$ ansible-vault create hosts/group_vars/all/secrets.yml
```

or edit this file:

```
$ ansible-vault edit hosts/group_vars/all/secrets.yml
```

Show secret content:

```
$ ansible-vault view hosts/group_vars/all/secrets.yml
my_secret: password
```


## Execute demo

Start vagrant server

```
$ vagrant up
```

Test if vagrant ping:

```
$ ansible -m ping all                                                                                                                                                                           ✘ 2 master ◼
server | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

Execute test playbook which use [Ansible Vault](http://docs.ansible.com/ansible/2.4/vault.html)

```
ansible-playbook playbooks/demo.yml                                                                                                                                                              

PLAY [server] ******************************************************************

TASK [Gathering Facts] *********************************************************
ok: [server]

TASK [demo : Upload template file to /home/ubuntu/test1.txt] *******************
ok: [server]

PLAY RECAP ********************************************************************
server                     : ok=2    changed=0    unreachable=0    failed=0
```

Check secret content in `test1.txt` file:

```
$ vagrant ssh -c "cat test1.txt"
variable: password
```

## How Ansible Vault password is configured

Master key is stored in [secrets/ansible-vault-password.txt.gpg](secrets/ansible-vault-password.txt.gpg) file.
Read [secrets/README.md](secrets/README.md) to know how to generate this file.

Default Vault password file is configured like this in [ansible.cfg](ansible.cfg)

```
vault_password_file = secrets/ansible-vault.sh
```

The [secrets/ansible-vault.sh](secrets/ansible-vault.sh) file is a script which use gpg to decrypt master key:

```
#!/bin/bash
gpg --decrypt --quiet --batch secrets/ansible-vault-password.txt.gpg
```
