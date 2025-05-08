## 🔧 Simple Ansible Playbook to Set Up a WireGuard Tunnel from a Remote Host to Your Server

This repository includes:
- A basic Ansible playbook to configure a WireGuard VPN server on a remote Ubuntu host.
- Some optional cleanup of unnecessary Ubuntu defaults (like snapd, motd-news, etc.).

---

### 🚀 How to Use

1. **Generate WireGuard keys on your local machine**:
   ```bash
   wg genkey | tee server_private_key | wg pubkey > server_public_key
   ```
    Put the 2 generated files in the playbooks directory

2. **Update your `inventory.ini`** with the correct IP address, SSH user, and key path.

3. **Edit `wg0.conf.j2`** (the Jinja2 template) and insert the generated server public key in the appropriate place.

4. **Run the playbook**:
   ```bash
   ansible-playbook -i inventory.ini playbooks/master-server.yml
   ```

---

### 💻 WireGuard Client Example Configuration

You can use the included `setup-client.yml` playbook or manually install WireGuard on your client system and use a configuration like:

Tip: if you need to generate the keys for your client you can use:

```bash
wg genkey | tee client_private_key | wg pubkey > client_public_key
```

```ini
[Interface]
PrivateKey = <CLIENT_PRIVATE_KEY>
Address = 10.0.0.2/32

[Peer]
PublicKey = <SERVER_PUBLIC_KEY>
Endpoint = <SERVER_PUBLIC_IP>:51820
PersistentKeepalive = 25
AllowedIPs = 10.0.0.1
```

> Replace the placeholders with your actual key and server info.
