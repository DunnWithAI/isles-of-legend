# Isles of Legend

A block-building adventure game that runs in the browser. Explore eleven islands, help each island's villagers, earn the blessings of six gods at the temple, and face the Hollow King on Shadow Island.

The whole game is one file, `index.html`. It needs no build step and has no dependencies to install.

## Run it locally

Open `index.html` in a browser, or serve the folder:

```sh
npx serve .
# or
python3 -m http.server 8000
```

## Hosting (e.g. on the Arcade site)

Copy `index.html` (and `thumb.jpg` if you want the preview image) into any static host, such as a subfolder of the Arcade site on Cloudflare Pages. It loads two things from the internet at runtime:

- three.js r128 from `cdnjs.cloudflare.com`
- the Pixelify Sans font from Google Fonts

If the Arcade site has a strict Content Security Policy, allow those two origins. Alternatively, vendor three.js locally and update the `<script src>`.

## What works outside Claude

The game was built as a claude.ai artifact. A few features use capabilities that only exist inside Claude (`window.claude.use(...)`):

| Feature | Inside Claude | Static hosting |
|---|---|---|
| Single-player, Creative and Survival, quests, saving | ✅ | ✅ (saved in the browser's localStorage) |
| **Servers** button (daily world "islesoflegend.com") | ✅ shared with everyone, builds saved for the day | ⚠️ loads the day's world, but solo and builds aren't shared |
| Invite friends by email, chat, seeing other players | ✅ | ❌ shows a message instead |
| Install as app | ✅ (Claude desktop app) | ❌ hidden |

The code handles all of this gracefully: when `claude.use(...)` is unavailable, those features switch off without errors.

### Adding real multiplayer later

The multiplayer code is in the `MP` and `OFF` sections of `index.html`. It needs:

- **A realtime room**: presence (player positions at about 10 Hz) and broadcast messages (`blk` for block edits, `chat`). A Cloudflare Durable Object with WebSockets is a natural fit.
- **A small key-value store** for the daily server's builds. These are documents keyed `world/<YYYY-MM-DD>_<chunkIndex>` holding `{d, c, e: [[blockIndex, blockId], ...]}`. D1 or KV works.
- **Player names**, if you want name tags.

Swap the `claude.use('room')` and `claude.use('db')` calls for a thin client over those, and everything else stays the same.

## Controls

WASD to move, Shift or double-tap W to sprint, Space to jump, F to fly (Creative), E for all blocks, B or right-click to place, Q or left-click to break, M for the map, J for quests, O for outfits, V for third-person view, H to go home, P for servers and friends, T to chat, N to toggle sound, and Esc to pause. Touch controls appear on phones and tablets.
