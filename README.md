![](.github/images/repo_header.png)

[![Uptime Kuma](https://img.shields.io/badge/Uptime_Kuma-1.23.10-blue.svg)](https://github.com/louislam/uptime-kuma/releases/tag/1.23.10)
[![Dokku](https://img.shields.io/badge/Dokku-Repo-blue.svg)](https://github.com/dokku/dokku)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://github.com/louislam/uptime-kuma/graphs/commit-activity)

# Run Uptime Kuma on Dokku

## Perquisites

### What is Uptime Kuma?

[Uptime Kuma](https://github.com/louislam/uptime-kuma) is a self-hosted monitoring tool like "Uptime Robot".

### What is Dokku?

[Dokku](http://dokku.viewdocs.io/dokku) is the smallest PaaS implementation you've ever seen - _Docker
powered mini-Heroku_.

### Requirements

* A working [Dokku host](http://dokku.viewdocs.io/dokku/getting-started/installation)

# Setup

**Note:** We are going to use the domain `uptime.example.com` for demonstration purposes. Make sure to
replace it with your own domain name.

## App and plugins

### Create the app

Log onto your Dokku Host to create the Uptime Kuma app:

```bash
dokku apps:create uptime-kuma
```

## Domain

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

```bash
# Via SSH
git clone git@github.com:d1ceward/uptime_kuma_on_dokku.git

# Via HTTPS
git clone https://github.com/d1ceward/uptime_kuma_on_dokku.git
```

### Set up your Dokku server as a Git remote

```bash
git remote add dokku dokku@example.com:uptime-kuma
```

### Push Uptime Kuma to Dokku

```bash
git push dokku master
```

## SSL certificate

Last but not least, we can go an grab the SSL certificate from [Let's
Encrypt](https://letsencrypt.org).

```bash
# Install letsencrypt plugin
dokku plugin:install https://github.com/dokku/dokku-letsencrypt.git

# Set certificate contact email
dokku letsencrypt:set uptime-kuma email you@example.com

# Generate certificate
dokku letsencrypt:enable uptime-kuma
```

## Wrapping up

Your Uptime Kuma instance should now be available on [https://uptime.example.com](https://uptime.example.com).

### Possible issue with proxy ports mapping

If the Plausible instance is not available at the address https://plausible.example.com check the return of this command :
```bash
dokku proxy:ports plausible
```

```bash
### Valid return
-----> Port mappings for plausible
    -----> scheme  host port  container port
    http           80         5000

### Invalid return
-----> Port mappings for plausible
    -----> scheme  host port  container port
    http           5000       5000
```

If the return is not the expected one, execute this command :

```bash
dokku proxy:ports-set plausible http:80:5000
```

If the return of the command was valid and Plausible is still not available, feel free to fill an issue in the issue tracker.
