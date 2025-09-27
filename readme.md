Raspberry Pi 3 WiFi Hotspot with Domain Redirection Using Pi-hole
Overview
This project transforms a Raspberry Pi 3 Model B into a wireless hotspot and DNS controller. All client devices connecting to the Pi’s WiFi hotspot will not only gain internet access (via Ethernet or USB-tethered uplink), but also have specific DNS queries to blocked domains redirected to a demo website hosted locally or externally. This mechanism combines WiFi access point setup with DNS-based redirection via Pi-hole.

Tech Stack & Dependencies
Raspberry Pi 3 Model B with Raspberry Pi OS

NetworkManager for hotspot configuration

Pi-hole for DNS level domain blocking and redirection

Basic Web Server (e.g. lighttpd, nginx) for demo site hosting (optional)

Setup Instructions
1. Raspberry Pi WiFi Hotspot
Ensure Raspberry Pi OS is installed and updated.

Install NetworkManager:

text
sudo apt update
sudo apt install network-manager -y
Create WiFi hotspot (replace SSID/PASSWORD):

text
sudo nmcli device wifi hotspot ifname wlan0 ssid <SSID> password <PASSWORD>
Connect external device to the Pi’s WiFi to verify hotspot presence.

2. Establish Internet Uplink
Connect via Ethernet for automatic uplink.

For USB phone tethering:

Attach a smartphone to Pi via USB.

Enable USB tethering on the phone.

Confirm new network interface (typically usb0) provides internet access.

3. Install and Configure Pi-hole (DNS Control)
Install Pi-hole:

text
curl -sSL https://install.pi-hole.net | bash
Access the Pi-hole Admin Panel from a browser:

text
http://<Pi_IP_Address>/admin
4. Block & Redirect Domains Using Pi-hole
In Pi-hole, add target domains (e.g. abc.com) to the Blacklist.

Under Local DNS → DNS Records, define DNS mapping:

Domain: Specify the blocked domain (e.g. abc.com).

IP Address: Enter the IP of your demo web server (123.com or Pi’s own IP).

Save configuration. Now DNS queries for abc.com resolve to the demo site IP.

5. Host Demo Website On Pi (Optional)
Install a simple web server:

text
sudo apt install lighttpd
sudo systemctl enable lighttpd
Place site files in /var/www/html/.

Ensure firewall allows access to port 80.

6. Usage Flow
Client joins Pi hotspot WiFi.

DNS queries to blocked domains (e.g. abc.com) are intercepted by Pi-hole.

Pi-hole redirects DNS mapping to demo site IP.

Client browser loads demo site upon requesting the blocked domain.

Notes
DNS redirection occurs by spoofing the resolved IP for the domain; HTTPS may cause browser warnings if SSL does not match.

All network traffic from client devices should flow through Pi for redirection and blocking logic.

Extensive control can be achieved by modifying Pi-hole’s block lists and local DNS records.

References
Raspberry Pi Hotspot Setup

Pi-hole DNS Blocking & Redirection

This documentation provides a clear, technical walkthrough for replicating the WiFi access point and DNS-based domain redirection system on a Raspberry Pi 3 Model B.## Raspberry Pi 3 DNS Hotspot & Redirection - README

Project Overview
Deploy a Raspberry Pi 3 Model B as a wireless access point and DNS controller, serving internet through Ethernet or USB-tethering. All devices connecting to the Pi's WiFi hotspot are filtered by Pi-hole, which blocks selected domains and redirects requests to a custom demo website via DNS mapping.

Technical Workflow
WiFi Hotspot Setup:
Utilized NetworkManager (nmcli device wifi hotspot ifname wlan0 ssid <SSID> password <PASSWORD>) to configure Raspberry Pi as a local WiFi AP.

Internet Sharing:
Provided uplink through either the Ethernet port or via USB tethering from a phone, detected as a DHCP network interface on the Pi.

DNS Control & Redirection:
Installed Pi-hole for DNS-level domain manipulation.

Blacklisted domains (e.g., abc.com) via Pi-hole dashboard.

Added custom DNS record in Pi-hole to resolve those domains to a target IP (e.g., demo site hosted on Pi or any external IP).

Webserver (Optional):
Deployed a basic HTTP server (lighttpd) on the Pi (sudo apt install lighttpd), serving custom content for redirected requests.

Summary of Steps
Prepare and update Raspberry Pi OS.

Configure NetworkManager hotspot; verify connectivity.

Establish internet uplink using Ethernet or USB tethering.

Install and configure Pi-hole:

Blacklist undesired domain(s).

Map blocked domains to demo site IP via Pi-hole’s DNS records.

Optionally, host your demo website locally.

Devices connected to Pi WiFi and using Pi-hole DNS are redirected to your demo site if attempting to visit blocked domains.

Technical Considerations
DNS redirection via Pi-hole overrides only DNS resolution (URL remains unchanged); HTTPS sites may trigger certificate warnings due to domain/IP mismatch.

Clients must be connected to hotspot and configured to use Pi-hole DNS (set via DHCP by default).

All instructions and tools used are standard for Raspberry Pi OS and can be replicated or customized for expanded blocking/redirecting logic.
