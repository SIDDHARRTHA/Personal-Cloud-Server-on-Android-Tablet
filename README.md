# 🚀 Personal Cloud Server on Android Tablet

This project demonstrates how to transform an Android tablet into a **production-like backend server** using **Termux, Node.js, Nginx, PM2, and Cloudflare Tunnel**.

---

## 📌 Features

* 🌐 Public server hosted from a tablet
* 🔁 Reverse proxy using Nginx
* ⚙️ Process management with PM2
* 🔐 HTTPS with custom domain (Cloudflare)
* 🚇 Secure tunneling (no port forwarding required)
* 🧩 Multi-service architecture (`/test`, `/ai`)

---

## 🛠️ Tech Stack

* Node.js (Backend)
* Express.js
* Nginx (Reverse Proxy)
* PM2 (Process Manager)
* Cloudflare Tunnel
* Termux (Android environment)

---

# 🧱 1. Environment Setup (Termux)

```bash
pkg update && pkg upgrade
pkg install nodejs git nginx nano
```

---

# 📁 2. Project Structure

```bash
mkdir -p ~/server/projects
cd ~/server/projects

mkdir test ai
```

---

# 🧪 3. Create Test Server

```bash
cd ~/server/projects/test
npm init -y
npm install express
nano app.js
```

```js
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Test Server Running 🚀");
});

app.listen(3000);
```

---

# 🤖 4. Create AI Server

```bash
cd ~/server/projects/ai
npm init -y
npm install express
nano app.js
```

```js
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("AI Server Running 🤖");
});

app.listen(3001);
```

---

# ⚙️ 5. Install & Setup PM2

```bash
npm install -g pm2

pm2 start ~/server/projects/test/app.js --name test
pm2 start ~/server/projects/ai/app.js --name ai

pm2 save
```

---

# 🌐 6. Configure Nginx

```bash
nano $PREFIX/etc/nginx/nginx.conf
```

Replace config with:

```nginx
events {}

http {
    server {
        listen 8080;

        location / {
            return 200 "Main Server Running 🚀";
        }

        location /test/ {
            proxy_pass http://127.0.0.1:3000/;
        }

        location /ai/ {
            proxy_pass http://127.0.0.1:3001/;
        }
    }
}
```

Start nginx:

```bash
nginx
```

---

# 🌍 7. Public Access (Cloudflare Tunnel)

## Install

```bash
pkg install cloudflared
```

---

## Quick Tunnel (Temporary)

```bash
cloudflared tunnel --url http://localhost:8080
```

---

## Permanent Domain Setup

```bash
cloudflared tunnel login
cloudflared tunnel create myserver
```

Create config:

```bash
nano ~/.cloudflared/config.yml
```

```yaml
tunnel: myserver
credentials-file: /data/data/com.termux/files/home/.cloudflared/<ID>.json

ingress:
  - hostname: yourdomain.com
    service: http://localhost:8080

  - service: http_status:404
```

---

## Connect DNS

```bash
cloudflared tunnel route dns myserver yourdomain.com
```

---

## Run Tunnel

```bash
cloudflared tunnel run myserver
```

---

# 🔁 8. One-Command Startup (Automation)

Edit:

```bash
nano ~/.bashrc
```

Add:

```bash
alias startserver='echo "Starting..." && pm2 resurrect && pm2 start all; pkill nginx 2>/dev/null; nginx; cloudflared tunnel run myserver'
```

Reload:

```bash
source ~/.bashrc
```

Run:

```bash
startserver
```

---

# 🧪 9. Testing

### Local:

```
http://localhost:8080/test/
http://localhost:8080/ai/
```

### Public:

```
https://yourdomain.com/test/
https://yourdomain.com/ai/
```

---

# ⚠️ Troubleshooting

### Nginx port error

```bash
pkill nginx
nginx
```

### PM2 apps not running

```bash
pm2 start all
pm2 save
```

### Tunnel not working

```bash
cloudflared tunnel run myserver
```

---

# 🚀 Future Improvements

* 🤖 Integrate ML model (Apna Sehat)
* 🗄️ Add database (MongoDB / SQLite)
* 📱 Connect Android app
* 📂 File upload system
* 🔐 Authentication system

---

# 🧠 What This Project Demonstrates

* Backend deployment skills
* DevOps fundamentals
* Networking & reverse proxy setup
* Cloud tunneling & domain configuration
* Resource-constrained system engineering

---

# 📌 Author

**Siddharrtha Shankar**

---

# ⭐ If you like this project

Give it a star ⭐ on GitHub!
