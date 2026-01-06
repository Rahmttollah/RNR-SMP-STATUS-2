# RnR SMP Minecraft Server Dashboard

## Overview

This is a Minecraft server monitoring and management system for the RnR SMP community. It provides real-time server status monitoring, a Discord bot for notifications and admin controls, and a web dashboard for players to check server status.

The system monitors an Aternos-hosted Minecraft server, tracks player counts, server latency, MOTD changes, and queue status. It sends Discord alerts when the server goes offline and provides interactive commands for community management.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Core Components

**Express Backend (index.js, routes.js)**
- Lightweight Express server serving both API endpoints and static files
- Single `/api/status` endpoint returns current server state
- Static files served from `/public` directory for the web dashboard

**Minecraft Monitor (minecraft.js)**
- Uses `minecraft-server-util` library to poll server status every 5 seconds
- Detects online/offline/queue states based on server response and MOTD content
- Implements failure threshold (5 consecutive failures) before marking server offline to handle brief instability

**Status Manager (statusManager.js)**
- Central state management for all server metrics (status, players, latency, MOTD)
- Persists critical timestamps and queue state to JSON file for restart recovery
- Tracks last online/offline times, crash events, and alert counts

**State Persistence (statePersistence.js, persistent_state.json)**
- File-based JSON persistence for state that needs to survive restarts
- Stores timestamps, queue information, and alert tracking data

**Discord Bot (discord.js)**
- Built with discord.js v14 for slash commands and interactive buttons
- Sends offline alerts with admin pings
- Provides server status embeds with real-time information
- Implements rate-limit safe batched alerting system

**Settings (settings.js)**
- Centralized configuration for all adjustable parameters
- Includes server IP/port, admin IDs, alert intervals, and URLs
- No hard-coded values in other files - all read from settings

### Design Decisions

**Polling vs Webhooks**: Uses polling (every 5 seconds) because Aternos servers don't support outbound webhooks. The interval balances responsiveness with resource usage.

**Failure Threshold**: Server marked offline only after 5 consecutive failures (~25 seconds) to prevent false alerts during brief network hiccups.

**File-based State**: Uses JSON file instead of database for state persistence. Simple and sufficient for single-server monitoring with low data volume.

**Monolithic Structure**: All components in root directory rather than separated folders. Appropriate for the project's complexity level and makes deployment straightforward.

## External Dependencies

**Discord Bot**
- Requires `DISCORD_TOKEN` environment variable
- Uses `DISCORD_CHANNEL_ID` for status messages
- discord.js v14 for API interactions

**Minecraft Server**
- Monitors `ncr-1.candynodes.xyz:25571` (configurable in settings.js)
- Uses `minecraft-server-util` for Java Edition server status protocol

**Web Frontend**
- Tailwind CSS loaded from CDN
- Lucide icons from unpkg CDN
- No build step required - static HTML/CSS/JS

**Environment Variables**
- `DISCORD_TOKEN` - Discord bot authentication
- `DISCORD_CHANNEL_ID` - Target channel for bot messages
- `PORT` - HTTP server port (defaults to 5000)