
Find all available SUID files in the system

		find / -perm -u=s -type f 2>/dev/null

Flag Descriptions:

- -perm: define the permissions to search for
- -u=s: search for files with the SUID permission
- -type f: search for regular file
- 2>dev/null: errors will be deleted automatically

Another Example:

		find / -type f -perm -4000 2>/dev/null



