Markdown
# ERPNext v16 Installation Guide (Ubuntu 25)

This guide covers the installation of **Frappe & ERPNext v16 (RC)** on **Ubuntu 25**, utilizing **Python 3.14** and **Node.js 24**.

## Step 1: Update System
Update your system packages to the latest versions.

```bash
sudo apt update -y
sudo apt upgrade -y
Step 2: Create a Frappe User
It is recommended not to run Bench as root. Create a dedicated user.

Bash
sudo adduser frappe_user
sudo usermod -aG sudo frappe_user
su - frappe_user
cd /home/frappe_user
Note: You can use your existing user; just replace frappe_user with your username.

Step 3: Install Dependencies
Install the required system packages, including Python 3.14 tools.

Bash
sudo apt install -y git python3-dev python3-setuptools python3-pip python3.14-venv software-properties-common curl build-essential
Step 4: Install MariaDB
Install the database server and run the security script.

Bash
sudo apt install -y mariadb-server
sudo mariadb-secure-installation
Recommended options during setup:

Enter current password for root: ENTER

Switch to unix_socket authentication? Y

Change root password? Y

Remove anonymous users? Y

Disallow root login remotely? N

Remove test database? Y

Reload privilege tables? Y

Step 5: Configure MariaDB
Edit the MySQL configuration file to ensure correct character encoding.

Bash
sudo nano /etc/mysql/my.cnf
Add the following configuration block:

Ini, TOML
[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci

[mysql]
default-character-set = utf8mb4
Restart the MariaDB service to apply changes:

Bash
sudo service mysql restart
Step 6: Install Redis
Install Redis server for caching and background jobs.

Bash
sudo apt install -y redis-server
Step 7: Install Node.js 24 & Yarn
Frappe v16 requires Node.js 24.

1. Remove old versions (if any):

Bash
sudo apt remove -y nodejs npm
sudo apt autoremove -y
2. Install prerequisites:

Bash
sudo apt update
sudo apt install -y curl ca-certificates gnupg build-essential
3. Install Node.js 24:

Bash
curl -fsSL [https://deb.nodesource.com/setup_24.x](https://deb.nodesource.com/setup_24.x) | sudo -E bash -
sudo apt install -y nodejs
4. Verify Installation:

Bash
node -v  # should show v24.x.x
npm -v
5. Install Yarn globally:

Bash
sudo npm install -g yarn
yarn -v  # should show 1.22.x
Step 8: Install PDF Rendering Tools
Install wkhtmltopdf and necessary fonts.

Bash
sudo apt install -y xvfb libfontconfig wkhtmltopdf
Step 9: Install Bench via pipx
Use pipx to install Bench in an isolated environment.

Bash
python3.14 -m pip install --user pipx
python3.14 -m pipx ensurepath
source ~/.bashrc
pipx install frappe-bench
Step 10: Initialize Frappe Bench
Initialize the bench using Python 3.14 and the v16 Release Candidate branch.

Bash
bench init frappe-bench \
  --frappe-branch v16.0.0-rc.1 \
  --python python3.14
Step 11: Create a New Site
Create a new site within your bench directory.

Bash
cd frappe-bench
bench new-site yoursite_name
Prompts:

MySQL root user: root (or your MySQL user)

MySQL root password: (The password you set in Step 4)

Administrator password: (Set a password for the Frappe Admin)

Step 12: Get ERPNext & HRMS Apps (v16 RC)
Download the specific release candidate branches for ERPNext and HRMS.

Bash
# Get ERPNext
bench get-app erpnext --branch v16.0.0-rc.1

# Get HRMS
bench get-app hrms --branch v16.0.0-rc.1
Step 13: Install Apps on Your Site
Install the downloaded apps onto your specific site.

Bash
bench --site yoursite_name install-app erpnext
bench --site yoursite_name install-app hrms
Set this as the default site:

Bash
bench use yoursite_name
Step 14: Start Development Server
Start the server to access the application.

Bash
bench start
Open your browser and navigate to:http://127.0.0.1:8000
