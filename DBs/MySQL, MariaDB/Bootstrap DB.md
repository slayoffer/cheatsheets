Here are the Docker commands to achieve the steps you described:

1. Login into the MariaDB container:

```bash
docker exec -it <container_name> /bin/bash
```

Replace `<container_name>` with the name or ID of your MariaDB container.

1. Login into MariaDB:

```bash
mysql -u root -p
```

Enter the root password when prompted.

1. Create a new user `passbolt` with password `passbolt`:

```sql
CREATE USER 'passbolt'@'%' IDENTIFIED BY 'passbolt';
```

1. Create a new database `passbolt`:

```sql
CREATE DATABASE passbolt;
```

1. Grant all privileges on the `passbolt` database to the `passbolt` user:

```sql
GRANT ALL PRIVILEGES ON passbolt.* TO 'passbolt'@'%';
```

1. Exit MariaDB:

```sql
exit
```

1. Exit the container:

```bash
exit
```

Note: You can also create a script that runs these commands automatically when the container starts up. You can do this by creating a `.sh` script file and then adding it to the `entrypoint` of your container.