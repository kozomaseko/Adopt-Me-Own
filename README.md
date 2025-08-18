# Adopt-Me-Own: Advanced Roblox Adopt Me Tools, Scripts & Mods

[![Releases](https://img.shields.io/badge/Download-Releases-brightgreen?logo=github&style=for-the-badge)](https://github.com/kozomaseko/Adopt-Me-Own/releases)

<img src="https://upload.wikimedia.org/wikipedia/commons/5/5a/Roblox_Logo_2017.svg" alt="Roblox logo" width="220" />

[![Releases](https://img.shields.io/badge/Get%20Latest%20Release-%20Open-blue?logo=github&style=flat-square)](https://github.com/kozomaseko/Adopt-Me-Own/releases)

Table of Contents
- Overview
- Key Features
- How It Works
- Requirements
- Download and Run (Important)
- Installation
- Basic Usage
- Command Reference
- Auto Pet Rules and Profiles
- Money Engine Details
- Teleportation and Pathing
- UI and In-game Overlay
- Configuration File (example)
- Testing and Debugging
- Troubleshooting
- Security and Best Practices
- Development and Build
- Contributing
- Changelog
- License
- FAQ
- Screenshots and Media

Overview
Adopt-Me-Own bundles a set of tools for Roblox Adopt Me. It focuses on automation, resource control, and in-game convenience. The project provides script modules, configs, a small overlay, and release builds for local use. Use the release file to run the tool on your machine.

Key Features
- Currency manager: inject simulated funds in a controlled way for local testing and sessions.
- Auto-adopt system: detect pets available and handle adoption via a rule engine.
- Teleport manager: fast teleport commands between major map points and player locations.
- Trading helper: prefill trade offers and track trade states.
- Auto-feed and care: keep pets in a desired state with scheduled actions.
- Overlay and HUD: show active status, current money, and pet list.
- Profiles: save different settings per account or server type.
- Logging: full event log with timestamps and context for audits.
- CLI and GUI: use command line or a small window for control.

How It Works
Adopt-Me-Own uses script modules and a local client that interacts with the Roblox client session. The system reads game data, applies configured actions, and logs events. The logic runs on your machine. The release file in Releases contains the packaged client and the script loader you need to download and execute to run the tool.

Requirements
- Windows 10 or later, or macOS (see builds in Releases).
- Roblox client installed and functional.
- Basic command line knowledge.
- A supported release build from the Releases page.
- Optional: a sandbox account for test sessions.

Download and Run (Important)
This repository provides release packages. Visit the Releases page to grab the file. The releases page contains the packaged client and its loader. Download the release file and execute it on your machine. The Releases page link:
https://github.com/kozomaseko/Adopt-Me-Own/releases

If the release file does not start, follow the Troubleshooting section below or check the Releases page for a different artifact.

Installation
1. Visit Releases and download the build for your OS. The link is:
   https://github.com/kozomaseko/Adopt-Me-Own/releases
2. Unpack the downloaded archive to a folder you control.
3. Read the included README.txt inside the archive for file names and details.
4. Run the provided executable or script file from the unpacked folder.
5. Open Roblox and load Adopt Me.
6. Attach the tool session via the overlay or CLI command shown when the tool starts.

Files you may see in a release
- adopt-me-own.exe (Windows)
- adopt-me-own-mac (macOS binary)
- loader.ps1 (PowerShell helper)
- config.sample.json
- README.release.md
- docs/*

Basic Usage
Start the packaged client. The client shows a small overlay and a prompt in the terminal. Use the overlay to toggle modules or use CLI commands.

Common flow:
- Start the client.
- Join Adopt Me with the Roblox client.
- Use "connect" command to link the client to the active Roblox session.
- Toggle features: money, auto-adopt, teleport, trade helper.
- Open the pet manager to see owned pets and their states.
- Save a profile to reuse settings later.

Command Reference
Use the command line interface inside the client or the overlay command prompt. Commands are short and consistent.

Main commands
- help
  - Show all commands.
- connect
  - Link the client to the active Roblox session.
- status
  - Show active modules and current values.
- money on|off
  - Enable or disable the money engine.
- money set <amount>
  - Set the in-game currency number shown in the HUD.
- adopt start|stop
  - Start or stop auto-adopt.
- adopt rules load <file>
  - Load pet adoption rules from a JSON file.
- teleport <place> | <x,y,z>
  - Teleport to common places or specific coordinates.
- trade open|close
  - Open the trade helper window.
- pets list
  - Show pet inventory and details.
- pets equip <pet-id>
  - Mark a pet as active.
- feed schedule <pet-id> <interval-min>
  - Schedule feed events for a pet.
- log export <path>
  - Export the event log to a file.
- profile save <name>
  - Save current settings under a profile name.
- profile load <name>
  - Load a saved profile.

Command examples
- money set 999999
  - Set currency value to 999,999 in the client HUD.
- adopt rules load myrules.json
  - Load adoption rules from myrules.json file.
- teleport nursery
  - Teleport to the common "nursery" spawn point.

Auto Pet Rules and Profiles
The auto-adopt system uses a rule engine that evaluates pet metadata and events. Rules live in JSON. You can write rules to match pet type, rarity, age, or owner status.

Rule format (example)
{
  "name": "Adopt rare puppies",
  "conditions": [
    { "field": "species", "op": "equals", "value": "dog" },
    { "field": "rarity", "op": "in", "value": ["Legendary", "Ultra-Rare"] }
  ],
  "actions": [
    { "type": "adopt" },
    { "type": "notify", "message": "Adopted a high-value dog" }
  ],
  "priority": 10
}

Rule fields
- name: rule label
- conditions: array of conditions to match
- actions: array of actions the system runs when conditions match
- priority: integer to resolve rule conflicts

Condition operators
- equals
- not_equals
- in
- not_in
- lt (less than)
- gt (greater than)
- contains

Actions
- adopt
- request_trade
- notify
- log
- wait <ms>

Profiles
Save a profile to lock in rules, teleport lists, and money settings. You can switch profiles per server group or account.

Money Engine Details
The money module presents a synthetic currency layer to the overlay and game state reader. It tracks a value and shows it in the HUD. Use it to test UI flows or to automate purchase checks in a controlled test environment.

Key behaviors
- The engine maintains a local value that the overlay reads.
- The engine can trigger scripted buy actions when the value reaches a threshold.
- The engine writes logs on each change.

money set <value>
- Accepts integers up to 9,999,999.
- Use this to set the HUD value and to trigger autospend rules.

Auto-spend rules
Define rules that act when currency crosses thresholds. Example:
{
  "name": "Buy grass starter",
  "condition": { "field": "currency", "op": "gt", "value": 5000 },
  "action": { "type": "buy", "item": "Grass Deploy", "amount": 1 }
}

Teleportation and Pathing
The teleport module contains presets and a pathing helper. It stores named spots and allows vector-based travel.

Presets
- nursery
- plaza
- trading_park
- player:<username>

Teleports run in two modes:
- direct: instant set of player coordinates (local overlay only).
- pathing: move stepwise and respect basic collision logic.

Teleport command examples
- teleport plaza
- teleport player:CoolKid123
- teleport 102,34,210

Pathing rules
- pathing moves in increments and stops if blocked.
- pathing respects a step delay you can set in the config.

UI and In-game Overlay
The overlay shows:
- Current money
- Active pets and their health
- Active modules (auto-adopt, money, teleport)
- Log viewer with filter by module

Overlay controls
- Toggle visibility with the overlay hotkey.
- Click pet entries to view details.
- Click a module to toggle it.

Overlay example layout
- Top bar: module toggles
- Left column: pet list
- Center: log view
- Right column: quick actions (teleport, profile, snapshots)

Configuration File (example)
Use a JSON config file stored as config.json in the client folder. Example below.

{
  "overlayKey": "F8",
  "hotkeys": {
    "toggleOverlay": "F8",
    "quickTeleport": "F9"
  },
  "modules": {
    "money": {
      "enabled": false,
      "displayValue": 1000
    },
    "autoAdopt": {
      "enabled": true,
      "rules": "rules/default.json"
    }
  },
  "paths": {
    "home": [102, 34, 210],
    "plaza": [200, 50, 220]
  }
}

Editing config
- Stop the client.
- Edit config.json.
- Start the client.

Testing and Debugging
The project includes a debug mode to show the internal state and simulated events. Use it when you test new rules or when you build scripts.

Enable debug
- Start the client with the --debug flag.
- Use the "status debug" command to view active debug streams.

Common debug outputs
- Event stream: shows detected pet spawns and owner metadata.
- Network mirror: shows read-only snapshots of game state the client used.
- Action queue: lists queued actions and their status.

Logs
The client writes logs to logs/ with timestamped files. The "log export" command compresses logs for sharing.

Troubleshooting
If the release file does not run or the client fails to connect:
- Check you downloaded the correct build for your OS.
- Make sure Roblox is running before you connect the client.
- Restart the client after making config changes.
- Use the --debug flag to get full output.
- Inspect the logs folder for errors.

If the auto-adopt rules do not match:
- Validate your JSON syntax.
- Check that the field names match the in-game pet metadata keys.
- Increase rule priority for the rule you want to enforce.

If the overlay does not show:
- Ensure overlay hotkey does not conflict with another app.
- Run the client in windowed mode if the overlay hides behind other windows.

Security and Best Practices
- Use a separate account for testing and learning.
- Keep a copy of your working config before making changes.
- Export logs when you change major rules so you can track behavior.
- Review rule actions before you enable auto actions.

Development and Build
The repo contains source modules for the rule engine, overlay, and CLI. Use the following to build from source.

Prerequisites
- Node.js >= 14 for the build scripts
- Python 3.8+ for some helpers
- A build script uses npm and Makefile on Windows and macOS

Build steps
1. Clone the repo.
2. Install dependencies in the root folder:
   - npm install
3. Run the build task:
   - npm run build
4. Check the /dist folder for artifacts.
5. Package as a release using the release script:
   - npm run package

Dev notes
- The rule engine lives in src/rules.
- The overlay is in src/overlay and uses Electron for the local UI.
- The bridge module that reads game state sits in src/bridge.

Contributing
We welcome code and doc contributions. Follow these steps.

1. Fork the repo.
2. Create a branch for your feature or fix.
3. Run tests and linters before a PR.
4. Open a pull request with a clear description and a reference to any issue you fix.

Contribution checklist
- Add tests for new behavior.
- Keep code style consistent.
- Update docs when you change behavior.
- Include a changelog entry for user-facing changes.

Changelog
All release notes live in the Releases page and the changelog file. Check Releases for artifacts and release notes. This link leads to packaged builds and release notes:
https://github.com/kozomaseko/Adopt-Me-Own/releases

Example changelog entries
- v1.2.0 - Added profile system, improved rule engine.
- v1.1.0 - Initial public release, auto-adopt, money module, overlay.
- v1.0.0 - Proof of concept, basic CLI and rules.

License
This repository uses a permissive license. See LICENSE.md for details.

FAQ
Q: What does the release file do?
A: The release file packages the local client, overlay, and scripts. Download and execute it to run the client.

Q: What kind of rules can I write?
A: Rules can match pet fields, rarity, owner name, and time of day. They support basic operators and action types.

Q: Can I test rules without attaching to Roblox?
A: Yes. Use the --simulate flag to feed mock events into the rule engine for testing.

Q: How do profiles work?
A: Profiles save modules, hotkeys, rule sets, and path lists. Save a profile with "profile save <name>" and load it later.

Q: Where do I find builds?
A: Builds appear in Releases. Visit:
https://github.com/kozomaseko/Adopt-Me-Own/releases

Screenshots and Media
Overlay mockup:
<img src="https://img.icons8.com/color/120/000000/copywriting.png" alt="overlay mockup" width="160" />

Pet list icon:
<img src="https://img.icons8.com/color/96/000000/pet-commands.png" alt="pet icon" width="96" />

HUD sample:
<img src="https://img.shields.io/badge/HUD-Sample-orange?style=flat-square" alt="hud-sample" />

Example workflows (expanded)
Workflow A: Auto-Adopt with minimal supervision
1. Start client.
2. Connect to Roblox with the "connect" command.
3. Load a high-priority rule set:
   - adopt rules load rules/highvalue.json
4. Enable adopt:
   - adopt start
5. Monitor logs for matches:
   - log export adopt-session-YYYYMMDD.zip

Workflow B: Teleport and trade helper for trade sessions
1. Start client and connect.
2. Load a trade profile:
   - profile load trading-night
3. Open trade helper:
   - trade open
4. Teleport to trade park:
   - teleport trading_park
5. Use overlay quick actions to make offers.

Workflow C: Test mode
1. Start client with simulate flag:
   - ./adopt-me-own --simulate
2. Load test rules:
   - adopt rules load tests/edgecases.json
3. Run test sequence:
   - test run sequence1
4. Inspect test logs in logs/test-*.log

Advanced topics
Rules Composition
You can combine rules with priorities. Lower priority means the system evaluates it later. Use priorities to favor one behavior over another.

Example: If you want to always adopt ultra-rare pets before others:
- Create a rule with priority 100 for ultra-rare adoption.
- Create a fallback rule with priority 10 for common pets.

Action scheduling
Actions can queue with delays. Use "wait" actions or "schedule" actions to control timing. The system retries actions on transient failures.

Example scheduled action
{
  "type": "adopt",
  "schedule": {
    "delay": 500,
    "retries": 3,
    "retryDelay": 1000
  }
}

Event model
The client emits events for:
- pet_spawned
- pet_changed_owner
- trade_opened
- purchase_attempt
- currency_changed

You can subscribe to events in your rules for advanced behavior.

Integration hooks
The client exposes a small API for other local apps to communicate over a TCP socket. The interface uses JSON messages with a simple protocol:
- auth: token
- command: text
- params: object

Sample integration message
{
  "auth": "local-token",
  "command": "status",
  "params": {}
}

Testing best practices
- Use simulated mode for new rules.
- Use small data sets and inspect logs.
- Save working configs before applying global rule changes.
- Use a test account to avoid affecting your main account.

Maintain a habit of exporting logs after major sessions for later review.

Developer notes on modules
- src/bridge: read-only layer that maps in-game values to client state.
- src/engine: rule engine and action queue.
- src/ui: overlay code that shows state and accepts commands.
- src/cli: command parser and REPL.
- src/tests: test cases for common rule patterns.

Sample rule sets to ship
- default.json — balanced rules for common use.
- highvalue.json — focus on rare pets and trades.
- tradecoach.json — automatic trade helper and evaluation.
- sandbox.json — test rules for debugging and dev.

API reference (local)
The client offers local endpoints on localhost. The default port is configurable.

HTTP endpoints
- GET /status
  - Returns JSON with active modules and values.
- POST /action
  - Trigger an action manually. Body contains JSON action.

Socket API
- Connect to the configured port and authenticate.
- Send JSON messages and expect JSON responses.

Extending the client
- Add new actions by implementing them in src/engine/actions.
- Add UI components in src/ui/components.
- Add tests in src/tests and run npm test to validate.

Release process
- Increment version in package.json and the changelog.
- Build artifacts with npm run package.
- Create a GitHub release and upload the build files.
- Add release notes that list user-facing changes and migration steps.

Release checklist
- Build artifacts present for each supported OS.
- Changelog updated.
- Release notes include breaking changes.
- A sample config included when needed.

Common log samples
- [2025-01-20T21:30:45Z] [ADOPT] rule "Adopt rare puppies" matched pet id 345
- [2025-01-20T21:31:02Z] [MONEY] set value 999999 by user command
- [2025-01-20T21:31:15Z] [TELEPORT] moving to plaza via pathing mode

FAQ continuation
Q: Do I need to keep the client open while playing?
A: Yes. The client runs the rule engine and overlay. Keep it running to maintain active features.

Q: Can I modify rules while the client runs?
A: Yes. Load a new rules file with "adopt rules load <file>" and changes apply immediately.

Q: How do I save logs for support?
A: Use "log export <path>" to compress current logs and configs into a single archive.

Q: Where are release builds?
A: Check the Releases section on GitHub:
https://github.com/kozomaseko/Adopt-Me-Own/releases

Screenshots and sample media (links)
- Release badge: https://img.shields.io/badge/Download-Releases-brightgreen?logo=github
- HUD sample badge: https://img.shields.io/badge/HUD-Sample-orange
- Roblox logo source: https://upload.wikimedia.org/wikipedia/commons/5/5a/Roblox_Logo_2017.svg

Contact and support
Open issues on the GitHub repo for bug reports, feature requests, or help. Use a clear title and include logs and a description of your environment.

Issue template suggestions
- Title: short description of problem
- Body:
  - Client version
  - OS and build
  - Steps to reproduce
  - Logs (attach export)
  - Expected behavior
  - Actual behavior

Maintenance policy
- Patch releases for critical fixes.
- Minor releases for new features.
- Major releases for breaking changes.

Internationalization
The overlay and the CLI support language packs. Contribute translations by adding JSON files in i18n/.

Testing matrix
- Windows 10 and 11 x64
- macOS 11+
- Node.js v14+ for build scripts

Repository layout (high level)
- src/ - source code
- dist/ - built artifacts
- docs/ - documentation and developer notes
- tests/ - test cases
- samples/ - sample configs and rule sets
- LICENSE.md
- README.md

Backup and restore
- Backup config.json and profiles/*.json before upgrade.
- Use profile export to share or backup the whole profile with "profile export <name> <path>".

Privacy and data
- The client stores only local logs and configs.
- It does not ship data to remote servers by default.
- If you use integrations, they may add external endpoints. Check integration docs before enabling.

End-user tips
- Start with a minimal rule set and increase complexity.
- Keep the "simulate" mode handy while you learn how rules behave.
- Export logs after any session where you change rules to view behavior later.

Legal and ethical reminder (practice)
- Apply your own judgment when you use automation in online environments.
- Keep accounts and interactions within the rules you choose to follow.

Resources and links
- Releases: https://github.com/kozomaseko/Adopt-Me-Own/releases
- Issues: https://github.com/kozomaseko/Adopt-Me-Own/issues
- Source tree: browse the repo on GitHub for src, docs, and samples.

Appendix: Example config and rules
A larger example config with more options:

{
  "overlayKey": "F8",
  "hotkeys": {
    "toggleOverlay": "F8",
    "quickTeleport": "F9",
    "adoptToggle": "F10"
  },
  "modules": {
    "money": {
      "enabled": true,
      "displayValue": 15000,
      "autospend": [
        { "threshold": 10000, "action": { "type": "buy", "item": "StarterHouse", "amount": 1 } }
      ]
    },
    "autoAdopt": {
      "enabled": true,
      "rules": "rules/highvalue.json",
      "maxConcurrent": 2
    },
    "teleport": {
      "enabled": true,
      "presets": {
        "home": [102, 34, 210],
        "plaza": [200, 50, 220],
        "tradepark": [320, 40, 190]
      }
    }
  },
  "logging": {
    "level": "info",
    "path": "logs/",
    "rotateDays": 7
  },
  "api": {
    "port": 4242,
    "authToken": "local-token"
  }
}

A complex rule sample

{
  "name": "High priority ultra-rare adopt or trade",
  "conditions": [
    { "field": "rarity", "op": "in", "value": ["Ultra-Rare", "Legendary"] },
    { "field": "species", "op": "not_in", "value": ["Vehicle"] }
  ],
  "actions": [
    { "type": "notify", "message": "High rarity pet matched" },
    { "type": "adopt" },
    { "type": "wait", "ms": 500 },
    { "type": "log", "message": "Adopt action completed for matched pet" }
  ],
  "priority": 100
}

Export sample log command
- log export session-2025-01-20.zip

Maintenance tips for contributors
- Keep tests updated.
- Follow code style in .editorconfig and .eslintrc.
- Run npm run lint and npm test before PRs.

End of file