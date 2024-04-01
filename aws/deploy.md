## Setting up EC2

Launch instance EC2 with `Ubuntu`

**NOTE:** Remember to allow SSH access to the instance.

Copy the public IPv4 address of the instance (For example: `237.84.2.178`)

SSH into the instance and run:

```sh
ssh -i key.pem ubuntu@<INSTANCE_IP>
```

## Installing docker engine

- Follow this: [Install docker engine](https://docs.docker.com/engine/install/ubuntu/)

### Clone applications

```sh
mkdir applications
cd applications
```

Clone the applications to the `applications` folder (Remeber to setup docker)

```sh
docker compse up -d
```

## Setting `nginx` as a reverse proxy

### Step 1 - Installing Nginx

```sh
sudo apt update
sudo apt install nginx
```

### Step 2 - Adjusting the Firewal

```sh
sudo ufw app list
```

Output should be:

```sh
Output
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
```

```sh
sudo ufw allow 'Nginx HTTP'
sudo ufw allow 'OpenSSH'
```

```sh
sudo ufw status
```

Enable `ufw` firewall:

```sh
sudo ufw enable
```

### Step 3 - Checking the web server

```sh
systemctl status nginx
```
