_Last Update: **May 2022**_

In this guide we are going to set up the backup routine on one of our Hetzner servers.

This guide assumes that you are using Plesk, and you have access via SHH to the backup server.

# Installing

Log into the Plesk admin of the server being backed up and go to:

    Extension -> Extensions Catalogue

Use the search function to find and install this extension (https://www.plesk.com/extensions/sshfs/):

    “SSHFS - Remote Storage” by D4FR

Once installed, go to:

    Extension -> My Extensions -> SSHFS - Remote Storage -> Open    

This should take you to the extension’s page.

    Click Open

You’ll see familiar looking fields asking for connection credentials, use the backup server’s SFTP/SHH account to connect, and enter the directory on the remove server you want the backup files to go.. 

    Hit save

    Hit enable

If there are any issues connecting the extension will give you a warning, but if you have the green light, the remote server should now be mounted as a usable backup drive at “/mnt/sshfs/nd0/”.

We need to change the directory Plesk uses for backups, so log into the server that is being backed up via SSH and enter:

    nano /etc/psa/psa.conf

Find the section headed “# Backups directory” and set the following to use the new mounted remote drive to “/mnt/sshfs/nd0/”.:

    DUMP_D /mnt/sshfs/nd0

Save the file

    CRTL + O or CMD + O
    Hit enter to confirm

Exit the nano editor

    CRTL + X or CMD + X

Check to make sure that’s saved by entering the following:. 

    cat /etc/psa/psa.conf | grep -w DUMP_D

The output should show the new directory..

Now restart Plesk with the following: 

    service sw-cp-server restart

Go back in to the Plesk dashboard and go to: 

    Tools & Settings -> Tools & Resources -> Backup Manager

Run a backup to test the connection, then use an SFTP client to access the backup server to verify the backup files are going where you want them too.

Once you have run a manual backup and can see the files on the backup server, we’ll move on to setting up the schedule.

    Click On The Schedule Button

    Check “activate this backup task”

    Run between 3-6am daily

    Uncheck “Use incremental backup” 

    Check to backup Configuration

    Check to backup User Files and Databases

    Don’t exclude logs, and make sure the error email is set.

Once that’s filled in,

    Hit apply/ok


Come back the next morning, and periodically check to make sure all is good, and you’re done.
