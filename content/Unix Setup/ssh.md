+++

Title = "ssh"

weight = 3

tags = ["ssh","keygen"]

+++

Generate a keypair for server-side user accounts

````bash
#!/bin/bash -e
rlwrap ssh-keygen -t rsa -b 2048
````

* Do not specify password for keypair
* Additionally rename your keys something sensible!

- `id_ed25519` : Private key
- `id_ed25519.pub` : Public key

Now as a good practice rename keys according to server. For example :

`smk.prvkey` and `smk.pubkey`

- Never share your **<u>private key</u>**. Never ever never!

- `smk.pubkey` should be appended to `"$HOME/.ssh/authorized_keys"` on the **server**. This is the key you should share if someone asks for `ssh` keys.

- Finally 

  ```shel
  service ssh restart
  ```

#### Local Setup for Accessing Server

Next Step is setting up a config file locally so that we can access the server easily

```bash
touch ~/.ssh/config
```

Now open the file in favorite editor and write this block of lines for each server. (Notice the tab spacing!)

````
host *
Protocol 2
Compression no
StrictHostKeyChecking no
host smk
	Hostname 195.223.83.209
	port 22
	User smk
	IdentifyFile ~/.ssh/smk.prvkey
host smkbsd
	Hostname 208.231.216.195
	port 22
	User smk
	IdentifyFile ~/.ssh/smk.prvkey
````

So above config file gives us access to two servers which have our public key appended into authorized_keys file

A good practice is to limit the permission on files and directory  `.ssh`. This is from man page of `ssh`

- Recommended Permissions : `~/.ssh/` : `700` , `authorized_keys` : `600`, `smk.pubkey` : `644`
- Mandatory Permissions : `smk.prvkey` : `600` and `config` : `600` 

to change permissions use `chmod` for e. g. `sudo chmod 700 ~/.ssh` or `sudo chmod 600 ~/.ssh/config`

Now to login into the server

````shell
ssh smk
````

or

```shell
ssh smkbsd
```

