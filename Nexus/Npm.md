### Set registry

```bash
# create config file
sudo vim .npmrc

# set registry
npm config set registry https://nexus.rubius.com/repository/rubius-npm-group/

# hash credentials
echo -n 'anton.evseev:password' | openssl base64

# set credentials hash
npm config set _auth YW50b24uZXZzZWV2OjUwQ2VudDRsaWZl

# settings example file
package-lock=true
always-auth=true
registry=https://nexus.rubius.com/repository/rubius-npm-group/
//nexus.rubius.com/repository/:username=anton.evseev
//nexus.rubius.com/repository/:_password="NTBDZW50NGxpZmU="

# or just login
npm login https://nexus.rubius.com/repository/rubius-npm-group
```

### Login, Logout

```bash
npm login https://nexus.rubius.com/repository/rubius-npm-proxy

npm logout --registry=https://nexus.rubius.com/repository/rubius-npm-proxy
```

### List npm packages

```bash
# in current dir
npm ls --depth=0

# globally
npm ls -g --depth=0
```

### Pull

```bash
# do it only after doing settings above
npm --loglevel info install libphonenumber-js

# or
npm logout --registry=https://nexus.rubius.com/repository/rubius-npm-proxy

# create config
touch .npmrc

# add
registry=https://nexus.rubius.com/repository/rubius-npm-registry

# login
npm login https://nexus.rubius.com/repository/rubius-npm-registry

# pull
npm i -S npm-app1
```

### Publish & push to private registry

```bash
# for test create private npm package dir
mkdir npm-app1 && cd npm-app1

# init npm
npm init -y

# add to package.json
{
"name": "npm-app1",
"version": "1.0.0",
"description": "",
"main": "index.js",
"publishConfig": {
"registry": "https://nexus.rubius.com/repository/rubius-npm-registry/"
},
"scripts": {
"test": "echo \"Error: no test specified\" && exit 1"
},
"keywords": [],
"author": "",
"license": "ISC"
}

# login to registry
npm login --registry=https://nexus.rubius.com/repository/rubius-npm-registry/

# publish
npm publish
```