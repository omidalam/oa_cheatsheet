To generate an SSH key
ssh-keygen -t rsa -b 4096 -C "USEFUL-COMMENT e.g. mac-server"

Adding SSH key to ssh-agent

1. Make sure ssh-agent is enabled:
``` bash
eval "$(ssh-agent -s)"
```
2. add you key.
``` bash
ssh-add -K LINK_TO_SSH_PRIVATE_KEY e.g.:  ~/.ssh/id_rsa
```

You'll be asked to provide your pasword for the private_key if you've setted one
_Note:_ If you’re using a passphrase AND on macOS Sierra 10.12.2 and higher, you need to do one more thing. 
Create a file ~/.ssh/config with these contents:

``` bash
Host *
    AddKeysToAgent yes
    UseKeychain yes
```





