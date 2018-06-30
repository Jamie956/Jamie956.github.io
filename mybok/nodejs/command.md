# Command

```
===node===
node -v
node <js>

===debugging===
node inspect app.js
or
npm I -g node-inspect
node-inspect app.js

```


### nvm
```
===nvm===
sudo apt-get update
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.25.2/install.sh | bash
export NVM_DIR="/home/jamie/.nvm"[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" 
nvm
nvm install <version>
nvm ls
nvm use <version>
nvm alias default <version>
```