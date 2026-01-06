# Render Deployment Guide for RNR-SMP Dashboard

To deploy this project on Render:

1.  **Create a New Web Service** on Render.
2.  **Connect your Repository**.
3.  **Configure Environment**:
    *   **Runtime**: `Node`
    *   **Build Command**: `npm install`
    *   **Start Command**: `node index.js`
4.  **Add Environment Variables**:
    *   `DISCORD_TOKEN`: Your Discord bot token.
    *   `DISCORD_CHANNEL_ID`: The ID of the channel for status updates.
    *   `PORT`: `5000` (Render usually detects this automatically).

The project is already set up to use file-based persistence, so no database is required.
