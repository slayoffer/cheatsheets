[Installation link](https://docs.timescale.com/self-hosted/latest/install/installation-linux/)

## [Installing self-hosted TimescaleDB on Debian-based systems](https://docs.timescale.com/self-hosted/latest/install/installation-linux/#installing-self-hosted-timescaledb-on-debian-based-systems)

1.  At the command prompt, as root, add the PostgreSQL third party repository to get the latest PostgreSQL packages:
    
    apt install gnupg postgresql-common apt-transport-https lsb-release wget
    
    Copy
    
2.  Run the PostgreSQL repository setup script:
    
    /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh

3.  Add the TimescaleDB third party repository:
    
    echo "deb https://packagecloud.io/timescale/timescaledb/ubuntu/ $(lsb_release -c -s) main" | sudo tee /etc/apt/sources.list.d/timescaledb.list
    
4.  Install TimescaleDB GPG key
    
    wget --quiet -O - https://packagecloud.io/timescale/timescaledb/gpgkey | sudo apt-key add -
    
    For Ubuntu 21.10 and later use this command to install TimescaleDB GPG key `wget --quiet -O - https://packagecloud.io/timescale/timescaledb/gpgkey | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/timescaledb.gpg`
    
5.  Update your local repository list:
    
    apt update
    
    Copy
    
6.  Install TimescaleDB:
    
    apt install timescaledb-2-postgresql-14
      
    ###### NOTE
    
    If you want to install a specific version of TimescaleDB, instead of the most recent, you can specify the version like this: `apt-get install timescaledb-2-postgresql-12='2.6.0*' timescaledb-2-loader-postgresql-12='2.6.0*'`
    
    You can see the full list of TimescaleDB releases by visiting the [releases page](https://packagecloud.io/timescale/timescaledb). Note that older versions of TimescaleDB don't always support all the OS versions listed above.
    
7.  Configure your database by running the `timescaledb-tune` script, which is included with the `timescaledb-tools` package. Run the `timescaledb-tune` script using the `sudo timescaledb-tune` command. For more information, see the [configuration](https://docs.timescale.com/self-hosted/latest/configuration/) section.
    

## [Set up the TimescaleDB extension](https://docs.timescale.com/self-hosted/latest/install/installation-linux/#set-up-the-timescaledb-extension)

When you have PostgreSQL and TimescaleDB installed, you can connect to it from your local system using the `psql` command-line utility.

### [Install psql on Linux](https://docs.timescale.com/self-hosted/latest/install/installation-linux/#install-psql-on-linux)

You can use the `apt` on Debian-based systems, `yum` on Red Hat-based systems, and `pacman` package manager to install the `psql` tool.

Debian

Red Hat

ArchLinux

### [Installing psql using the apt package manager](https://docs.timescale.com/self-hosted/latest/install/installation-linux/#installing-psql-using-the-apt-package-manager)

1.  Make sure your `apt` repository is up to date:
    
    apt-get update
    
    Copy
    
2.  Install the `postgresql-client` package:
    
    apt-get install postgresql-client
    
    Copy
    

Debian

Red Hat

ArchLinux

### [Setting up the TimescaleDB extension on Debian-based systems](https://docs.timescale.com/self-hosted/latest/install/installation-linux/#setting-up-the-timescaledb-extension-on-debian-based-systems)

Restart PostgreSQL and create the TimescaleDB extension:

1.  Restart the service after enabling TimescaleDB with `timescaledb-tune`:
    
    systemctl restart postgresql
    
    Copy
    
2.  On your local system, at the command prompt, open the `psql` command-line utility as the `postgres` superuser:
    
    sudo -u postgres psql
    
    Copy
    
    If your connection is successful, you'll see a message like this, followed by the `psql` prompt:
    
    psql (15.0 (Ubuntu 15.0-1.pgdg20.04+1), server 14.5 (Ubuntu 14.5-2.pgdg20.04+2))
    
    SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, compression: off)
    
    Type "help" for help.
    
    Copy
    
3.  Set the password for the `postgres` user:
    
    \password postgres
    
    Copy
    
4.  Exit from PostgreSQL:
    
    \q
    
5.  Use `psql` client to connect to PostgreSQL:
    
    psql -U postgres -h localhost
    
6.  At the `psql` prompt, create an empty database. Our database is called `tsdb`:
    
    CREATE database tsdb;
    
7.  Connect to the database you created:
    
    \c tsdb
    
8.  Add the TimescaleDB extension:
    
    CREATE EXTENSION IF NOT EXISTS timescaledb;
    
    
9.  Check that the TimescaleDB extension is installed by using the `\dx` command at the `psql` prompt. Output is similar to:
    
    tsdb-# \dx
    
                                          List of installed extensions
    
        Name     | Version |   Schema   |                            Description
    
    -------------+---------+------------+-------------------------------------------------------------------
    
     plpgsql     | 1.0     | pg_catalog | PL/pgSQL procedural language
    
     timescaledb | 2.7.0   | public     | Enables scalable inserts and complex queries for time-series data
    
    (2 rows)
    
    Copy
    

After you have created the extension and the database, you can connect to your database directly using this command:

psql -U postgres -h localhost -d tsdb