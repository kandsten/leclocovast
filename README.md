# LeCloCoVaSt

**L**et's **E**ncrypt + **Clo**udFlare + **Co**nfigure **Va**rious **St**uff

Register and/or refresh Let's Encrypt certificates, proving ownership via CloudFlare
managed DNS rather than via HTTP. Will also install and/or refresh said certificates on
Various Stuff. Uses Ansible to do so.

## "Supported" Various Stuff ##

* Ubiquity EdgeRouter - tested with X SFP, may work with others as well
* Ubiquity UniFi controller - tested on Ubuntu
* VMWare ESXi
* Hass.io
* Plex - tested on Ubuntu
* Proxmox

## Is this for you?

Rationale as follows:

* I run various services on a LAN.
* I prefer to have working TLS wherever possible.
* I'm too lazy to manually refresh certs for a number of services every 90 days.
* I don't fancy letting the Internet at large connect to said services, necessitating DNS based verification.
* I don't particularly fancy maintaining certbot installs (and CloudFlare API keys) on more than one host.

Use Ansible to manage the certs in one location and distribute them to wherever they need to
be, restart services as necessary.

There may be some rough edges. It Works For me™. Your setup might be just ever so slightly
different.

If you think this makes sense, then you may or may not find this useful. If you don't think
this makes sense, I wish you luck in finding something that scratches your particular itch
in a better fashion.

## Getting Started

Copy files from the `sample-conf/` directory to the repo root, edit contents to fit your
setup.

As the playbook is setup to tie roles to inventory groups, you don't want to change the
group names found in the sample inventory.

The `certbot` role will register certs for every host in the inventory. As such, you'll
want to enter the FQDN, even if you normally use the plain hostname for SSH/Ansible.

Verify that the code does nothing nefarious. This thing is fetching certificates on your
behalf and you're handing it your CloudFlare API key. You don't do that without reading
the code first.

...right?

### Prerequisites

* Ansible (tested with 2.4)
* Certbot
* Working SSH pubkey authentication for the devices you wish to handle certs for

## Running

```
ansible-playbook -i inventory.yml playbook.yml
```

The Certbot module managing CloudFlare is a bit on the slower side. If you need to 
register or renew certs, give it some time to finish.

**Do note that this playbook will run Certbot with the --agree-tos flag**. Please read and
understand the [Subscriber Agreement](https://letsencrypt.org/repository/), or a herd of
rabid tigers will surely devour your firstborn. You have been warned.

## Contributing

I don't particularly expect any contributions, but if you have something you think fits,
feel free to shoot me a pull request.

## Author

* **Kriss Andsten** - [kandsten](https://github.com/kandsten)

## License

This project is licensed under the MIT License - see the [LICENSE.txt](LICENSE.txt) file for details

## Acknowledgments

* Various Stack Overflow posts, forum posts and random tidbits.
* CloudFlare, whose services I've been mooching off of for years and for being my employer crush.
* Let's Encrypt, for finally ensuring that free/libre certs are a thing and not horribly
  botching the job like those who came before.