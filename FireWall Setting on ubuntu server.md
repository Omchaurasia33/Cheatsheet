
# React App Deployment on Ubuntu Server  

This guide outlines the steps to deploy a React app on an Ubuntu server with a basic firewall setup for security.  

---

## 1. **Prerequisites**  
- Ubuntu server with root or sudo access.  
- React app ready for deployment (`npm run build` completed).  
- Domain name (optional but recommended).  
- Installed tools:  
  - Node.js and npm  
  - Nginx (or your preferred web server)  

---

## 2. **Steps to Deploy**  

### **Step 1: Build the React App**  
1. Navigate to your project directory:  
   ```bash  
   cd /path/to/your/react-app  
   ```  
2. Build the app for production:  
   ```bash  
   npm run build  
   ```  
   The production files will be in the `build` folder.  

---

### **Step 2: Set Up the Web Server (Nginx)**  
1. Install Nginx:  
   ```bash  
   sudo apt update  
   sudo apt install nginx -y  
   ```  
2. Configure Nginx to serve the React app:  
   - Open the configuration file:  
     ```bash  
     sudo nano /etc/nginx/sites-available/react-app  
     ```  
   - Add this configuration:  
     ```nginx  
     server {  
         listen 80;  
         server_name your-domain.com;  # Replace with your domain or server IP  

         root /path/to/your/react-app/build;  
         index index.html;  

         location / {  
             try_files $uri /index.html;  
         }  
     }  
     ```  
   - Save and exit the file.  

3. Enable the site:  
   ```bash  
   sudo ln -s /etc/nginx/sites-available/react-app /etc/nginx/sites-enabled/  
   sudo nginx -t  # Test configuration  
   sudo systemctl restart nginx  
   ```  

---

### **Step 3: Configure Firewall (UFW)**  
1. Enable UFW:  
   ```bash  
   sudo ufw enable  
   ```  
2. Allow necessary ports:  
   ```bash  
   sudo ufw allow 80/tcp    # HTTP  
   sudo ufw allow 443/tcp   # HTTPS  
   sudo ufw allow 22/tcp    # SSH  
   ```  
3. Check the status:  
   ```bash  
   sudo ufw status verbose  
   ```  

---

## 3. **Optional: Add SSL with Let's Encrypt**  
For HTTPS:  
```bash  
sudo apt install certbot python3-certbot-nginx  
sudo certbot --nginx -d your-domain.com -d www.your-domain.com  
sudo certbot renew --dry-run  # Test renewal  
```  

---

## 4. **Access Your App**  
- Visit `http://your-domain.com` or `http://your-server-ip` to see your deployed app!  

---

## 5. **FAQ**  
- **Q:** What if I need to make changes to my app?  
  **A:** Rebuild the app (`npm run build`) and replace the contents of the `build` folder.  
- **Q:** How do I secure my server?  
  **A:** Close unnecessary ports and use HTTPS for communication.  

---  

Enjoy your React app! 🚀  
