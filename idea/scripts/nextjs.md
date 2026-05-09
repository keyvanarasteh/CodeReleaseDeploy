# Next.js Build & Release Scripts

## Overview
This document covers the build and deployment processes for Next.js applications (both standalone server output and static exports).

## Build Commands

### Standard Node Server Build
Produces a `.next` folder and `standalone` output for efficient Docker deployment.
```bash
# Ensure next.config.js has output: 'standalone'
bun run build

# Run the standalone server
node .next/standalone/server.js
```

### Static Export Build
Produces an `out/` folder of purely static HTML/CSS/JS (requires `output: 'export'` in config).
```bash
bun run build
```

## Deployment Targets
- **Vercel**: Native zero-config deployment.
- **Docker**: Using the `standalone` output target.
- **Static Hosting**: Using the `out` export directory.
