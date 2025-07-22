# Specify Web Asset Server with Caddy

This repository contains the three files necessary to start a [web-asset-server](https://github.com/specify/web-asset-server) with [Caddy](https://caddyserver.com/). Compared to nginx, Caddy offers certificate management out of the box, and handles renewal automatically. While most institutions will have their own certificates to install, this setup can be useful if you want to quickly spin up an asset server with https, or if you don't have custom certificates to install.

## Installation and Usage

Clone this repository on the server you wish to use as the asset service (or just manually copy paste the files).

```bash
git clone https://github.com/mark-pitblado/specify-asset-with-caddy
cd specify-asset-with-caddy
```

Ensure that both ports 80 and 443 are accessible to the wider internet, caddy needs to use letsencrypt/zerossl to handle the certificate issuance and renewal. If you don't want to use an HTTP challenge, there are [alternative ways to complete the ACME challenge](https://caddyserver.com/docs/automatic-https#acme-challenges), including methods that do not require any open ports.

### Values to edit and replace

You will need to configure a few values based on your setup

1. In the `compose.yml` file, replace the `ATTACHMENT_KEY` and the `SERVER_NAME` with values for your setup.
2. In the `Caddyfile` file, replace the `assets.example.com` hostname with your hostname.

### Start docker

Run `docker compose up -d`. If this is your first time running the command, you may need to wait a bit for the caddy build to finish. The reason that this takes a while is so the [replace-response plugin](https://github.com/caddyserver/replace-response) can be loaded into the image. 

> [!WARNING]
> The build process can hang if the system has too few resources. This configuration has loaded fine on a system with 1GB of memory and 1 AMD vCPU.

## Further considerations

This repository, and the instructions above, are *just* about the changes required to use caddy instead of nginx. There are further things that you should consider, such as backing up the location where the attachments are stored.
