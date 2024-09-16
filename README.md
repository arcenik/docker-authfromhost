
This allow you to authenticate in your containers services using your host account (from this [blog post](https://jhrozek.wordpress.com/2015/03/31/authenticating-a-docker-container-against-hosts-unix-accounts/))

----

## Versions

This docker image is available with the following base systems.

#### Debian

|Release|Size||
|---|---|---|
|debian-buster        | ![](https://img.shields.io/docker/image-size/francois75/docker-authfromhost/debian-buster.svg?style=for-the-badge) |
|debian-buster-slim   | ![](https://img.shields.io/docker/image-size/francois75/docker-authfromhost/debian-buster-slim.svg?style=for-the-badge) |
|debian-bullseye      | ![](https://img.shields.io/docker/image-size/francois75/docker-authfromhost/debian-bullseye.svg?style=for-the-badge) |
|debian-bullseye-slim | ![](https://img.shields.io/docker/image-size/francois75/docker-authfromhost/debian-bullseye-slim.svg?style=for-the-badge) |
|debian-bookworms     | ![](https://img.shields.io/docker/image-size/francois75/docker-authfromhost/debian-bookworms.svg?style=for-the-badge) |
|debian-bookworms-slim| ![](https://img.shields.io/docker/image-size/francois75/docker-authfromhost/debian-bookworms-slim.svg?style=for-the-badge) |
|debian-trixie        | ![](https://img.shields.io/docker/image-size/francois75/docker-authfromhost/debian-trixie.svg?style=for-the-badge) |
|debian-trixie-slim   | ![](https://img.shields.io/docker/image-size/francois75/docker-authfromhost/debian-trixie-slim.svg?style=for-the-badge) |
|debian-sid           | ![](https://img.shields.io/docker/image-size/francois75/docker-authfromhost/debian-sid.svg?style=for-the-badge) |
|debian-sid-slim      | ![](https://img.shields.io/docker/image-size/francois75/docker-authfromhost/debian-sid-slim.svg?style=for-the-badge) |

----
## Usage

#### Quick test container :

The pipe from sssd daemon must be exported from the host to the container

```
$ docker run -ti --rm \
  -v /var/lib/sss/pipes/:/var/lib/sss/pipes/ \
  francois75/docker-authfromhost:debian-buster \
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
