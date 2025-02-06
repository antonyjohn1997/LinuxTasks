### How to execute regularly with Linux [CRONTAB] 

1. Create the script
   Open a terminal and run:

   * nano /home/antony/backup.sh
   
   
2. Paste the script inside nano.
   
     #!/bin/bash

     # Create a backup directory if it doesn't exist
     mkdir -p /home/antony/backup

     # Copy files to the backup folder with a timestamp
     cp -r /home/antony /home/antony/backup/home_backup_$(date +\%Y\%m\%d\%H\%M\%S)

     # Print a success message
     echo "Backup completed at $(date)"


3. Save and exit: Press CTRL + X, then Y, then Enter.
4. Give execution permission:   
   * chmod +x /home/antony/backup.sh
5. Schedule it with cron (so it runs automatically):
   * crontab -e
    ## Understand the Structure of Cron Jobs

      Each line in the crontab file represents a single cron job. The structure is as follows:

       * * * * * /path/to/command

       Each * represents a time field (minute, hour, day, month, day of the week). The /path/to/ command is the command or script you want to run.

Here’s what each field means:

    Minute (0-59): The minute when the task will run.
    Hour (0-23): The hour when the task will run.
    Day of the Month (1-31): The day of the month when the task will run.
    Month (1-12): The month when the task will run.
    Day of the Week (0-7): The day of the week when the task will run (0 or 7 is Sunday, 1 is Monday, etc.).


    */5 * * * * /home/antony/backup.sh

    Run the script every 5 minutes

6. Verify the Cron Jobs

Once saved, you can verify that the cron jobs were successfully added. Run:

  * crontab -l

This command will list all the cron jobs scheduled for your user.

7.  Check Cron Job Logs

If a cron job isn’t running as expected, check the logs to troubleshoot:

    To view the logs for cron jobs:

  * journalctl -u cron --no-pager | tail -50

This will show the last 50 lines of logs related to cron.
8. Remove or Comment Out the Cron Job
    To edit the cron jobs:
      crontab -e
    Find the specific cron job and:
      Delete the line to remove it permanently.
      Comment it out by adding # at the beginning.  


### op:

* pwd
/home/antony
* ls
backup     Desktop    Downloads  Music     Public          snap       ubuntu
backup.sh  Documents  lfcs       Pictures  shell_programs  Templates  Videos
* cd backup
* ls
home_backup_20250131122501  home_backup_20250131151002
home_backup_20250131123001  home_backup_20250131151502
home_backup_20250131123501  home_backup_20250131152001
home_backup_20250131124001  home_backup_20250131172501
home_backup_20250131124501  home_backup_20250131173001
home_backup_20250131125001  home_backup_20250131174501
home_backup_20250131125501
* cd home_backup_20250131125501
* ls
backup     Desktop    Downloads  Pictures  snap       Videos
backup.sh  Documents  Music      Public    Templates






