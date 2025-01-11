## DayZ Server Manager for Linux
### An alternative to the Linux version of Omega Manager
Custom script for managing your DayZ Server on Linux <br/>
Currently being used without issue on `UNIT487` DayZ Server.<br/><br/>

### WARNING
Use this [script](https://github.com/neorule/StandAlone_DayZ_Linux_Server_Script/blob/main/dayzserver.sh) at your own risk.<br/>
You are free to copy or fork the script and edit to your liking.<br/>
All I ask is that you don't remove the credits at the top of the main script. Thank you.

### Main Features
- Takes care of downloading SteamCMD and DayZ Server.
- Updates the DayZ Server and Mods at each startup/restart.
- Backs up your Profile and Mission folders at each startup/restart.
- Auto Start, Stop and Restarts are done with the help of Crontab.
- Simple to set up.

## First RUN
1. Download [dayzserver.sh](https://github.com/neorule/StandAlone_DayZ_Linux_Server_Script/blob/main/dayzserver.sh) to the root of your DayZ server home drive.
2. Then: `chmod +x dayzserver.sh`
3. Run the script: `./dayzserver.sh`
4. Wait until it has finished, then follow the instructions.
5. Edit the **.config.ini** file with your preferences.
   - Main things to change here are **steamlogin** and **port** for now.
   - This is also where you add your @modNames to the launch paramaters. 
7. Add your Mod ID's to the **.workshop.cfg** file.
   - One Mod ID per line. 
   - Don't worry about adding the mod name. The script will do that later.
   - If you must though. Leave one space between the ID and the name `123456 ModName`
8. Edit the `serverfiles/battleye/beserver_x64*.cfg`
   - Change the RCon password and port number, or leave as default.
   - Make sure the RCon and the Game Server ports aren't the same.
   - Defaults are usualy: Rcon 2305 and Gameport 2302.
   - Game port is changed in the **.config.ini**.
9. In your `serverfiles/serverDZ.cfg` file change your hostname and other settings.
   - Make sure there is a **steamQueryPort** setting. i.e `steamQueryPort = 27016;`
10. Run the script again and your server will be online within minutes.

## .workshop.cfg 
**Editing the `.workshop.cfg` file is real easy.**<br/>
- Add one workshop ID per line.<br/>
- The script will automatically check and append mod names each time you run: `./dayzserver.sh workshop`<br/>
- Optional Mod update notifications can be sent to your Discord channel. <br/>
   - Just add your Discord URL in `.config.ini`
   - Mod names and timestamps are stored in `mod_timestamps.json` (don't touch this file).
- You still need to manually add the **@modname;@modnametwo** to the `workshop=""` setting in **.config.ini**
   - I've made this a manual process because certain mods need to be loaded in a specific order.
   - Please remember to use lowercase. The script will convert all mod folder names to lowercase for you.
- **.workshop.cfg** tells the script which MODS need to be downloaded and checked for updates.

## Auto Restarts and Updates
The following cron jobs can be added to your DayZ Server users contab:<br/>
```
@reboot /home/dayz/dayzserver.sh start > /dev/null 2>&1
*/1 * * * * /home/dayz/dayzserver.sh monitor > /dev/null 2>&1
# */30 * * * * /home/dayz/dayzserver backup > /dev/null 2>&1
```
- Line 1 simply starts the server when the Linux machine is rebooted or turned on.
- Line 2 checks to see if the server is crashed or has been shutdown remotely, then restarts it.
- Line 3 (OPTIONAL) makes regular backups of your **storage** and **profile** folders.
   - By default the script backs up your folders during server startup/restart. 

For server restarts, I recommend using the `messages.xml` file located in your missions DB folder.<br/>
You can also use a service like CFTools to run server restarts (they have a free tier).<br/>
**Your preferred "restart server schedular" shuts down the server and** `monitor` **cron job will start the server back up**.<br/><br/>

**IMPORTANT NOTE**<br/> `monitor` will only try to restart the server if a **RCON** or **messages.xml** shutdown command was used or the server crashed. Typing `./dayzserver stop` will shutdown the server and prevent `monitor` from restarting it. 
