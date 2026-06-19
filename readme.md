# Free Claude Code Setup with Google Gemini

A complete setup guide for configuring Claude Code with Gemini models using `claude-code` and `claude-code-router`. This guide includes instructions for both **Windows (PowerShell)** and **Ubuntu/Linux (Bash)**.

---

## 1. Prerequisites

### Verify Node.js Installation

Check if Node.js is installed (Version 18.x or higher is required).

**Windows (PowerShell):**
```powershell
node --version
```
*If not installed, download the LTS release from [nodejs.org](https://nodejs.org).*

**Ubuntu/Linux (Bash):**
```bash
node --version
```
*If not installed, you can install it via NodeSource:*
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```

---

## 2. Generate Your Google API Key

1. Visit: [aistudio.google.com](https://aistudio.google.com)
2. Select **Get API Key**
3. Click **Create API Key**
4. Copy the generated key (it should look something like `AIzaSy...............`).

---

## 3. Install Required CLI Tools

**Windows (PowerShell - Run as Administrator):**
```powershell
npm install -g @anthropic-ai/claude-code @musistudio/claude-code-router
```

**Ubuntu/Linux:**
```bash
sudo npm install -g @anthropic-ai/claude-code @musistudio/claude-code-router
```
*(If you use `nvm` or have configured npm for global installs without sudo, you can omit `sudo`.)*

This installs both Claude Code and the router globally on your system.

---

## 4. Create Configuration Directories

**Windows (PowerShell):**
```powershell
mkdir $HOME/.claude-code-router
mkdir $HOME/.claude
```

**Ubuntu/Linux (Bash):**
```bash
mkdir -p ~/.claude-code-router ~/.claude
```

---

## 5. Create the Router Configuration File

**Windows (PowerShell):**
Create the file using Notepad:
```powershell
notepad $HOME/.claude-code-router/config.json
```

**Ubuntu/Linux (Bash):**
Create the file using Nano:
```bash
nano ~/.claude-code-router/config.json
```

**Paste the following JSON configuration exactly as shown:**

```json
{
  "LOG": true,
  "LOG_LEVEL": "info",
  "HOST": "127.0.0.1",
  "PORT": 3456,
  "API_TIMEOUT_MS": 600000,
  "Providers": [
    {
      "name": "gemini",
      "api_base_url": "https://generativelanguage.googleapis.com/v1beta/models/",
      "api_key": "$GOOGLE_API_KEY",
      "models": [
        "gemini-2.5-flash",
        "gemini-2.0-flash"
      ],
      "transformer": {
        "use": ["gemini"]
      }
    }
  ],
  "Router": {
    "default": "gemini,gemini-2.5-flash",
    "background": "gemini,gemini-2.5-flash",
    "think": "gemini,gemini-2.5-flash",
    "longContext": "gemini,gemini-2.5-flash",
    "longContextThreshold": 60000
  }
}
```

Save and close the file (`Ctrl+O`, `Enter`, then `Ctrl+X` for Nano).

---

## 6. Configure Environment Variable (API Key)

**Windows (PowerShell - Run as Administrator):**
```powershell
[System.Environment]::SetEnvironmentVariable('GOOGLE_API_KEY', 'YOUR_API_KEY_HERE', 'User')
```
*(Replace `YOUR_API_KEY_HERE` with the actual key from Step 2. Then close and reopen PowerShell.)*

**Ubuntu/Linux (Bash):**
```bash
echo 'export GOOGLE_API_KEY="YOUR_API_KEY_HERE"' >> ~/.bashrc
source ~/.bashrc
```
*(Replace `YOUR_API_KEY_HERE` with the actual key from Step 2.)*

### Verify the Key

**Windows:**
```powershell
echo $env:GOOGLE_API_KEY
```

**Ubuntu/Linux:**
```bash
echo $GOOGLE_API_KEY
```

If the key appears, the environment variable is configured correctly.

---

## 7. Validate Installation

Run the following commands to ensure everything is set up correctly:

```bash
claude --version
ccr version
```

If all commands return output without errors, the setup is successful.

---

## 8. Run the Services

### Start the Router (Terminal 1)

Open a new terminal window and start the Claude Code Router:

```bash
ccr start
```

Wait until you see: `✔ Service started successfully`

### Use Claude Code (Terminal 2)

Open another terminal window, navigate to your project directory, and launch Claude Code through the router:

```bash
cd your-project-folder
ccr code
```

---

## 9. Quick Test

Once you have launched `ccr code` in Terminal 2, type a simple greeting:

```
hi
```

If Claude responds, your Claude Code + Gemini setup is fully operational and ready to use!

---

## 10. Open Source

This setup relies on tools like `claude-code-router`, which are built on open-source principles. Feel free to contribute, report issues, or suggest improvements to the open-source community that makes these integrations possible!
