# Angular Deployment 
- Required Node.js and Angular-CLI on Development PC

# Build Angular App
- ng build --prod 

# Copy Build files to Server 
- scp -r /dist/project_name remote_servername@remote_serverip:/server_path

# Nginx 
- apt install nginx 
- mv project folder /var/www/

# Configure Soure File Permission 
- sudo chown -R username:username /var/www/path
- sudo chmod -R 755 /var/www/path

# Nginx Web Server Configuration 
# Create configuration file (/etc/nginx/sites-available/example.com or example)
- server {
       listen 80;
       listen [::]:80;
       root /var/www/my-dashboard;
       index index.html;
       location / {
               try_files $uri $uri/ =404;
       }
}

# Enabling your Website (By Creating a Symlink)
- sudo ln -s /etc/nginx/sites-available/example.com or example /etc/nginx/sites-enabled/example.com or example

# 