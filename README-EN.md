SCP: Secret Laboratory Dedicated Server + Pterodactyl

A quick guide to installing the SCP: Secret Laboratory Dedicated Server on a VPS using Pterodactyl Panel + Wings.

⚠️ This guide has been tested on Debian 13. Installation steps may differ on other operating systems.

1. VPS Preparation

Connect via SSH:

```
ssh root@YOUR_IP
```

Update the system:

```
apt update && apt upgrade -y apt install -y curl wget sudo 2. Pterodactyl Installation
```

Run the installer:

```
bash <(curl -s https://pterodactyl-installer.se)
```
Select:

2) Install both Panel and Wings

During installation, specify the following:

```
Database password — your password 
Timezone — Europe/Moscow 
Email — your email 
Admin login/password — admin credentials FQDN — domain or IP 
SSL — y (if using a domain) 
Firewall — y
```

After installation, open the panel:

```
https://YOUR_DOMAIN 3. Creating a Node
```

In the panel:

```
Admin Panel → Locations → Create New
```

Create a Location, for example:

Short Code: local Description: Local server

Then:

Admin Panel → Nodes → Create New

Example:

Name: Node-01 Location: local FQDN: YOUR_DOMAIN_OR_IP Daemon Port: 8080 SFTP Port: 2022

Set RAM and Disk values ​​according to your VPS specifications. After creating the Node, open the Configuration, copy the configuration data, and paste it into:

nano /etc/pterodactyl/config.yml

Restart Wings:

systemctl restart wings systemctl enable wings

Check the status:

systemctl status wings

It should show:

active (running) 4. Allocation

Open:

Admin Panel → Nodes → Node-01 → Allocations

Add:

IP: 0.0.0.0 Port: 7777

For additional servers, you can use:

7778 7779 7780 5. SCP:SL Egg

Create:

Admin Panel → Nests → Create New

For example:

Name: SCP:SL

Then, import your JSON Egg via:

Import Egg

Use your prepared .json file. The Egg settings must match the SCP:SL version being used.

6. Creating the server

Servers → Create New

Select:

Nest: SCP:SL Egg: SCP:SL Memory: 4096 MB Disk: 10 GB+ Allocation: 7777

Create the server and click Start.

Pterodactyl will automatically install the server via SteamCMD.

7. LabAPI

After installing SCP:SL, use the appropriate LabAPI structure.

LabAPI plugins are usually uploaded to:

LabAPI/plugins/global/

Restart the server after uploading the .dll file.

⚠️ The plugin must be compatible with the SCP:SL and LabAPI versions.

8. EXILED

If EXILED is used instead of LabAPI, install the EXILED version compatible with your SCP:SL version.

After installation, place EXILED plugins in:

EXILED/Plugins/

Configurations:

EXILED/Configs/

⚠️ Do not install LabAPI and EXILED plugins without checking for compatibility. They are different modding ecosystems. 9. Opening ports

If using UFW:

```
ufw allow 7777/tcp ufw allow 7777/udp ufw allow 8080/tcp ufw allow 2022/tcp
```

Check status:

ufw status 10. Done

After startup:

```
VPS ├── Pterodactyl Panel ├── Wings └── SCP: Secret Laboratory ├── LabAPI └── Plugins
```
The server can now be fully managed via Pterodactyl:

start/stop; restart; console; files; plugins; multiple SCP:SL servers on a single VPS.

⚠️ SCP:SL, LabAPI, and EXILED are constantly being updated. Check version compatibility before installation.