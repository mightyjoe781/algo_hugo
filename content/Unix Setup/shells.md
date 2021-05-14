+++

Title = "shells"

weight = 2

tags = ["bash", "shell"]

+++

#### Debian

Check the file`/etc/shells`

if it doesn't contain following line then add the line

```shell
/bin/bash
```

#### FreeBSD

````c++
sudo pkg install bash
chsh -s /usr/local/bin/bash
````

Installing bash using `pkg` automatically updates  `/etc/shells`

#### Installing Some Utilities

````
dnsutils	# dig, nslookup
joe			# text editor
lynx		# text-mode web browser
pbzip2		# paralled bzip2
pigz		# parallel gzip
rlwrap		# enables backspace for some CLI commands
````

#### Installing from a packages list

````bash
#!/bin/bash -e
cd /root/setupfiles/
apt-get install `cat *-dpkg.lst`
````

Parallel commands in FreeBSD are

`pkg install dnsutils joe lynx` like this.

#### Setup "dnsmasq"

`apt-get install dnsmasq`

create file `resolv.conf` using this bash script

````she
#!/bin/bash -e
touch resolv.conf
echo "nameserver" 127.0.0.1 > resolv.conf
chmod 644 resolv.conf
````

and `"dnsmasq.conf"` using this

````bash
#!/bin/bash -e
touch dnsmasq.conf
echo "all-servers
cache-size=10000
log-queries
interface=lo
no-resolv

address=/bacon.eggs/2.3.4.5

server=208.67.220.220	#OpenDNS
server=8.8.8.8			#Google
server=1.1.1.1" > dnsmasq.conf
chmod 644 dnsmasq.conf
````



next replace `"/etc/dnsmasq.conf"` and `"/etc/resolv.conf"` with files generated above

````bash
#!/bin/bash -e
chattr +i /etc/resolv.conf
service dnsmasq restart
````

So command `chattr +i` make files immutable means you can't change the file! unless you run `chattr -i` on the file.

After the edit run `chattr +i` again.

Now check whether its working of not do this

```bash
nslookup bacon.eggs
```

#### Setup LC_ALL

create a file `"/etc/profile.d/lc_all.sh"` that contains 

```
export LC_ALL=C
```

file need to be readable, but need not be executable

Restart shell session to see changes.