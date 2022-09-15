# Nginx

### Installing Nginx

Import Nginx signing key:

```
curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor \
    | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null
```

Verify downloaded key:

```
gpg --dry-run --quiet --import --import-options import-show \
    /usr/share/keyrings/nginx-archive-keyring.gpg
```

Output should contain `573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62`.

Set up apt repository for stable Nginx packages:

```
echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
http://nginx.org/packages/ubuntu `lsb_release -cs` nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list
```

Set up repository pinning to prefer above packages over distribution-provided ones:

```bash
echo -e "Package: *\nPin: origin nginx.org\nPin: release o=nginx\nPin-Priority: 900\n" \
    | sudo tee /etc/apt/preferences.d/99nginx
```

Install Nginx:

```bash
sudo apt update
sudo apt install nginx
```

Check if Nginx is running by going to [localhost:80](http://localhost) or [95.111.202.121:80](http://95.111.202.121).

### Configurating Nginx

Configuration file is `/etc/nginx/nginx.conf`.

View configuration file:

```bash
cat /etc/nginx/nginx.conf
```

Edit configuration file:

```bash
sudo nano /etc/nginx/nginx.conf
```

Hit Ctrl+S Ctrl+X to save.

Apply new configuration:

```bash
sudo ngonx -s reload
```

### Getting Nginx processes list

For getting the list of all running Nginx processes, the `ps` utility may be used, for example, in the following way:

```bash
ps -ax | grep nginx
```

Nginx master process ID can be found in `/var/run/nginx.pid`:

```bash
cat /var/run/nginx.pid
```

### Serving static files

The `server` block look like this:

```
server {
    location / {
        root /data/www;
    }

    location /images/ {
        root /data;
    }
}
```

This means:

In response to the `http://localhost/` request Nginx will send the `/data/www/index` file.

In response to the `http://localhost/images/example.png` request Nginx will send the `/data/images/example.png` file.

So, to serve the `http://localhost/` request for landing page and the `http://localhost/app` request for web app. This has to be configured as:

```
server {
    location / {
        root /home/admin/deploy/website/index;
    }
    location /app {
        root /home/admin/deploy/website;
}
```
