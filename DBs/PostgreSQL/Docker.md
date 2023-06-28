### Check DB

```bash
docker exec -it postgres bash
# login into DB
psql --username postgres
# list DBs
\l
# quit
\q
```

### Create DB and User in PG CT

```bash
# Here are the Docker commands to create a database named "nextcloud" with username "nextcloud" and password "nextcloud" in a PostgreSQL container named "postgres":

# Connect to the PostgreSQL container:
docker exec -it postgres psql -U postgres

# Create a new user and database
CREATE USER nextcloud WITH PASSWORD 'nextcloud';

CREATE DATABASE nextcloud OWNER nextcloud;

# Grant privileges to the new user
GRANT ALL PRIVILEGES ON DATABASE nextcloud TO nextcloud;

# quit
\q
```

### To allow access to a PostgreSQL database from a Docker container running on the same host, follow these steps:

1. Configure PostgreSQL to accept remote connections:
   
   - Open the `postgresql.conf` file, usually located in `/etc/postgresql/{version}/main/` or `/var/lib/pgsql/data/` depending on your PostgreSQL installation. You can use any text editor, for example `nano` or `vim`.
     
   - Locate the line with `listen_addresses`. Uncomment it (remove the `#` at the beginning of the line) and set its value to `'*'`:
     
     ```bash
     listen_addresses = '*'
     ```
   
- Save and close the file.

2. Update the `pg_hba.conf` file to allow connections from the Docker container's IP range:
   
   - Open the `pg_hba.conf` file, usually located in the same directory as the `postgresql.conf` file.
     
   - Add a new line at the end of the file to allow connections from the Docker container's IP range. For example, if your Docker containers have IP addresses in the range of `172.17.0.0/16`, you could add:
     
     ```bash
     host    all             all             172.17.0.0/16            md5
     ```
     
     This allows any container within the specified IP range to connect to the PostgreSQL server using a password.
     
   - Save and close the file.

3. Restart the PostgreSQL service to apply the changes:
   
   ```bash
   sudo systemctl restart postgresql
   ```

4. Create a user and a database for your application (if you haven't already):
   
   ```bash
   sudo -u postgres createuser -P myuser
   sudo -u postgres createdb -O myuser mydatabase
   ```

5. Configure your Docker container to connect to the PostgreSQL server:
   
   - In your Docker container's configuration or `Dockerfile`, ensure the PostgreSQL client libraries are installed (if your application requires them).
     
   - Update your application's database configuration to use the host's IP address (or hostname) as the PostgreSQL server address. The host's IP address can typically be found using the `ifconfig` or `ip addr` command. Look for the IP address associated with the main network interface (usually `eth0` or `enpXs0`).
     
   - Provide the username, password, and database name you created earlier.

6. Run your Docker container:
   
   ```bash
   docker run --name mycontainer -d myimage
   ```

Your Docker container should now be able to connect to the PostgreSQL server running on the host machine.

If you are using the Uncomplicated Firewall (UFW) on your host machine, you will need to open the PostgreSQL port to allow connections from your Docker container. By default, PostgreSQL listens on port 5432.

To add a rule to UFW to allow incoming connections on port 5432, follow these steps:

1. Add a rule to allow connections from the Docker container's IP range. For example, if your Docker containers have IP addresses in the range of `172.17.0.0/16`, run:

   ```bash
   sudo ufw allow from 172.17.0.0/16 to any port 5432
   ```

2. Enable the UFW firewall, if it is not already enabled:

   ```bash
   sudo ufw enable
   ```

3. Check the status of UFW to ensure the rule has been added:

   ```bash
   sudo ufw status
   ```

Now your Docker container should be able to connect to the PostgreSQL server through the UFW firewall.