# Keenetic Installer

Automated installer for `soundtouch-service` on Keenetic routers.

## Usage

```bash
curl -fsSL https://raw.githubusercontent.com/gesellix/Bose-SoundTouch/main/scripts/keenetic/install.sh | \
  ROUTER_IP=192.168.1.1 bash
```

## Options

| Variable | Default | Description |
|----------|---------|-------------|
| `ROUTER_IP` | (required) | Router IP address |
| `VERSION` | `v0.93.1` | Release version |
| `ARCH` | (auto) | Architecture override |
| `DATA_DIR` | `/tmp/data` | Data directory |
| `PORT` | `8080` | Service port |

## Supported Models

- ARM64: Hopper, Titan, Hero DSL
- ARMv7: Ultra, Giga, Hero, 4G Ultra
- MIPS/MIPS-LE: Extra, Omni, Lite, Start
