Asterisk 101: A Beginner's Guide
Part 1: Setting Up Asterisk
1. Installing Asterisk

On a Linux-based system like Ubuntu, you can install Asterisk using the package manager:

sudo apt-get update
sudo apt-get install asterisk

2. Initializing and Starting Asterisk

To start the Asterisk service:

sudo systemctl start asterisk


And to make sure it starts on boot:

sudo systemctl enable asterisk

3. Accessing the Asterisk Console

Connect to the Asterisk CLI with verbose output to see what's going on:

sudo asterisk -rvvv

4. Adding a Simple Dial Plan

Edit your dial plan in /etc/asterisk/extensions.conf and add the following under the [default] context:

[default]
exten => 100,1,Answer()
exten => 100,2,Playback(demo-congrats)
exten => 100,3,Hangup()

5. Updating and Reloading the Dial Plan

After saving your changes, reload the dial plan from the Asterisk console:

dialplan reload

6. Checking Asterisk Status

To check if Asterisk is running:

sudo systemctl status asterisk


Or inside the Asterisk console:

core show uptime

Part 2: Connecting Zoiper to Asterisk
1. Setting Up a SIP Extension

In sip.conf (or pjsip.conf), add a SIP user:

[100]
type=friend
secret=mysecretpassword
host=dynamic
context=default

2. Configuring Zoiper

Username: Use the extension number (e.g., 100)

Password: The password you set (e.g., mysecretpassword)

Domain/Host: The IP address of your Asterisk server (e.g., 192.168.1.10)

Save the settings, and once Zoiper registers, you can dial 100 to test your connection.