# Frappe v16 Installation Guide (Ubuntu 25)

A step-by-step guide to installing Frappe Framework v16 (RC.1), ERPNext, and HRMS on Ubuntu 25, ensuring Python 3.14 and Node.js 24 compatibility.

---

## Step 1: Update System

Update your package lists and upgrade existing packages.

```bash
sudo apt update -y
sudo apt upgrade -y
Step 2: Create a Frappe User
It is recommended not to run Bench as root. Create a dedicated user.

bash
Copy code
sudo adduser frappe_user
sudo usermod -aG sudo frappe_user
su - frappe_user
cd /home/frappe_user
Note: You can use your existing user; just replace frappe_user with your username.

Step 3: Install Dependencies
Install essential build tools, Python 3.14, and other prerequisites.

bash
Copy code
sudo apt install -y git python3-dev python3-setuptools python3-pip python3.14-venv software-properties-common curl build-essential
Step 4: Install MariaDB
Install the MariaDB database server and secure it.

bash
Copy code
sudo apt install -y mariadb-server
sudo mariadb-secure-installation
Recommended settings during setup:

pgsql
Copy code
Enter current password for root: (Press ENTER)
Switch to unix_socket authentication? Y
Change root password? Y
Remove anonymous users? Y
Disallow root login remotely? N
Remove test database? Y
Reload privilege tables? Y
Step 5: Configure MariaDB
Edit the configuration file to ensure correct character encoding.

bash
Copy code
sudo nano /etc/mysql/my.cnf
Add the following configuration block under [mysqld] and [mysql]:

ini
Copy code
[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci

[mysql]
default-character-set = utf8mb4
Save and restart MariaDB:

bash
Copy code
sudo service mysql restart
Step 6: Install Redis
bash
Copy code
sudo apt install -y redis-server
Step 7: Install Node.js 24 & Yarn
Frappe v16 requires Node.js 24.

1. Remove old versions
bash
Copy code
sudo apt remove -y nodejs npm
sudo apt autoremove -y
2. Install prerequisites
bash
Copy code
sudo apt update
sudo apt install -y curl ca-certificates gnupg build-essential
3. Install Node.js 24
bash
Copy code
curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash -
sudo apt install -y nodejs
4. Verify installation
bash
Copy code
node -v  # Should show v24.x.x
npm -v
5. Install Yarn globally
bash
Copy code
sudo npm install -g yarn
yarn -v  # Should show 1.22.x
Step 8: Install PDF Rendering Tools
bash
Copy code
sudo apt install -y xvfb libfontconfig wkhtmltopdf
Step 9: Install Bench via pipx
Ensure pipx is installed using Python 3.14 to manage the environment correctly.

bash
Copy code
python3.14 -m pip install --user pipx
python3.14 -m pipx ensurepath
source ~/.bashrc
pipx install frappe-bench
Step 10: Initialize Frappe Bench
Initialize the bench using Python 3.14 and the v16 Release Candidate branch.

bash
Copy code
bench init frappe-bench \
  --frappe-branch v16.0.0-rc.1 \
  --python python3.14
This command resolves Python and Node/Yarn dependencies correctly.

Step 11: Create a New Site
bash
Copy code
cd frappe-bench
bench new-site yoursite_name
Enter MySQL root user: root (or your MySQL user)

MySQL root password: (Your password from Step 4)

Administrator password: (Set a password for the Frappe Administrator)

Step 12: Get ERPNext & HRMS Apps (v16 RC)
Download the Release Candidate versions of the apps.

bash
Copy code
# Get ERPNext
bench get-app erpnext --branch v16.0.0-rc.1

# Get HRMS
bench get-app hrms --branch v16.0.0-rc.1
Step 13: Install Apps on Your Site
bash
Copy code
bench --site yoursite_name install-app erpnext
bench --site yoursite_name install-app hrms
Set this as the current site:

bash
Copy code
bench use yoursite_name
Step 14: Start Development Server
bash
Copy code
bench start
Access your site at: http://127.0.0.1:8000
