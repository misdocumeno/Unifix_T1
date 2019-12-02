# Unifix T1
This repository contains all the files for all the [Unifix T1 servers](https://steamcommunity.com/groups/UnifixServers).

***only adminis can unload T1 to call a vote for a match***

### Configs
- ZoneMod 1.9.5
- SkeetMod 0.0.3
- T1 V2

## Install Steps

### Installing Prerequisites
- A clean installation of **Ubuntu 18.04**
- A user with **sudo** privileges

### 1) Install libraries and git
- Login as root or any user with sudo privileges and put the script below in a file named `server_pre`
- You can use `nano server_pre` to do that
- Run it with `bash server_pre` **(`sudo bash server_pre` if you aren't logged in as root)**

```bash
# Install steam and L4D2
wget http://media.steampowered.com/installer/steamcmd_linux.tar.gz
tar -xvzf steamcmd_linux.tar.gz
./steamcmd.sh << STEAM
login anonymous
force_install_dir ./serverT1
app_update 222860 validate
quit
STEAM

# Server download
git clone https://github.com/misdocumeno/Unifix_T1
cp -r Unifix_T1/* serverT1/left4dead2/

# Screen script
printf 'screen -S UnifixT1 ./serverT1/srcds_run -tickrate 100 +map "c10m1_caves" +sv_clockcorrection_msecs 15 -timeout 10 +ip 0.0.0.0 -port 27016 +precache_all_survivors 1' > server_startT1
chmod u+x server_startT1

# Per server config backup
# Edit these files instead of the server ones
# This will be copied to the server directory when update
mkdir -p serverconfigsT1/cfg/
cp Unifix_T1/cfg/server.cfg serverconfigsT1/cfg/
mkdir -p serverconfigsT1/cfg/sourcemod/
cp Unifix_T1/cfg/sourcemod/vpn_ip.cfg serverconfigsT1/cfg/sourcemod/vpn_ip.cfg
mkdir -p serverconfigsT1/addons/sourcemod/configs/sourcebans/
cp Unifix_T1/addons/sourcemod/configs/sourcebans/sourcebans.cfg serverconfigsT1/addons/sourcemod/configs/sourcebans/
cp Unifix_T1/addons/sourcemod/configs/databases.cfg serverconfigsT1/addons/sourcemod/configs/

# Update script
printf '%s\n' \
'killall screen' \
'cd Unifix_T1' \
'git pull' \
'rm -rf ../serverT1/left4dead2/addons/sourcemod/' \
'rm -rf ../serverT1/left4dead2/cfg/sourcemod/' \
'rm -rf ../serverT1/left4dead2/cfg/cfgogl/' \
'rm -rf ../serverT1/left4dead2/cfg/stripper/' \
'rm ../serverT1/left4dead2/cfg/generalfixes.cfg' \
'rm ../serverT1/left4dead2/cfg/sharedplugins.cfg' \
'rm ../serverT1/left4dead2/cfg/confogl_personalize.cfg' \
'rm ../serverT1/left4dead2/cfg/server.cfg' \
'rm ../serverT1/left4dead2/host.txt' \
'rm ../serverT1/left4dead2/motd.txt' \
'cp -r * ../serverT1/left4dead2/' \
'git reset HEAD --hard' \
'cd ..' \
'cd serverconfigsT1' \
'rm ../serverT1/left4dead2/cfg/server.cfg' \
'rm ../serverT1/left4dead2/cfg/sourcemod/vpn_ip.cfg' \
'rm ../serverT1/left4dead2/cfg/sourcemod/configs/databases.cfg' \
'rm ../serverT1/left4dead2/cfg/sourcemod/configs/sourcebans/sourcebans.cfg' \
'cp -r * ../serverT1/left4dead2/' \
'rm ../server_installT1' \
'rm ../server_pre' \
> server_updateT1
chmod u+x server_updateT1
```

### 3) Edit Configuration Files
- Edit `serverconfigsT1/cfg/server.cfg` and set a proper hostname, rcon password and steam groups id
- Edit `serverconfigsT1/addons/sourcemod/configs/databeses.cfg` and set the mysql password
- Edit `serverconfigsT1/addons/sourcemod/configs/sourcebans/sourcebans.cfg` and set the sourcebans ServerID
- Run the update script to copy those files into the server path

### 4) Start the Server
- Run the server with `bash server_startT1`

### 5) Update
- Update server files when necessary using `bash server_updateT1`

