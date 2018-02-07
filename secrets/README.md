
Prerequisite:

```
$ brew install pwgen
```

Generate random password:

```
$ pwgen 20 1 -s -1 > ansible-vault-password.txt
```

Export your GPG ID in `MAIN_GPG_KEY`.

Encrypt file:

```
$ gpg -e -r $MAIN_GPG_KEY ansible-vault-password.txt
```

Note: don't commit this `ansible-vault-password.txt`, it is present in [../.gitignore](../.gitignore)

You can decrpypt the file with:

```
$ gpg --decrypt --quiet --batch ansible-vault-password.txt.gpg
```
