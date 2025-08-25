# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Architecture

This is a Docker-based homelab infrastructure repository containing multiple self-hosted application stacks. Each service is defined in its own directory under `stacks/` with a dedicated `docker-compose.yml` file.

### Structure
- `stacks/` - Contains individual service directories, each with a `docker-compose.yml`
- `stacks/stack.env` - Shared environment variables used across services
- Each stack follows a consistent pattern for volumes, networks, and configuration

### Portainer
Some of the stacks are synchronized automatically to Portainer.

## Environment Configuration

The repository uses a centralized environment file at `stacks/stack.env` that defines:
- User/Group IDs (PUID=1000, PGID=1000)
- Timezone (Europe/Zurich)
- Storage paths for SSD (`/srv/dim-server`) and HDD (`/mnt/HDD_8TB/server`)
- Application data, cache, downloads, and media directories

## Storage Architecture

- **SSD Storage** (`/srv/dim-server`): Application data, cache, transcoding, incomplete downloads
- **HDD Storage** (`/mnt/HDD_8TB/server`): Complete downloads, media library, backups, configs

## Common Operations

### Managing Stacks
```bash
# Start a specific service stack
cd stacks/<service-name>
docker compose up -d

# View logs for a service
docker compose logs -f <service-name>

# Stop and remove a service
docker compose down

# Update a service to latest image
docker compose pull && docker compose up -d
```

### Working with the Environment
- Some services override specific environment variables in their compose files
- Path mappings follow the storage architecture with SSD for app data and HDD for media/downloads

## Development Notes

- All services use `restart: unless-stopped` for automatic recovery
- Most services use LinuxServer.io images (lscr.io/linuxserver/) for consistent behavior
- Services that need hardware access (like Jellyfin) may have additional device mappings
- Network configuration is primarily host-mode or bridge with specific port mappings