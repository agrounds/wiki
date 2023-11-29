I use Cloudflare for DNS services. I chose them because they make it relatively easy to use [DDNS](https://en.wikipedia.org/wiki/Dynamic_DNS) regardless of the platform I run my DDNS process on (which is the box that also hosts my services).
# Design

My goal was to have some subdomains of `groundsfam.com` resolve to publicly accessible services and some resolve to internal-only services. For instance, https://example.groundsfam.com should be accessible on the public internet, but https://plex.int.groundsfam.com should only be accessible on my local network. To make this work, I need `example.groundsfam.com` to resolve to a public internet IP address and plex.int.groundsfam.com to resolve to a local network IP address.

**Name/IP associations I'd like to achieve**

| Name | IP address |
| - | - |
| groundsfam.com | 192.63.67.72 |
| example.groundsfam.com | 192.63.67.72 |
| plex.int.groundsfam.com | 192.168.86.210 |

# Public internet DNS records

In Cloudflare, I created an A record for `groundsfam.com` containing my public IP address. For now, all my publicly available services run on servers inside my house, so they all use this same IP address. Therefore I added CNAME records for each subdomain like so.

**Sample of Cloudflare DNS records**

| Type | Name | Content |
| - | - | - |
| A | groundsfam.com | 192.63.67.72 |
| CNAME | example | groundsfam.com |

# Local network DNS records

Because my router does not support [NAT hairpinning](https://en.wikipedia.org/wiki/Network_address_translation#NAT_hairpinning), I cannot simply add additional DNS records to Cloudflare for my internal services. The DNS resolution needs to happen inside my local network instead.

I use a [Synology DiskStation](https://www.synology.com/en-global/products?product_line=ds_plus) for data storage and service hosting, so using it as a DNS server was a natural choice. I use the DNS Server application that comes with Synology's DSM operating system. In Synology DNS, I created a zone for `int.groundsfam.com` and created DNS records for internal subdomains. I also added an `NS` record to make this server the authoritative name server for the `int.groundsfam.com` zone (i.e. all subdomains of `int.groundsfam.com`).

**Sample of Synology DNS records**

| Type | Name | Content |
| - | - | - |
| A | nas.int.groundsfam.com. | 192.168.86.210 |
| A | ns.int.groundsfam.com. | 192.168.86.210 |
| A | plex.int.groundsfam.com. | 192.168.86.210 |
| NS | int.groundsfam.com. | ns.int.groundsfam.com. |

Next, I added more records to Cloudflare to delegate DNS resolution of `*.int.groundsfam.com` subdomains to the Synology DNS server.

**Additional Cloudflare DNS records**

| Type | Name | Content |
| - | - | - |
| CNAME | ns | groundsfam.com |
| NS | int | ns.groundsfam.com |

Finally, I configured my home router to forward TCP and UDP packets on port 53 to my Synology DiskStation so that DNS requests would go through.

The end effect is that hosts on my local network are able to resolve subdomains of `int.groundsfam.com` to local IP addresses, and this all happens through "real" DNS, i.e. without reconfiguring my network to delegate all DNS queries to the Synology. This means the Synology is not the bottleneck for resolving other domains like `google.com`, and it's also helpful for managing SSL certificates for internal services (more on that later).
# DDNS

Because I do not have a fixed IP address (my ISP doesn't offer this to residential customers), I have to use a DDNS process to keep the `A` record for `groundsfam.com` in sync with my current public IP address. For this I use the [oznu/cloudflare-ddns](https://hub.docker.com/r/oznu/cloudflare-ddns/) Docker image. I run it on my Synology using the Container Manager application provided by Synology DSM.

The process needs a Cloudflare API key in order to interact with Cloudflare on your behalf. In the [Cloudflare dashboard](https://dash.cloudflare.com/), I clicked into the `groundsfam.com` zone and then "get your API token." Here, I created a new API token with read access to `Zone.Zone` and edit access to `Zone.DNS`.

I used the Create Container wizard in Container Manager. I lowered its max memory to only 16 MB (which is probably still way more than it needs) and set the `ZONE=groundsfam.com` and `API_KEY=Cloudflare API token from previous step` environment variables. I also configured it to auto-restart. Inside, it runs on a cron to check my current public IP address and update the `A` record in Cloudflare if needed.