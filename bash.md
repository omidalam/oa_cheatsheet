Cheat sheet for bash:


_run a command after reboot_
Add the coomand to /etc/rc.local
Remember to add "#!/bin/sh -e" at the top. -e flag ensures that itquits if it fails to run any of the commands. This is important in case the 
functions are relying on each other.

_Run Sublime3 from terminal_
ln -s "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" /usr/local/bin/sublime
This will add a link in folder for sublime file.

_Selecting some columns_
awk 'BEGIN{OFS="\t"}{print $1,$2,$3,$NF}' h3k4me_clusters.bed > h3k4me3_cl_renamed.bed

$NF is the last column
OFS is the output field separator.




