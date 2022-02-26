![](.github/images/repo_header.png)

[![Uptime Kuma](https://img.shields.io/badge/Uptime_Kuma-1.12.1-blue.svg)](https://github.com/louislam/uptime-kuma/releases/tag/1.12.1)
[![Dokku](https://img.shields.io/badge/Dokku-Repo-blue.svg)](https://github.com/dokku/dokku)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://github.com/louislam/uptime-kuma/graphs/commit-activity)

# Run Uptime Kuma on Dokku

## Perquisites

### What is Uptime Kuma?

Uptime Kuma is a self-hosted monitoring tool like "Uptime Robot".

### What is Dokku?

[Dokku](http://dokku.viewdocs.io/dokku/) is the smallest PaaS implementation you've ever seen - _Docker
powered mini-Heroku_.

### Requirements

* A working [Dokku host](http://dokku.viewdocs.io/dokku/getting-started/installation/)

# Setup

**Note:** We are going to use the domain `uptime.example.com` for demonstration purposes. Make sure to
replace it with your own domain name.

## App and plugins

### Create the app

Log onto your Dokku Host to create the Uptime Kuma app:

```bash
dokku apps:create uptime-kuma
```

## Domain setup

To get the routing working, we need to apply a few settings. First we set the domain.

```bash
dokku domains:set uptime-kuma uptime.example.com
```

## Persistent storage

To persists user uploads (e.g. avatars) between restarts, create a folder on the host machine and tell Dokku to mount it to the app container.

```bash
sudo mkdir -p /var/lib/dokku/data/storage/uptime-kuma
dokku storage:mount uptime-kuma /var/lib/dokku/data/storage/uptime-kuma:/app/data
```

## Push Uptime Kuma to Dokku

### Grabbing the repository

First clone this repository onto your machine.

#### Via SSH

```bash
git clone git@github.com:D1ceWard/uptime_kuma_on_dokku.git
```

#### Via HTTPS

```bash
git clone https://github.com/D1ceWard/uptime_kuma_on_dokku.git
```

### Set up git remote

Now you need to set up your Dokku server as a remote.

```bash
git remote add dokku dokku@example.com:uptime-kuma
```

### Push Uptime Kuma

Now we can push Uptime Kuma to Dokku (_before_ moving on to the [next part](#domain-and-ssl-certificate)).

```bash
git push dokku master
```

## SSL certificate

Last but not least, we can go an grab the SSL certificate from [Let's
Encrypt](https://letsencrypt.org/).

```bash
# Install letsencrypt plugin
dokku plugin:install https://github.com/dokku/dokku-letsencrypt.git

# Set certificate contact email
dokku config:set --no-restart uptime-kuma DOKKU_LETSENCRYPT_EMAIL=you@example.com

# Generate certificate
dokku letsencrypt uptime-kuma
```

## Wrapping up

Your Uptime Kuma instance should now be available on [https://uptime.example.com](https://uptime.example.com).
