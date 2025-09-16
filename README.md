# vanilla Installation Guide

vanilla is a free and open-source community forum. Vanilla provides modern forum software focused on successful communities

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Supported Operating Systems](#supported-operating-systems)
3. [Installation](#installation)
4. [Configuration](#configuration)
5. [Service Management](#service-management)
6. [Troubleshooting](#troubleshooting)
7. [Security Considerations](#security-considerations)
8. [Performance Tuning](#performance-tuning)
9. [Backup and Restore](#backup-and-restore)
10. [System Requirements](#system-requirements)
11. [Support](#support)
12. [Contributing](#contributing)
13. [License](#license)
14. [Acknowledgments](#acknowledgments)
15. [Version History](#version-history)
16. [Appendices](#appendices)

## 1. Prerequisites

- **Hardware Requirements**:
  - CPU: 1 core minimum
  - RAM: 512MB minimum
  - Storage: 1GB for data
  - Network: HTTP/HTTPS access
- **Operating System**: 
  - Linux: Any modern distribution (RHEL, Debian, Ubuntu, CentOS, Fedora, Arch, Alpine, openSUSE)
  - macOS: 10.14+ (Mojave or newer)
  - Windows: Windows Server 2016+ or Windows 10
  - FreeBSD: 11.0+
- **Network Requirements**:
  - Port 80 (default vanilla port)
  - None
- **Dependencies**:
  - See official documentation for specific requirements
- **System Access**: root or sudo privileges required


## 2. Supported Operating Systems

This guide supports installation on:
- RHEL 8/9 and derivatives (CentOS Stream, Rocky Linux, AlmaLinux)
- Debian 11/12
- Ubuntu 20.04/22.04/24.04 LTS
- Arch Linux (rolling release)
- Alpine Linux 3.18+
- openSUSE Leap 15.5+ / Tumbleweed
- SUSE Linux Enterprise Server (SLES) 15+
- macOS 12+ (Monterey and later) 
- FreeBSD 13+
- Windows 10/11/Server 2019+ (where applicable)

## 3. Installation

### RHEL/CentOS/Rocky Linux/AlmaLinux

```bash
# Install EPEL repository if needed
sudo dnf install -y epel-release

# Install vanilla
sudo dnf install -y vanilla

# Enable and start service
sudo systemctl enable --now vanilla

# Configure firewall
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --reload

# Verify installation
vanilla --version
```

### Debian/Ubuntu

```bash
# Update package index
sudo apt update

# Install vanilla
sudo apt install -y vanilla

# Enable and start service
sudo systemctl enable --now vanilla

# Configure firewall
sudo ufw allow 80

# Verify installation
vanilla --version
```

### Arch Linux

```bash
# Install vanilla
sudo pacman -S vanilla

# Enable and start service
sudo systemctl enable --now vanilla

# Verify installation
vanilla --version
```

### Alpine Linux

```bash
# Install vanilla
apk add --no-cache vanilla

# Enable and start service
rc-update add vanilla default
rc-service vanilla start

# Verify installation
vanilla --version
```

### openSUSE/SLES

```bash
# Install vanilla
sudo zypper install -y vanilla

# Enable and start service
sudo systemctl enable --now vanilla

# Configure firewall
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --reload

# Verify installation
vanilla --version
```

### macOS

```bash
# Using Homebrew
brew install vanilla

# Start service
brew services start vanilla

# Verify installation
vanilla --version
```

### FreeBSD

```bash
# Using pkg
pkg install vanilla

# Enable in rc.conf
echo 'vanilla_enable="YES"' >> /etc/rc.conf

# Start service
service vanilla start

# Verify installation
vanilla --version
```

### Windows

```bash
# Using Chocolatey
choco install vanilla

# Or using Scoop
scoop install vanilla

# Verify installation
vanilla --version
```

## Initial Configuration

### Basic Configuration

```bash
# Create configuration directory
sudo mkdir -p /etc/vanilla

# Set up basic configuration
# See official documentation for detailed configuration options

# Test configuration
vanilla --version
```

## 5. Service Management

### systemd (RHEL, Debian, Ubuntu, Arch, openSUSE)

```bash
# Enable service
sudo systemctl enable vanilla

# Start service
sudo systemctl start vanilla

# Stop service
sudo systemctl stop vanilla

# Restart service
sudo systemctl restart vanilla

# Check status
sudo systemctl status vanilla

# View logs
sudo journalctl -u vanilla -f
```

### OpenRC (Alpine Linux)

```bash
# Enable service
rc-update add vanilla default

# Start service
rc-service vanilla start

# Stop service
rc-service vanilla stop

# Restart service
rc-service vanilla restart

# Check status
rc-service vanilla status
```

### rc.d (FreeBSD)

```bash
# Enable in /etc/rc.conf
echo 'vanilla_enable="YES"' >> /etc/rc.conf

# Start service
service vanilla start

# Stop service
service vanilla stop

# Restart service
service vanilla restart

# Check status
service vanilla status
```

### launchd (macOS)

```bash
# Using Homebrew services
brew services start vanilla
brew services stop vanilla
brew services restart vanilla

# Check status
brew services list | grep vanilla
```

### Windows Service Manager

```powershell
# Start service
net start vanilla

# Stop service
net stop vanilla

# Using PowerShell
Start-Service vanilla
Stop-Service vanilla
Restart-Service vanilla

# Check status
Get-Service vanilla
```

## Advanced Configuration

See the official documentation for advanced configuration options.

## Reverse Proxy Setup

### nginx Configuration

```nginx
upstream vanilla_backend {
    server 127.0.0.1:80;
}

server {
    listen 80;
    server_name vanilla.example.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name vanilla.example.com;

    ssl_certificate /etc/ssl/certs/vanilla.example.com.crt;
    ssl_certificate_key /etc/ssl/private/vanilla.example.com.key;

    location / {
        proxy_pass http://vanilla_backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Apache Configuration

```apache
<VirtualHost *:80>
    ServerName vanilla.example.com
    Redirect permanent / https://vanilla.example.com/
</VirtualHost>

<VirtualHost *:443>
    ServerName vanilla.example.com
    
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/vanilla.example.com.crt
    SSLCertificateKeyFile /etc/ssl/private/vanilla.example.com.key
    
    ProxyRequests Off
    ProxyPreserveHost On
    
    ProxyPass / http://127.0.0.1:80/
    ProxyPassReverse / http://127.0.0.1:80/
</VirtualHost>
```

### HAProxy Configuration

```haproxy
frontend vanilla_frontend
    bind *:80
    bind *:443 ssl crt /etc/ssl/certs/vanilla.pem
    redirect scheme https if !{ ssl_fc }
    default_backend vanilla_backend

backend vanilla_backend
    balance roundrobin
    server vanilla1 127.0.0.1:80 check
```

## Security Configuration

### Basic Security Setup

```bash
# Set appropriate permissions
sudo chown -R vanilla:vanilla /etc/vanilla
sudo chmod 750 /etc/vanilla

# Configure firewall
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --reload

# Enable SELinux policies (if applicable)
sudo setsebool -P httpd_can_network_connect on
```

## Database Setup

See official documentation for database configuration requirements.

## Performance Optimization

### System Tuning

```bash
# Basic system tuning
echo 'net.core.somaxconn = 65535' | sudo tee -a /etc/sysctl.conf
echo 'net.ipv4.tcp_max_syn_backlog = 65535' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

## Monitoring

### Basic Monitoring

```bash
# Check service status
sudo systemctl status vanilla

# View logs
sudo journalctl -u vanilla -f

# Monitor resource usage
top -p $(pgrep vanilla)
```

## 9. Backup and Restore

### Backup Script

```bash
#!/bin/bash
# Basic backup script
BACKUP_DIR="/backup/vanilla"
DATE=$(date +%Y%m%d_%H%M%S)

mkdir -p "$BACKUP_DIR"
tar -czf "$BACKUP_DIR/vanilla-backup-$DATE.tar.gz" /etc/vanilla /var/lib/vanilla

echo "Backup completed: $BACKUP_DIR/vanilla-backup-$DATE.tar.gz"
```

### Restore Procedure

```bash
# Stop service
sudo systemctl stop vanilla

# Restore from backup
tar -xzf /backup/vanilla/vanilla-backup-*.tar.gz -C /

# Start service
sudo systemctl start vanilla
```

## 6. Troubleshooting

### Common Issues

1. **Service won't start**:
```bash
# Check logs
sudo journalctl -u vanilla -n 100
sudo tail -f /var/log/vanilla/vanilla.log

# Check configuration
vanilla --version

# Check permissions
ls -la /etc/vanilla
```

2. **Connection issues**:
```bash
# Check if service is listening
sudo ss -tlnp | grep 80

# Test connectivity
telnet localhost 80

# Check firewall
sudo firewall-cmd --list-all
```

3. **Performance issues**:
```bash
# Check resource usage
top -p $(pgrep vanilla)

# Check disk I/O
iotop -p $(pgrep vanilla)

# Check connections
ss -an | grep 80
```

## Integration Examples

### Docker Compose Example

```yaml
version: '3.8'
services:
  vanilla:
    image: vanilla:latest
    ports:
      - "80:80"
    volumes:
      - ./config:/etc/vanilla
      - ./data:/var/lib/vanilla
    restart: unless-stopped
```

## Maintenance

### Update Procedures

```bash
# RHEL/CentOS/Rocky/AlmaLinux
sudo dnf update vanilla

# Debian/Ubuntu
sudo apt update && sudo apt upgrade vanilla

# Arch Linux
sudo pacman -Syu vanilla

# Alpine Linux
apk update && apk upgrade vanilla

# openSUSE
sudo zypper update vanilla

# FreeBSD
pkg update && pkg upgrade vanilla

# Always backup before updates
tar -czf /backup/vanilla-pre-update-$(date +%Y%m%d).tar.gz /etc/vanilla

# Restart after updates
sudo systemctl restart vanilla
```

### Regular Maintenance

```bash
# Log rotation
sudo logrotate -f /etc/logrotate.d/vanilla

# Clean old logs
find /var/log/vanilla -name "*.log" -mtime +30 -delete

# Check disk usage
du -sh /var/lib/vanilla
```

## Additional Resources

- Official Documentation: https://docs.vanilla.org/
- GitHub Repository: https://github.com/vanilla/vanilla
- Community Forum: https://forum.vanilla.org/
- Best Practices Guide: https://docs.vanilla.org/best-practices

---

**Note:** This guide is part of the [HowToMgr](https://howtomgr.github.io) collection. Always refer to official documentation for the most up-to-date information.
