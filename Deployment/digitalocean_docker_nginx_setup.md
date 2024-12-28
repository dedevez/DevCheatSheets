### **DevCheatSheet: Setting Up a DigitalOcean Droplet with Nginx for SSL Termination and Docker Application Deployment**

This cheat sheet outlines how to set up your DigitalOcean droplet to serve a Dockerized application securely with **Nginx on the droplet** for SSL termination and reverse proxying and **Nginx in the Docker container** to serve the application over HTTP. 

---

### **1. Connect to Your Droplet**
SSH into your droplet as the `root` user:
```bash
ssh -i ~/.ssh/id_rsa_yourkey root@your_droplet_ip
```

---

### **2. Update and Upgrade the System**
Ensure your droplet is up-to-date:
```bash
sudo apt update && sudo apt upgrade -y
```

---

### **3. Create a Non-Root User**
For security, avoid using the `root` user for daily operations or deployments.

1. Create a new user (replace `youruser` with your desired username):
   ```bash
   adduser youruser
   ```

2. Grant the user sudo privileges:
   ```bash
   usermod -aG sudo youruser
   ```

3. Add your SSH key to the new user:
   ```bash
   mkdir -p /home/youruser/.ssh
   cp ~/.ssh/authorized_keys /home/youruser/.ssh/
   chown -R youruser:youruser /home/youruser/.ssh
   chmod 700 /home/youruser/.ssh
   chmod 600 /home/youruser/.ssh/authorized_keys
   ```

4. Test the login as the new user:
   ```bash
   ssh -i ~/.ssh/id_rsa_yourkey youruser@your_droplet_ip
   ```

---

### **4. Configure the Firewall**
Set up a firewall to allow only necessary services.

1. Allow OpenSSH traffic:
   ```bash
   sudo ufw allow OpenSSH
   ```

2. Allow HTTP and HTTPS traffic:
   ```bash
   sudo ufw allow 'Nginx Full'
   ```

3. Enable the firewall:
   ```bash
   sudo ufw enable
   ```

4. Verify the firewall status:
   ```bash
   sudo ufw status
   ```

---

### **5. Install Nginx on the Droplet**
Nginx will handle SSL termination and proxy requests to the Docker container.

1. Install Nginx:
   ```bash
   sudo apt install nginx -y
   ```

2. Start and enable the Nginx service:
   ```bash
   sudo systemctl start nginx
   sudo systemctl enable nginx
   ```

3. Verify that Nginx is running:
   ```bash
   sudo systemctl status nginx
   ```

4. Test by visiting your server’s IP address in a browser. You should see the default Nginx welcome page.

---

### **6. Obtain SSL Certificates with Let's Encrypt**
Secure your domain using Let's Encrypt certificates.

1. Install Certbot and the Nginx plugin:
   ```bash
   sudo apt install certbot python3-certbot-nginx -y
   ```

2. Obtain an SSL certificate for your domain:
   ```bash
   sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
   ```

3. Verify that HTTPS is working by visiting `https://yourdomain.com`.

4. Test automatic renewal:
   ```bash
   sudo certbot renew --dry-run
   ```

5. Set up a cron job to automatically renew SSL certificates:
   ```bash
   sudo crontab -e
   ```
   Add the following line:
   ```bash
   0 0 * * * certbot renew --quiet && systemctl reload nginx
   ```

---

### **7. Configure Nginx on the Droplet as a Reverse Proxy**
Nginx will forward HTTPS traffic to the Docker container running your application.

1. Create a new Nginx configuration file:
   ```bash
   sudo nano /etc/nginx/sites-available/yourdomain.com
   ```

2. Add the following configuration:
   ```nginx
   server {
       listen 80;
       server_name yourdomain.com www.yourdomain.com;

       return 301 https://$host$request_uri;
   }

   server {
       listen 443 ssl;
       server_name yourdomain.com www.yourdomain.com;

       ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
       ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;
       include /etc/letsencrypt/options-ssl-nginx.conf;
       ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

       location / {
           proxy_pass http://localhost:8080; # Proxy to the container
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
       }
   }
   ```

3. Enable the site and restart Nginx:
   ```bash
   sudo ln -s /etc/nginx/sites-available/yourdomain.com /etc/nginx/sites-enabled/
   sudo nginx -t
   sudo systemctl reload nginx
   ```

---

### **8. Build Your Docker Application**
Your application will include Nginx in the Docker container to serve HTTP traffic locally.

1. **Create a `Dockerfile`**:
   ```dockerfile
   # Stage 1: Build the application (for React, Vue, or any frontend framework)
   FROM node:16-alpine as build
   WORKDIR /app
   COPY package*.json ./
   RUN npm install
   COPY . .
   RUN npm run build

   # Stage 2: Serve the application with Nginx
   FROM nginx:alpine
   COPY --from=build /app/build /usr/share/nginx/html
   COPY nginx.conf /etc/nginx/conf.d/default.conf
   EXPOSE 8080
   CMD ["nginx", "-g", "daemon off;"]
   ```

2. **Create the Nginx Configuration (`nginx.conf`)**:
   ```nginx
   server {
       listen 8080;
       server_name localhost;

       location / {
           root /usr/share/nginx/html;
           index index.html;
           try_files $uri /index.html;
       }
   }
   ```

3. **Build and Run the Docker Container**:
   ```bash
   docker build -t yourapp .
   docker run -d --name yourapp -p 8080:8080 yourapp
   ```

---

### **9. Test the Setup**
1. Visit your site at `https://yourdomain.com`.
2. Verify the following:
   - The SSL certificate is working.
   - The website is being served correctly by the Nginx inside the container via the droplet-level Nginx reverse proxy.

---

### **Best Practices**
1. **Keep Your System Updated**:
   Regularly update your droplet and Docker images:
   ```bash
   sudo apt update && sudo apt upgrade -y
   docker pull yourapp:latest
   ```

2. **Monitor Your Server**:
   Use monitoring tools like DigitalOcean Monitoring or external tools like New Relic.

3. **Automate Deployments**:
   Use a CI/CD pipeline (e.g., GitHub Actions) to build and deploy Docker images to your droplet.

4. **Secure SSH Access**:
   Limit SSH access to your IP address:
   ```bash
   sudo ufw allow from your_ip_address to any port 22
   ```
