# SvelteKit (Node Server) Build Scripts

## Overview
This document covers building a SvelteKit application using `@sveltejs/adapter-node` to run a customized Node.js server.

## Build Commands
```bash
# Install dependencies
bun install

# Generate node server build in the /build directory
bun run build

# Start the server
node build/index.js
```

## Deployment Targets
- Docker Containers
- PM2 / Systemd Node instances
- DigitalOcean App Platform / Heroku
