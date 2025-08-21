# Deployment Guide

This guide provides advanced deployment instructions for running the SNMP MIB Platform in a production environment.

## 📋 Prerequisites

Ensure you have completed the steps in the [Quick Start](../README_EN.md#--quick-start) section of the main README file. This includes cloning the repository and running the initial setup script.

## 🔧 Service Management

The `deploy-simple.sh` script generates three helpful scripts in the root directory for managing the application:

-   `start.sh`: Starts the frontend and backend services in the background using `nohup`.
-   `stop.sh`: Stops the services by reading their process IDs from `.pid` files.
-   `status.sh`: Checks if the processes are running and reports their status.

## ⚙️ Running as a `systemd` Service (Linux)

For production deployments on Linux, it is highly recommended to run the platform as a `systemd` service for automatic startup and process management.

### 1. Install Services

The repository includes pre-configured service files. Run the installation script with `sudo` to copy them to the correct system directory:

```bash
sudo ./install-systemd-services.sh
```
This script will copy the service files and set the correct paths, assuming you have the project located at `/opt/snmp-mib-ui`. If your project is in a different location, you will need to edit the `WorkingDirectory` and `ExecStart` paths in the `systemd/snmp-mib-backend.service` and `systemd/snmp-mib-frontend.service` files before running the script.

### 2. Manage Services

You can now manage the platform using standard `systemctl` commands:

-   **Start all services:**
    ```bash
    sudo systemctl start snmp-mib-platform.target
    ```

-   **Stop all services:**
    ```bash
    sudo systemctl stop snmp-mib-platform.target
    ```

-   **Check overall status:**
    ```bash
    sudo systemctl status snmp-mib-platform.target
    ```

-   **Check individual service status:**
    ```bash
    sudo systemctl status snmp-mib-backend.service
    sudo systemctl status snmp-mib-frontend.service
    ```

-   **Enable auto-start on boot:**
    ```bash
    sudo systemctl enable snmp-mib-platform.target
    ```

-   **View logs:**
    ```bash
    sudo journalctl -u snmp-mib-backend.service -f
    sudo journalctl -u snmp-mib-frontend.service -f
    ```

## 🔒 Production Security

### Reverse Proxy (Nginx)

It is recommended to run the application behind a reverse proxy like Nginx to handle HTTPS, apply rate limiting, and serve traffic from standard ports (80/443).

Below is an example Nginx configuration. This assumes you have an SSL certificate and key.

```nginx
# /etc/nginx/sites-available/snmp-platform.conf

server {
    listen 80;
    server_name your-domain.com;

    # Redirect all HTTP traffic to HTTPS
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name your-domain.com;

    ssl_certificate /path/to/your/cert.pem;
    ssl_certificate_key /path/to/your/key.pem;

    # Security headers
    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";
    add_header Referrer-Policy "no-referrer";
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;

    # Proxy to the frontend Next.js app
    location / {
        proxy_pass http://localhost:12300;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # Proxy to the backend Go API
    # All requests starting with /api/ will be forwarded
    location /api/ {
        proxy_pass http://localhost:17880/api/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Firewall

Use a firewall (like `ufw` on Ubuntu) to restrict access to the application ports. If you are using a reverse proxy, you only need to allow access to the proxy's ports (e.g., 80 and 443).

```bash
# Allow SSH, HTTP, and HTTPS
sudo ufw allow 22
sudo ufw allow 80
sudo ufw allow 443

# Deny direct access to the application ports
sudo ufw deny 12300
sudo ufw deny 17880

# Enable the firewall
sudo ufw enable
```

## 📄 Environment Variables

You can customize the application's behavior using environment variables. These can be set in your shell profile (e.g., `~/.bashrc`) or in the `systemd` service files.

-   `FRONTEND_PORT`: The port for the Next.js frontend. Defaults to `12300`.
-   `SERVER_PORT`: The port for the Go backend API. Defaults to `17880`.
-   `SQLITE_DB_PATH`: The file path for the SQLite database. Defaults to `./data/snmp_platform.db`.
-   `JWT_SECRET`: A secret key for signing JWTs. A default is provided, but it's recommended to change it for production.
-   `CACHE_MAX_MEMORY`: The maximum memory (in MB) for the in-memory cache. Defaults to `256`.
-   `LOG_LEVEL`: The logging level. Can be `DEBUG`, `INFO`, `WARN`, or `ERROR`. Defaults to `INFO`.
