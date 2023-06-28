The script is asking for a password because it is not using any of the password-less authentication methods I mentioned earlier. To avoid having to enter a password when running the script, you can create a `.pgpass` file containing the necessary credentials.

Here's how to create and configure a `.pgpass` file:

1. Open a terminal on the machine where the script will run.
2. Navigate to the home directory of the user who will run the script: `cd ~`
3. Create a `.pgpass` file using a text editor (e.g., `nano`, `vim`, or `vi`): `nano .pgpass`
4. Add a line to the `.pgpass` file with the following format:

   ```
   hostname:port:database:username:password
   ```
   
   Replace `hostname`, `port`, `database`, `username`, and `password` with the appropriate values for your PostgreSQL server. If you want to allow password-less access to all databases and users on the server, you can use an asterisk (`*`) as a wildcard:

   ```
   hostname:port:*:*:password
   ```

5. Save and close the `.pgpass` file.
6. Set the appropriate file permissions to restrict access to the `.pgpass` file: `chmod 0600 .pgpass`

Now, when running the script, it should automatically use the credentials from the `.pgpass` file, and you won't be prompted for a password.