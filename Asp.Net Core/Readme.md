# Asp.Net Core Deployment 
```
Required Asp.net Core SDK on Development PC
```

# Build .net Core App
```
dotnet build 
```

# Release and Publish .net Core App 
```
dotnet publish -c Release -o ./Folder_Path
```

# Asp.net Core Deploy on IIS 
```
Require IIS Runtime Windows Hosting Bundle  
```

# Asp.net Core Deploy on Ubuntu 
# Install .net core Run times on Ubuntu 20.04 
```
wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb
sudo apt-get update && \
  sudo apt-get install -y aspnetcore-runtime-6.0
```

# Install Nginx 
```
apt install nginx 
mv project folder /var/www/
```

# Configure Soure File Permission 
```
sudo chown -R username:username /var/www/path
sudo chmod -R 755 /var/www/path
```

# Nginx Web Server Configuration 
# Create configuration file (/etc/nginx/sites-available/example.com or example)
```
server {
       listen 80;
       listen [::]:80;
       root /var/www/Release_Folder;
       index index.html;
       location / {
               try_files $uri $uri/ =404;
       }
}
```

# Enabling your Website (By Creating a Symlink)
```
sudo ln -s /etc/nginx/sites-available/example.com or example /etc/nginx/sites-enabled/example.com or example
```

