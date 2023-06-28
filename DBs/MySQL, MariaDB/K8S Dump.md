### K8s to Docker

```bash
kubectl exec -n dashboard-rc db-0 -- /usr/bin/mysqldump -u root -ppass db > dump.sql
```

There are a few steps you need to take in order to accomplish this. Here's a general approach:

1. First, copy the `dump.sql` file to the new server where the Docker container is running. You can use `scp` (secure copy) for this if the servers are Linux-based:

    ```bash
    scp -i ~/.ssh/rubius_ed25519 dump.sql a_evseev@212.233.92.30:~
    ```

    Replace `username` with the username on the destination server, `destination` with the IP address or hostname of the destination server, and `/path/to/destination` with the path on the destination server where you want to place the file.

2. Then, on the destination server, copy the `dump.sql` file into the running Docker container. You can do this with the `docker cp` command:

    ```bash
    docker cp dump.sql container_name:/path/to/container_destination
    ```

    Replace `container_name` with the name or ID of your Docker container, and `/path/to/container_destination` with the path in the container where you want to place the file.

3. Now, you should log into the MySQL in the Docker container and import the dump file:

    ```bash
    docker exec -it regboard_db_stage /bin/bash
    mysql -u user -ppass db < /dump.sql
    ```

    Replace `container_name` with the name or ID of your Docker container, `username` with your MySQL username, `database_name` with the name of your MySQL database, and `/path/to/container_destination` with the path where you copied the dump file in the container.

Remember, these commands should be adapted to your specific setup. The actual commands you use might differ based on your environment, the image your Docker container is based on, the MySQL credentials, etc. Always ensure you're following best practices for your environment, particularly when it comes to handling sensitive information like database credentials.