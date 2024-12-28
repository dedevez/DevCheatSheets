### **DevCheatSheet: Setting Up a DigitalOcean Droplet for Website Deployment**

---

### **1. Connect to Your Droplet**
First, SSH into your droplet as the `root` user:
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
For security, avoid using the `root` user for daily operations or deployment.

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

### **4. Update SSH Configuration**
Enhance security by disabling password authentication and root login.

1. Edit the SSH configuration file:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```

2. Update the following settings:
   ```plaintext
   PermitRootLogin no
   PasswordAuthentication no
   PubkeyAuthentication yes
   UsePAM yes
   ```

3. Restart the SSH service:
   ```bash
   sudo systemctl restart ssh
   ```

4. Test SSH access as the non-root user to ensure it works before logging out.

---

### **5. Configure a Firewall**
Set up a basic firewall to allow only necessary services.

1. Allow OpenSSH through the firewall:
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

### **6. Install Nginx**
Nginx is a lightweight and high-performance web server.

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

4. Test by visiting your server’s IP address in a browser.

---

### **7. Secure the Website with Let's Encrypt**
Install an SSL certificate using Certbot for HTTPS.

1. Install Certbot and the Nginx plugin:
   ```bash
   sudo apt install certbot python3-certbot-nginx -y
   ```

2. Obtain an SSL certificate:
   ```bash
   sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
   ```

3. Test the automatic renewal process:
   ```bash
   sudo certbot renew --dry-run
   ```

4. Set up a cron job to automatically renew SSL certificates:
   ```bash
   sudo crontab -e
   ```
   Add the following line:
   ```bash
   0 0 * * * certbot renew --quiet && systemctl reload nginx
   ```

---

### **8. Configure Nginx for Your Website**
Set up Nginx to serve your website.

1. Create an Nginx configuration file for your website:
   ```bash
   sudo nano /etc/nginx/sites-available/yourdomain.com
   ```

2. Add the following configuration (replace `yourdomain.com` and `/path/to/your/site` with your domain and site path):
   ```plaintext
   server {
       listen 80;
       listen [::]:80;
       server_name yourdomain.com www.yourdomain.com;

       root /path/to/your/site;
       index index.html index.htm;

       location / {
           try_files $uri $uri/ =404;
       }

       listen 443 ssl;
       ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
       ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;

       include /etc/letsencrypt/options-ssl-nginx.conf;
       ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;
   }
   ```

3. Enable the site:
   ```bash
   sudo ln -s /etc/nginx/sites-available/yourdomain.com /etc/nginx/sites-enabled/
   ```

4. Test the Nginx configuration:
   ```bash
   sudo nginx -t
   ```

5. Reload Nginx to apply changes:
   ```bash
   sudo systemctl reload nginx
   ```

---

### **9. Harden the Server (Optional but Recommended)**
1. **Disable Unused Ports**:
   Close any ports not in use (e.g., FTP, Telnet).

2. **Install Fail2Ban**:
   Protect against brute-force attacks:
   ```bash
   sudo apt install fail2ban -y
   ```

3. **Enable Automatic Updates**:
   ```bash
   sudo apt install unattended-upgrades -y
   sudo dpkg-reconfigure --priority=low unattended-upgrades
   ```

---

### **10. Deploy Your Application**
1. Upload your application to the droplet using `scp` or a deployment pipeline:
   ```bash
   scp -r /local/path/to/your/app youruser@your_droplet_ip:/remote/path/to/your/app
   ```

2. Restart Nginx to serve the new content:
   ```bash
   sudo systemctl reload nginx
   ```

---

### **Best Practices**
- **Regular Backups**: Use DigitalOcean’s snapshot feature or a backup solution.
- **Use a Monitoring Tool**: Install a monitoring agent like New Relic or DigitalOcean Monitoring.
- **Keep Software Updated**: Regularly update your server software to patch vulnerabilities:
  ```bash
  sudo apt update && sudo apt upgrade -y
  ```

---

### **Summary**
1. Create a non-root user and secure SSH.
2. Set up a firewall and install Nginx.
3. Add an SSL certificate with Let's Encrypt.
4. Configure Nginx to serve your website.
5. Harden the server and keep it updated.
