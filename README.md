# VPS SETUP

### STEP 1
<pre>
  <code id="example-code">
   ssh root@your_ip_adress
  </code>
</pre>
Then enter your password

### STEP 2
Updated backdated packges
<pre>
  <code id="example-code">
   sudo apt update
  </code>
</pre>
<pre>
  <code id="example-code">
   sudo apt upgrade
  </code>
</pre>

### STEP 3
Enabling and Configuring UFW (Uncomplicated Firewall)

<pre>
  <code id="example-code">
   sudo ufw enable
  </code>
</pre>

<pre>
  <code id="example-code">
   sudo ufw allow 22
  </code>
</pre>

Check allow port status 

<pre>
  <code id="example-code">
   sudo ufw status
  </code>
</pre>

### STEP 3
Install NVM & Node.js

<pre>
  <code id="example-code">
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
  </code>
</pre>

<pre>
  <code id="example-code">
    nvm -v
    nvm install --lts
    nvm install 18.17.1 (for specific version)
    node -v
    npm -v
  </code>
</pre>
Note: If version not showing please close the terminal and try again

## STEP 4
git hub connection 
<pre>
  <code id="example-code">
    ls -al ~/.ssh
  </code>
</pre>
<pre>
  <code id="example-code">
    ssh-keygen -t rsa -b 4096 -C "github@smtech24.com"
  </code>
</pre>
<pre>
  <code id="example-code">
    eval "$(ssh-agent -s)"
  </code>
</pre>
<pre>
  <code id="example-code">
    cat ~/.ssh/id_rsa.pub
  </code>
</pre>
Then copy the ssh key and pas it on your SSH and GPG kesy on your git hub clik new ssh key and write project name and past the key

<pre>
  <code id="example-code">
   ssh -T git@github.com
  </code>
</pre>

### STEP 5
Setup nginx 
<pre>
  <code id="example-code">
    sudo ufw allow 80/tcp
    sudo ufw allow 3000
    sudo ufw allow 443/tcp
    sudo ufw reload
    sudo apt install nginx -y
    sudo systemctl status nginx
    sudo systemctl start nginx
    sudo systemctl enable nginx
    sudo ufw allow 'Nginx Full'
    sudo ufw enable
  </code>
</pre>

### STEP 6
Configure nginx 
<pre>
  <code id="example-code">
    sudo nano /etc/nginx/sites-available/yourdomain.com

    #this is for next js
    server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;
    location / {
        proxy_pass http://localhost:3000; # Next.js runs on port 3000
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        }
    }
    
    sudo ln -s /etc/nginx/sites-available/yourdomain.com /etc/nginx/sites-enabled

    #this is backend server setup
    server{
        listen 80;
        server_name api.myfinancialtrading.com;

        location / {
                proxy_pass  http://localhost:3000;
            # Add other proxy settings if needed
               proxy_set_header Host $host;
               proxy_set_header X-Forwarded-Host $host;
               proxy_set_header X-Forwarded-Proto $scheme;
               proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
     }
    
    sudo ln -s /etc/nginx/sites-available/api.yourdomain.com /etc/nginx/sites-enabled
    sudo nginx -t
    sudo systemctl reload nginx
  </code>
</pre>



### STEP 7
Install Project & Setup
<pre>
  <code id="example-code">
    git clone <git repository using ssh>
    npm install
    npm run build
    npm run start
    npm install -g pm2
    pm2 --version
      pm2 start npm --name "project-frontend" -- start (for next js frontend)
      pm2 start dist/server.js --name bigmama-backend (for backend)
    pm2 list
    pm2 startup
    pm2 save
    pm2 restart all (restart)
      pm2 stop nextjs-app (stop server)
      pm2 delete nextjs-app (delete server)
  </code>
</pre>

### STEP 8
Install SSL
<pre>
  <code id="example-code">
    sudo apt install certbot python3-certbot-nginx -y
    sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
  </code>
</pre>
