# Personal Documentation
## Linux / Servers / MineCraft

### Index
- [Prerequisites](#Prerequisites)
- [Installing Java Runtime Environment](#Installing_Java_Runtime_Environment)
- [Creating Minecraft User](#Creating_Minecraft_User)
- [Installing Minecraft on Ubuntu](#Installing_Minecraft_on_Ubuntu)
- [Downloading Minecraft Server](#Downloading_Minecraft_Server)
- [Creating Systemd Unit File](#Creating_Systemd_Unit_File)
- [Adjusting Firewall](#Adjusting_Firewall)
- [Configuring Backups](#Configuring_Backups)
- [Accessing Minecraft Console](#Accessing_Minecraft_Console)
- [Conclusion](#Conclusion)

### Prerequisites
The user you are logged in as must have sudo privileges to be able to install packages.

Install the packages required to build the mcrcon tool:
```terminal
sudo apt update
sudo apt install git build-essential
```
### Installing Java Runtime Environment
Minecraft requires Java 8 or greater. Because the Minecraft Server doesn’t need a graphical user interface, we’ll install the headless version of the JRE. This version is more suitable for server applications since it has fewer dependencies and uses less system resources.

Install the headless OpenJRE 8 package by running:
```terminal
sudo apt install openjdk-8-jre-headless
```
Verify the installation by printing the java version:
```terminal
java -version
```
Output:
```terminal
openjdk version "1.8.0_212"
OpenJDK Runtime Environment (build 1.8.0_212-8u212-b03-0ubuntu1.18.04.1-b03)
OpenJDK 64-Bit Server VM (build 25.212-b03, mixed mode)
```

### Creating Minecraft User
For security purposes, Minecraft should not be run under the root user. We will [create a new system user](https://linuxize.com/post/how-to-create-users-in-linux-using-the-useradd-command/) and group with home directory /opt/minecraft that will run the Minecraft server:
```terminal
sudo useradd -r -m -U -d /opt/minecraft -s /bin/bash minecraft
```
We are not going to set a password for this user. This is good security practice because this user will not be able to login via SSH. To change to the `minecraft` user you’ll need to be logged in to the server as root or user with sudo privileges.
### Installing Minecraft on Ubuntu
Before starting with the installation process, make sure you switch to `minecraft` user.
```terminal
sudo su - minecraft
```
Run the following command to create three new directories inside the user home directory:
```terminal
mkdir -p ~/{backups,tools,server}
```
- The backups directory will store your server backup. You can later synchronize this directory to your remote backup server.
- The tools directory will store the mcrcon client and the backup script.
- The server directory will contain the actual Minecraft server and its data

#### Downloading and Compiling mcrcon
RCON is a protocol that allows you to connect to the Minecraft servers and execute commands. [mcron](https://github.com/tiiffi/mcrcon) is RCON client built in C.

We’ll download the source code from GitHub and build the mcrcon binary.

Start by navigating to the ~/tools directory and clone the Tiiffi/mcrcon repository from GitHub using the following command:
```terminal
cd ~/tools && git clone https://github.com/Tiiffi/mcrcon.git
```
When the cloning is finished, switch to the repository directory:
```terminal
cd ~/tools/mcrcon
```
Start the compilation of the mcrcon utility by typing:
```terminal
gcc -std=gnu11 -pedantic -Wall -Wextra -O2 -s -o mcrcon mcrcon.c
```
Once completed, you can test it by typing:
```terminal
./mcrcon -h
```
The output will look something like this:

```terminal
Usage: mcrcon [OPTIONS]... [COMMANDS]...
Sends rcon commands to Minecraft server.

Option:
  -h		Print usage
  -H		Server address
  -P		Port (default is 25575)
  -p		Rcon password
  -t		Interactive terminal mode
  -s		Silent mode (do not print received packets)
  -c		Disable colors
  -r		Output raw packets (debugging and custom handling)
  -v		Output version information

Server address, port and password can be set using following environment variables:
  MCRCON_HOST
  MCRCON_PORT
  MCRCON_PASS

Command-line options will override environment variables.
Rcon commands with arguments must be enclosed in quotes.

Example:
	mcrcon -H my.minecraft.server -p password "say Server is restarting!" save-all stop

mcrcon 0.6.1 (built: May 19 2019 23:39:16)
Report bugs to tiiffi_at_gmail_dot_com or https://github.com/Tiiffi/mcrcon/issues/
```

### Downloading Minecraft Server
There are several Minecraft server mods such as [Craftbukkit](https://getbukkit.org/download/craftbukkit) or [Spigot](https://www.spigotmc.org/) that allows you to add features (plugins) on your server and further customize and tweak the server settings. In this guide, we will install the latest Mojang’s official vanilla Minecraft server.

The latest Minecraft server’s Java archive file (JAR) is available for download from the [Minecraft download page](https://minecraft.net/en-us/download/server/).

At the time of writing, the latest version is 1.14.1. Before continuing with the next step you should check the download page for a new version.

Run the following [wget](https://linuxize.com/post/wget-command-examples/) command to download the Minecraft jar file in the ~/server directory:
```terminal
wget https://launcher.mojang.com/v1/objects/ed76d597a44c5266be2a7fcd77a8270f1f0bc118/server.jar -P ~/server
```

#### Configuring Minecraft Server
Once the download is completed, [navigate](https://linuxize.com/post/linux-cd-command/) to the `~/server` directory and start the Minecraft server:
```terminal
cd ~/server
java -Xmx1024M -Xms512M -jar server.jar nogui
```
When you start the server for the first time it executes some operations and creates the `server.properties` and `eula.txt` files and stops.
Output:
```terminal
[23:41:44] [main/ERROR]: Failed to load properties from file: server.properties
[23:41:45] [main/WARN]: Failed to load eula.txt
[23:41:45] [main/INFO]: You need to agree to the EULA in order to run the server. Go to eula.txt for more info.
```
As you can see from the output above we need to agree to the Minecraft EULA in order to run the server. Open the `eula.txt` file and change `eula=false` to `eula=true`:
```terminal
nano ~/server/eula.txt
```

~/server/eula.txt:
```terminal
#By changing the setting below to TRUE you are indicating your agreement to our EULA (https://account.mojang.com/documents/minecraft_eula).
#Sun May 19 23:41:45 PDT 2019
eula=true
```
Close and save the file.

Next, we need to edit the server.properties file to enable the rcon protocol and set the rcon password. Open the file using your text editor:
```terminal
nano ~/server/server.properties
```
Locate the following lines and update their values as shown below:
```terminal
rcon.port=25575
rcon.password=strong-password
enable-rcon=true
```
Do not forget to change the strong-password to something more secure. If you don’t want to connect to the Minecraft server from remote locations make sure the rcon port is blocked by your firewall.

While here, you can also adjust the server’s default properties. For more information about the possible settings visit the [server.properties](https://minecraft.gamepedia.com/Server.properties) page
### Creating Systemd Unit File
To run Minecraft as a service we will create a new Systemd unit file.

Switch back to your sudo user by typing `exit`.

Open your text editor and create a file named `minecraft.servic`e in the `/etc/systemd/system/`:
```terminal
sudo nano /etc/systemd/system/minecraft.service
```
Paste the following configuration:

```terminal
[Unit]
Description=Minecraft Server
After=network.target

[Service]
User=minecraft
Nice=1
KillMode=none
SuccessExitStatus=0 1
ProtectHome=true
ProtectSystem=full
PrivateDevices=true
NoNewPrivileges=true
WorkingDirectory=/opt/minecraft/server
ExecStart=/usr/bin/java -Xmx1024M -Xms512M -jar server.jar nogui
ExecStop=/opt/minecraft/tools/mcrcon/mcrcon -H 127.0.0.1 -P 25575 -p strong-password stop

[Install]
WantedBy=multi-user.target
```
Modify the `Xmx` and `Xms` flags according to your server resources. The `Xmx` flag defines the maximum memory allocation pool for a Java virtual machine (JVM), while `Xms` defines the initial memory allocation pool. Also, make sure that you are using the correct `rcon` port and password.

Save and close the file and reload the systemd manager configuration:
```terminal
sudo systemctl daemon-reload
```
Now you can start the Minecraft server by executing:
```terminal
sudo systemctl start minecraft
```
The first time you start the service it will generate several configuration files and directories including the Minecraft world.

Check the service status with the following command:
```terminal
sudo systemctl status minecraft
```
Output:
```terminal
* minecraft.service - Minecraft Server
   Loaded: loaded (/etc/systemd/system/minecraft.service; disabled; vendor preset: enabled)
   Active: active (running) since Sun 2019-05-19 23:49:18 PDT; 9min ago
 Main PID: 11262 (java)
    Tasks: 19 (limit: 2319)
   CGroup: /system.slice/minecraft.service
           `-11262 /usr/bin/java -Xmx1024M -Xms512M -jar server.jar nogui
```
Finally, enable the Minecraft service to be automatically started at boot time:
```terminal
sudo systemctl enable minecraft
```

### Adjusting Firewall
If your server is [protected by a firewall](https://linuxize.com/post/how-to-setup-a-firewall-with-ufw-on-ubuntu-18-04/) and you want to access Minecraft server from the outside of your local network you need to open port 25565.

To allow traffic on the default Minecraft port 25565 type the following command:
```terminal
sudo ufw allow 25565/tcp
```

### Configuring Backups
In this section, we’ll create a backup shell script and cronjob to automatically backup the Minecraft server.

Start by [switching to user](https://linuxize.com/post/su-command-in-linux/) minecraft:
```terminal
sudo su - minecraft
```
Open your text editor and create the following file:
```terminal
nano /opt/minecraft/tools/backup.sh
```
Paste the following configuration:
```terminal
#!/bin/bash

function rcon {
  /opt/minecraft/tools/mcrcon/mcrcon -H 127.0.0.1 -P 25575 -p strong-password "$1"
}

rcon "save-off"
rcon "save-all"
tar -cvpzf /opt/minecraft/backups/server-$(date +%F-%H-%M).tar.gz /opt/minecraft/server
rcon "save-on"

## Delete older backups
find /opt/minecraft/backups/ -type f -mtime +7 -name '*.gz' -delete
```
Save the file and make the script executable by running the following [chmod](https://linuxize.com/post/chmod-command-in-linux/) command:
```terminal
chmod +x /opt/minecraft/tools/backup.sh
```
Next, [create a cron job](https://linuxize.com/post/scheduling-cron-jobs-with-crontab/) that will run once in a day automatically at a fixed time.

Open the crontab file by typing:
```terminal
crontab -e
```
To run the backup script every day at 23:00 paste the following line:
```terminal
0 23 * * * /opt/minecraft/tools/backup.sh
```

### Accessing Minecraft Console
To access the Minecraft Console you can use the `mcrcon` utility. The syntax is as follows, you need to specify the host, rcon port, rcon password and use the `-t` switch which enables the mcrcon terminal mode:

```terminal
/opt/minecraft/tools/mcrcon/mcrcon -H 127.0.0.1 -P 25575 -p strong-password -t
```
current in use:
```terminal
/opt/minecraft/tools/mcrcon/mcrcon -H 192.168.0.100 -P 25575 -p 9ZnJv8a97zNFRZSN -t
```
Output:
```terminal
Logged in. Type "Q" to quit!
> 
```
When accessing the Minecraft Console from a remote location make sure the rcon port is not blocked.

If you are regularly connecting to the Minecraft console, instead of typing this long command you should create a [bash alias](https://linuxize.com/post/how-to-create-bash-aliases/).

### Conclusion 
You have successfully installed Minecraft server on your Ubuntu system and set up a daily backup.


### Resources 
- [linuxize](https://linuxize.com/post/how-to-install-minecraft-server-on-ubuntu-18-04/)