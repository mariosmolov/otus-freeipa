# otus-freeipa

LDAP
1. Установить FreeIPA
2. Написать playbook для конфигурации клиента
3*. Настроить авторизацию по ssh-ключам

# Работоспособность

Поднимаем кластер `vagrant up`

Заходим на `ipa` в админку freeipa c паролем otus12345
```
[root@ipa ~]# kinit admin
Password for admin@OTUS.LAN:
[root@ipa ~]#
[root@ipa ~]# klist
Ticket cache: KEYRING:persistent:0:0
Default principal: admin@OTUS.LAN
...
```

Создадим юзера
```
[root@ipa ~]# ipa user-add --first="User"  --last="Otus" --cn="User Otus" user
-----------------
Added user "user"
-----------------
  User login: user
  First name: User
  Last name: Otus
  Full name: User Otus
  Display name: User Otus
  Initials: UO
  Home directory: /home/user
  GECOS: User Otus
  Login shell: /bin/sh
  Principal name: user@OTUS.LAN
  Principal alias: user@OTUS.LAN
  Email address: user@otus.lan
  UID: 171400001
  GID: 171400001
  Password: False
  Member of groups: ipausers
  Kerberos keys available: False
  ```
  Зададим пароль юзеру и ключ SSH
```
[root@ipa ~]# ipa user-mod user --password
Password:
Enter Password again to verify:

[root@ipa ~]# ipa user-mod user --sshpubkey="ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC5VIFyz4yab977MhDn4V+WjqUpnmZVBWjRcZeITBqO7KIR8pp/5D7+F4j3oSpQTs8ixFW/vVJt8agjwvcVW2vF8fKR2zZkHNmZ9s+wk60oTsHadk3ABMCr7mEzKDl4Uwkn2c3kSmo+1bKSqnGtYf9ZiQauM3HUlV4s11HHsV7Rsa25TcN/IZQnvBAxww8B+VSFZGFo6BefspPJ0kXFuNw3DhmndINFNDMOrG5S4bvUswKVfOPfO9c7RQ80TOpvUg/CMoTLZ+oxTVhyR69IqVkqouTrbw1Fovkr3d7wErVAPqY8chN/Dd0+bmRefZbpKxcExx68ArgJrkAT5sH8BgIv mario"
```
И заходим на сервер по ключу
```
[vagrant@client ~]$ ssh -i ~/.ssh/id_rsa user@ipa.otus.lan
Enter passphrase for key '/home/vagrant/.ssh/id_rsa':
Creating home directory for user.
-sh-4.2$
```
