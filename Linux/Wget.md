### Basic

```bash
# check install
which wget

# download
wget https://wordpress.org/latest.zip

# download with custom file name
wget -O ~/1.zip https://www.dundeecity.gov.uk/sites/default/files/publications/civic_renewal_forms.zip # -O for option

# download to folder
wget -P /home/slayo/Downloads https://wordpress.org/latest.zip # -P for path
```

### Advanced

```bash
# for large files in case download process was interrupted to resume the process
wget -c https://releases.ubuntu.com/22.04.1/ubuntu-22.04.1-desktop-amd64.iso?_ga=2.12817076.2053977061.1668138935-1421778380.1667269103 # -c for continue

# or
wget --continue --progress=dot:mega --tries=0 https://releases.ubuntu.com/22.04.1/ubuntu-22.04.1-desktop-amd64.iso
# The continue option tells wget to try and restart any downloads where they left off. The progress option indicates 3MB per line of dots rather than 384k; appropriate for a file of my size ~1GB. And finally, tries=0 means keep trying forever regardless of how many times the connection fails. If the server closes the connection unexpectedly or you lose connectivity you can easily re-run the command to download where you left off. Hopefully, this works for your use case as well.

# create file with links to download
nano fetchlist.txt

# input the download links there
# close the file
# initiate wget on it
wget -i fetchlist.txt
```



