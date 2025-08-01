# Manual Setup Guide

**Prefer AI assistance?** → Use [AI-SETUP-ASSISTANT.md](AI-SETUP-ASSISTANT) instead.

This guide is for users who want to set up VLESS+REALITY manually without AI help.

## Prerequisites

- ✅ VPS server (1GB RAM, Ubuntu 20.04+ or Debian 11+)
- ✅ Domain name pointed to your server IP
- ✅ Root SSH access to your server
- ✅ Basic Linux command line knowledge

## Step 1: Server Preparation

### Update System
```bash
apt update && apt upgrade -y
apt install -y curl wget unzip
```

### Configure Firewall
```bash
ufw allow ssh
ufw allow 443
ufw --force enable
```

### Set Timezone (Optional)
```bash
timedatectl set-timezone UTC
```

## Step 2: Install Xray-core

### Download and Install
```bash
wget -O install.sh https://raw.githubusercontent.com/XTLS/Xray-install/main/install-release.sh
bash install.sh
```

### Verify Installation
```bash
xray version
```
Should show Xray version information.

## Step 3: Generate Security Keys

### Generate UUID for Client Authentication
```bash
xray uuid
```
**Save this UUID** - you'll need it for configuration.

### Generate REALITY Private Key
```bash
xray x25519
```
**Save both private and public keys** from the output.

### Generate Short ID
```bash
openssl rand -hex 8
```
**Save this short ID** - use first 8 characters.

## Step 4: Create Server Configuration

### Create Config Directory
```bash
mkdir -p /usr/local/etc/xray
```

### Create Configuration File
```bash
nano /usr/local/etc/xray/config.json
```

**Use this template** and replace the placeholders:

```json
{
  "log": {
    "loglevel": "warning"
  },
  "inbounds": [
    {
      "port": 443,
      "protocol": "vless",
      "settings": {
        "clients": [
          {
            "id": "YOUR_GENERATED_UUID_HERE",
            "email": "user@example.com"
          }
        ],
        "decryption": "none"
      },
      "streamSettings": {
        "network": "tcp",
        "security": "reality",
        "realitySettings": {
          "show": false,
          "dest": "one.one.one.one:443",
          "serverNames": [
            "www.microsoft.com"
          ],
          "privateKey": "YOUR_GENERATED_PRIVATE_KEY_HERE",
          "shortIds": [
            "YOUR_GENERATED_SHORT_ID_HERE"
          ]
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom"
    }
  ]
}
```

**Replace these values:**
- `YOUR_GENERATED_UUID_HERE` → UUID from Step 3
- `YOUR_GENERATED_PRIVATE_KEY_HERE` → Private key from Step 3  
- `YOUR_GENERATED_SHORT_ID_HERE` → Short ID from Step 3

## Step 5: Start Xray Service

### Enable and Start Service
```bash
systemctl enable xray
systemctl start xray
```

### Check Service Status
```bash
systemctl status xray
```
Should show "active (running)" in green.

### Check Logs
```bash
journalctl -u xray -f
```
Should show no errors. Press Ctrl+C to exit.

## Step 6: Test Server Connection

### Test Local Connection
```bash
ss -tlnp | grep :443
```
Should show Xray listening on port 443.

### Test External Connection
```bash
curl -I https://YOUR_DOMAIN.com
```
Should show connection attempt (may timeout - that's normal).

## Step 7: Create Client Configuration

Create this configuration for your clients:

```json
{
  "inbounds": [
    {
      "port": 1080,
      "protocol": "socks",
      "settings": {
        "udp": true
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "vless",
      "settings": {
        "vnext": [
          {
            "address": "YOUR_DOMAIN.com",
            "port": 443,
            "users": [
              {
                "id": "YOUR_GENERATED_UUID_HERE",
                "encryption": "none"
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "tcp",
        "security": "reality",
        "realitySettings": {
          "fingerprint": "chrome",
          "serverName": "www.microsoft.com",
          "shortId": "YOUR_GENERATED_SHORT_ID_HERE",
          "publicKey": "YOUR_GENERATED_PUBLIC_KEY_HERE"
        }
      }
    }
  ]
}
```

**Replace these values:**
- `YOUR_DOMAIN.com` → Your actual domain name
- `YOUR_GENERATED_UUID_HERE` → Same UUID from server config
- `YOUR_GENERATED_SHORT_ID_HERE` → Same short ID from server config
- `YOUR_GENERATED_PUBLIC_KEY_HERE` → Public key from Step 3

## Step 8: Mobile App Setup

### For Android (v2rayNG):
1. Install v2rayNG from Google Play
2. Tap "+" → "Import config from clipboard"
3. Paste the client configuration above
4. Tap the config to connect

### For iOS (Shadowrocket):
1. Install Shadowrocket from App Store
2. Tap "+" → "Type" → "VLESS"
3. Fill in the details from your client configuration
4. Tap save and connect

### For Windows (v2rayN):
1. Download v2rayN from GitHub releases
2. Right-click → "Import bulk URL from clipboard"
3. Paste your client configuration
4. Double-click to connect

## Troubleshooting

### Service Won't Start
```bash
# Check configuration syntax
xray run -test -config /usr/local/etc/xray/config.json

# Check logs for errors
journalctl -u xray --no-pager
```

### Can't Connect from Client
```bash
# Check if port 443 is open
netstat -tlnp | grep :443

# Check firewall
ufw status

# Test domain resolution
nslookup YOUR_DOMAIN.com
```

### Still Having Issues?
1. Verify all UUIDs and keys match between server and client
2. Ensure domain is properly pointed to server IP
3. Check server time is synchronized
4. Try restarting Xray service: `systemctl restart xray`

## Security Reminders

⚠️ **This setup provides protocol obfuscation, not complete anonymity**

- Traffic metadata is still visible to sophisticated adversaries
- Use dedicated infrastructure for sensitive activities
- Regularly update Xray to latest version
- Monitor server logs for unusual activity

## Next Steps

- Set up monitoring with `htop` or similar tools
- Configure log rotation to prevent disk filling
- Consider setting up automated backups of your configuration
- Test connection speed and optimize if needed

---

**Setup complete!** Your VLESS+REALITY server should now be running and accessible to clients.