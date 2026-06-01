```markdown
# Run `opencode-ai` in Termux (Ubuntu via proot-distro)

This guide walks you through setting up **Termux** on Android, installing Ubuntu using `proot-distro`, and running the `opencode-ai` package globally via Node.js.

## Prerequisites

- Android device with **Termux** installed (from F-Droid or GitHub).
- At least 2–3 GB free storage (for Ubuntu + Node.js + projects).
- Internet connection.

---

## 1. Prepare Termux

Update Termux packages and install `proot-distro` (for running Linux distributions).

```bash
pkg update && pkg upgrade -y
pkg install proot-distro -y
```

---

2. Set up Storage & Install Ubuntu

Grant Termux access to your phone’s shared storage, then install Ubuntu.

```bash
termux-setup-storage
proot-distro install ubuntu
```

Note: When you run termux-setup-storage, a system permission dialog will appear – tap Allow.

---

3. Login to Ubuntu (with phone storage bind‑mount)

Start Ubuntu and bind your phone’s internal storage to /mobile_storage inside the container. This lets you access files from /sdcard (e.g., Download, Documents).

```bash
proot-distro login ubuntu --bind /storage/emulated/0:/mobile_storage
```

You are now inside an Ubuntu environment. The prompt will change to something like root@localhost:~#.

---

4. Install Node.js 20.x

Inside the Ubuntu container, update packages, install curl, and add the NodeSource repository.

```bash
apt update && apt upgrade -y
apt install curl -y
curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
apt install -y nodejs
```

Verify the installation:

```bash
node --version   # Should be v20.x.x
npm --version    # Should be 10.x.x or later
```

---

5. Install opencode-ai globally.

```bash
npm i -g opencode-ai
```

After installation, the opencode-ai command will be available system‑wide inside Ubuntu.

---

6. Run opencode-ai from your phone’s storage

Navigate to the Download folder on your phone, create a project directory, and launch the AI tool.

```bash
cd /mobile_storage/Download
mkdir AI_Projects && cd AI_Projects
opencode-ai
```

💡 Tip: You can replace Download with any other folder (e.g., Documents, Projects) as long as it exists under /mobile_storage.

---

Additional Notes

· To exit the Ubuntu container, type exit or press Ctrl + D.
· To re‑enter Ubuntu later, simply run the same proot-distro login ... command again.
· If you get permission errors with termux-setup-storage, restart Termux and try again.
· The --bind option maps your real Android storage to a path inside Ubuntu. Any file you create in /mobile_storage will be visible in Android’s normal file manager (under /storage/emulated/0).

---

Troubleshooting

Issue Possible solution
proot-distro: command not found Run pkg install proot-distro -y again and restart Termux.
apt fails inside Ubuntu Run apt update --fix-missing and check your internet connection.
opencode-ai not found after npm i -g Log out and back into Ubuntu, or run source ~/.bashrc.
Permission denied when accessing /mobile_storage Make sure you ran termux-setup-storage and allowed the permission.

---

License

This guide is provided as‑is. opencode-ai and its dependencies are subject to their own licenses.

```
