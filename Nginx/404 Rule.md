If your custom 404 page is located at `/not-found` instead of `/404.html`, you can modify the Nginx configuration accordingly. Here are the steps to redirect all 404 errors to your custom 404 page:

1. Create the custom error page in HTML format. For example, you can create a file named `not-found.html` in your Nginx document root directory `/var/www/html` with the following content:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>404 Not Found</title>
  </head>
  <body>
    <h1>404 Not Found</h1>
    <p>The requested URL was not found on this server.</p>
  </body>
</html>
```

1. Open your Nginx configuration file using a text editor. The location of the configuration file may vary depending on your operating system and installation method. Common locations include `/etc/nginx/nginx.conf` or `/usr/local/nginx/conf/nginx.conf`.
2. Add the following code to the `http` block of your Nginx configuration file:

```bash
error_page 404 /not-found;
location = /not-found {
  internal;
}
```

This code sets up a custom error page for 404 errors and redirects all 404 errors to the `/not-found` page.

1. Save and close the Nginx configuration file.
2. Test your Nginx configuration using the command `sudo nginx -t` to make sure there are no syntax errors.
3. Reload Nginx using the command `sudo systemctl reload nginx` (if you installed Nginx using a package manager on a Linux distribution that uses systemd) or `sudo service nginx reload` (on other Linux distributions).

That's it! Now, whenever a user encounters a 404 error on your website, they will be redirected to your custom 404 error page located at `/not-found`.