
#Run those command

`
sudo  mkdir pay.git && cd pay.git && git init --bare
nano hooks/post-receive

#!/bin/sh
git --work-tree=/var/www/client --git-dir=/var/repo/client.git checkout -f


. $HOME/.nvm/nvm.sh
nvm install v14.18.1
nvm use v14.18.1

cd /var/www/client

echo ‘post-receive: building…’
npm install && npm run build && npm install -g pm2 && pm2 start npm --name "client" -- start && pm2 restart client

`

After setup run this command

`
cd hooks
sudo chmod +x post-receive
`

