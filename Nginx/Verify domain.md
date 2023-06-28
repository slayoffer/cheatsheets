### Webroot method

```bash
certbot certonly -a webroot -w /usr/share/nginx/html -d registry.example.com

#  `-a webroot`: This flag specifies the "webroot" authenticator plugin. It tells Certbot to use the webroot method to prove to Let's Encrypt that you control the domain. This method involves placing a specific file in the webroot directory of your website so that Let's Encrypt can verify your domain ownership.
    
# `-w /usr/share/nginx/html`: This flag sets the webroot path to `/usr/share/nginx/html`, which is the default webroot directory for the Nginx web server. Certbot will create a temporary file in this directory to prove domain ownership during the domain validation process.
```