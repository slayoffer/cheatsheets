To retrieve the process name or id of a running application managed by PM2, you can use the `pm2 list` command. Here is how to do it:

1. Open a terminal.
2. Type the following command:
    ```bash
    pm2 list
    ```
This command will show you a list of all running applications managed by PM2 along with their details, including the App name, id, mode, PID, status, restart, uptime, CPU usage, memory usage, etc. 

The "App name" column shows the name of the applications, and the "id" column shows the id of the applications. These are the values that you would use to interact with your applications through PM2 (for example, when stopping or restarting an application).

### To completely remove PM2 (a popular process manager for Node.js applications) from an Ubuntu server, you can follow these steps:

1. Stop all PM2 processes: First, you need to stop all running processes managed by PM2. Open a terminal and run the following command:

    ```bash
    pm2 kill
    ```

2. Uninstall PM2: After stopping all PM2 processes, you can uninstall PM2 by using the Node Package Manager (npm). Run the following command:

    ```bash
    npm uninstall -g pm2
    ```

    The `-g` flag indicates that PM2 should be uninstalled globally.

3. Remove PM2 directory: PM2 stores some data in the `.pm2` directory located in the home directory. You can remove it by running:

    ```bash
    rm -r ~/.pm2
    ```

4. Check if PM2 is removed: Finally, you can check if PM2 has been removed successfully by running:

    ```bash
    pm2
    ```

    If PM2 has been removed correctly, you should see a message stating that 'pm2' is not recognized as a command. If not, you might need to log out and log in again, or reboot the server to refresh the environment variables and completely remove PM2.

Please remember, these steps should be performed by a user with appropriate system permissions.