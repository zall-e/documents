# How to Serve a Project on a Server Using Nginx

This guide explains how to serve your project on a server using Nginx. Follow the steps below to expose the required port, install Nginx, create and enable your configuration file, and verify the setup.

---

## 1. Expose the Port

Allow the desired port (e.g., **8080**) through the firewall. Replace `8080` with your preferred port if needed.

```bash
ufw allow 8080
```

---

## 2. Install Nginx

Install Nginx if it's not already installed:

```bash
sudo apt update
sudo apt install nginx
```

---

## 3. Configure Nginx

### a. Navigate to the Configuration Directory

```bash
cd /etc/nginx/sites-available/
```

### b. Create a New Configuration File

Create a new file for your project configuration (replace `your_project` with an appropriate name):

```bash
sudo nano your_project
```

*Write your server block configuration in this file. For example:*

```nginx
server {
    listen 8080;
    server_name your_domain_or_IP;

    location / {
        proxy_pass http://localhost:3000;  # Change 3000 to your application's port if different
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

*Save the file and exit the editor (`Ctrl+O`, then `Ctrl+X` in nano).*

### c. Enable the Configuration

Create a symbolic link in the `sites-enabled` directory:

```bash
sudo ln -s /etc/nginx/sites-available/your_project /etc/nginx/sites-enabled/
```

---

## 4. Test and Reload Nginx

### a. Test the Nginx Configuration

Make sure there are no syntax errors:

```bash
sudo nginx -t
```

### b. Reload or Restart Nginx

Reload the configuration if the test is successful:

```bash
sudo systemctl reload nginx
```

Alternatively, restart Nginx:

```bash
sudo systemctl restart nginx
```
## Conclusion

Your project should now be accessible via the specified port using Nginx. Adjust the configuration as needed to suit your specific project requirements.

---
