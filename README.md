
This allow you to authenticate in your containers services using your host account (from this [blog post](https://jhrozek.wordpress.com/2015/03/31/authenticating-a-docker-container-against-hosts-unix-accounts/))

----
#### Current test status

| | Status|
|---|---|
| Travis-CI | [![](https://travis-ci.org/arcenik/docker-authfromhost.svg?branch=master)](https://travis-ci.org/arcenik/docker-authfromhost)|

## Versions

This docker image is available with the following base systems.

#### Debian

|debian-jessie| [![](https://images.microbadger.com/badges/image/francois75/docker-authfromhost:debian-jessie.svg)](http://microbadger.com/images/francois75/docker-authfromhost:debian-jessie "Get your own image badge on microbadger.com")|[![](https://images.microbadger.com/badges/version/francois75/docker-authfromhost:debian-jessie.svg)](http://microbadger.com/images/francois75/docker-authfromhost:debian-jessie "Get your own version badge on microbadger.com")|
|---|---|---|
|debian-jessie-slim| [![](https://images.microbadger.com/badges/image/francois75/docker-authfromhost:debian-jessie-slim.svg)](http://microbadger.com/images/francois75/docker-authfromhost:debian-jessie-slim "Get your own image badge on microbadger.com")|[![](https://images.microbadger.com/badges/version/francois75/docker-authfromhost:debian-jessie-slim.svg)](http://microbadger.com/images/francois75/docker-authfromhost:debian-jessie-slim "Get your own version badge on microbadger.com")|
|debian-stretch| [![](https://images.microbadger.com/badges/image/francois75/docker-authfromhost:debian-stretch.svg)](http://microbadger.com/images/francois75/docker-authfromhost:debian-stretch "Get your own image badge on microbadger.com")|[![](https://images.microbadger.com/badges/version/francois75/docker-authfromhost:debian-stretch.svg)](http://microbadger.com/images/francois75/docker-authfromhost:debian-stretch "Get your own version badge on microbadger.com")|
|debian-stretch-slim| [![](https://images.microbadger.com/badges/image/francois75/docker-authfromhost:debian-stretch-slim.svg)](http://microbadger.com/images/francois75/docker-authfromhost:debian-stretch-slim "Get your own image badge on microbadger.com")|[![](https://images.microbadger.com/badges/version/francois75/docker-authfromhost:debian-stretch-slim.svg)](http://microbadger.com/images/francois75/docker-authfromhost:debian-stretch-slim "Get your own version badge on microbadger.com")|
|debian-buster| [![](https://images.microbadger.com/badges/image/francois75/docker-authfromhost:debian-buster.svg)](http://microbadger.com/images/francois75/docker-authfromhost:debian-buster "Get your own image badge on microbadger.com")|[![](https://images.microbadger.com/badges/version/francois75/docker-authfromhost:debian-buster.svg)](http://microbadger.com/images/francois75/docker-authfromhost:debian-buster "Get your own version badge on microbadger.com")|
|debian-buster-slim| [![](https://images.microbadger.com/badges/image/francois75/docker-authfromhost:debian-buster-slim.svg)](http://microbadger.com/images/francois75/docker-authfromhost:debian-buster-slim "Get your own image badge on microbadger.com")|[![](https://images.microbadger.com/badges/version/francois75/docker-authfromhost:debian-buster-slim.svg)](http://microbadger.com/images/francois75/docker-authfromhost:debian-buster-slim "Get your own version badge on microbadger.com")|
|debian-sid| [![](https://images.microbadger.com/badges/image/francois75/docker-authfromhost:debian-sid.svg)](http://microbadger.com/images/francois75/docker-authfromhost:debian-sid "Get your own image badge on microbadger.com")|[![](https://images.microbadger.com/badges/version/francois75/docker-authfromhost:debian-sid.svg)](http://microbadger.com/images/francois75/docker-authfromhost:debian-sid "Get your own version badge on microbadger.com")|
|debian-sid-slim| [![](https://images.microbadger.com/badges/image/francois75/docker-authfromhost:debian-sid-slim.svg)](http://microbadger.com/images/francois75/docker-authfromhost:debian-sid-slim "Get your own image badge on microbadger.com")|[![](https://images.microbadger.com/badges/version/francois75/docker-authfromhost:debian-sid-slim.svg)](http://microbadger.com/images/francois75/docker-authfromhost:debian-sid-slim "Get your own version badge on microbadger.com")|

----
## Usage

#### Quick test container :

The pipe from sssd daemon must be exported from the host to the container

```
$ docker run -ti --rm \
  -v /var/lib/sss/pipes/:/var/lib/sss/pipes/ \
  francois75/docker-authfromhost:debian-jessie \
  /bin/bash -l
```

#### For your docker images

Use it like any other images, using the tag to specify the system and release.

```
FROM fancois75/docker-authfromhost:debian
```

----
## Configure the host

Here is the step to have your host ready.

#### Debian host :

* Install sssd

```
# apt install sssd
```

* Configure pam

/etc/pam.d/sss_proxy :

```
auth required pam_unix.so
account required pam_unix.so
password required pam_unix.so
session required pam_unix.so
```

* Configure sssd (file must be owned by root and permission set to 0600)

/etc/sssd/sssd.conf

```
[sssd]
services = nss, pam
config_file_version = 2
domains = proxy

[nss]

[pam]

[domain/proxy]
id_provider = proxy
# The proxy provider will look into /etc/passwd for user info
proxy_lib_name = files
# The proxy provider will authenticate against /etc/pam.d/sss_proxy
proxy_pam_target = sss_proxy
```

* Restart sssd and verify a user can be retrieved with sssd

```
# chown root.root /etc/sssd/sssd.conf
# chmod 0600 /etc/sssd/sssd.conf
# systemctl restart sssd
$ getent passwd -s sss localuser
```

#### Archlinux host :

* Install sssd

```
# pacman -S sssd
```

* Configure pam

/etc/pam.d/sss_proxy :

```
auth required pam_unix.so
account required pam_unix.so
password required pam_unix.so
session required pam_unix.so
```

* Configure sssd (file must be owned by root and permission set to 0600)

/etc/sssd/sssd.conf :

```
[sssd]
services = nss, pam
config_file_version = 2
domains = proxy

[nss]

[pam]

[domain/proxy]
id_provider = proxy
# The proxy provider will look into /etc/passwd for user info
proxy_lib_name = files
# The proxy provider will authenticate against /etc/pam.d/sss_proxy
proxy_pam_target = sss_proxy
```

* Restart sssd and verify a user can be retrieved with sssd

```
# chown root.root /etc/sssd/sssd.conf
# chmod 0600 /etc/sssd/sssd.conf
# systemctl enable sssd
Created symlink /etc/systemd/system/multi-user.target.wants/sssd.service → /usr/lib/systemd/system/sssd.service.
# systemctl restart sssd
$ getent passwd -s sss localuser
```

#### CentOS host :

TODO
