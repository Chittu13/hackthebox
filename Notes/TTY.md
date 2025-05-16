# 🛠️ Reverse Shell Upgrade Guide

> **Last Modified:** 16-May-2025
> When you get a reverse shell, it often lacks features like tab completion or clear output formatting. This guide shows how to upgrade it for better usability.

---

## 🔧 Step 1: Spawn a TTY Shell

Once you're connected to the reverse shell, use one of the following commands to spawn a more functional shell:

```bash
# If Python 3 is available
python3 -c 'import pty; pty.spawn("/bin/bash")'

# If Python 2 is available
python -c 'import pty; pty.spawn("/bin/bash")'

# Alternative (if script command is available)
SHELL=/bin/bash script -q /dev/null
```

---

## ⬆️ Step 2: Get Full TTY Control

After spawning the shell:

1. Press `Ctrl + Z` to background the shell.
2. Run the following on your local terminal:

```bash
stty raw -echo
fg
```

3. Press `Enter` twice to return to the shell.
4. Then set a proper terminal type:

```bash
export TERM=xterm
```

---

## ✅ Done!

You now have a much more interactive shell with improved capabilities like:
- Proper text formatting
- Command history
- Tab auto-completion

> These upgrades make post-exploitation work much easier and smoother.
