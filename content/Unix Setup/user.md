+++

Title = "user"

weight = 4

tags = ["user","process"]

+++

#### Adding a ordinary user

```bash
rlwrap adduser sally
```

Initially, only the account name and passwords are important. Other fields can be left empty. Account name should be lower-case!

then execute this

````bash
THEM=sally
HOUSE=/home/$THEM
chmod 700 $HOUSE
mkdir -p  $HOUSE/.ssh
chmod 700 $HOUSE/.ssh
touch     $HOUSE/.ssh/authorized_keys
chown -R  $THEM.$THEM $HOUSE
````

Replace sally with appropriate user name.

Public key of the user should who should have access should go into authorized_keys

```bash
cat /tmp/sally.pubkey >> /home/sally/.ssh/authorized_keys
```

Giving `sudo` access to the user created above

```bash
usermod -a sally -G sudo
```

#### Enabling "sudo" w/o password

creating a file name `"90-cloud-init-users"`

````bash
touch 90-cloud-init-users
chmod 440 90-cloud-init-users
````

Now open the file and write this line for each trusted "sudo user"

````
root ALL=(ALL) NOPASSWD:ALL
smk ALL=(ALL) NOPASSWD:ALL
````

now copy this file to `"etc/sudoers.d"` 

***Warning :*** This step if not done properly can brick the box!

Also do not edit the file once its in directly. This may also brick the box. Safest way is to edit is somewhere else and then copy result into the directory in question

#### Removing a user from box

