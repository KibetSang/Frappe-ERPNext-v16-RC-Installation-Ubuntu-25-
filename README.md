Step 1: Update

sudo apt update -y
sudo apt upgrade -y

Step 2: Create a Frappe Userr
It’s recommended not to run Bench as root.

sudo adduser frappe_user
sudo usermod -aG sudo frappe_user
su - frappe_user
cd /home/frappe_user

You can also use your existing user; just replace frappe_user with your username.

Step 3: Install Dependencies
sudo apt install -y git python3-dev python3-setuptools python3-pip python3.14-venv software-properties-common curl build-essential
Step 4: Install MariaDB
sudo apt install -y mariadb-server
sudo mariadb-secure-installation

During setup, recommended options:

Enter current password for root: ENTER

Switch to unix_socket authentication? Y

Change root password? Y

Remove anonymous users? Y

Disallow root login remotely? N

Remove test database? Y

Reload privilege tables? Y

Step 5: Configure MariaDB
sudo nano /etc/mysql/my.cnf

Add under [mysqld]:

[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
[mysql]
default-character-set = utf8mb4

Restart MariaDB:

sudo service mysql restart

Step 6: Install Redis
sudo apt install -y redis-server

Step 7: Install Node.js 24 & Yarn (Frappe v16 requirement)
Remove old Node.js (if any)
sudo apt remove -y nodejs npm
sudo apt autoremove -y

Install prerequisites
sudo apt update
sudo apt install -y curl ca-certificates gnupg build-essential

Install Node.js 24
curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash -
sudo apt install -y nodejs

Verify:

node -v  # should show v24.x.x
npm -v

Install Yarn globally
sudo npm install -g yarn
yarn -v  # should show 1.22.x

Step 8: Install PDF Rendering Tools
sudo apt install -y xvfb libfontconfig wkhtmltopdf

Step 9: Install Bench via pipx
python3.14 -m pip install --user pipx
python3.14 -m pipx ensurepath
source ~/.bashrc
pipx install frappe-bench

Step 10: Initialize Frappe Bench
Use Python 3.14 and Frappe v16 RC:

bench init frappe-bench \
  --frappe-branch v16.0.0-rc.1 \
  --python python3.14

This resolves Python & Node/Yarn dependencies correctly.

Step 11: Create a New Site
cd frappe-bench
bench new-site yoursite_name

Enter MySQL root user: root (or your MySQL user)

MySQL root password: your MySQL password

Set Frappe Administrator password when prompted

Step 12: Get ERPNext & HRMS Apps (v16 RC)
ERPNext:
bench get-app erpnext --branch v16.0.0-rc.1

HRMS:
bench get-app hrms --branch v16.0.0-rc.1

Step 13: Install Apps on Your Site
bench --site yoursite_name install-app erpnext
bench --site yoursite_name install-app hrms

Set current site:

bench use yoursite_name

Step 14: Start Development Server
bench start

Open your browser:

http://127.0.0.1:8000
