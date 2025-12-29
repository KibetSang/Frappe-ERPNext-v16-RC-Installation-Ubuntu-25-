# ERPNext v16 Installation Guide (Ubuntu 25)

This guide covers the installation of **Frappe & ERPNext v16 (RC)** on **Ubuntu 25**, utilizing **Python 3.14** and **Node.js 24**.

## Step 1: Update System
Update your system packages to the latest versions.

```bash
sudo apt update -y
sudo apt upgrade -y


Step 2: Create a Frappe User
It is recommended not to run Bench as root. Create a dedicated user.


sudo adduser frappe_user
sudo usermod -aG sudo frappe_user
su - frappe_user
cd /home/frappe_user
