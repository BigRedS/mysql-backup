# mysql-backup

A `mysqldump`ing backup script. 

this is meant to be cronned and used to produce a series of copies of the db 
to be kept as backups.

Stick something like this in your crontab:

    0 1 * * *  mysql-backup --defaults-file ~/backups.my.cnf --label daily --max-days 7
    0 3 1 * *  mysql-backup --defaults-file ~/backups.my.cnf --label monthly --max-days 300

to keep about seven days' worth of daily backups and a year's worth of monthly
ones, stored something like this:

    /home/mysql-backup/daily/2018-01-20_01:00:00_sat_mysql-backup.tar
    /home/mysql-backup/daily/2018-01-21_01:00:00_sun_mysql-backup.tar
    /home/mysql-backup/daily/2018-01-22_01:00:00_mon_mysql-backup.tar
    /home/mysql-backup/daily/2018-01-23_01:00:00_tue_mysql-backup.tar
    /home/mysql-backup/daily/2018-01-24_01:00:00_wed_mysql-backup.tar
    ...
    /home/mysql-backup/monthly/2018-01-01_03:00:00_mon_mysql-backup.tar
    /home/mysql-backup/monthly/2018-02-01_03:00:00_thu_mysql-backup.tar
    /home/mysql-backup/monthly/2018-03-01_03:00:00_thu_mysql-backup.tar
    /home/mysql-backup/monthly/2018-04-01_03:00:00_sun_mysql-backup.tar

Each of these is a tarball containing a directory for each database,each of 
which contains a gzipped sql file for each table. 

See the --help output (or `./help.txt`) for an explanation of what the labels 
mean.

Logfiles are written so as to be self-rotating; the above backups will have 
produced these logfiles:

    /var/log/mysql-mysqlbackup/jan-monthly
    /var/log/mysql-mysqlbackup/feb-monthly
    /var/log/mysql-mysqlbackup/mar-monthly
    /var/log/mysql-mysqlbackup/apr-monthly
    ...
    /var/log/mysql-mysqlbackup/sat-daily
    /var/log/mysql-mysqlbackup/sun-daily
    /var/log/mysql-mysqlbackup/mon-daily
    /var/log/mysql-mysqlbackup/tue-daily
    /var/log/mysql-mysqlbackup/wed-daily

so you'll only ever have 12 of the monthly ones, and seven of the dailies, and
can always inspect the last file to get the status of the last backup (see my 
`check_log` project for how I do that in icinga).
