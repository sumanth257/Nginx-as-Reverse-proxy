# Nginx-as-Reverse-proxy
*******REVERSE PROXY*******
A reverse proxy is a server that receives client requests and forwards them to backend servers, then sends the response back to the client.

NGINX is one of the most popular tools used as a reverse proxy in production.

 File: /etc/nginx/sites-available/default
Update the existing server block or create a new one:

server {
    listen 80;
    server_name localhost;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
Breakdown:
proxy_pass → forwards requests to your backend app
proxy_set_header → preserves original request metadata (like IP and host)
🧪 Demo: Reverse Proxy to a Node.js App
Step 1: Install Node.js (optional if using your own backend)
sudo apt update
sudo apt install nodejs npm -y
Step 2: Create a simple backend app
mkdir ~/node-backend && cd ~/node-backend
nano server.js

Paste this:

const http = require('http');
http.createServer((req, res) => {
  res.end('Hello from Node.js backend!');
}).listen(3000);
Run it:

node server.js
Your app is now running at http://localhost:3000

Step 3: Configure NGINX as reverse proxy
Edit the NGINX default site:

sudo nano /etc/nginx/sites-available/default
Replace the location / {} block with:

location / {
    proxy_pass http://localhost:3000;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
}
Step 4: Test and reload NGINX
Check config for syntax errors:

sudo nginx -t
Reload NGINX:

sudo systemctl reload nginx
Step 5: Test in browser
Visit:

http://localhost
✅ You should see: Hello from Node.js backend!
