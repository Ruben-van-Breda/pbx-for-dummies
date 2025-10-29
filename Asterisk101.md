# Asterisk 101: A Beginner's Guide

## Part 1: Setting Up Asterisk

### 1. Installing Asterisk

On a Linux-based system like Ubuntu, you can install Asterisk using the package manager:

```bash
sudo apt-get update
sudo apt-get install asterisk
```

### 2. Initializing and Starting Asterisk

To start the Asterisk service:

```bash
sudo systemctl start asterisk
```

And to make sure it starts on boot:

```bash
sudo systemctl enable asterisk
```

### 3. Accessing the Asterisk Console

Connect to the Asterisk CLI with verbose output to see what's going on:

```bash
sudo asterisk -rvvv
```

### 4. Adding a Simple Dialplan

Edit your dialplan in `/etc/asterisk/extensions.conf` and add the following under the `default` context:

```ini
[default]
exten => 100,1,Answer()
exten => 100,2,Playback(demo-congrats)
exten => 100,3,Hangup()
```

### 5. Updating and Reloading the Dialplan

After saving your changes, reload the dialplan from the Asterisk console:

```asterisk
dialplan reload
```

### 6. Checking Asterisk Status

To check if Asterisk is running:

```bash
sudo systemctl status asterisk
```

Or inside the Asterisk console:

```asterisk
core show uptime
```

## Part 2: Connecting Zoiper to Asterisk

### 1. Setting Up a SIP Extension

In `sip.conf` (or `pjsip.conf`), add a SIP user:

```ini
[100]
type=friend
secret=mysecretpassword
host=dynamic
context=default
```

> Note: Modern Asterisk typically uses `pjsip.conf`. If you use PJSIP, configure a transport, endpoint, aor, and auth blocks accordingly.

### 2. Configuring Zoiper

- **Username**: Use the extension number (e.g., 100)
- **Password**: The password you set (e.g., `mysecretpassword`)
- **Domain/Host**: The IP address of your Asterisk server (e.g., `192.168.1.10`)

Save the settings, and once Zoiper registers, you can dial `100` to test your connection.