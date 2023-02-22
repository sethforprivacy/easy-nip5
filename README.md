# easy-nip5

An extremely simple NIP-05 compliant server.

## Requirements

* [Git](https://git-scm.com/downloads)
* [Docker Engine](https://docs.docker.com/engine/install/)
* [Docker Compose](https://docs.docker.com/compose/install/)
* Ports `80/tcp` and `443/tcp` exposed/forwarded to the host
  * NOTE: if using a VPS, you usually don't need to do any port-forwarding
* DNS entries for your top-level domain pointing to your server's external IP address

## How does it work?

This repo relies on Docker Compose to configure and run an nginx container serving `nostr.json`, leveraging Traefik to automatically expose it, [request and maintain](https://doc.traefik.io/traefik/user-guides/docker-compose/acme-tls/) Let's Encrypt certificates for SSL, and handle all proxying.

## Quick configuration

You will need to clone this repository to the host you want running these services first:

```bash
git clone https://github.com/sethforprivacy/easy-nip05.git
cd easy-nip05
```

Once cloned, set the email address and domain name to use in the `docker-compose.yml` file, replacing:

* `replace_with_working_email_address` with an email address like `email@email.com`
* `replace_with_your_domain` with your domain, like `sethforprivacy.com`

Edit the `nostr.json` file, replacing:

* `YOUR_NOSTR_NAME` with whatever name you prefer, such as `seth`
  * This name will be appended to your domain name (i.e. `seth@sethforprivacy.com`), so usually best to keep it short and sweet
  * You can also set it to just `_` and your NIP-05 verification will be *just* your domain (i.e. `sethforprivacy.com`). You would need to use `_@domain.com` for your NIP05 config in your client.
* `YOUR_NOSTR_PUBLIC_KEY_IN_HEX` with your Nostr public key, in hex format, like `58ead82fa15b550094f7f5fe4804e0fe75b779dbef2e9b20511eccd69e6d08f9`

It should look like the following afterwards:

```json
{
  "names": {
    "seth": "58ead82fa15b550094f7f5fe4804e0fe75b779dbef2e9b20511eccd69e6d08f9"
  }
}
```

## Start the server

Start-up the services with Docker Compose:

* `docker-compose up -d`

You can verify that it's available at the full URL now, i.e `https://sethforprivacy.com/.well-known/nostr.json`. If that gives you the contents of your `nostr.json` file you edited earlier, it's working properly!

Now add the new NIP-05 identity to your Nostr profile, and you should have that fancy check-mark :)
