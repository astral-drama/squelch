# Squelch Security Suite

Squelch is a comprehensive security hardening package for Debian-based servers, providing automated protection against common attacks and security monitoring tools.

## Features

- **fail2ban Integration**: Advanced configuration with custom filters for web servers
- **SSH Hardening**: Secure SSH configuration templates and monitoring
- **Real-time Monitoring**: Live security status dashboards and alerts
- **Multi-platform Support**: Works with Caddy, Nginx, Apache web servers
- **IP Management**: Easy banning/unbanning of malicious IPs

## Components Installed

### Configuration Files
- `/etc/fail2ban/jail.d/squelch.conf` - Main fail2ban configuration
- `/etc/fail2ban/filter.d/squelch-*.conf` - Custom filters for various attacks

### Monitoring Tools
- `squelch-status` - Security status dashboard
- `squelch-monitor` - Real-time security monitoring
- `squelch-ban` - IP banning utility

## Quick Start

After installation, run:

```bash
# View security status
squelch-status

# Start real-time monitoring
squelch-monitor

# Ban a malicious IP
sudo squelch-ban ban 192.168.1.100

# List all banned IPs
sudo squelch-ban list
```

## Configuration

### Web Server Setup

The package includes filters for:
- **Caddy**: Automatically enabled if `/var/log/caddy/access.log` exists
- **Nginx**: Enable manually in `/etc/fail2ban/jail.d/squelch.conf`
- **Apache**: Enable manually in `/etc/fail2ban/jail.d/squelch.conf`

### SSH Security

Recommended SSH hardening steps:

1. Change SSH port from default 22
2. Disable password authentication
3. Disable root login
4. Use key-based authentication only

### Network Configuration

Update the `ignoreip` setting in `/etc/fail2ban/jail.d/squelch.conf` to match your local network:

```
ignoreip = 127.0.0.1/8 ::1 192.168.0.0/24 10.0.0.0/8 172.16.0.0/12
```

## Jails Included

| Jail Name | Purpose | Log Source |
|-----------|---------|------------|
| `sshd` | SSH brute force protection | systemd journal |
| `squelch-caddy-auth` | Caddy authentication failures | `/var/log/caddy/access.log` |
| `squelch-caddy-badbots` | Bad bot detection | `/var/log/caddy/access.log` |
| `squelch-caddy-scan` | Directory scanning attempts | `/var/log/caddy/access.log` |
| `squelch-nginx-auth` | Nginx auth failures | `/var/log/nginx/access.log` |
| `squelch-apache-auth` | Apache auth failures | `/var/log/apache2/access.log` |

## Commands Reference

### squelch-status
Displays comprehensive security status including:
- System information
- fail2ban jail status
- SSH security configuration
- Recent activity logs
- Network connections
- Top accessing IPs

### squelch-monitor
Real-time monitoring dashboard showing:
- Live jail status updates
- System load and memory usage
- Recent connection attempts
- Auto-refreshing every 5 seconds

### squelch-ban
IP management utility:
```bash
# Ban an IP
sudo squelch-ban ban <IP> [jail]

# Unban an IP
sudo squelch-ban unban <IP> [jail]

# List banned IPs
sudo squelch-ban list [jail]

# Check if IP is banned
sudo squelch-ban check <IP>
```

## Integration with OAuth

Squelch works seamlessly with OAuth2 setups:
- Ignores authenticated users from bans
- Monitors authentication endpoints
- Protects against OAuth enumeration attacks

## Maintenance

### Log Rotation
Automatic log rotation is configured for Caddy logs if the directory exists.

### Updates
To update configurations:
```bash
sudo systemctl reload fail2ban
```

### Monitoring
Check fail2ban status:
```bash
sudo systemctl status fail2ban
sudo fail2ban-client status
```

## Troubleshooting

### Common Issues

1. **fail2ban not starting**: Check log files and configuration syntax
2. **No bans occurring**: Verify log file paths and permissions
3. **False positives**: Adjust `maxretry` values or add IPs to `ignoreip`

### Debug Commands
```bash
# Test filter patterns
sudo fail2ban-regex /var/log/caddy/access.log /etc/fail2ban/filter.d/squelch-caddy-auth.conf

# Check jail configuration
sudo fail2ban-client -d

# View detailed logs
sudo journalctl -u fail2ban -f
```

## Security Best Practices

1. **Regular Updates**: Keep squelch and dependencies updated
2. **Log Monitoring**: Review security status daily with `squelch-status`
3. **Network Segmentation**: Use proper firewall rules alongside fail2ban
4. **Backup**: Backup configurations before changes
5. **Testing**: Test bans with `squelch-ban check` before implementing

## Support

For issues and updates, visit: https://github.com/your-org/squelch

## License

MIT License - See LICENSE file for details.