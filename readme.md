# Assignment 3

## Step 1: Starting from a Fresh Debian 12 server on DigitalOcean
### 1. Create new droplet on DigitalOcean, copy the IP address of the new droplet
### 2. Login to your server as the root user using the command: 
    ssh -i ~/.ssh/do-key root@your-server-ip-address 
    
    


## Step 2: Create a new regular user: useradd your-username
### 1. Enable user performing administrative task with the following command: 
    usermod -aG sudo your-username
### 2. User has bash as login shell by: 
    sudo chsh -s /bin/bash your-username  
### 3. User can access the server via SSH: 
    cp -r .ssh /home/your-username
    chown -R your-username:your-username /home/your-username/.ssh

## Step 3: To prevent the root user from connecting to the server via SSH, first you have to exit the root user by typing "exit".
### 1. Login to your new user: 
    ssh -i ~/.ssh/do-key your-username@your-server-ip-address
### 2. Open the SSH file: 
    sudo vim /etc/ssh/sshd_config
### 3. Look for the line "PermitRootLogin yes" change the permission to "no", save and exit vim.
### 4. Restart the SSH server: 
    sudo systemctl restart ssh

## Step 4: Install nginx
    sudo apt install nginx

## Step 5: Configure nginx to serve a sample website
### 1. Go to: 
    cd /var/www
### 2. Create document to serve: Create a new directory and then create an index.html file that contains the code below:
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>2420</title>
        <style>
            body {
                display: flex;
                align-items: center;
                justify-content: center;
                height: 100vh;
                margin: 0;
            }
            h1 {
                text-align: center;
            }
        </style>
    </head>
    <body>
        <h1>Hello, World</h1>
    </body>
    </html>

### 3. Open Nginx server block configuration file: 
    sudo vim /etc/nginx/sites-available/default
### 4. Scroll all the way down and uncomment those lines:
    server {
	    listen 80 default_server;
	    listen [::]:80 default_server;
	
	    root /var/www/html;
	
	    index index.html index.htm index.nginx-debian.html;
	
	    server_name _;
	
	    location / {
		    # First attempt to serve request as file, then
		    # as directory, then fall back to displaying a 404.
		    try_files $uri $uri/ =404;
	    }
    }

### 5. Save and exit the file.
### 6. Test Nginx configuration file:
    sudo nginx -t
 




