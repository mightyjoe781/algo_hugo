+++

Title = "user bins"

weight = 5

tags = ["user","scripts", "bins"]

+++

##### Steps : Creating directories for utilities

````bash
#!/bin/bash -e
mkdir -p /opt/minebest/mtbin
mkdir -p /opt/smkbin
````

Next we create a text file `"/etc/profile.d/smkbin.sh"` containing

````bash
for dir in \
	/opt/minebest/mtbin \
	/opt/minebest/smkbin \
	/opt/smkbin
do
	if [ -d $dir ]; then PATH=$dir:$PATH; fi
done
````

File needs to be readable but doesn't need to be executable

Next Step : editing "secure_path" setting in `"/etc/sudoers"` to prepend the directories from preceding script which exists

`secure_path="/opt/minebest/mtbin:/opt/smkbin:/usr/local/sbin:...."`

Log-out and back in

If we are ordinary user do `sudo bash`

Now we can install various tools or useful scripts and tools to two new directories.

##### Installing `"youtube-dl"` in smkbin

````bash
#!/bin/bash -e
TARGET=/opt/smkbin/youtube-dl
rm -fr $TARGET
curl -L \
https://yt-dl.org/downloads/latest/youtube-dl \
	-o $TARGET
chmod a+rx $TARGET
````

