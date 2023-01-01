# openssl-fix
Unsupported hash type ripemd160 with hashlib in Python
```
ValueError: unsupported hash type ripemd160
```
Hashlib uses OpenSSL for ripemd160 and apparently OpenSSL disabled some older crypto algos around version 3.0 in November 2021. All the functions are still there but require manual enabling. See issue 16994 of OpenSSL github project for details.

To quickly enable it, find the directory that holds your OpenSSL config file or a symlink to it, by running the below command:
```
openssl version -d
```
You can now go to the directory and edit the config file:
```
sudo nano openssl.cnf
```
Make sure that the config file contains following lines:

```
openssl_conf = openssl_init

[openssl_init]
providers = provider_sect

[provider_sect]
default = default_sect
legacy = legacy_sect

[default_sect]
activate = 1

[legacy_sect]
activate = 1
```

Tested on: OpenSSL 3.0.2, Python 3.10.4, Linux Ubuntu 22.04 LTS aarch64
