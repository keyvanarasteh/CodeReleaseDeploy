# React (Node/Express) Build Scripts

## Overview
This document covers deploying a React frontend alongside a custom Node.js/Express backend (SSR or API co-location).

## Build Commands
```bash
# Build frontend
cd client && bun run build

# Build/Transpile backend
cd server && bun run build

# Start combined production server
NODE_ENV=production node server/dist/index.js
```

## Deployment Targets
- Docker / Kubernetes
- VPS with PM2/Systemd
