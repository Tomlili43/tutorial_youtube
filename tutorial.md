Certainly! Here's a tutorial formatted version of the guide:

---

# Setting Up FreeSSL with ACME for Automatic SSL Certificate Renewal (Supports Wildcard Domains)

Managing SSL certificates, especially for domains with multiple subdomains, can be a cumbersome task. FreeSSL provides a solution with its ACME (Automated Certificate Management Environment) automatic renewal feature. This tutorial will guide you through the process of setting up FreeSSL with ACME using the acme.sh script for automatic SSL certificate renewal, including support for wildcard domains.

## Prerequisites:

1. Linux Server: Ensure you have access to a server running a Linux distribution.
2. Domain Name: Have a registered domain for which you want to obtain a wildcard SSL certificate.
3. Root Access: You need root or sudo access to install packages and configure the server.

## Step 1: Install acme.sh

```bash
sudo apt install socat
curl https://get.acme.sh | sh -s email=<your_email_address>
source ~/.bashrc
```

## Step 2: Apply for SSL Certificate on FreeSSL

- Create a new SSL certificate on the FreeSSL website.
- Set the brand to "亚洲诚信续期."
- Add your main domain and wildcard domain (e.g., example.com and *.example.com).

## Step 3: Configure ACME Settings

- Add the domain to ACME and configure DNS resolution.
- Wait for DNS resolution to take effect, then complete the configuration.

## Step 4: Deploy the Certificate on the Server

```bash
acme.sh --issue -d example.com -d *.example.com --dns dns_dp --server <server>
acme.sh --install-cert -d example.com \
  --key-file /etc/nginx/ssl/example.key \
  --fullchain-file /etc/nginx/ssl/example.pem \
  --reloadcmd "service nginx force-reload"
```

## Step 5: Configure Nginx

```nginx
server {
    listen 443 ssl;
    listen [::]:443;
    server_name example.com;
    ssl on;
    ssl_certificate /etc/nginx/ssl/example.pem;
    ssl_certificate_key /etc/nginx/ssl/example.key;
}
```

## Step 6: Handling Multiple Subdomains

```nginx
server {
    listen 443 ssl;
    listen [::]:443;
    server_name test.example.com;
    ssl on;
    ssl_certificate /etc/nginx/ssl/example.pem;
    ssl_certificate_key /etc/nginx/ssl/example.key;
}
```

This tutorial provides a step-by-step guide to simplify SSL certificate management using FreeSSL with ACME, ensuring automatic renewal and supporting wildcard domains. Follow these steps to streamline the process and avoid the hassle of manually renewing certificates.