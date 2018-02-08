
Prerequisite:

```
$ brew install pwgen
```

Generate random password:

```
$ pwgen 20 1 -s -1 > ansible-vault-password.txt
```

Import some keys in GPG:

```
$ gpg --keyserver hkp://pgp.key-server.io/ --search-keys contact@example.com
```

Encrypt file with multi user recipients:

```
$ gpg -e \
  -r user1@example.com \
  -r contact@example.com \
  ansible-vault-password.txt
```

Note: don't commit this `ansible-vault-password.txt`, it is present in [../.gitignore](../.gitignore)

You can decrpypt the file with:

```
$ gpg --decrypt --quiet --batch ansible-vault-password.txt.gpg
```
