# Nginx Reverse Proxy - Ubuntu Configuration

## Prerequisites:
- Ubuntu Server
- Nginx installed (Installation guide: [Nginx Linux Packages](https://nginx.org/en/linux_packages.html))

## Steps to Set Up:
1. Install Nginx if not already installed:
    ```bash
    sudo apt install nginx
    ```
2. Edit the Nginx configuration file:
    ```bash
    sudo nano /etc/nginx/nginx.conf
    ```
3. Copy the following configuration into `nginx.conf`:
    ```nginx
    worker_processes auto;

    events {
        worker_connections 1024;
    }

    http {
        include       mime.types;
        default_type  application/octet-stream;

        sendfile        on;
        keepalive_timeout  65;

        server {
            listen 80;
            server_name domaine.com;

            location / {
                proxy_pass http://127.0.0.1:3000;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
            }
        }
    }
    ```
4. Test the Nginx configuration:
    ```bash
    sudo nginx -t
    ```
5. Reload Nginx to apply the changes:
    ```bash
    sudo nginx -s reload
    ```

## Notes:
- Ensure your application is running on http://127.0.0.1:3000.
- Adjust `server_name` in the configuration to match your domain.

