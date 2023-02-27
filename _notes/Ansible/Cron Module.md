[[Index]] 



```yaml

  - name: Run daily DB backup script at 00:00
    ansible.builtin.cron:
      name: "Run daily DB backup script at 00:00"
      minute: "0"
      hour: "0"
      job: "/usr/local/bin/db_backup_script.sh > /var/log/db_backup_script.sh.log 2>&1"


```

## Options for the ansible.builtin.cron Module

name - The comment identifying the Cron job.

job  - The command to run.

minute, hour, day, month,
weekday - The value for the field in the time specification for the job in
the crontab entry. If not set, "*" (all values) is assumed.

state - If set to present, it creates the Cron job (the default);
absent removes it.

user -  The Cron job runs as this user. If cron_file is not specified,
the job is set in that user's crontab file.

cron_file - If set, create a system Cron job in cron_file. You must
specify user and a time specification. If you use a relative
path, then the file is created in /etc/cron.d.