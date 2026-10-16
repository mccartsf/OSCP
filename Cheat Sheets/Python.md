
Spin up a Python Server

	python3 -m http.server 8000

Shell Upgrade (Once a foothold is established)

	python -c 'import pty; pty.spawn("/bin/bash")'

#### Python Environment :

Create a python virtual environment

	python3 -m venv .venv
	
Check which environment is running

	which python

Activate the Environment

	source .venv/bin/activate

Deactivate the environment 

	deactivate

