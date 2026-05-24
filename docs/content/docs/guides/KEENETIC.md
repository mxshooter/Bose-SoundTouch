---
title: "Keenetic Router Installation Guide"
---

# Keenetic Router Installation Guide

This guide explains how to install `soundtouch-service` on Keenetic routers. The automated installer handles all setup steps: downloading the correct binary, creating init scripts, and configuring auto-start.

## Supported Models

| Model | Architecture | CPU |
|-------|--------------|-----|
| Hopper (KN-3811), Titan (KN-3810), Hero DSL | ARM64 | MediaTek MT7981 |
| Ultra, Giga, Hero, 4G Ultra | ARMv7 | ARM Cortex-A |
| Extra, Omni, Lite, Start | MIPS / MIPS-LE | MediaTek MT7621 |

## Prerequisites

- Keenetic router with [Entware](https://help.keenetic.com/en/section/entware.html) installed
- SSH access to the router (root user)

## Quick Install

One command to install the latest release:

```bash
curl -fsSL https://raw.githubusercontent.com/gesellix/Bose-SoundTouch/main/scripts/keenetic/install.sh | \
  ROUTER_IP=192.168.1.1 bash
```

The installer will:

1. Detect your router's CPU architecture automatically
2. Download the correct binary from GitHub releases
3. Create the init script at `/opt/etc/init.d/S99soundtouch`
4. Configure auto-start on boot
5. Start the service and verify it's healthy

## Installation Options

```bash
# Install specific version
curl -fsSL ... | ROUTER_IP=192.168.1.1 VERSION=v0.94.0 bash

# Custom data directory
curl -fsSL ... | ROUTER_IP=192.168.1.1 DATA_DIR=/opt/data bash

# Custom port
curl -fsSL ... | ROUTER_IP=192.168.1.1 PORT=8080 bash
```

## Management

After installation, manage the service via SSH:

```bash
# Check status
ssh root@192.168.1.1 /opt/etc/init.d/S99soundtouch status

# Start / Stop / Restart
ssh root@192.168.1.1 /opt/etc/init.d/S99soundtouch start
ssh root@192.168.1.1 /opt/etc/init.d/S99soundtouch stop
ssh root@192.168.1.1 /opt/etc/init.d/S99soundtouch restart

# View logs (foreground mode)
ssh root@192.168.1.1 /tmp/soundtouch-service -v
```

## Updating

Re-run the installer to update:

```bash
curl -fsSL https://raw.githubusercontent.com/gesellix/Bose-SoundTouch/main/scripts/keenetic/install.sh | \
  ROUTER_IP=192.168.1.1 bash
```

Or specify a version:

```bash
curl -fsSL ... | ROUTER_IP=192.168.1.1 VERSION=v0.94.0 bash
```

## Manual Installation

If you prefer to install manually or need to build from source:

### 1. Build the Binary

```bash
# For ARM64: Hopper, Titan, Hero DSL
make build-keenetic-arm64

# For ARMv7: Ultra, Giga, Hero
make build-keenetic-arm

# For MIPS: Extra, Omni, Lite
make build-keenetic-mips
make build-keenetic-mipsle

# Build all variants
make build-keenetic
```

### 2. Copy to Router

```bash
scp build/<BINARY> root@192.168.1.1:/tmp/
ssh root@192.168.1.1 "chmod +x /tmp/<BINARY>"
```

### 3. Run the Service

```bash
ssh root@192.168.1.1
mkdir -p /tmp/data
cd /tmp && ./<BINARY>
```

## Troubleshooting

### Service not responding

Run in foreground to see errors:

```bash
ssh root@192.168.1.1 /tmp/soundtouch-service -v
```

### USB storage for persistent data

Mount a USB drive before starting the service:

```bash
ssh root@192.168.1.1
mount /dev/sda1 /tmp/data
```

### Device discovery not working

Ensure mDNS (UDP port 5353) is allowed in the firewall.

## See Also

- [Getting Started Guide](GETTING-STARTED.md)
- [Deployment Overview](DEPLOYMENT-OVERVIEW.md)
- [External Host Walkthrough](EXTERNAL-HOST-WALKTHROUGH.md)
