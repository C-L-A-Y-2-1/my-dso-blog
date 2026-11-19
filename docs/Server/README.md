# V-Server Setup

This document describes the setup of a virtual server for this Docusaurus project. It documents the SSH access, NGINX installation and configuration, Git setup, and the validation steps required for submission.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Server information](#server-information)
- [1. SSH key setup](#1-ssh-key-setup)
- [2. Disable password authentication](#2-disable-password-authentication)
- [3. Install and configure NGINX](#3-install-and-configure-nginx)
- [4. Configure Git](#4-configure-git)
- [5. Configure GitHub access on the server](#5-configure-github-access-on-the-server)
- [Testing](#testing)

## Prerequisites

- A Linux virtual server
- A local computer with an SSH client
- An SSH key pair generated on the local computer
- A GitHub account
- The NGINX package available through the server's package manager
- The existing Docusaurus project repository

Do not place private keys, passwords, or private configuration values in this repository.

## Server information

Replace the placeholders below with the actual values for the server:

```text
Server IP address: <SERVER_IP>
Server username: <SERVER_USER>
Website URL: http://<SERVER_IP>
```

The server IP address may be shared in the project submission so that the mentor can verify the configuration.

## 1. SSH key setup

### Generate a key pair locally

Generate the key pair on the local computer, not on the server:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

The private key should remain on the local computer. Only the public key is added to the server.

### Add the public key to the server

On the local computer, copy the public key to the server:

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub <SERVER_USER>@<SERVER_IP>
```

The public key is added to `~/.ssh/authorized_keys` for the selected server user.

Alternatively, log in to the server and append the public key manually:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

Never paste the private key or its content into GitHub or this documentation.

### Verify key-based login

Before disabling password authentication, verify that key-based login works:

```bash
ssh <SERVER_USER>@<SERVER_IP>
```

If the login succeeds, the server may proceed to the password-authentication change.

## 2. Disable password authentication

Edit the SSH daemon configuration:

```bash
sudo nano /etc/ssh/sshd_config
```

Set or confirm the following value by removing the leading comment marker:

```text
PasswordAuthentication no
```

The other SSH settings were left at their default values. Public-key authentication remains enabled by the existing server configuration.

Check the effective configuration and restart the SSH service:

```bash
sudo sshd -t
sudo systemctl restart ssh
```

Keep the current SSH session open while testing. From a second terminal, verify that an SSH key still works:

```bash
ssh <SERVER_USER>@<SERVER_IP>
```

Verify password authentication behavior with a normal login attempt from a second terminal:

```bash
ssh <SERVER_USER>@<SERVER_IP>
```

The login with the configured SSH key succeeds without asking for the server password. Since `PasswordAuthentication no` is configured, a login attempt that does not provide a valid SSH key should not prompt for or accept a password.

## 3. Install and configure NGINX

### Install NGINX

Update the package index and install NGINX:

```bash
sudo apt update
sudo apt install nginx
sudo systemctl enable nginx
sudo systemctl start nginx
```

Check its status:

```bash
sudo systemctl status nginx
```

### Configure a custom entry page

For a custom HTML entry point, create a web directory and HTML file:

```bash
sudo mkdir -p /var/www/server-entry
sudo nano /var/www/server-entry/index.html
```

Example content:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>V-Server is running</title>
  </head>
  <body>
    <h1>NGINX is running</h1>
    <p>This virtual server is configured successfully.</p>
  </body>
</html>
```

Create or edit an NGINX server block:

```bash
sudo nano /etc/nginx/sites-available/server-entry
```

Example configuration:

```nginx
server {
    listen 80;
    listen [::]:80;

    server_name _;
    root /var/www/server-entry;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Enable the server block and test the configuration:

```bash
sudo ln -s /etc/nginx/sites-available/server-entry /etc/nginx/sites-enabled/server-entry
sudo nginx -t
sudo systemctl reload nginx
```

The default NGINX configuration may need to be disabled if it captures port 80 first:

```bash
sudo rm /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl reload nginx
```

For the existing Docusaurus project, `/var/www/server-entry` can instead contain the generated `build/` directory. The directory must remain readable by the `www-data` user.

## 4. Configure Git

Configure Git with the same name and email used for GitHub commits:

```bash
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
```

Verify the configuration:

```bash
git config --global --get user.name
git config --global --get user.email
```

These values should match the server user's GitHub profile or commit identity.

## 5. Configure GitHub access on the server

Generate a separate SSH key pair on the server for GitHub:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
cat ~/.ssh/id_ed25519.pub
```

Add the displayed public key to GitHub under **Settings > SSH and GPG keys > New SSH key**. Do not add the private key.

Verify the GitHub connection:

```bash
ssh -T git@github.com
```

Clone or pull the repository using SSH:

```bash
git clone git@github.com:C-L-A-Y-2-1/my-dso-blog.git
cd my-dso-blog
git pull origin main
```

## Testing

Complete all checks before submission:

- [ ] Key-based SSH login succeeds.
- [ ] Password authentication is disabled.
- [ ] NGINX is active.
- [ ] `nginx -t` completes without errors.
- [ ] The custom page or Docusaurus build is visible in the browser.

Test key-based SSH access:

```bash
ssh <SERVER_USER>@<SERVER_IP>
```

Test the password-login behavior with a normal login attempt:

```bash
ssh <SERVER_USER>@<SERVER_IP>
```

If public-key authentication is successful and `PasswordAuthentication no` is active, no password prompt should appear. The server should not accept password-based login.

Test the NGINX main configuration:

```bash
sudo nginx -t
```

Test an isolated configuration file when applicable:

```bash
sudo nginx -t -c /path/to/nginx.conf
```

Open the following address in a browser:

```text
http://<SERVER_IP>
```

The browser should display the custom HTML page or the deployed Docusaurus page instead of the default NGINX page.
