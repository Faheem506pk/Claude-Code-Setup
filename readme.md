# Free Claude Code Setup with Google Gemini

A complete setup guide for configuring Claude Code with Gemini models using `claude-code` and `claude-code-router`.

---

## 1. Prerequisites

### Verify Node.js Installation

Open PowerShell and run:

```powershell
node --version
```

- If the version is 18.x or higher, you're good to continue.
- If not, download and install the latest LTS release from [nodejs.org](https://nodejs.org).

---

## 2. Generate Your Google API Key

1. Visit: [aistudio.google.com](https://aistudio.google.com)
2. Select **Get API Key**
3. Click **Create API Key**
4. Copy the generated key (it should look something like `AIzaSy...............`).

---

## 3. Install Required CLI Tools

Open PowerShell (Run as Administrator) and execute:

```powershell
npm install -g @anthropic-ai/claude-code @musistudio/claude-code-router
```

This installs both Claude Code and the router globally on your system.

---

## 4. Create Configuration Directories

Run the following in a normal PowerShell window:

```powershell
mkdir $HOME/.claude-code-router
mkdir $HOME/.claude
```

---

## 5. Create the Router Configuration File

Since Windows does not support `cat << EOF` syntax easily in PowerShell, create the configuration file using Notepad:

```powershell
notepad $HOME/.claude-code-router/config.json
```

Paste the following JSON configuration exactly as shown:

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

Save and close Notepad once done.

---

## 6. Configure Environment Variable (API Key)

Run PowerShell as Administrator to set the system-wide environment variable:

```powershell
[System.Environment]::SetEnvironmentVariable('GOOGLE_API_KEY', 'YOUR_API_KEY_HERE', 'User')
```

*(Replace `YOUR_API_KEY_HERE` with the actual key from Step 2)*

### Verify the Key

Close the current PowerShell window, open a **new** window, and run:

```powershell
echo $env:GOOGLE_API_KEY
```

If the key appears, the environment variable is configured correctly.

---

## 7. Validate Installation

Run the following commands to ensure everything is set up correctly:

```powershell
claude --version
ccr version
echo $env:GOOGLE_API_KEY
```

If all commands return output without errors, the setup is successful.

---

## 8. Run the Services

### Start the Router (Terminal 1)

Open a new PowerShell window and start the Claude Code Router:

```powershell
ccr start
```

Wait until you see: `✔ Service started successfully`

### Use Claude Code (Terminal 2)

Open another PowerShell window, navigate to your project directory, and launch Claude Code through the router:

```powershell
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
