+++

Title = "updates"

weight = 1

tags = ["updates","upgrade"]

+++

Probably first thing after installing an OS is to update it

#### Debian Guide

````bash
apt-get update
apt-get upgrade
apt-get dist-upgrade
apt-get autoremove
````

This should upgrade the OS to latest `debian` release : Note that it only upgrades distro within its series for example 10.1 to 10.8

Checking the distro

```shell
cat /etc/os-release
```

Then install basic utilities.



#### FreeBSD Guide

Upgrading packages within same release

````shell
freebsd-update fetch
freebsd-update install
````

if anything goes wrong do this

```shell
freebsd-update rollback
```

<hr>

For major and minor version upgrades

```shell
freebsd-update -r 13.0-RELEASE upgrade
```

Above command fetches generic kernel and writes config files

Installing the Upgrade

```shel
freebsd-update install
```

After major upgrades we need to rebuild ports

```shell
portmaster -af
freebsd-update install
```

