---

# Nginx Deployment with IP Whitelist using Ansible

## Overview

This project provides an automated solution to deploy and configure the Nginx web server on a group of servers using Ansible. It allows for restricting access to the server by specifying a list of allowed IP addresses dynamically.

---

## Features

- Automated installation of the Nginx web server.
- Dynamically generated configuration file for Nginx using a Jinja2 template.
- Restricts access to specific IP addresses while denying access by default.
- Automatically reloads the Nginx service upon configuration changes.

---

## Prerequisites

- **Ansible** must be installed on the control machine.
- SSH access to all target servers with a user having `sudo` privileges.
- Properly configured inventory file specifying the target servers.

---

## Project Structure

```
.
├── Nginx.yaml          # Main Ansible playbook
├── nginx.conf.j2       # Jinja2 template for Nginx configuration
├── inventory.ini       # Inventory file with server details
```

---

## How to Use

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/nginx-deployment.git
cd nginx-deployment
```

### 2. Configure the Inventory File

Edit the `inventory.ini` file to define your target servers, the SSH user (e.g., `ubuntu`), and any required connection details.

### 3. Run the Playbook

Run the following command to deploy and configure Nginx:

```bash
ansible-playbook -i inventory.ini Nginx.yaml
```

### 4. Verify the Setup

- Check the server by accessing it via the browser or `curl` from allowed IPs.
- Ensure that unauthorized IPs are denied access.

---

## Configuration Details

### Variables in `Nginx.yaml`

- **`allowed_ips`**: The list of IPs allowed to access the server.
- **`server_name`**: The server name (default: `mynginx.local`).
- **`document_root`**: Path to the web root directory (default: `/var/www/html`).

### Example Configuration File (Rendered)

The rendered Nginx configuration (`/etc/nginx/sites-available/default`) will look like this:

```nginx
server {
    listen 80;
    server_name mynginx.local;
    location / {
        root /var/www/html;
        index index.html index.htm;
        # Deny access by default
        deny all;
        # Allow access only from specified IPs
        allow 192.168.1.50;
        allow 192.168.1.60;
        allow 192.168.1.70;
    }
}
```

---

## Customization

- **Add New IPs**: Update the `allowed_ips` variable in the playbook to add or remove allowed IPs.
- **Change Server Name**: Modify the `server_name` variable in the playbook.
- **Modify Web Root**: Update the `document_root` variable to point to a different location.

---

## Troubleshooting

- **Check Nginx Configuration Syntax**:
  ```bash
  sudo nginx -t
  ```
- **Debug Ansible Playbook**:
  Run the playbook with verbose output:
  ```bash
  ansible-playbook -i inventory.ini Nginx.yaml -vvv
  ```
- **Restart Nginx Manually**:
  ```bash
  sudo systemctl restart nginx
  ```

---

## License

This project is open-source and licensed under the MIT License. Feel free to use and adapt it as needed.

---
