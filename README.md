🚀 NGINX as a Reverse Proxy
📌 Overview

A reverse proxy is a server that receives client requests, forwards them to backend servers, and then returns the backend response to the client.

NGINX is one of the most widely used reverse proxy solutions in production environments due to its performance, scalability, and reliability.

In this project, NGINX is configured to forward incoming HTTP requests to a Node.js backend application running on port 3000.

🏗 Architecture

Client → NGINX (Port 80) → Node.js Backend (Port 3000)

NGINX handles:

Request forwarding

Preserving client headers

Acting as a single entry point to backend services

⚙️ NGINX Reverse Proxy Configuration

File: /etc/nginx/sites-available/default

server {
    listen 80;
    server_name localhost;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}

*****************🔍 Configuration Breakdown***************
Directive	Purpose
                       proxy_pass	:Forwards client requests to the backend server
                       proxy_set_header Host:	Passes the original host header to backend
                       proxy_set_header X-Real-IP:	Sends the real client IP address

                       
*******************🧪 Demo: Reverse Proxy to a Node.js Application************************
**Step 1**: Install Node.js
sudo apt update
sudo apt install nodejs npm -y

**Step 2**: Create a Simple Backend Application
*mkdir ~/node-backend && cd ~/node-backend*
nano server.js


server.js

const http = require('http');

http.createServer((req, res) => {
  res.end('Hello from Node.js backend!');
}).listen(3000);

console.log("Server running on port 3000");


Run the application:

node server.js


Your backend is now running at:

http://localhost:3000

**Step 3**: Configure NGINX

Edit the default NGINX site:

sudo nano /etc/nginx/sites-available/default


Ensure the location / block contains:

location / {
    proxy_pass http://localhost:3000;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
}

**Step 4**: Test and Reload NGINX

Check configuration syntax:

sudo nginx -t


Reload NGINX:

sudo systemctl reload nginx

Step 5: Verify in Browser

Open:

http://localhost


✅ Expected Output:

Hello from Node.js backend!


This confirms NGINX is successfully forwarding requests to the Node.js application.
