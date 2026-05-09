# SvelteKit (Static Adapter) Build Scripts

## Overview
This document covers generating a fully static Single Page Application (SPA) or Static Site Generation (SSG) output using `@sveltejs/adapter-static`.

## Setup & Configuration
Ensure `svelte.config.js` uses `adapter-static` and `fallback: 'index.html'` if doing SPA routing.

## Build Commands
```bash
# Install dependencies
bun install

# Generate static build in the /build directory
bun run build
```

## Deployment Targets
- GitHub Pages
- Cloudflare Pages
- Vercel / Netlify
- Nginx / Apache static hosting
