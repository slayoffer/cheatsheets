## Setting up certbot

To set up certbot, I will be using snaps (a package manager developed by Canonical).

And the first step is to remove any existing certbot package on the Ubuntu system:

```
sudo apt remove certbot 
```

Once you are done with the setup, use the following command to install certbot:

```
sudo snap install --classic certbot
```

And finally, create a symlink to the certbot directory:

```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

To verify the installation, check the installed version of certbot:

```
certbot --version
```

## Install certificates

You can request 50 certificates per week (maximum).

As you can request a limited number of certificates per week, using the test certificate is the best practice to find possible errors.

To install testing certificates, use the following command:

```
sudo certbot --nginx --test-cert
```

And it will ask the following:

-   Enter your email address to receive urgent renewals and change in policies.
-   Use the link to download the PDF of the terms and conditions and press `Y` and hit enter if you agree.
-   It is optional to subscribe to the mailing list by which you will receive newsletters.
-   It will list available domain names for the request. You can select one or two manually. And if you want certificates for every domain listed, leave it blank and hit enter (that's what I did).

If you find no errors, you can proceed with installing the actual certificate:

```
sudo certbot --nginx
```

It will ask the same set of questions but will add one different question.

As you already have installed the test certificates, you have two choices:

-   Reinstall the existing certificates (test certificates in my case)
-   Renew and replace certificates

Choose the 2nd option and hit enter:

That's it! You have secured your website with HTTPS.

And now, if you check, the connection to the site will be secured:

Certbot is scheduled to run every 12 hours and will renew certificates if the existing one is expired. You can check the times using:

```
systemctl list-timers
```

And if you want to update them manually, you can use the following command:

```
sudo certbot renew
```

### Show current certs

```bash
sudo certbot certificates
```

### Renew

```bash
sudo certbot renew
```

### Force renew

```bash
certbot certonly --force-renew -d example.com

# check
sudo certbot renew --dry-run
```

### Standard flow

```bash
sudo snap install core; sudo snap refresh core

sudo apt remove certbot

sudo snap install --classic certbot

# link the certbot command from the snap install directory to your path
sudo ln -s /snap/bin/certbot /usr/bin/certbot

sudo nano /etc/nginx/sites-available/example.com

# Find the existing server_name line. It should look like this:
/etc/nginx/sites-available/example.com
...
server_name example.com www.example.com;
...

# If it does, exit your editor and move on to the next step.

# If it doesn’t, update it to match. Then save the file, quit your editor, and verify the syntax of your configuration edits:
sudo nginx -t

sudo ufw allow 'Nginx Full'
sudo ufw delete allow 'Nginx HTTP'
sudo ufw status

# If you get an error, reopen the server block file and check for any typos or missing characters. Once your configuration file’s syntax is correct, reload Nginx to load the new configuration:

sudo systemctl reload nginx

sudo certbot --nginx -d example.com -d www.example.com

sudo systemctl status snap.certbot.renew.service

sudo certbot renew --dry-run
```

### Manual certs (without Nginx)

```bash
# for manual cert creating
sudo certbot certonly --standalone
```

### Certs rebind (for server move, etc)

```bash
# EASY WAY - turn off nginx/proxy to free up port 80 and use
sudo certbot certonly -d dorooma.rubius.com

# HARD WAY - if cant turn off proxy server then do all these:

# remove old certs if any
sudo rm -r /etc/letsencrypt/live/app.qubius.com

# remove old renewal configs if any
sudo rm -r /etc/letsencrypt/renewal/app.qubius.com.conf

# remove site enabled soft link
sudo rm /etc/nginx/sites-enabled/app.qubius.com.conf

# backup original nginx config file
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bak

# create new cert
sudo certbot -d app.qubius.com

# certbot will spoil nginx default config file, so remove it - CHECK Which config it changes!
sudo rm /etc/nginx/sites-available/default

# and restore original one
sudo cp /etc/nginx/sites-available/default.bak /etc/nginx/sites-available/default

# create soft link for site
sudo ln -s /etc/nginx/sites-available/app.qubius.com.conf /etc/nginx/sites-enabled/

# check nginx and reload
sudo nginx -t
sudo nginx -s reload

# check certs renewal
sudo certbot renew --dry-run
```

### Manual cert renew

```bash
# You can check the status of the timer with the following:
sudo systemctl status snap.certbot.renew.service
# The output will confirm that your certificate renewal is active.

# However, if your certificate has expired and you want to force renewal, you can do so with the following command. This is also useful to do if you have different domains you want to renew a certificate for:
certbot certonly –force-renew -d example.com

# Even though the certbot package comes with a certificate renewal script with /etc/cron.d, there are other options as well. For example, you can set up the renew_hook option with Certbot so that you can run other tasks after renewal. To do this, you need to add renew_hook to the Certbot renewal configuration file. Begin by opening up the file with your preferred text editor:
sudo nano /etc/letsencrypt/renewal/example.com.conf

# Once you’re inside the file, add the hook to the last line to reload the web-facing services so that they can be set up to use the renewed certificate:
renew_hook = systemctl reload your_service

# Once you’re finished, you can save and close the file. If you’re using nano, press CTRL + X, Y, and then ENTER.

# No matter what you choose when setting up your certificate renewal process, you can always confirm that certbot is running the renewal process as intended with the following command:
sudo certbot renew --dry-run

# Finally, check for any syntax errors with sudo nginx -t and then restart Nginx with sudo systemctl restart nginx to ensure your changes are implemented. After you’ve done all of this, navigate to your web browser at https://example.com to confirm the redirect is working correctly.
```

### Add existing certs to Certbot

```bash
# IT DOESNT WORK! 
sudo certbot certonly -d app.qubius.com --cert-path /etc/letsencrypt/live/app.qubius.com/cert.pem --key-path /etc/letsencrypt/live/app.qubius.com/privkey.pem --ful
lchain-path /etc/letsencrypt/live/app.qubius.com/fullchain.pem
```

### For "duplicate server name" error debugging

```bash
grep -r onboardingbot.rubius.com /etc/nginx/sites-enabled/*
```





