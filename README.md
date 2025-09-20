# Piholecrypt

<p align="center">
  <a href="https://pi-hole.net"><img src="https://pi-hole.github.io/graphics/Vortex/Vortex_with_text.png" height="225" alt="Pi-hole logo"></a>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://dnscrypt.info"><img src="https://raw.githubusercontent.com/dnscrypt/dnscrypt-proxy/master/logo.png" height="225" alt="Pi-hole logo"></a>
</p>

A simple Docker Compose setup to run **Pi-hole** for network-wide ad-blocking alongside **DNSCrypt Proxy** for secure, encrypted DNS queries on a Raspberry Pi or any other Docker host.

---

## ✨ Features

* **Network-Wide Ad Blocking**: Pi-hole acts as a DNS sinkhole, blocking ads and trackers on all devices on your network.
* **Encrypted DNS**: DNSCrypt Proxy encrypts your DNS traffic, preventing ISP snooping and DNS spoofing.
* **Easy Deployment**: Uses Docker and Docker Compose for a quick, isolated, and reproducible setup.
* **Lightweight**: Perfect for running on a Raspberry Pi.

---

## ✅ Prerequisites

Before you begin, ensure you have the following installed on your machine:
* [Docker](https://docs.docker.com/engine/install/)
* [Docker Compose](https://docs.docker.com/compose/install/)

---

## 🚀 Quick Start / Deployment

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/xetxezarreta/piholecrypt.git](https://github.com/xetxezarreta/piholecrypt.git)
    cd piholecrypt
    ```

2.  **Configure your setup:**
    Open the `docker-compose.yml` file and edit the environment variables for the `pihole` service to match your needs. See the configuration section below for more details.

3.  **Build and run the containers:**
    Run the following command to build the DNSCrypt image and start both services in the background.
    ```bash
    docker-compose up -d
    ```
    The initial startup may take a few minutes.

---

## 🔧 Configuration

All configuration is handled within the `docker-compose.yml` file.



The most important settings are in the `environment` section of the `pihole` service:

* `TZ`: **Set your timezone** (e.g., `Europe/Madrid`) to ensure logs are timestamped correctly.
* `WEBPASSWORD`: **Set a secure password** for the Pi-hole web admin interface. If you leave it blank, a random password will be generated on the first run (viewable with `docker-compose logs pihole`).
* `DNS1`: This is the **primary DNS server** Pi-hole will use. It's pre-configured to point to the DNSCrypt container at `10.0.1.2#5300`.
* `DNS2`: The secondary DNS server. It is set to `no` to ensure all queries go through DNSCrypt.

---

## 🖥️ Post-Installation

Once the containers are running, your new DNS server is ready.

1.  **Access the Admin Interface**:
    Navigate to `http://<your-raspberry-pi-ip>/admin` in your web browser and log in with the password you set.

2.  **Configure Your Network**:
    To start blocking ads, you need to point your devices to Pi-hole. The easiest way is to configure your router's DHCP settings to use your Raspberry Pi's IP address as the **sole DNS server** for all clients on your network.

---

## ⬆️ Updating

To update your containers to the latest versions of Pi-hole and DNSCrypt Proxy:

1.  **Pull the latest images:**
    ```bash
    docker-compose pull
    ```

2.  **Recreate the containers:**
    The `up` command will automatically stop, remove, and recreate any containers whose images have been updated. Your persistent volume data will be retained.
    ```bash
    docker-compose up -d
    ```
