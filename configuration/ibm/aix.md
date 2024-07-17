# AIX

### System Configuration Files

* /etc/passwd - Contains local user account information.
* /etc/security/passwd - Contains encrypted user passwords.
* /etc/shadow - If exists, contains shadow passwords.
* /etc/group - Contains group information.
* /etc/security/group - Contains additional group definitions.
* /etc/services - Defines network services and ports.
* /etc/inittab - Initialization table.
* /etc/hosts - Contains local DNS entries.
* /etc/resolv.conf - Contains DNS client configuration.
* /etc/exports - NFS export configuration.
* /etc/security/user - Contains user-specific security settings.
* /etc/sudoers - Contains sudo configuration.

### User Directories

* /home/\* - User home directories often contain interesting files.

### Log Files

* /var/log/audit/audit.log - Audit logs.
* /var/log/syslog - System logs.
* /var/adm/sulog - Contains log of su command usage.
* /var/adm/wtmp and /var/adm/lastlog - Contains login records.

### Network Configuration

* /etc/rc.tcpip - Contains TCP/IP startup script.
* /etc/protocols - Contains network protocols definitions.
* /etc/rpc - Contains Remote Procedure Call information.

### Software

* /usr/bin/ - Common binaries.
* /usr/sbin/ - System binaries.
* /usr/local/bin/ - Locally installed binaries.
* /usr/local/sbin/ - Locally installed system binaries.
* /opt/ - Optional add-on software.

### System Information

* /proc - Contains process information.
* /etc/oslevel - Contains OS level information.

### Backup and Archive Files

Any .bak, .tar, .zip, .gz, etc. files, especially in directories associated with important services or applications.

```bash
find / -type f -name "*.zip" 2>/dev/null
```
