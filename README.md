# Setting Up Matomo with Nginx on Ubuntu
## Prerequisites
Before starting, ensure you have:
 
- Ubuntu server installed
- Basic knowledge of working with the command line and installing packages
- Install Nginx

```bash
sudo apt update
sudo apt install nginx
```

- Install MySQL Server and PHP Dependencies
  ```bash
  sudo apt install mysql-server php php-cli php-fpm php-mysql php-gd php-curl php-zip php-xml php-mbstring unzip -y
  ``` 
# Installation steps
### 1. Install MySQL Server and PHP Dependencies
sudo apt install mysql-server php php-cli php-fpm php-mysql php-gd php-curl php-zip php-xml php-mbstring unzip -y

### 2. Remove Apache (Optional)
sudo apt purge apache2 -y

### 3. Download and Configure Matomo
# Download Matomo
wget https://builds.matomo.org/matomo-latest.zip
unzip matomo-latest.zip
# Move Matomo to web directory
sudo mv matomo /var/www/html/matomo
# Set permissions
sudo chown -R www-data:www-data /var/www/html/matomo
sudo chmod -R 755 /var/www/html/matomo

### 4. Configure Nginx for Matomo
Create a new Nginx server block configuration for Matomo:
    sudo nano /etc/nginx/sites-available/matomo

Copy and paste the following configuration into the file. Replace example.com with your domain name or IP address:
    server {
    listen 5124 default_server;
    listen [::]:5124 default_server;

    server_name example.com;  # Replace with your domain name or IP address

    root /var/www/html/matomo;
    index index.php;

    location / {
        try_files $uri $uri/ /index.php$is_args$args;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php-fpm.sock;
    }
}

### 5. Enable the Nginx Configuration
sudo ln -s /etc/nginx/sites-available/matomo /etc/nginx/sites-enabled/

### 6. Test Nginx Configuration and Restart
sudo nginx -t
sudo systemctl restart nginx

### 7. Access Matomo
Open your web browser and navigate to http://example.com:5124 (replace example.com with your domain name or IP address). Follow the on-screen instructions to complete the Matomo installation.
