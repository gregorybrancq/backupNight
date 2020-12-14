# backupNight

## Info
Launch the backup during the night when computer is not used.

Once you defined an hour to wake up computer :
 - the program creates a specific file to indicate it's running
 - it shutdowns screens to reduce power consuming
 - it computes backups (with the good Rsnapshot tool)
 - it sends status by email
 
Differents options are available :
 - --debug : print debug information
 - --enable/disable : enable/disable the program for one night
 - --backup_now : compute backup immediately

## Run

2 choices :
 - after wake up
 - with crontab

### after wake up
The program backupNightWakeUp.sh is used to launch it after wake up.

### crontab
Set in cron :
 - launch it before computerLock sets the computer in sleep mode :\
    4   3   *   *   *    export DISPLAY=:0.0 && /home/greg/Config/env/bin/backupNight
 - if the program fails to compute the next wake up, add also this command to be sure :\
    0   12  *   *   *    export DISPLAY=:0.0 && echo 0 > /sys/class/rtc/rtc0/wakealarm && date -u --date "Tomorrow 2:00:00" +%s  > /sys/class/rtc/rtc0/wakealarm
 
As a reminder, before the backup was made with this cron :

 Backup Home
 - 30 12   *   *   *    nice /usr/bin/rsnapshot -c /home/greg/Config/tools/rsnapshot/rsnapshot_home.conf daily 2>&1 | /usr/local/bin/rsnapreport.pl | mail -s "Rsnapshot Home : daily"  gregory.brancq@free.fr
 - 20 12   *   *   1    nice /usr/bin/rsnapshot -c /home/greg/Config/tools/rsnapshot/rsnapshot_home.conf weekly  2>&1 | /usr/local/bin/rsnapreport.pl | mail -s "Rsnapshot Home : weekly" gregory.brancq@free.fr
 - 10 12  1-7  *   *    [ "$(date '+\%u')" -eq 1 ] && nice /usr/bin/rsnapshot -c /home/greg/Config/tools/rsnapshot/rsnapshot_home.conf monthly 2>&1 | /usr/local/bin/rsnapreport.pl | mail -s "Rsnapshot Home : monthly" gregory.brancq@free.fr
 - 00 12  1-7 1,4,8,12  *    [ "$(date '+\%u')" -eq 1 ] && nice /usr/bin/rsnapshot -c /home/greg/Config/tools/rsnapshot/rsnapshot_home.conf yearly 2>&1 | /usr/local/bin/rsnapreport.pl | mail -s "Rsnapshot Home : yearly" gregory.brancq@free.fr

 Backup Vps
 - 30 18   *   *   *    nice /usr/bin/rsnapshot -c /home/greg/Config/tools/rsnapshot/rsnapshot_vps.conf daily 2>&1 | /usr/local/bin/rsnapreport.pl | mail -s "Rsnapshot Vps : daily"  gregory.brancq@free.fr
 - 20 18   *   *   1    nice /usr/bin/rsnapshot -c /home/greg/Config/tools/rsnapshot/rsnapshot_vps.conf weekly  2>&1 | /usr/local/bin/rsnapreport.pl | mail -s "Rsnapshot Vps : weekly" gregory.brancq@free.fr
 - 10 18  1-7  *   *    [ "$(date '+\%u')" -eq 1 ] && nice /usr/bin/rsnapshot -c /home/greg/Config/tools/rsnapshot/rsnapshot_vps.conf monthly 2>&1 | /usr/local/bin/rsnapreport.pl | mail -s "Rsnapshot Vps : monthly" gregory.brancq@free.fr
 - 00 18  1-7 1,4,8,12  *    [ "$(date '+\%u')" -eq 1 ] && nice /usr/bin/rsnapshot -c /home/greg/Config/tools/rsnapshot/rsnapshot_vps.conf yearly 2>&1 | /usr/local/bin/rsnapreport.pl | mail -s "Rsnapshot Vps : yearly" gregory.brancq@free.fr
