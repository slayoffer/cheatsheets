you can add a `try_files` directive to the `location` block in your Nginx configuration to try serving a different file before returning a 404 error. Here's an example configuration that uses `try_files` to first check if a file exists at the requested URL, and if not, it falls back to serving the custom 404 error page:

```bash
error_page 404 /not-found.php;

location / {
    # other configuration options here
    try_files $uri $uri/ /not-found.php;
}

location = /not-found.php {
    internal;
    fastcgi_pass unix:/var/run/php/php7.4-fpm.sock; # replace with the path to your PHP-FPM socket file
    include fastcgi_params;
    fastcgi_param REQUEST_METHOD $request_method;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param SCRIPT_NAME $fastcgi_script_name;
    fastcgi_param QUERY_STRING $query_string;
    fastcgi_param CONTENT_TYPE $content_type;
    fastcgi_param CONTENT_LENGTH $content_length;
}
```

In this configuration, the `location /` block specifies that Nginx should first try to serve the requested URL directly (`$uri`), then try to serve the URL with a trailing slash (`$uri/`), and finally fall back to serving the custom 404 error page (`/not-found.php`) if neither of those files exist. The `location = /not-found.php` block is the same as in the previous example and specifies the configuration for serving the custom 404 error page.

With this configuration, if a user requests a URL that doesn't exist, Nginx will first try to serve that URL as a file. If the file doesn't exist, Nginx will try to serve the URL with a trailing slash (in case it is a directory). If that also fails, Nginx will fall back to serving the custom 404 error page located at `/not-found.php`.