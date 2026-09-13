🧪 SCP: Secret Laboratory Dedicated Server + Pterodactyl

A quick and easy-to-follow guide to installing SCP: Secret Laboratory Dedicated Server on a VPS using Pterodactyl Panel + Wings.

«⚠️ This guide has been tested on Debian 13.
Installation steps may differ on other operating systems.»

---

1. VPS Preparation

Connect to your VPS via SSH:

```
ssh root@YOUR_IP
```

Update the system and install basic utilities:

```
apt update && apt upgrade -y
apt install -y curl wget sudo
```

---

2. Pterodactyl Installation

Run the Pterodactyl installer:

```
bash <(curl -s https://pterodactyl-installer.se)
```

Select:

2) Install both Panel and Wings

During the installation, specify the following:

Database password — your database password
Timezone — Europe/Moscow
Email — your email address
Admin login/password — your administrator credentials
FQDN — your domain or IP address
SSL — y (if using a domain)
Firewall — y

After the installation is complete, open the panel:

https://YOUR_DOMAIN

---

3. Creating a Node

In the Pterodactyl panel, open:

Admin Panel → Locations → Create New

Create a Location, for example:

Short Code: local
Description: Local server

Then open:

Admin Panel → Nodes → Create New

Example configuration:

Name: Node-01
Location: local
FQDN: YOUR_DOMAIN_OR_IP
Daemon Port: 8080
SFTP Port: 2022

Set the RAM and Disk values according to your VPS specifications.

After creating the Node, open the Configuration tab and copy the configuration data.

Open the Wings configuration file:

```
nano /etc/pterodactyl/config.yml
```

Paste the configuration and save the file.

Restart Wings:

```
systemctl restart wings
systemctl enable wings
```

Check the status:

systemctl status wings

It should show:

Active: active (running)

---

4. Allocation

Open:

Admin Panel → Nodes → Node-01 → Allocations

Add an allocation:

IP: 0.0.0.0
Port: 7777

For additional SCP:SL servers, you can use separate ports:

7778
7779
7780

---

5. SCP:SL Egg

Create a new Nest:

Admin Panel → Nests → Create New

For example:

Name: SCP:SL

Then import your prepared JSON Egg using:

Import Egg

«📦 Use your own prepared ".json" Egg file.»

Make sure that the Egg configuration matches the SCP: Secret Laboratory version you are going to use.

---

6. Creating the Server

Open:

Servers → Create New

Select:

Nest: SCP:SL
Egg: SCP:SL
Memory: 4096 MB
Disk: 10 GB+
Allocation: 7777

Create the server and click:

Start

Pterodactyl will automatically install the server using SteamCMD.

«⏳ The first installation may take some time.»

---

7. LabAPI

After installing SCP:SL, use the appropriate LabAPI structure for your version.

LabAPI plugins are usually placed in:

```
LabAPI/
└── plugins/
    └── global/
        ├── Plugin1.dll
        ├── Plugin2.dll
        └── Plugin3.dll
```

Upload your ".dll" plugin files to:

LabAPI/plugins/global/

Restart the server after uploading the plugins.

«⚠️ The plugin must be compatible with both your SCP:SL and LabAPI versions.»

---

8. EXILED

If you are using EXILED, install the EXILED version compatible with your SCP:SL version.

EXILED plugins are placed in:

```
EXILED/
└── Plugins/
    ├── Plugin1.dll
    └── Plugin2.dll
```

Configuration files are located in:

```
EXILED/
└── Configs/
```

«⚠️ Check plugin compatibility before installation. LabAPI and EXILED are different modding ecosystems, and plugins designed for one framework should not automatically be assumed to work with the other.»

---

9. Opening Ports

If you are using UFW, allow the required ports:

```
ufw allow 7777/tcp
ufw allow 7777/udp
ufw allow 8080/tcp
ufw allow 2022/tcp
```

Check the firewall status:

ufw status

«💡 If you use a different SCP:SL port, replace "7777" with your chosen port.»

---

10. Done! 🎉

After successful installation, your setup will look like:

```
VPS
├── Pterodactyl Panel
├── Wings
└── SCP: Secret Laboratory
    ├── LabAPI
    │   └── Plugins
    └── EXILED
        ├── Plugins
        └── Configs
```

The server can now be managed through Pterodactyl:

```
- ▶️ Start / Stop
- 🔄 Restart
- 🖥️ Console
- 📁 File management
- 🔌 Plugin management
- 🌐 Multiple SCP:SL servers on one VPS
```

---

⚠️ Important

«SCP:SL, LabAPI, and EXILED are constantly being updated.

Always check version compatibility before installing the server, framework, or plugins.»

Make sure the following versions are compatible:

```
SCP:SL
   ↓
LabAPI / EXILED
   ↓
Plugins
```

«⚠️ Game, API, and plugin versions must be compatible with each other.»

---

⭐ Support

If this guide was useful, consider giving this repository a ⭐.

Made for the SCP: Secret Laboratory community 🧪