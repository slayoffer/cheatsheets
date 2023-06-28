### Check URL or file

```bash
curl www.google.com
# or
curl -s www.google.com # -s for silent
```

### Download file

```bash
curl -O http://somesite.com/sometext.txt # WILL NOT WORK WITH FOLDERS!

# download with new name
curl -o custom_file.tar.gz https://testdomain.com/testfile.tar.gz

# download several files
curl -O https://testdomain.com/testfile.tar.gz -O https://testdomain.com/testfile2.tar.gz
```

### Add and enable repo on server

```bash
curl --silent --location https://rpm.nodesource.com/setup_13.x | sudo bash -
```

### Detailed connection

```bash
# with connection details
curl -I http://www.rubius.com
# or with connection details and html code
curl -i http://www.rubius.com
```

### Continue interrupted download

```bash
# start download
curl -L -O http://url
# continue interrupted download
curl -L -O -C - http://url

# or
curl -L -O --retry 999 --retry-max-time 0 -C - http://url
# -C -: resume where the previous download left off
# --retry 999 : retrying so many times
# --retry-max-time 0 : prevent it from timing out on retrying

# or
curl -L -o 'filename' -C - http://url
```

### Non-secure curl

```bash
# curl without checking server certs (for self-signed, etc)
curl -k https://regboard.stage.asicloud.local
```