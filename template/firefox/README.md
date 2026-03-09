# ClawCloud Run Template for Firefox

## Overview

This template provides a Kubernetes-based deployment solution for [Mozilla Firefox](https://www.mozilla.org/firefox/) on the ClawCloud Run platform. The template creates a containerized Firefox instance with both web (noVNC) and VNC access, making it easy to deploy a browser environment with persistent storage.

## Features

- **Browser Access**: Modern web interface via noVNC (port 5800)
- **VNC Access**: Direct VNC connection (port 5900)
- **Persistent Storage**: Configurable local storage (default 1Gi)
- **Time Zone Support**: Customizable timezone settings
- **Display Configuration**: Adjustable window size (1280x720 default)
- **Security**: VNC password protection
- **Font Support**: CJK font support for Asian languages
- **Resource Limits**: Configurable CPU and memory limits

## Configuration Parameters

### Environment Variables

| Variable | Description | Default Value |
|----------|-------------|---------------|
| `TZ` | Timezone setting | Asia/Tokyo |
| `DISPLAY_WIDTH` | Browser window width | 1280 |
| `DISPLAY_HEIGHT` | Browser window height | 720 |
| `KEEP_APP_RUNNING` | Keep application running if crashes | 1 |
| `ENABLE_CJK_FONT` | Enable CJK font support | 1 |
| `VNC_PASSWORD` | VNC access password | admin |

### Resource Limits

| Resource | Request | Limit |
|----------|---------|-------|
| CPU | 100m | 1000m |
| Memory | 1024Mi | 1024Mi |

### Storage

| Volume | Size | Path | Access Mode |
|--------|------|------|-------------|
| config-volume | 1Gi (configurable) | /config | ReadWriteOnce |

## Access Methods

### Web Access (noVNC)
- **URL**: `http://${{ defaults.app_host }}.${{ CLAWCLOUD_CLOUD_DOMAIN }}:5800`
- **Port**: 5800
- **Protocol**: HTTP (noVNC)

### Direct VNC Access
- **Host**: `${{ defaults.app_host }}.${{ CLAWCLOUD_CLOUD_DOMAIN }}`
- **Port**: 5900
- **Protocol**: VNC
- **Password**: Configured via `VNC_PASSWORD` environment variable (default: "admin")

## Deployment Notes

1. **Persistent Storage**: The template includes a persistent volume claim for storing browser configuration and data. The default size is 1Gi but can be modified through the `volume_size` input parameter.

2. **Security**: 
   - VNC access is password protected (default: "admin")
   - Web access uses the platform's SSL certificate
   - Consider changing the default VNC password in production environments

3. **Scaling**: The deployment is configured as a StatefulSet with a single replica (min/max 1). For browser applications, multiple replicas may cause session issues.

4. **Resource Allocation**: The template allocates 1 CPU core and 1GB of memory by default, which should be sufficient for typical browsing workloads.

## Support and Troubleshooting

For assistance with this template or the Firefox deployment:

1. Check the [original Docker Firefox documentation](https://github.com/jlesage/docker-firefox/blob/master/README.md) for container-specific details
2. Consult the [ClawCloud Run documentation](https://docs.run.claw.cloud) for platform-specific guidance
