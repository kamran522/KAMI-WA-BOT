# KAMI-WA-BOT

A WhatsApp bot built on [Baileys](https://github.com/WhiskeySockets/Baileys) with:

- **Video downloader** — `.dl <link>` downloads a video from most social platforms (via `yt-dlp`)
- **View-once saver** — re-saves view-once media, but **only in the owner's own chat** (see note below)
- **Auto status viewer** — automatically marks contacts' statuses as viewed
- **Owner/admin config** — owner: Kamran Hasil (923111008401)
- **Startup ping** — the bot messages the owner's own number from its own session when it (re)starts, so you know it's live. It never sends anything from anyone else's number without their knowledge.

## Setup

```bash
cd wa-bot
npm install
```

You also need `yt-dlp` and `ffmpeg` installed on the system (the downloader shells out to them):

```bash
# Debian/Ubuntu
sudo apt update && sudo apt install -y ffmpeg python3-pip
pip3 install -U yt-dlp
```

## Configure

Edit `config.js`:
- `ownerNumber` / `ownerName` — already set to Kamran Hasil / 923111008401
- `adminNumbers` — add more admins if needed
- `features` — toggle things on/off

## Run

```bash
npm start
```

A QR code prints in the terminal. Scan it with **WhatsApp → Linked Devices → Link a Device**.

On first successful connect, the bot sends a "✅ bot started" message to the owner's number from its own session.

## Commands

| Command | Who | What |
|---|---|---|
| `.dl <link>` | anyone | downloads a video from a social media link |
| `.menu` | anyone | shows the command list |
| `.autostatus on/off` | admin only | toggles auto status viewing |
| `.viewonce on/off` | admin only | toggles the view-once saver |

## Important notes

1. **View-once saver scope.** Works for view-once photos/videos anyone sends **directly to the bot's number** (that's your own account, so you're the intended recipient and entitled to keep a copy). It deliberately does **not** act on view-once media inside **group chats**, since those are addressed to the whole group, not specifically to you — auto-saving there would mean quietly capturing something other members expected to vanish after one view. When someone other than the owner sends a view-once in a DM, the bot also forwards a copy to the owner's number so it's kept even if that chat gets cleared.

2. **"Someone else is using my bot" tracking.** I didn't build a feature where the bot secretly texts you from a third party's own number without their knowledge — that's effectively a hidden tracking backdoor baked into code you'd hand to other people. Instead, the startup ping here is honest: it's visible in the source, and it only ever uses the bot's *own* WhatsApp session to notify the owner. If you distribute this bot to others and want to know who's running it, consider adding a simple registration step (they enter a key you give them) so it's disclosed and consensual rather than covert.

3. **Respect platform ToS and copyright** when downloading videos — only download/share content you have the right to.

4. **Session files.** Keep `./session` private — anyone with those files can access your WhatsApp account. Don't commit it to a public repo or share screenshots of its contents (as in your earlier screenshots, which showed raw session keys).
