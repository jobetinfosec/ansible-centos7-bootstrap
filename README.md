# Initial setup of CentOS 7 server


## Description

Initial setup of CentOS 7 server.

**First role (sudo_user)**

* System update
* Check if reboot is required
* Create sudo admin, add ssh public key and set no password in sudoers file

**Second role (bootstrap)**

* Set timezone
* Install, setup chrony utility and synchronize time and date against remote ntp server


## Tested on

CentOS 7 server (ver. 7.8)


## Requisites

a) Cloud Platform account<br />
b) A VPS server with CentOS 7.8 already installed<br />
c) An SSH key pair<br />



## Instructions

a) On your laptop create a working directory


b) In your working directory open a console window


c) Clone github repository

```
git clone https://github.com/jobetinfosec/ansible-centos7-bootstrap.git
```


d) Switch to **first** role directory

```
cd ansible-centos7-bootstrap/sudo_user
```


e) Open the hosts file

```
nano hosts
```

f) Replace <TEMPORARY_ITEMS> with your own data:

`<SERVER_IP>`		replace <SERVER_IP> with the actual IP address of your remote server<br />
`<PRIVATE_KEY_NAME>`	replace it with your private key's name<br />

then save and close the file


g) Open the defaults main.yml file

```
nano roles/basic/defaults/main.yml
```

h) Replace <TEMPORARY_ITEMS> with your own data:

`<USER_NAME>`		replace <USER_NAME> with the name of sudo user<br />
`<PASSWORD_HASH>`	replace it with the hash of sudo user's password (to create a password hash use mkpasswd --method=sha-512 command. If mkpasswd is not installed, install it with apt-get install whois)<br />
`<PUBLIC_KEY_NAME>`	replace it with your public key's name

then save and close the file


i) Check if you are able to ping remote server

```
ansible -m ping all
```

You should receive a SUCCESS message


j) Check if any error shows up

```
ansible-playbook sudo_user.yml --check
```


k) Launch installation

```
ansible-playbook sudo_user.yml
```

l) Switch to **second** role directory

```
cd ansible-centos7-bootstrap/bootstrap
```

m) Open the hosts file

```
nano hosts
```

n) Replace <TEMPORARY_ITEMS> with your own data:

`<SERVER_IP>`		replace <SERVER_IP> with the actual IP address of your remote server<br />
`<USER_NAME>`		replace it with sudo user name<br />
`<SUDO_USER_PASSWORD>`	replace it with sudo user password<br />

then save and close the file


o) Open the defaults main.yml file

```
nano roles/basic/defaults/main.yml
```

p) Replace <TEMPORARY_ITEMS> with your own data:

`<USER_NAME>`		replace <USER_NAME> with the name of sudo user<br />
`<PASSWORD_HASH>`	replace it with the hash of sudo user's password (already created during first role "sudo_user")<br />
`<PUBLIC_KEY_NAME>`	replace it with your public key's name

then save and close the file


q) Check if you are able to ping remote server with sudo user's name

```
ansible -u <SUDO_USER_NAME> -m ping all
```

You should receive a SUCCESS message


r) Check if any error shows up

```
ansible-playbook centos_bootstrap.yml --check
```


s) Launch installation

```
ansible-playbook centos_bootstrap.yml
```

## Licence

MIT licence
