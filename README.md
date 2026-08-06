# NEXUS

One-command VLESS proxy server with TLS.
Supports VLESS+XHTTP by default, with optional VLESS+WS.

## Requirements

- Debian/Ubuntu/RHEL
- Public IPv4 on server (domain is optional)
- `curl`, `openssl` available
- Root access

## Usage

```bash
curl -fsSL https://raw.githubusercontent.com/vayzur/nexus/main/install.sh | bash
```

Outputs a QR code and `vless://` URI ready to import into your client.

## Configuration

All variables are optional.

| Variable | Required | Default |
|---|---|---|
| `NEXUS_HOST` | **NO** | server primary IPv4 |
| `NEXUS_XHTTP` | **NO** | `1` (enabled) |
| `NEXUS_WS` | **NO** | `0` (disabled) |
| `NEXUS_XHTTP_PATH` | **NO** | auto-generated |
| `NEXUS_UUID` | **NO** | auto-generated |
| `NEXUS_UUID_XHTTP` | **NO** | `NEXUS_UUID` |
| `NEXUS_UUID_WS` | **NO** | `NEXUS_UUID` |
| `NEXUS_WS_PATH` | **NO** | auto-generated |
| `NEXUS_STAGING` | **NO** | — |
| `NEXUS_FORCE` | **NO** | — |

`NEXUS_HOST` accepts either a domain or an IP.

- If `NEXUS_HOST` is an IP (or unset and auto-detected as IP), the script requests an IP certificate via acme.sh using Let's Encrypt short-lived profile.
- If `NEXUS_HOST` is a domain, the script requests a normal domain certificate.
- `NEXUS_XHTTP=1` enables XHTTP transport (default).
- `NEXUS_WS=1` enables WebSocket transport.
- You can run both transports at the same time.
- UUID precedence:
- XHTTP uses `NEXUS_UUID_XHTTP`, then `NEXUS_UUID`, then auto-generated UUID.
- WS uses `NEXUS_UUID_WS`, then `NEXUS_UUID`, then auto-generated UUID.

## Examples

```bash
# production (auto primary IP cert)
curl -fsSL https://raw.githubusercontent.com/vayzur/nexus/main/install.sh | bash

# production (domain cert)
export NEXUS_HOST=sub.example.com
curl -fsSL https://raw.githubusercontent.com/vayzur/nexus/main/install.sh | bash

# xhttp + ws together
export NEXUS_XHTTP=1
export NEXUS_WS=1
curl -fsSL https://raw.githubusercontent.com/vayzur/nexus/main/install.sh | bash

# ws only
export NEXUS_XHTTP=0
export NEXUS_WS=1
curl -fsSL https://raw.githubusercontent.com/vayzur/nexus/main/install.sh | bash

# testing (staging cert)
export NEXUS_HOST=203.0.113.10
export NEXUS_STAGING=1
curl -fsSL https://raw.githubusercontent.com/vayzur/nexus/main/install.sh | bash

# force re-issue certificate
export NEXUS_HOST=203.0.113.10
export NEXUS_FORCE=1
curl -fsSL https://raw.githubusercontent.com/vayzur/nexus/main/install.sh | bash
```

## Client

Import the output `vless://` URI into any xray-based client (v2rayN, v2rayNG, Hiddify).

Set `alpn` to `h2,http/1.1` if your client doesn't parse it from the URI.
