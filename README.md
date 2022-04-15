# SSL-on-arch-with-nginx for node apps

### Clone the repo that you wish

### Install pm2 for running process in background

<pre>sudo npm i -g pm2</pre>
 
### Change package.json to start with pm2

<pre>"prod": "pm2 start index.js --watch"</pre>

### Start app in background

<pre>npm run prod</pre>

### See app status with pm2

<pre>pm2 status</pre>

### Other pm2 commands

<pre>pm2 logs</pre>
<pre>pm2 flush</pre>
<pre>pm2 startup arch</pre>
<pre>pm2 restart all</pre>
<pre>pm2 stop all</pre>

### Update arch

<pre>sudo pacman -Syu</pre>

### Install nginx

<pre>sudo pacman -S nginx</pre>

### Enable nginx

<pre>sudo systemctl start nginx</pre>
<pre>sudo systemctl enable nginx</pre>

### Config nginx.conf

Put this in the specific location of the file

<pre>
server {
    server_name  domain www.domain.whatever;

    location / {
        proxy_pass http://localhost:3000; #port u use
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
</pre>

### Install certbot

<pre>sudo pacman -S certbot</pre>
<pre>sudo pacman -S certbot-nginx</pre>

### Create specific folder

<pre>/usr/local/share/webapps/letsencrypt/</pre>

### Install certificate

<pre>sudo certbot --nginx -d domain -d www.domain.whatever</pre>

### Renew Certificate

<pre>wget https://dl.eff.org/certbot-auto && chmod a+x certbot-auto</pre>
<pre>sudo mv certbot-auto /etc/letsencrypt/</pre>
<pre>cd /etc/letsencrypt/ && ./certbot-auto renew</pre>

### U can use cron to auto renew

<pre>sudo crontab -e</pre>

### Add this to the cron file (this will execute every month)

<pre>0 0 1 * * cd /etc/letsencrypt/ && ./certbot-auto renew && sudo systemctl restart nginx</pre>
