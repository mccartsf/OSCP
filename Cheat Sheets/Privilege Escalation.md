
## SUID 

Find all available SUID files in the system

	find / -type f -perm -4000 2>/dev/null

Another Ex.

	find / -perm -u=s -type f 2>/dev/null

Flag Descriptions:

- -perm: define the permissions to search for
- -u=s: search for files with the SUID permission
- -type f: search for regular file
- 2>dev/null: errors will be deleted automatically


## Cron Jobs

Find all available running cron jobs

	cat /etc/cron





