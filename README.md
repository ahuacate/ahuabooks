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
- [x] Deluge LXC with Deluge SW installed as per [Deluge LXC - Ubuntu 18.04](https://github.com/ahuacate/proxmox-lxc-media/blob/master/README.md#40-deluge-lxc---ubuntu-1804).
- [x] NZBGet LXC with NZBGet SW installed as per [NZBget LXC - Ubuntu 18.04](https://github.com/ahuacate/proxmox-lxc-media/blob/master/README.md#300-nzbget-lxc---ubuntu-1804)
- [x] Jackett installed as per [Jackett LXC - Ubuntu 18.04](https://github.com/ahuacate/proxmox-lxc-media/blob/master/README.md#500-jackett-lxc---ubuntu-1804)
- [x] LazyLibrarian LXC with LazyLibrarian SW installed as per [LazyLibrarian LXC - Ubuntu 18.04](https://github.com/ahuacate/proxmox-lxc-media/blob/master/README.md#1100-lazylibrarian-lxc---ubuntu-1804).

Tasks to be performed are:
- [1.00 Setup LazyLibrarian settings](#100-setup-lazylibrarian-settings)
	- [1.01 Setup LazyLibrarian interface settings](#101-setup-lazylibrarian-interface-settings)
	- [1.02 Setup LazyLibrarian importing settings](#102-setup-lazylibrarian-importing-settings)
	- [1.03 Setup LazyLibrarian downloader settings](#103-setup-lazylibrarian-downloader-settings)
	- [1.04 Setup LazyLibrarian providers settings](#104-setup-lazylibrarian-providers-settings)
	- [1.05 Setup LazyLibrarian processing settings](#105-setup-lazylibrarian-processing-settings)
	- [1.06 Setup LazyLibrarian categories settings](#106-setup-lazylibrarian-categories-settings)
- [2.00 Create Lazylibrarian backup](#200-create-lazylibrarian-backup)
- [3.00 Restore Lazylibrarian backup](#300-restore-lazylibrarian-backup)
- [4.00 Create an account at private trackers](#400-create-an-account-at-private-trackers)
- [00.00 Patches & Fixes](#0000-patches--fixes)

## 1.00 Setup LazyLibrarian settings
In your web browser type `http://192.168.50.118:5289` and click `Config`.

### 1.01 Setup LazyLibrarian interface settings
Click on the `Interface Tab`. There is little to do here except add a new user and password to the `Access Control` section. Enter your Username and Password details as folows:

| WebServer Options | Value
| :---  | :--- 
| User | `storm`
| Pass | `add your chosen password`

![alt text](https://raw.githubusercontent.com/ahuacate/lazylibrarian/master/images/interface.png)


### 1.02 Setup LazyLibrarian importing settings
Click on the `Importing Tab`.

This tab are the settings to import your saved books from either GoodReads or GoogleBooks. Personally I prefer GoodReads so these instructions are for Goodreads.

Make a GoodReads account at www.goodreads.com and then register for a [developer key](https://www.goodreads.com/api/keys). Once you have registered for a developer key, you will be given an API key and a Secret key.

Within LazyLibrarian’s `Importing tab` tick the `Enable GoodReads Sync box`. Copy and paste your GoodReads API and Secret keys into the appropriate boxes and then save changes.

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

![alt text](https://raw.githubusercontent.com/ahuacate/lazylibrarian/master/images/importing.png)

### 1.03 Setup LazyLibrarian downloader settings
Click on the `Downloaders Tab`.

**A) 3.01 Usenet Settings**
You should have NZBGet configured and its LXC running at http://192.168.30.112:6789/ . You will need your NZBGet `RestrictedUsername` (which is `client`) and the `RestrictedPassword` you configured [HERE](https://github.com/ahuacate/nzbget/blob/master/README.md#200-nzbget-security-preferences). 

Set the values as follows, remembering to click `Save changes` when finished:

| Usenet| Value
| :---  | :--- 
| Use Sabnzbd+ | `☐`
| Use NZBGet | `☑`
| **NZBGET**
| NZBGet Host | `192.168.30.112`
| Port | 6789
| Username | `client`
| Password | Type your NZBGet RestrictedPassword
| Category | `lazy`
| Priority | `0`
| Usenet Retention | `1500` Days

Remember to click `Save changes`.

**B) 3.02 Torrent Settings**
You should have Deluge configured and its LXC running at http://192.168.30.113:8112/ .

Set the values as follows, remembering to click `Save changes` when finished:

| Torrent| Value
| :---  | :--- 
| Use Deluge | `☑`
| **Deluge**
| Deluge Host | `192.168.30.113`
| Daemon or webUI Port | `58846`
| Deluge Certificate | Leave blank
| Deluge URL Base | Leave blank
| Username | `lazylibrarian`
| Password | `9c67cf728b8c079c2e0065ee11cb3a9a6771421a`
| Deluge Label | `lazy`
| Download directory | Leave blank

Remember to click `Save changes`.

![alt text](https://raw.githubusercontent.com/ahuacate/lazylibrarian/master/images/downloaders.png)

### 1.04 Setup LazyLibrarian providers settings
Click on the `Providers Tab`. Complete the settings as follows. Most important are Torznab Provider - Jackett and your Newznab Providers.

![alt text](https://raw.githubusercontent.com/ahuacate/lazylibrarian/master/images/providers.png)

### 1.05 Setup LazyLibrarian processing settings
Click on the `Processing Tab`. Only input required is the `Folders > Download Directories` setting. Complete the settings as follows.

| Processing| Value
| :---  | :--- 
| **Folders**
| Download Directories | `/mnt/downloads/nzbget/completed/lazy, /mnt/downloads/deluge/complete/lazy`


![alt text](https://raw.githubusercontent.com/ahuacate/lazylibrarian/master/images/processing.png)

### 1.06 Setup LazyLibrarian categories settings
Click on the `Categories Tab`. Complete the settings as follows.

![alt text](https://raw.githubusercontent.com/ahuacate/lazylibrarian/master/images/categories.png)

## 2.00 Create Lazylibrarian backup
With the Proxmox web interface go to `typhoon-01` > `118 (lazy)` > `>_ Shell` and type the following:
```
sudo systemctl stop lazy.service &&
sleep 5 &&
su -c 'cp /opt/LazyLibrarian/lazylibrarian.ini* /mnt/backup/lazylibrarian' media &&
su -c 'cp /opt/LazyLibrarian/.lazylibrarian/{lazylibrarian.db,magazines.csv} /mnt/backup/lazylibrarian' media &&
sudo systemctl restart lazy.service
```

## 3.00 Restore Lazylibrarian backup
With the Proxmox web interface go to `typhoon-01` > `118 (lazy)` > `>_ Shell` and type the following:
```
sudo systemctl stop lazy.service &&
sleep 5 &&
rm -rf /opt/LazyLibrarian/lazylibrarian.ini* || true &&
su -c 'cp /mnt/backup/lazylibrarian/lazylibrarian.ini* /opt/LazyLibrarian' media &&
su -c 'cp /mnt/backup/lazylibrarian/{lazylibrarian.db,magazines.csv} /opt/LazyLibrarian/.lazylibrarian' media &&
chown 1605:65605 /opt/LazyLibrarian/lazylibrarian.ini* &&
sudo systemctl restart lazy.service
```

## 4.00 Create an account at private trackers
Best create a account with a specialist book indexer like audiobookbay.nl. Then add audiobookbay to Jackett. 

---

## 00.00 Patches & Fixes
Nothing yet.
