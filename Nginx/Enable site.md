To enable a website or server block in the `sites-available` directory, you need to create a symbolic link from the `sites-available` directory to the `sites-enabled` directory.

Here is an example command to enable a website:

```bash
sudo ln -s /etc/nginx/sites-available/app.qubius.com.conf /etc/nginx/sites-enabled/
```

In this command, `example.com` is the configuration file of the website that you want to enable. The `ln -s` command creates a symbolic link from the `sites-available/example.com` file to the `sites-enabled` directory. This will make the website configuration active and ready to be served by NGINX.

After creating the symbolic link, you can check if the site is enabled by running:

```bash
ls -l /etc/nginx/sites-enabled/
```

This will show you the contents of the `sites-enabled` directory, including the symbolic link that you just created. You can also restart NGINX to apply the changes by running:

```bash
sudo service nginx restart
```

Note that when you create a symbolic link to enable a website, don't forget to remove the default symbolic link named `default` from the `sites-enabled` directory. If it remains there, when restarting or reloading NGINX configuration, NGINX would try to serve a default config instead of your site.