---
title: Encrypting Files
---
Use this command to create an encrypted, password-protected version of a file:

```shell
openssl enc -pbkdf2 -aes-256-cbc \
    -in foo.bar \
    -out foo.bar.enc
```

You will be prompted to type and then confirm a password for the file.

To decrypt this file somewhere else, use this:

```shell
openssl enc -d -pbkdf2 -aes-256-cbc \
    -in foo.bar.enc \
    -out foo.bar
```

You will be prompted to provide the same password to perform decryption.