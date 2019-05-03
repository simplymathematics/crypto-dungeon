# Installation steps

* Set-up a 1 core vm somewhere

* Using Ubuntu 16.04: 

* [Add users, firefall, and ssh keys.](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04)

* Setup your root user

* add a user to host the server (we used dungeon-master). No permissions are necessary unless you want to git commit as this user.* 

```bash
# clone beacon
git clone https://github.com/ansuz/beacon

# clone gestalt
git clone https://github.com/ansuz/gestalt

# get nodejs with nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

# get nodejs with nvm
nvm ls-remote # list options, v10.15.3 looks good
nvm install 10.15.3 # this is our default node version

# uhhh, now what...
# let's get a server going:
cd gestalt && npm i; # install deps
node server; # got a warning about Buffer() being deprecated. I'll have to look into that..

# now let's get a client going
cd ~/beacon/ && npm i;
# I expect this will blow up, but I wanna see how
npm run debug; # janky build script, has some global dependencies, which I'll install as this user
# won't touch teh rest of the system

npm i -g jshint less browserify

# try again
npm run debug # builds in steps, compiled js and less (css preprocessor)
# seemed to work, so now we serve the result with gestalt

# set path for www in ~/gestalt/config.js

#ta da!
# cut a hole in the firewall
sudo ufw allow 3005

# I'm connected, as you can see
# install a thing to keep the server going
npm i -g forever
forever start server.js




```
