# Unraid Templates

Docker container templates for [Unraid Community Applications](https://forums.unraid.net/topic/38582-plug-in-community-applications/).

## Available Templates

### Shelly Manager

Local management platform for Shelly IoT devices without cloud connectivity.

**Features:**
- Device discovery (mDNS and network scanning)
- Firmware management and updates
- Bulk configuration operations
- Modern web UI

**Installation:**
1. In Unraid, go to **Apps** > **Settings** > **Docker Repositories**
2. Add: `https://github.com/jfmlima/unraid-templates`
3. Search for "Shelly Manager" in the Apps tab
4. Click Install and configure settings

**Network Discovery:**

For mDNS device discovery to work properly, you may need to use **host** network mode instead of bridge. Edit the container after installation if devices are not discovered automatically.

**Configuration:**
| Variable | Description | Default |
|----------|-------------|---------|
| `SHELLY_SECRET_KEY` | Encryption key for stored credentials and backups. Generate one with `openssl rand -base64 32 \| tr '+/' '-_'` and keep it, since changing it makes what is already stored unreadable | (required) |
| `LOG_LEVEL` | Logging verbosity | `info` |
| `SHELLY_FIRMWARE_ADVERTISED_BASE_URL` | Address your devices can reach this container on, e.g. `http://192.168.1.50:8080`. Only needed to update a device that has no internet access, where the manager downloads the firmware once and the device fetches it from here | (none) |
| `PUID` | User ID for file permissions | `1000` |
| `PGID` | Group ID for file permissions | `1000` |

## Links

- [Shelly Manager Project](https://github.com/jfmlima/shelly-manager)
- [Issues & Support](https://github.com/jfmlima/shelly-manager/issues)

## License

MIT License - see individual template directories for details.
