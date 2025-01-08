---
title: "State of the Server 2025"
date: 2025-01-01T17:22:52-06:00
---

Starting new this year is the State of the Server inspired by [The Orange One's series by the same name](https://theorangeone.net/posts/state-of-the-server-2025/). I've completely ripped off his format as a starting point. (thanks, Jake)

I will enjoy cataloging how things change and move through the year and years. 
<!--more-->
So much of the courage and inspiration for what follows is owed to [Self-Hosted Show](https://www.jupiterbroadcasting.com/show/self-hosted/) and [LINUX Unplugged](https://www.jupiterbroadcasting.com/show/linux-unplugged/) over the years.

# Applications
- [AudioBookshelf](https://www.audiobookshelf.org/) - Audiobook and podcast hosting. Companion app is Android only at the moment.
- [Crafty Controller](https://craftycontrol.com/) - Minecraft server manager
- [Dozzle](https://dozzle.dev/) - Docker log viewer
- [ErsatzTV](https://ersatztv.org/) - Personal IPTV channels from your media files
- [HealthChecks](https://healthchecks.io/) - Cron job monitoring
- [Immich](https://immich.app/) - Self-hosted Google photos replacement
- [Jellyfin](https://jellyfin.org/) - Media server
- [LubeLogger](https://lubelogger.com/) - Auto maintenance log
- [Lychee](https://lycheeorg.github.io/) - Web-based photo albums
- [Nextcloud](https://nextcloud.com/) - Calendar, file storage, contacts
- [Nginx Proxy Manager](https://nginxproxymanager.com/) - Reverse proxy
- [Octoprint](https://octoprint.org/) - 3d printer interface
- [orbital-sync](https://orbitalsync.com/) - Pi-Hole synchronization
- [Pi.Alert](https://github.com/pucherot/Pi.Alert) - Network client monitoring 
- [Pi-hole](https://pi-hole.net/) - Network DNS/Ad blocker
- [PinchFlat](https://github.com/kieraneglin/pinchflat) - YouTube media manager
- [Plausible](https://plausible.io/) - Website analytics
- [Plex](https://www.plex.tv/) - Media server
- [Tandoor](https://docs.tandoor.dev/) - Recipes 
- [Tautulli](https://tautulli.com/) - Plex metrics 
- [SearXNG](https://github.com/searxng/searxng) - Self-hosted search engine
- [SilverBullet](https://silverbullet.md/) - Web-based markdown editor for notes
- [Syncthing](https://syncthing.net/) - File syncronization
- [Vaultwarden](https://github.com/dani-garcia/vaultwarden) - Password manager

# Hardware
- [Unraid](https://unraid.net/) Ryzen 3700x + LSI HBA + Intel Quad NIC
- [Proxmox](https://www.proxmox.com/en/) cluster
	- Node 1: Xeon E3-1245v2 + LSI HBA
	- Node 2: Lenovo Tiny M720q i5-8500T + Intel Quad NIC
	- Node 3: Lenovo Tiny M720q i5-8400T  

Thanks to [ServeTheHome for the excellent resource thread on these affordable Lenovo Tiny boxes](https://forums.servethehome.com/index.php?threads/lenovo-thinkcentre-thinkstation-tiny-project-tinyminimicro-reference-thread.34925/)!

## Storage
[Unraid](https://unraid.net/) primary storage server 
- 6 HDD (5 data, 1 parity) 
- 3 SSD (cache, xfer, Immich pool)

[Truenas Scale](https://www.truenas.com/truenas-scale/) secondary storage server
- 4 HDD, two 2-drive mirrors in a single ZFS pool 

Stable year, no drive failures. There was a third 2-drive mirror in the ZFS pool. The drives were very old (2012, 2016 mfg dates) and the 2016 drive was reporting SMART errors. I didn't need the capacity so they were removed and not replaced. 

# Backups
I am failing here. I do replicate data from [Unraid](https://unraid.net/) services to the Truenas ZFS array, but that does not count as it is under the same roof. This year, I will implement off-site, encrypted backups to (probably) [Backblaze](https://www.backblaze.com/) for the family photos and critical docs that has been put off for years. 

# Remote Access
I use a combination of [Cloudflare](https://www.cloudflare.com/) DNS/custom domains/reverse proxy and [Tailscale](https://tailscale.com/) routing. I'm expanding my use of Tailscale and will most likely decrease the number of apps running on the reverse proxy in 2025. Tailscale is changing the way I think about remote access. 

In December 2024, I created a Factorio server VM. Rather than proxy the connection, I added it to my tailnet and shared it with my brother. Easy, simple, fast. 

Also in December, my brother-in-law asked for a few ISOs and other files. He talked about bringing a hard drive with him for the holidays for the transfer. Instead, he shared his storage server with me over Tailscale. I was able to mount his share via NFS and complete the transfer remotely. 

# Network
The biggest change this year was in rebuilding my home network. I switched from a pair of Asus Wifi routers to an Opnsense router and Ubiquity switches and access points. I wanted VLANS, more wired connectivity, WiFi 6, more DNS control and more DHCP control. I also replaced the single, between-floor ethernet connection with a run of 4 cables in actual conduit. I am a network engineer now. 

## DNS
As part of the network remodel, I've implemented a layered DNS approach. I run a pair of redundant Pihole LCX containers (on separate [Proxmox](https://www.proxmox.com/en/) hosts). Each instance has a network interface for each VLAN allowing them to serve the entire network.  Opnsense advertises these servers as DNS for clients. Pihole uses Unbound DNS running on Opnsense as its upstream server and Unbound uses [Cloudflare DNS for families](https://blog.cloudflare.com/introducing-1-1-1-1-for-families/) as its upstream DNS.

Somewhat circular, but the reason is to allow Pihole to resolve network hostnames from the Opnsense firewall in the interface. 

I'm running redundant Pihole instances for two reasons. I had issues running a single instance  where Pihole will stop responding and it more or less takes the internet side of the network down. This hasn't happened since I've deployed the second server. I'm interested to see how it response if/when it happens next. The second reason is to continue to provide DNS resolution for the network when the Proxmox is offline (say, for a reboot). As long as both machines are not offline simultaneously, DNS should continue to function. Pihole instances are synced through [orbital-sync](https://orbitalsync.com/).

I am going to look into AdGuardHome in 2025, but for now, it works!

# VPS
This year I added another bringing the total to 4 remote web servers that I manage (1 for me, 3 for others) - all Linodes running versions of Ubuntu, Nginx and Docker services. I continue to be happy with the price and performance from Linode. 

In my [Tailscale](https://tailscale.com/) exploration, I will toy with using a tailscale backplane between the public internet (Linode VPS) and a webserver on my local network. A version of this is how Jupiter Broadcasting is hosting their site. This would be beneficial if more storage or processing is needed for photo serving for example. 

# OS/Hypervisor
My primary server is running [Unraid](https://unraid.net/) 6.12 as of year end 2024. I run storage, docker services and virtual machines off this server. This instance of Unraid has been installed since April 2021. [Unraid 7](https://unraid.net/blog/unraid-7-rc-2) is expected in 2025 with a lot of really nice enhancements around [Tailscale](https://tailscale.com/) integration, ZFS and plenty more. 

This year has seen a dramatic increase in my usage of [Proxmox](https://www.proxmox.com/en/). I expanded from a single server to a three node cluster, adding two Lenovo 1L M720q machines to the existing Xeon box. I've slowly started to add more services as LXCs to Proxmox. I really like the way the LXCs are assigned a unique IP address (vs default Docker) and its simpler to add each LXC (service) to Tailscale as a separate machine. [Proxmox VE Helper-Scripts](https://community-scripts.github.io/ProxmoxVE) have made a lot of this much simpler. I have changes (mostly RAM capacity) planned for 2025.

I spent a bunch of time learning Nix on the desktop this year. Not sure it will stick as I still prefer Arch based systems on the desktop. I will play with a NixOS server in 2025.

# Services
**Email**: [Postale.io](https://postale.io/) 
Affordable, reliable, multiple domains, multiple users, wildcard aliasing. Everything I need. 

**Music**: [Spotify](https://open.spotify.com/) (and [Plexamp](https://www.plex.tv/plexamp/))
I can't get off Spotify. I do use Plexamp for some of the otherwise unavailable Christmas music and when I want to feel cool. 

**Editor**: [VSCodium](https://vscodium.com/), [Vim](https://www.vim.org/) and I started messing with [Neovim](https://neovim.io/) 
VSCodium for [Hugo](https://gohugo.io/) website development. Vim for server administration. Neovim is really cool. I have a goal to implement a VSCode-like Nvim interface. 

**Notes**: [SilverBullet](https://silverbullet.md/)
Web based markdown editor. Lots of flexibility but its kind of a DIY solution. Easy to set up, nothing but plain text markdown files (and a small db file) behind it. Simple for keeping notes all in the same place on the desktop and phone. I am looking into [Obsidian](https://obsidian.md/). I hear really great things and it offers more features and organization. 

**Podcasts**: [Pocket Casts](https://pocketcasts.com/)
I've been on PocketcCasts since switching to Android in 2019 and I'm happy with the free tier.

**Browser**: [Brave](https://brave.com/) on everything 
I can't not have tab groups

**Search engine**: [SearXNG](https://github.com/searxng/searxng)
I have completely switched to this self-hosted search engine on my desktop. With tailscale, I can see this being implemented on all of my devices.

# 2025
Things are working well and in a good, stable place. I'm never one to leave well enough alone. This year, I'm looking forward to:
- utilizing more of Tailscale's features across my services including using a proxy service. 
- try Obsidian as my note taking app
- replace the aging and power hungry Xeon box with a smaller, more efficient machine in my Proxmox cluster
- expand the RAM in on of the Lenovo Tiny nodes
- replace one drive in my Unraid server to increase capacity
- use the replaced drive from Unraid (with another drive on the shelf) to replace/upgrade one of the two ZFS mirrors in Truenas
- implement off-site encrypted backups of the family photos
- migrate my Opnsense DHCP to Kea
- migrate some VPS bases services to local instances
- finally try Home Assistant and add energy monitoring and other smart thingys
- finish cataloging and importing our old home videos
- migrate the last of the photo archive into Immich

Plenty of other projects to poke at too. Should be fun. 