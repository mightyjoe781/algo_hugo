+++

Title = "postgreSQL"

weight = 8

tags = ["database","postgresql"]

+++

### PostgreSQL

Downloading on Debian

````bash
#!/bin/bash -e
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get -y install postgresql
sudo apt-get -y install postgresql-12
````

Service Restart ( use it to start the server )

`sudo service postgresql restart`

Connecting to DB as the above setup creates a user process named `postgresql`

````bash
sudo -i -u postgres
psql
````

Creating User

```bash
sudo -u postgres createuser smk
```

Creating Database

```bash
sudo -u createdb dvdrental
```

Giving the user a password

```bash
sudo -u postgres psql
psql=# alter user smk with encrypted password 'cowsaysmoo';
```

Granting privilege's on database

```bash
psql=# grant all privileges on database dvdrental to smk
```

