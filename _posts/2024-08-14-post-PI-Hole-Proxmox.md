---
title: "Getting PI-hole with Orbital Sync setup on Proxmox"
date: 2024-08-14T11:07:30-04:00
categories:
  - Setup
tags:
  - Homelab
  - PI-hole
  - Orbital Sync
  - Proxmox
  - Setup
---
#### I've wanted to get PI-hole setup for a while now but worried about a failure which would take down my network DNS wise. After looking around I found [Gravity Sync](https://github.com/vmstan/gravity-sync/blob/master/README.md) which seemed to solve the issue with syncing 2 PI-holes [Under the Alternatives](https://github.com/vmstan/gravity-sync/wiki/Alternatives) there was an option for [Orbital Sync](https://orbitalsync.com) this is not archived and seemed to have less overhead. 


# PI-hole Setup

#### [I followed this guide](https://www.wundertech.net/how-to-install-pi-hole-on-proxmox/) with the only issue I had being the console not working on a reboot which I was able to solve [By changing the console setting from tty to console](https://www.reddit.com/r/Proxmox/comments/11cihzd/help_new_container_blank_screen/ja3ehgl/)

![](/assets/images/TTY_Error.png)

# Lists used

#### After looking around online I decided on the following blacklist 

#### *formatted for copypasting into the Adlist GUI*

https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts https://raw.githubusercontent.com/PolishFiltersTeam/KADhosts/master/KADhosts.txt https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.Spam/hosts https://v.firebog.net/hosts/static/w3kbl.txt https://adaway.org/hosts.txt https://v.firebog.net/hosts/AdguardDNS.txt https://v.firebog.net/hosts/Admiral.txt https://v.firebog.net/hosts/Easyprivacy.txt https://v.firebog.net/hosts/Prigent-Ads.txt https://raw.githubusercontent.com/anudeepND/blacklist/master/adservers.txt https://v.firebog.net/hosts/Easylist.txt https://pgl.yoyo.org/adservers/serverlist.php?hostformat=hosts&showintro=0&mimetype=plaintext https://raw.githubusercontent.com/FadeMind/hosts.extras/master/UncheckyAds/hosts https://raw.githubusercontent.com/bigdargon/hostsVN/master/hosts https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.2o7Net/hosts https://raw.githubusercontent.com/crazy-max/WindowsSpyBlocker/master/data/hosts/spy.txt https://hostfiles.frogeye.fr/firstparty-trackers-hosts.txt https://zerodot1.gitlab.io/CoinBlockerLists/hosts_browser https://raw.githubusercontent.com/DandelionSprout/adfilt/master/Alternate%20versions%20Anti-Malware%20List/AntiMalwareHosts.txt https://osint.digitalside.it/Threat-Intel/lists/latestdomains.txt https://v.firebog.net/hosts/Prigent-Crypto.txt https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.Risk/hosts https://bitbucket.org/ethanr/dns-blacklists/raw/8575c9f96e5b4a1308f2f12394abd86d0927a4a0/bad_lists/Mandiant_APT1_Report_Appendix_D.txt https://phishing.army/download/phishing_army_blocklist_extended.txt https://gitlab.com/quidsup/notrack-blocklists/raw/master/notrack-malware.txt https://v.firebog.net/hosts/RPiList-Malware.txt https://v.firebog.net/hosts/RPiList-Phishing.txt https://raw.githubusercontent.com/Spam404/lists/master/main-blacklist.txt https://raw.githubusercontent.com/AssoEchap/stalkerware-indicators/master/generated/hosts https://urlhaus.abuse.ch/downloads/hostfile/

# Orbital Sync

#### Get docker installed [Following this guide for your OS](https://docs.docker.com/engine/install/)

#### I kept getting errors with Orbital Sync I found this to be due to  [complex passwords](https://github.com/mattwebbio/orbital-sync/issues/185) which changed solved the issue. 

#### Using the following compose file I was able to get this running without any issues full info is [here](https://orbitalsync.com/CONFIG.html)

```yaml
services:
  orbital-sync:
    image: mattwebbio/orbital-sync:1
    environment:
     PRIMARY_HOST_BASE_URL: 'https://pihole1.example.com'
      PRIMARY_HOST_PASSWORD: 'your_password1'
      SECONDARY_HOSTS_1_BASE_URL: 'https://pihole2.example.com'
      SECONDARY_HOSTS_1_PASSWORD: 'your_password2'
      INTERVAL_MINUTES: 60
      VERBOSE: true
      SYNC_V5_WHITELIST: true
      SYNC_V5_REGEX_WHITELIST: true
      SYNC_V5_BLACKLIST: true
      SYNC_V5_REGEX_LIST: true
      SYNC_V5_AD_LIST: true
      SYNC_V5_CLIENT: true
      SYNC_V5_GROUP: true
      SYNC_V5_AUDIT_LOG: true
      NOTIFY_ON_SUCCESS: true
      NOTIFY_ON_FAILURE: true
```

#### A good quick guide to compose is in the [docker docs](https://docs.docker.com/compose/gettingstarted/#step-2-define-services-in-a-compose-file)

#### After this you should have have two PI-holes and one service syncing from the Primary PI-hole to the other PI-holes.

# Failover

#### For my router I can just list them as DNS 1 DNS 2 so if one goes down it can use the other, for your router you'll have to figure it out as it differs for each router.

![](/assets/images/DNS_Example.png)
