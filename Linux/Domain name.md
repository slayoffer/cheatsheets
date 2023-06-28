To set a domain name for your Ubuntu server so that you can connect to it via SSH, you will need to update the server's DNS (Domain Name System) configuration. Here's how:

1. Log in to your Ubuntu server using SSH.
2. Open the `/etc/hostname` file with a text editor, such as `nano`:

   ```
   sudo nano /etc/hostname
   ```

3. Replace the existing hostname with your desired domain name. For example, if you want to set the hostname to "example.com", change the contents of the file to:

   ```
   example.com
   ```

4. Save the file by pressing `Ctrl+X`, then `Y`, then `Enter`.

5. Next, you need to update the `/etc/hosts` file to map your domain name to the server's IP address. Open the file with your text editor:

   ```
   sudo nano /etc/hosts
   ```

6. Add the following line to the end of the file, replacing `10.0.0.1` with your server's IP address, and `example.com` with the hostname you set in step 3:

   ```
   10.0.0.1 example.com
   ```

   If your server has multiple IP addresses, repeat this line for each IP address.

7. Save the file by pressing `Ctrl+X`, then `Y`, then `Enter`.

8. Finally, restart your server's networking service to apply the changes:

```bash
sudo systemctl restart networking
```

You should now be able to connect to your Ubuntu server via SSH using the domain name you set. For example:

```bash
ssh username@example.com
```