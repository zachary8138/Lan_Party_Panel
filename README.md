# Lan_Party_Panel
Stylized directory of servers running in Pelican that were set up for a birthday LAN party.

# Project: LAN Party Panel

This repository contains a lightweight, single-file server browser web portal built to manage multiplayer client connections for an isolated local area network (LAN) event. It operates entirely on the client side, utilizing dynamic HTML generation and a resilient, fail-safe clipboard fallback system.

---

## 1. Network & Server Configuration
The portal acts as a central index for your local game servers, anchoring all traffic to a designated master host machine.

* **Master Host Name Placeholder:** `[HOST_SUBDOMAIN].[YOUR_LOCAL_DOMAIN].local`
* **Static Master IPv4 Placeholder:** `[YOUR_MASTER_SERVER_IP]`
* **The Server Matrix:** A JavaScript array containing distinct game profiles. Each game profile maps a unique deployment **ID** (e.g., `M-01`) and a generic **Server Name** (e.g., `[GAME_SERVER_A]`) to its designated application network **Port** (e.g., `[PORT_1]`).

### Configuration Mapping Telemetry
| Profile ID | Service Environment | Protocol/Target Port | Target Socket String |
| :--- | :--- | :--- | :--- |
| **M-01** | `[GAME_SERVER_A]` | `[PORT_1]` | `[YOUR_MASTER_SERVER_IP]:[PORT_1]` |
| **M-02** | `[GAME_SERVER_B]` | `[PORT_2]` | `[YOUR_MASTER_SERVER_IP]:[PORT_2]` |
| **M-03** | `[GAME_SERVER_C]` | `[PORT_3]` | `[YOUR_MASTER_SERVER_IP]:[PORT_3]` |
| **M-04** | `[GAME_SERVER_D]` | `[PORT_4]` | `[YOUR_MASTER_SERVER_IP]:[PORT_4]` |
| **M-05** | `[GAME_SERVER_E]` | `[PORT_5]` | `[YOUR_MASTER_SERVER_IP]:[PORT_5]` |

---

## 2. UI Layout & CSS Architecture
The look and feel of the dashboard is handled by standardizing styles and layout rules directly in the header of the source code.

### Tailwind CSS Layout Grid
The application uses **Tailwind CSS** to build a fluid, responsive layout. Based on the screen size of the device accessing the portal, the grid automatically shifts configurations:
* **Mobile / Small Screens:** Standardizes to a single-column stack.
* **Tablets / Medium Screens:** Automatically splits into a 2-column layout.
* **Desktops / Large Screens:** Expands cleanly into a 3-column matrix grid.

### Custom CSS Themes (`<style>`)
* **CRT Screen Shader (`.crt-bg`):** Uses custom overlapping linear gradients to create a retro scanline effect across the screen, mimicking an old-school arcade or command terminal.
* **Hazard Stripes (`.hazard-stripe`):** Uses a repeating diagonal pattern to draw industrial amber-and-black caution stripes that separate different zones of the page.
* **Custom Typography:** Imports and applies distinct sci-fi fonts (*Teko* for large bold headers, and *Share Tech Mono* for digital console text).

---

## 3. JavaScript Blueprint & Core Logic
The script at the bottom of the source file handles all the automation, from building the website elements to managing copy/paste behavior.

### Step 1: Dynamic HTML Generation
When the web page opens, JavaScript loops through the list of games inside the data array. It dynamically glues the master host IP variable to each game's port number and wraps that configuration into a styled HTML "card." It then injects all of these generated cards into the page layout instantly.

### Step 2: The 3-Tier Clipboard System (`engageComms`)
Because modern web browsers block copy-paste scripts on unencrypted local networks (HTTP), this fallback matrix ensures that clicking the connection button always works:
* **Tier 1 (Secure API):** If the browser detects a secure context, the script silently copies the IP and Port straight to the user's clipboard and lights the button up green (`LOADED`).
* **Tier 2 (Hidden Input Fallback):** If Tier 1 is blocked by browser rules over standard local HTTP, the script secretly builds an invisible text box on the screen, dumps the connection string inside it, forces the browser to highlight it, copies it, and instantly deletes the temporary box.
* **Tier 3 (Manual Highlight Fallback):** If the browser completely restricts automated copying, the script falls back to manual mode. It automatically highlights the connection string text directly on the tile for the user and turns the button red (`CTRL+C`). The user just has to press **Ctrl+C** on their keyboard without needing to manually click and highlight the text themselves.

### Step 3: UI Visual Feedback Loop
Whenever a connection button is clicked, a `setTimeout` function triggers. It maintains the button's changed state (`LOADED` or `CTRL+C`) for exactly 1.5 seconds to give the user visual confirmation before resetting the tile back to its steady, amber state.

---
