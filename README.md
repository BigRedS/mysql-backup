# mysql-backup

A `mysqldump`ing backup script.

this is meant to be cronned and used to produce a series of copies of the db
to be kept as backups.

On modern Debians, stick something like this in your root crontab:

    0 1 * * *  mysql-backup --label daily --max-days 7
    0 3 1 * *  mysql-backup --label monthly --max-days 366

--username, --password and --hostname can be used to set credentials.
If a ~/.my.cnf file exists that will be used as a defaults file, unless
--no-defaults-file is passed.

That will keep about seven days' worth of daily backups and a year's worth of
monthly ones, stored something like this:

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

Each of these is a tarball containing a directory for each database, each of
which contains a gzipped sql file for each table.

See the --help output (or `./help.txt`) for an explanation of what the labels
mean.

Logfiles are written so as to be self-rotating; the above backups will have
produced these logfiles:

    /var/log/mysql-backup/jan-monthly
    /var/log/mysql-backup/feb-monthly
    /var/log/mysql-backup/mar-monthly
    /var/log/mysql-backup/apr-monthly
    ...
    /var/log/mysql-backup/sat-daily
    /var/log/mysql-backup/sun-daily
    /var/log/mysql-backup/mon-daily
    /var/log/mysql-backup/tue-daily
    /var/log/mysql-backup/wed-daily

so you'll only ever have 12 of the monthly ones, and seven of the dailies, and
can always inspect the last file to get the status of the last backup (see my
`check_log` project for how I do that in icinga).
