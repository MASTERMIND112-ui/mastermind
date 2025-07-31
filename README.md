# ELIASV4 / MASTERMIND112UI

WhatsApp group moderation & utility bot built on Baileys-style architecture with warning system, auto-saving contacts, export, and command logging.

## Features

- ⚠️ **Warn system**: Reply to a user to warn them; auto-kicks when warning limit is reached.  
- 📇 **Auto-save contacts**: Every sender (and replied-to user) is recorded with metadata (first seen, last seen, seen count).  
- 📋 **Contacts inspection**: `.contacts list`, `.contacts <jid>` and `.contacts export` to view or export saved contacts to CSV.  
- 🧾 **Command logging**: All uses of key commands (`warn`, `contacts`, etc.) are recorded with timestamp and context.  
- 🔧 **Pluggable command framework**: `master(...)`-based modules for easy extension.  
- 🗂️ JSON-backed persistence (easy to replace with SQLite/Postgres later).

## Quick Start

### Prerequisites

- Node.js v18+  
- Git (optional, for cloning)  
- WhatsApp connection setup (e.g., Baileys) already integrated or expected to be wired into `onMessageReceived`

### Installation

```bash
git clone <your-repo-url> eliasv4
cd eliasv4

# Create folders if not present
mkdir -p commands lib logs exports

# Initialize if needed
npm init -y

# Install dependencies (adjust if your stack differs)
npm install @adiwajshing/baileys
Files & Structure (what’s included)
arduino
Copy
Edit
ELIASV4/
├── commands/
│   ├── warn.js           # warn/reset/kick logic
│   └── contacts.js       # list/export contact info
├── lib/
│   ├── warn.js           # warn persistence logic
│   ├── contacts.js       # contact persistence logic
│   └── commandLogger.js  # logs command usage
├── config.js             # configuration (warning limit, etc.)
├── index.js             # loader + auto-save contact stub
├── logs/                # generated command-log.json
├── exports/             # exported contacts CSVs
└── ... other code (WhatsApp connection, master framework)
Configuration
Edit or review config.js:

js
Copy
Edit
module.exports = {
  WARN_COUNT: 3, // number of warns before kick
};
Running the Bot
Add a start script to package.json if missing:

json
Copy
Edit
"scripts": {
  "start": "node index.js"
}
Then start:

bash
Copy
Edit
npm run start
Ensure your real WhatsApp message hookup calls or merges the onMessageReceived logic from index.js so:

Contacts auto-save on incoming messages/replies

Commands like .warn and .contacts are registered via the master(...) modules

Usage
Moderation
.warn (reply to a user): Issue a warning.

.warn reset (reply): Reset that user’s warning count.

When a user reaches the limit (default 3), they are automatically removed from the group.

Contacts
.contacts list: Show up to the first 50 saved contacts with metadata.

.contacts <jid>: Show detailed info for a specific contact.

.contacts export: Export all saved contacts to a timestamped CSV in exports/.

Logging
All uses of warn and contacts commands are logged in logs/command-log.json with:

Timestamp

User JID

Command name

Arguments

Context (e.g., group vs private)

Persistence
Data is stored in JSON files for simplicity:

lib/warn-db.json — warning counts per JID

lib/contacts-db.json — saved contact metadata

logs/command-log.json — history of command invocations

You can migrate these to a proper database later (SQLite, Postgres, etc.).

Extending
Add new commands by creating a file in commands/ that calls master({...}, async (...) => { ... }).

Swap out JSON persistence for a database by replacing logic in lib/warn.js / lib/contacts.js.

Add admin/role management, per-group settings, rate limiting, or AI integrations (ask style helpers).

Development Helpers
You can use the provided patch files to bootstrap features:

bash
Copy
Edit
git apply complete_eliasv4_additions.patch
git apply export_and_logging.patch
Make sure to create the required directories first:

bash
Copy
Edit
mkdir -p commands lib logs exports
Example Flow
Someone sends/replies to a message → contact is auto-saved.

Admin replies to a user with .warn → warning increments; upon reaching limit the user is kicked.

Use .contacts list to inspect saved people.

Use .contacts export to produce a CSV of all contacts.

Check logs/command-log.json to audit usage.

Deployment
Run on any VPS, server, or container with Node.js.

Use a process manager like pm2 or systemd to keep the bot alive.

(Optional) Containerize with Docker: wrap the project, mount persistence volumes for lib/ and logs/.

Troubleshooting
Commands not loaded: Ensure index.js dynamically requires commands/*.js.

No contacts saved: Verify the message handler actually calls upsertContact(...) with the incoming message.

Permission errors: Ensure the bot process can write to lib/, logs/, and exports/.

Missing modules: npm install needed dependencies or fix relative import paths.

Contributing
Fork the repo

Add/modify command modules or storage layers

Test locally

Submit pull requests with clear descriptions

License
Specify your license here (e.g., MIT, Apache 2.0) or “All rights reserved” if internal.

perl
Copy
Edit

If you tell me your preferred style (short README, badge-enhanced, including example invite instructions, license, etc.), I can tailor a second variant. Which direction do you want next?
::contentReference[oaicite:0]{index=0}








Sources

Ask ChatGPT
