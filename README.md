1. VPS Preparation

1.1. Connect to the server via SSH as root:

```bash
ssh root@YOUR_IP
```

1.2. Update the system and install basic utilities:

```bash
apt update && apt upgrade -y
apt install -y curl wget sudo
```

---

2. Installing Pterodactyl (Panel + Wings)

2.1. Run the official installer (supports Debian 13):

```bash
bash <(curl -s https://pterodactyl-installer.se)
```

2.2. Select option 2 (Install both Panel and Wings) from the menu.

2.3. Answer the script's prompts:

· Database password: create and save a password.
· Timezone: Europe/Moscow (or your local timezone).
· Email: your email address.
· Admin Username / Password: administrator login and password.
· FQDN: enter your VPS IP address (e.g., 2.27.209.223).
· SSL (Let's Encrypt): answer N (no), since you are using an IP address rather than a domain.
· Firewall (UFW): answer y (yes).

2.4. Once finished, open http://YOUR_IP in your browser — the Pterodactyl login page should appear.

---

3. Configuring Wings (linking to the panel)

3.1. In the panel, navigate to: Admin Panel (gear icon) → Nodes → Create New.

3.2. Fill in the fields:

· Name: Node-01
· Location: create a location beforehand (Locations → Create New), e.g., Local.
· FQDN: your IP address.
· Total Memory: specify the VPS RAM in MB (e.g., 4096).
· Total Disk: specify the disk space in MB. · Daemon Port: 8080
· Daemon SFTP Port: 2022

Click **Create Node**.

3.3. On the newly created node's page, go to the **Configuration** tab. Copy the entire YAML code or click **Generate Token** for the auto-command.

3.4. Open the config file via SSH:

```bash
nano /etc/pterodactyl/config.yml
```

Paste the copied YAML. Save the file (Ctrl+O, Enter, Ctrl+X).

3.5. Start Wings:

```bash
systemctl restart wings
systemctl enable wings
```

3.6. Check the status:

```bash
systemctl status wings
```

It should show as `active (running)`.

---

4. Creating a Location (if you haven't already)

4.1. In the panel: **Admin Panel** → **Locations** → **Create New**.

· Short Code: local
· Description: Local server

Save.

---

5. Importing the Egg for SCP:SL

5.1. Download the official Egg: https://eggs.pterodactyl.io/egg/games-scp-sl/ (click the JSON download button).

5.2. In the panel: **Admin Panel** → **Nests** → **Create New**.

· Enter name: SCP:SL
· Save.

5.3. Return to the **Nests** page and click **Import Egg** next to the newly created nest. Select the downloaded JSON file.

---

6. Creating a Server

6.1. Go to: **Servers** → **Create New**.

6.2. Fill in the details:

· Nest: SCP:SL
· Egg: SCP:SL
· Memory: Minimum 3096 MB; 4096 MB recommended.
· Disk: 10 GB or more.
· Allocation: Select port 7777 (if unavailable, create an allocation on the node: **Nodes** → [your node] → **Allocations** → **Assign New**, IP 0.0.0.0, port 7777). 6.3. Click "Create Server".

---

7. First Launch

7.1. Click "Start" on the server page.

7.2. The panel will begin the installation via SteamCMD. Wait for the "Installation completed..." message to appear in the console.

7.3. The server will start automatically. You may need to accept the EULA upon the first launch—type "yes" in the console when prompted.

---

8. Server Verification

8.1. Enter the following command in the server console (within the panel):

· !verify static — if you have a static IP
· !verify dynamic — if you have a dynamic IP

8.2. Alternatively: send an email to safety.compliance@scpslgame.com with your IP address, port, and IP type (static/dynamic).

---

9. LabAPI (Important)

LabAPI does not require a separate installation; it is built into the SCP:SL dedicated server itself. After the first successful launch, the folder `LabAPI/plugins/global/` will appear in the server's file system. Upload plugin `.dll` files to this folder using the panel's file manager.

---

10. Opening Ports (If Necessary)

If the firewall is not configured automatically:

```bash
ufw allow 7777/tcp
ufw allow 7777/udp
ufw allow 8080/tcp
ufw allow 2022/tcp
```

Done. The server should appear in the SCP:SL server browser after verification.