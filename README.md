
# Jitsi Stack Self-Hosted

This project provides a self-hosted deployment of Jitsi Meet using Docker Compose, with NGINX as a reverse proxy to handle HTTPS and WebSocket connections.

---

## 📦 Stack Overview

The following components are deployed as part of this stack:

- **NGINX**: Serves as a reverse proxy and SSL terminator.
- **Jitsi Web**: The web frontend of Jitsi Meet.
- **Prosody**: The XMPP server responsible for user signaling.
- **Jicofo**: Jitsi Conference Focus component that manages conference sessions.
- **JVB (Jitsi VideoBridge)**: Handles video/audio streams between clients.

---

## 🧩 Why Use NGINX?

NGINX acts as a secure entry point to the platform:
- Manages HTTPS termination via SSL certificates.
- Proxies HTTP and WebSocket traffic to backend services (`web` and `prosody`).
- Enhances security and allows you to expose only port 443 to the internet.

---

## 🌐 Traffic Flow

1. Incoming user traffic reaches the **NGINX** server on port `443`.
2. NGINX proxies:
   - Root `/` requests to the **web** container (Jitsi frontend).
   - `/xmpp-websocket` requests to **prosody** for XMPP communication.
3. The **web** frontend connects to the XMPP server (`prosody`) and orchestrates conference creation through **Jicofo**.
4. **JVB** is used to handle real-time audio/video streams between participants via UDP port `10000`.

---

## 📁 Project Structure

```
.
├── docker-compose.yml        # Docker Compose file defining all services
├── nginx/
│   ├── jitsi.conf            # NGINX configuration
│   └── certs/                # Place your SSL certificates here
└── README.md                 # You're reading it!
```

---

## ⚙️ How to Deploy

1. Clone the repository:
   ```bash
   git clone https://github.com/YOUR_USERNAME/jitsi-stack-selfhosted.git
   cd jitsi-stack-selfhosted
   ```

2. Place your SSL certificates in the `nginx/certs` directory:
   - `fullchain.pem`
   - `private.key`

3. Modify `nginx/jitsi.conf` to replace `meet.example.com` with your domain name.

4. Launch the stack:
   ```bash
   docker-compose up -d
   ```

5. Open your browser and navigate to `https://your-domain.com`.

---

## 🔐 Ports to Open (Firewall Rules)

If deploying behind a firewall (like FortiGate), you need to allow:

- `TCP 443`: HTTPS traffic (for NGINX).
- `UDP 10000`: Media traffic (for JVB).
- Internal Docker networking handles the rest.

---

## 📌 Notes

- **Authentication** is disabled (`ENABLE_AUTH=0`). To enable secure meetings, authentication can be configured in the environment variables.
- Do **not** use real internal hostnames or production certificates when publishing this project publicly.

---

## 📛 Naming

This project is called `jitsi-stack-selfhosted` to reflect that it’s:
- A full self-contained deployment stack.
- Suitable for on-prem or private cloud installations.
