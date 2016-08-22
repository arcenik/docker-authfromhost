
This allow you to authenticate in your containers services using your host account (from this [blog post](https://jhrozek.wordpress.com/2015/03/31/authenticating-a-docker-container-against-hosts-unix-accounts/))

----
## Usage

#### Quick test container :

The pipe from sssd daemon must be exported from the host to the container

    docker run -ti \
    	authfromhost-debian-jessie:latest\
    	-v=/var/lib/sss/pipes/:/var/lib/sss/pipes/:rw \
    	/bin/bash


----
## Configure the host

#### Debian host :

1. Install sssd

    # apt install sssd

2. Configure pam

/etc/pam.d/sss_proxy :

    auth required pam_unix.so
    account required pam_unix.so
    password required pam_unix.so
    session required pam_unix.so

3. Configure sssd (file must be owned by root and permission set to 0600)

/etc/sssd/sssd.conf

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

4. Restart sssd and verify a user can be retrieved with sssd

    # chown root.root /etc/sssd/sssd.conf
    # chmod 0600 /etc/sssd/sssd.conf
    # systemctl restart sssd
    $ getent passwd -s sss localuser

#### Archlinux host :

1. Install sssd

    # pacman -S sssd

2. Configure pam

/etc/pam.d/sss_proxy :

    auth required pam_unix.so
    account required pam_unix.so
    password required pam_unix.so
    session required pam_unix.so

3. Configure sssd (file must be owned by root and permission set to 0600)

/etc/sssd/sssd.conf :

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

4. Restart sssd and verify a user can be retrieved with sssd

    # chown root.root /etc/sssd/sssd.conf
    # chmod 0600 /etc/sssd/sssd.conf
    # systemctl enable sssd
    Created symlink /etc/systemd/system/multi-user.target.wants/sssd.service → /usr/lib/systemd/system/sssd.service.
    # systemctl restart sssd
    $ getent passwd -s sss localuser

#### CentOS host :

TODO
