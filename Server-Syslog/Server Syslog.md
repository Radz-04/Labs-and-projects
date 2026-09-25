# Centralized Syslog Server Implementation

## Project Objectives

* **Goal:** Deploy a centralized Syslog server to collect, aggregate, and manage log data from all network devices.
* **Infrastructure Setup:**
  * **Hypervisor:** Oracle VM VirtualBox
  * **Operating Systems:** 3 Virtual Machines (Ubuntu / Debian)
  * **Network Mode:** Bridged Adapter (enabling direct communication between the VMs and the local router)

![Screenshot 1](Immagini/Immagine1.png)

---

## Why Centralize Logs?

* **Faster Troubleshooting:** Eliminates the need to log into individual endpoints to diagnose issues. All system events and error logs are consolidated into a single location.
* **Enhanced Security & Data Integrity:** If a host is compromised or encounters a system crash, local log files may be tampered with or lost. Transmitting logs in real time ensures an immutable off-site backup.
* **Full Infrastructure Visibility:** Delivers a comprehensive, real-time overview of overall network activity and health.

---

## Server Configuration

1) **System Update:** Run `sudo apt update && sudo apt upgrade` to ensure all packages are up to date.
2) **IP Verification:** Identify the active network interface and IP address using `ip a`.
3) **Enable Transport Protocols:** Uncomment the UDP reception directives in `/etc/rsyslog.conf` to enable the port 514 listener module. In addition to UDP, TCP support was enabled to accommodate devices requiring connection-oriented, guaranteed delivery.
4) **Restart Service:** Run `sudo systemctl restart rsyslog` to apply the changes.

![Screenshot 2](Immagini/Immagine2.png)

---

## Client Configuration

1) **Configure Forwarding:** Open `/etc/rsyslog.conf` on client machines and append `*.* @192.168.1.43` to route all generated logs over UDP to the central server.
2) **Restart Service:** Execute `sudo systemctl restart rsyslog` to apply the configuration and initiate log transmission.

![Screenshot 3](Immagini/Immagine3.png)

---

## Testing & Verification

1) **Live Server Monitoring:** Run `sudo tail -f /var/log/syslog` on the central server to monitor incoming event logs in real time.
2) **Generate Test Entry:** Execute `logger "CIAO"` on a client machine to trigger a test log submission.

![Screenshot 4](Immagini/Immagine4.png)

![Schermata 4](Immagini/Immagine4.png)
