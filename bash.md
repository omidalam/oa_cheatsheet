Cheat sheet for bash:


_run a command after reboot_
Add the coomand to /etc/rc.local
Remember to add "#!/bin/sh -e" at the top. -e flag ensures that itquits if it fails to run any of the commands. This is important in case the 
functions are relying on each other.


