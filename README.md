# LazyLibrarian Build
This recipe is for setting up LazyLibrarian.

Network Prerequisites are:
- [x] Layer 2 Network Switches
- [x] Network Gateway is `192.168.1.5`
- [x] Network DNS server is `192.168.1.5` (Note: your Gateway hardware should enable you to a configure DNS server(s), like a UniFi USG Gateway, so set the following: primary DNS `192.168.1.254` which will be your PiHole server IP address; and, secondary DNS `1.1.1.1` which is a backup Cloudfare DNS server in the event your PiHole server 192.168.1.254 fails or os down)
- [x] Network DHCP server is `192.168.1.5`

Other Prerequisites are:
- [x] Synology NAS, or linux variant of a NAS, is fully configured as per [SYNOBUILD](https://github.com/ahuacate/synobuild#synobuild).
- [x] Proxmox node fully configured as per [PROXMOX-NODE BUILDING](https://github.com/ahuacate/proxmox-node/blob/master/README.md#proxmox-node-building).
- [x] LazyLibrarian LXC with LazyLibrarian SW installed as per [LazyLibrarian LXC - Ubuntu 18.04](https://github.com/ahuacate/proxmox-lxc/blob/master/README.md#110-lazylibrarian-lxc---ubuntu-1804).

Tasks to be performed are:
- [ ] 1.0 Setup LazyLibrarian interface settings
- [ ] 2.0 Setup LazyLibrarian importing settings
- [ ] 3.0 Add media to the Jellyfin Library
- [ ] 4.0 Edit Jellyfin `Storm` User
- [ ] 5.0 Create Jellyfin Remote Access Users
- [ ] 6.0 Create Jellyfin Media Player Users
- [ ] 00.00 Patches & Fixes

## 1.0 Setup LazyLibrarian interface settings
In your web browser type `http://192.168.50.118:5289` and click on the `Interface Tab`. There is little to do here except add a new user and password to the `Access Control` section. Enter your Username and Password details as folows:

| WebServer Options | Value
| :---  | :--- 
| User | `storm`
| Pass | `add your chosen password`


## 2.0 Setup LazyLibrarian importing settings
In your web browser type `http://192.168.50.118:5289` and click on the `Importing Tab`.

This tab is used to import your saved books from either GoodReads or GoogleBooks. Personally I prefer GoodReads so these instructions are for Goodreads.

Make a GoodReads account at www.goodreads.com and then register for a [developer key](https://www.goodreads.com/api/keys). Once you have registered for a developer key, you will be given an API key and a Secret key. Within LazyLibrarian’s `Importing tab` tick the `Enable GoodReads Sync box`. Copy and paste your GoodReads API and Secret keys into the appropriate boxes and then save changes.

Next click `Request OAuth1` and you will directed to the GoodReads website to allow application access to LazyLibrarian. Once that is done, return to Lazylibrarian and click `Request OAuth2`, which will auto-fill the `Goodreads OAuth Token` and `Goodreads OAuth Secret`. Now LazyLibrarian and GoodReads can talk to one another to sync your books. It is also a good idea to fill out the Sync Wanted to Goodreads shelf section with `to-read` and the Sync Open/Have to Goodreads shelf to `owned`.

| Information Sources | Value
| :---  | :--- 
| Book Information | `Goodreads`
| Goodreads API | Get from your goodreads account
| Enable GoodReads Sync | See Above Instructions
| Goodreads Secret | See Above Instructions
| Goodreads OAuth Token | See Above Instructions
| Goodreads OAuth Secret | See Above Instructions
| Sync Wanted eBooks to Goodreads shelf | `to-read`
| Sync Owned eBooks to Goodreads shelf | `owned`
| Sync Wanted AudioBooks to Goodreads shelf | `audio-wanted`
| Sync Owned AudioBooks to Goodreads shelf | `owned`
| Delete from GoodReads Wanted shelf if also on Open/Have shelf | `☐`
| Goodreads Sync Interval | `48` Hours

Remember to click `Save changes`.

## 3.0 Setup LazyLibrarian downloader settings
In your web browser type `http://192.168.50.118:5289` and click on the `Downloaders Tab`.

### 3.1 Usenet Settings
You should have NZBGet configured and its LXC running at http://192.168.30.112:6789/ . You will need your NZBGet `RestrictedUsername` and `RestrictedPassword` you configured [HERE](http://192.168.30.112:6789/#SECURITY). 

Set the values as follows, remembering to click `Save changes` when finished:

| Usenet| Value
| :---  | :--- 
| Use Sabnzbd+ | `☐`
| Use NZBGet | `☑`
| **NZBGET**
| NZBGet Host | `192.168.30.112`
| Port | 6789
| Username | Type your NZBGet RestrictedUsername (i.e `client`)
| Password | Type your NZBGet RestrictedPassword
| Category | `lazy`
| Priority | `0`
| Usenet Retention | `1500` Days

Remember to click `Save changes`.


### 3.2 Torrent Settings
You should have Deluge configured and its LXC running at http://192.168.30.113:8112/ . You will need your Deluge  `RestrictedUsername` and `RestrictedPassword` you configured [HERE](http://192.168.30.112:6789/#SECURITY). 

Set the values as follows, remembering to click `Save changes` when finished:
| Torrent| Value
| :---  | :--- 
| Use Deluge | `☑`
| **Deluge**
| Deluge Host | `192.168.30.113`
| Daemon or webUI Port | `58846`
| Deluge Certificate | Leave blank
| Deluge URL Base | Leave blank
| Username | localclient
| Password | See instructions above
| Deluge Label | `lazy`
| Download directory | Leave blank

Remember to click `Save changes`.


