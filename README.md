# paperless-ng-docker-compose
Paperless ng


Define the volumes first:

  pgdata 	-	local	/var/lib/docker/volumes/pgdata/_data
  paperless_consume	-	local	/var/lib/docker/volumes/paperless_consume/_data
  paperless_data	-	local	/var/lib/docker/volumes/paperless_data/_data
  paperless_export	-	local	/var/lib/docker/volumes/paperless_export/_data
  paperless_media	-	local	/var/lib/docker/volumes/paperless_media/_data
  

sudo su - 
cd ~
mkdir paperlessng
cd paperlessng
wget https://raw.githubusercontent.com/8layer8/paperless-ng-docker-compose/main/docker-compose.yml
wget https://raw.githubusercontent.com/8layer8/paperless-ng-docker-compose/main/docker-compose.env
docker-compose up
(once it behaves, Control+C it back down)
docker-compose start

Set it up in npm to http://192.168.0.55:8010 (Or whichever host/port you have it running on) and set it up as external + https

NOTE!
The original Scans directory is not mapped here, this is being handled like it is expendable. The originals are always in the scans directory, copy them into the import directory to ingest them.

Still needs samba share for the consumer, also needs email address set up to email.
Bulk imports here on .55 host, or wherever is hosting it:
Copy stuff into /var/lib/docker/volumes/paperless_consume/_data/
and paperless-ng will process and import them

Scanning from scanner (via phone) to paperless-ng:
Install AndSMB (Run it, allow access if it asks) Click +, add 192.168.0.253, username: scanner, password: spongebob, remote dir: /scans
Install Mopria scanner app, install and let it find the scanner
Load up the ADF or flatbed, scan away

Once scannned:
You can save it locally (Down arrow/Inbox icon) 
or 
click the Share thing, choose AndSMB, choose scans OK, it should upload quickly, then choose done
Currently, that gets them into /mnt/pool_alpha/scans/*.jpg
Still have to get them into paperless. The consume directory has to be on linux (not NFS/Samba) because it uses inotify to see new files.
Setting up a small samba container as part of paperless-ng is probably the way to go

OR

Scanning from phone:
connect paperless app to the paperless install using the proper hostname etc. Self explanatory in the app.
open paperless, click +
snap photo of thing
crop, rotate, checkmark OK
done!

OR 

Drag/drop into web UI (Dashboard on the right side)

Be wary that anything coming in this way does NOT go into the scan directory and currently does not get copied anywhere for backups.
