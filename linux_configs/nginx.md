# Nginx

## pass_proxy

## SSL with certbot

### 1. add certbot ppa and update repos:

```bash
sudo apt-add-repository ppa:certbot/certbot
sudo apt update
```

### 2. install nginx and certbot:

```bash
sudo apt install nginx python-certbot-nginx
```

### 3. set `server_name` in nginx default site at `/etc/nginx/sites-available/default`

```nginx
[...]

server_name myhostname.com

[...]
```

### 4. reload nginx

```bash
# check syntax
sudo nginx -t
# reload nginx systemd service
sudo systemctl reload nginx
```
### 5. configure firewall

```bash
sudo ufw allow 'Nginx Full'
sudo ufw allow 'Nginx HTTP'
```
to verify that ports are configured the you can run:
```bash
sudo ufw status
```

### 6. obtain SSL-Cert

```bash
sudo certbot --nginx -d myhostname.com
```

after running that command you have
 1. to enter an valid email adress
 2. choose whether all traffic should be redirectedvia HTTPS [2] or not [1]

### 7. renewing the certificate

Let's Encrypt certs are only valid for 90 days. To renew it you just have to run the following and should be placed in a cron task:

```bash
sudo cerbot renew
```

To test the renewal process you can run
```bash
sudo certbot renew --dry-run
```
