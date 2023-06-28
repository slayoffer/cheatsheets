### Configs locations

```bash
# In a typical NGINX installation, there are several folders that contain configuration files:

/etc/nginx: # This is the main configuration folder for NGINX. It contains the main configuration file nginx.conf, as well as subfolders for additional configuration files.

/etc/nginx/conf.d: # This folder contains additional configuration files that are included in the main nginx.conf file. These files typically contain server block configurations for individual websites or applications.

/etc/nginx/sites-available and /etc/nginx/sites-enabled: # These folders are used for managing virtual hosts in NGINX. Configuration files for each virtual host are stored in the sites-available folder, and symbolic links to these files are created in the sites-enabled folder to enable them.

/etc/nginx/snippets: # This folder contains reusable configuration snippets that can be included in other configuration files using the include directive.


# It's worth noting that the exact folder structure may vary depending on the operating system and installation method used.
```



### Load balancer for different nodes

```bash
# config 1
http {
    upstream nodebackend {
     server nodeapp1:8080;
	 server nodeapp2:8080;
	 server nodeapp3:8080;

    }

   server {
        listen 8080;
        location / {
          proxy_pass http://nodebackend/;
         }
    }
}

events
{}
```

