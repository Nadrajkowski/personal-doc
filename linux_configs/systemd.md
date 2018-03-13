# systemd

## docker-compse service

### Configuration

In this example the service will be called **my-docker-compose** resulting in the file `/etc/systemd/system/my-docker-compose.service`

```
[Unit]
Description=My cool docker-compose service
After=docker.service
Requires=docker.service
User=user

[Service]
ExecStartPre=/usr/bin/docker-compose -f /home/user/api/docker-compose.yml down
ExecStart=/usr/bin/docker-compose -f /home/user/api/docker-compose.yml up

ExecStop=/usr/bin/docker-compose -f /home/user/api/docker-compose.yml stop

ExecReload=/usr/bin/docker-compose -f /home/user/api/docker-compose.yml down
ExecReload=/usr/bin/docker-compose -f /home/user/api/docker-compose.yml up

[Install]
WantedBy=multi-user.target
```

Replace **user** in both `User=user` and all paths (because my `docker-compose.yml` is placed in the users home folder) to the user, you want to run this service. 



It's importent to note that your path to the docker-compse binary may vary. I tested this exect on **Ubuntu 17.10.1** while installing **docker-compose** with by executing `sudo apt install docker-compose`. This resultes in my case in the path `/usr/bin/docker-compose` but I've also seen `/usr/local/bin/docker-compose` so be sure to check that.

**ATTENTION: Do not use the `-d` flag to daemonize the `docker-compose` execution. This will result in systemd stopping the service.**

### Starting the service

After all is configured all you have to do is `enable` and `start` the service:

```bash
sudo systemctl enable my-docker-compose.service;
sudo systemctl start my-docker-compose.service;
```

Then check if the service is running by executing:

```bash 
sudo systemctl status my-docker-compose.service;
```

### Additional notes
 If you want to, you can remove not used containers on startup by adding another `ExecPreStart=<command>` but if you named your containers in the `docker-compose.yml`, then there should only be the ones you started.