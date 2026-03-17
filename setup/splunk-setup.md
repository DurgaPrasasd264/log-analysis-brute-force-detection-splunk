# Splunk Setup (Ubuntu)

## Install Splunk
wget -O splunk.tgz https://download.splunk.com/products/splunk/releases/9.x/linux/splunk.tgz
tar -xvzf splunk.tgz
cd splunk/bin
sudo ./splunk start --accept-license

## Enable Receiving Port
Settings → Forwarding and Receiving → Add Port → 9997
