# GFX-P


## Installation gfxp


#!/bin/bash

# ============================================
# Full Python + LXD + PostgreSQL Setup Script
# Ubuntu/Debian VPS
# ============================================

clear

echo "======================================="
echo " Updating System"
echo "======================================="

apt update && apt upgrade -y

echo "======================================="
echo " Installing Basic Packages"
echo "======================================="

apt install -y \
python3 \
python3-pip \
python3-venv \
git \
curl \
wget \
unzip \
build-essential \
software-properties-common \
ca-certificates \
gnupg \
lsb-release

echo "======================================="
echo " Installing Snapd"
echo "======================================="

apt install -y snapd

systemctl enable snapd
systemctl start snapd

ln -s /var/lib/snapd/snap /snap 2>/dev/null

echo "======================================="
echo " Installing LXD"
echo "======================================="

snap install lxd

echo "======================================="
echo " Initializing LXD"
echo "======================================="

lxd waitready
lxd init --auto

echo "======================================="
echo " Installing PostgreSQL"
echo "======================================="

apt install -y postgresql postgresql-contrib

systemctl enable postgresql
systemctl start postgresql

echo "======================================="
echo " Installing Python Requirements"
echo "======================================="

if [ -f requirements.txt ]; then
    pip3 install --break-system-packages -r requirements.txt
else
    echo "requirements.txt not found!"
fi

echo "======================================="
echo " Setup Complete!"
echo "======================================="
echo ""
echo "Installed:"
echo "✔ Python3"
echo "✔ Pip"
echo "✔ Virtualenv"
echo "✔ Snap"
echo "✔ LXD"
echo "✔ PostgreSQL"
echo ""
echo "LXD Status:"
lxc info >/dev/null 2>&1 && echo "✔ LXD Running"

echo ""
echo "PostgreSQL Status:"
systemctl is-active postgresql

echo ""
echo "DONE!"

## Run cmd 

python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python gfx.py


## Dong enjoy port list - 9000
