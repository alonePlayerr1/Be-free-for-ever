# Manual Setup Guide

**Prefer AI assistance?** → Use this guide to avoid long back-and-forth conversation.

This guide is for users who want to set up VLESS+REALITY manually with AI help.

## Step 0: Copy and paste into AI chat

### AI Assistant Prompt for VLESS+REALITY Setup

**Ready-to-Use Prompt (Copy and Paste to Any AI)**

```
Act as an expert Linux system administrator and networking specialist. I need step-by-step guidance to set up a VLESS+REALITY server for bypassing internet censorship. This is for educational and legitimate privacy purposes only.

IMPORTANT CONTEXT:
- I am a beginner and need very detailed, exact commands
- I want to set up VLESS with REALITY protocol on my VPS
- I have basic Linux command line access
- Walk me through each step and explain what each command does
- Ask me to confirm each step before moving to the next
- If something goes wrong, help me troubleshoot

MY SETUP:
- Operating System: [I will tell you: Ubuntu/Debian/CentOS etc.]
- VPS Provider: [I will tell you: DigitalOcean/Vultr/Linode etc.]
- Domain: [I will tell you my domain name]
- Server IP: [I will tell you my server IP]

WHAT I NEED YOU TO DO:
1. Guide me through server preparation and security
2. Help me install Xray-core
3. Generate all necessary keys and IDs for me
4. Create the server configuration file
5. Start and test the service
6. Generate client configuration
7. Provide troubleshooting steps

REQUIREMENTS TO INCLUDE:
- Use VLESS protocol with REALITY security
- Masquerade as microsoft.com traffic
- Use port 443
- Provide both server and client configurations
- Include all commands I need to copy-paste
- Generate secure UUIDs and keys for me
- Test the setup works properly

CONFIGURATION TEMPLATE TO CUSTOMIZE:
```json
{
  "log": {"loglevel": "warning"},
  "inbounds": [{
    "port": 443,
    "protocol": "vless",
    "settings": {
      "clients": [{"id": "GENERATED_UUID"}],
      "decryption": "none"
    },
    "streamSettings": {
      "network": "tcp",
      "security": "reality",
      "realitySettings": {
        "show": false,
        "dest": "one.one.one.one:443",
        "serverNames": ["www.microsoft.com"],
        "privateKey": "GENERATED_PRIVATE_KEY",
        "shortIds": ["GENERATED_SHORT_ID"]
      }
    }
  }],
  "outbounds": [{"protocol": "freedom"}]
}
```

Start by asking me about my server details (OS, IP, domain) and then guide me through each step one by one. Make sure I understand what each command does before we proceed.

Remember:
- Give me exact commands to copy-paste
- Explain potential issues I might encounter
- Help me verify each step works before continuing
- Provide the complete client configuration at the end
- Include instructions for mobile apps (v2rayNG, etc.)

Begin by asking me for my server specifications and then start with step 1.


---

## Usage Instructions

### How to Use This Prompt:

1. **Copy the entire prompt above** (everything between the triple backticks)

2. **Paste it into any AI assistant:**
   - ChatGPT
   - Claude
   - Gemini/Bard
   - Bing Chat
   - Any other AI assistant

3. **The AI will ask you for your server details** - provide:
   - Your operating system (Ubuntu 20.04, Debian 11, etc.)
   - Your VPS IP address
   - Your domain name
   - VPS provider if relevant

4. **Follow the step-by-step instructions** the AI provides

5. **Don't skip steps** - confirm each one works before proceeding

### What This Prompt Will Do:

✅ **Complete server setup** from scratch  
✅ **Generate all security keys** automatically  
✅ **Create working configurations** for both server and client  
✅ **Test the connection** to ensure it works  
✅ **Provide mobile app setup** instructions  
✅ **Include troubleshooting** for common issues  
✅ **Explain each step** so you understand what's happening  

### Prerequisites You Need:

- **VPS server** (1GB RAM minimum) with root access
- **Domain name** pointed to your server IP
- **SSH access** to your server
- **Basic terminal/command line** access

### Security Notice:

⚠️ **This setup provides protocol obfuscation, not complete anonymity**  
⚠️ **Use responsibly and comply with local laws**  
⚠️ **For legitimate privacy and educational purposes only**  

### Mobile App Recommendations:

- **Android**: v2rayNG
- **iOS**: Shadowrocket, Quantumult X
- **Windows**: v2rayN
- **macOS**: V2rayU

---

## Advanced Version (For Technical Users)

If you're more technical and want additional features, use this enhanced prompt🤷‍♂️:

```
Act as a senior DevOps engineer specializing in network security and circumvention technologies. I need comprehensive guidance for deploying a production-ready VLESS+REALITY server with the following advanced requirements:

ADVANCED FEATURES NEEDED:
- Automated deployment script
- Systemd service configuration
- Log rotation and monitoring
- Firewall configuration (UFW/iptables)
- SSL certificate automation
- Multiple client support
- Performance optimization
- Automated updates
- Health check monitoring
- Backup configuration

SECURITY HARDENING:
- SSH key-only authentication
- Fail2ban configuration
- Non-standard SSH port
- Automatic security updates
- Resource monitoring
- Connection limits

OPERATIONAL REQUIREMENTS:
- Docker deployment option
- Reverse proxy setup (Nginx)
- Domain masquerading best practices
- Traffic analysis evasion
- Multiple protocol fallbacks
- Geographic distribution strategy

Provide infrastructure-as-code solutions where possible and include monitoring/alerting setup.

MY SETUP:
- Operating System: [specify]
- Infrastructure: [Cloud provider/bare metal]
- Scale: [number of expected users]
- Compliance requirements: [if any]

Begin with infrastructure assessment and security baseline establishment.
```

This advanced prompt is for users who want enterprise-grade deployment with monitoring, security hardening, and automation.