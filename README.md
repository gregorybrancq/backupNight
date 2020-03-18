# backupNight
Backup everything during the night when computer is not used.

Once you defined an hour to wake up computer :
 - the program creates a specific file to indicate it's running
 - it shutdowns screens to reduce power consuming
 - it computes backups (with the good Rsnapshot tool)
 - it sends status by email
 
 Differents options are available :
 - --debug : print debug information
 - --enable/disable : enable/disable the program for one night
 - --backup_now : compute backup immediately
