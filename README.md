# Frappe v16 Installation Guide (Ubuntu 25)

A step-by-step guide to installing Frappe Framework v16 (RC.1), ERPNext, and HRMS on Ubuntu 25, ensuring Python 3.14 and Node.js 24 compatibility.

## Step 1: Update System

Update your package lists and upgrade existing packages.

```bash
sudo apt update -y
sudo apt upgrade -y



Step 2: Create a Frappe User
It is recommended not to run Bench as root. Create a dedicated user.
