# arctica-install-companion

Note1: lettered steps are to be manually completed in the terminal but will eventually be scripts executed as user interacts with arctica GUI. Ideally this guide should be walked through alongside the front end with the simulator to identify discrepencies. Eventually this may be turned into a guide for users who prefer to walk through arctica setup manually (v2?)

<br> the (arctica setup/X) labels designate where the user is in the latest build of the front end simulator

step 1: Start on a fresh ubuntu installation on a dedicated laptop (this laptop will be your full node and ideally will have a fairly new 500gb internal SATA SSD

step 1a: [may remove or move this step] install bitcoin core to the internal disk and start syncing, make sure that ~/.bitcoin is where blockdata and chainstate are being stored, this will be important later

step 1b: create your .bitcoin directory inside of root
<br>`mkdir .bitcoin `

step 1c: create chainstate and blocks directories inside of .bitcoin, these will be symlinked later
<br>`mkdir chainstate ~/.bitcoin && mkdir blocks ~/.bitcoin`

step 2 (arctica setup/2): set up password

step 3 (arctica setup/2): confirm password

step 4 (arctica setup/3): label your 8 CDs, 7 DVDs, 7 SD cards (or usbs) and 7 envelopes

step 4a insert setup CD to receieve decryption key so BPS is not required during initial setup [this screen needs to be added to arctica]

step 4b generate encryption key to be used in pgp
example, not tested, should eventually output to a directory on the setup CD
`ssh-keygen -t rsa -N '' -b 4096 -C "your_email@example.com" -f $HOME/.ssh/id_rsa`

step 5a: import the tails signing key and add to the GnuPGP keyring 
<br>`wget https://tails.boum.org/tails-signing.key gpg --import < tails-signing.key`

step 5b: install the debian keyring 
<br>`sudo apt update && sudo apt install debian-keyring`

step 5c: import the openPGP key of Chris Lamb (former Debian lead) 
<br>`gpg --keyring=/usr/share/keyrings/debian-keyring.gpg --export chris@chris-lamb.co.uk | gpg --import`

step 5d: verify certifications made on tails signing key 
<br>`gpg --keyid-format 0xlong --check-sigs A490D0F4D311A4153E2BB7CADBB802B258ACD84F`

You are looking for something like 
<br>`sig! 0x1E953E27D4311E58 2020-03-19 Chris Lamb <chris@chris-lamb.co.uk>`

step 5e: certify the tails signing key 
<br>`gpg --lsign-key A490D0F4D311A4153E2BB7CADBB802B258ACD84F`

step 5f: download tails 
<br>`wget --continue http://dl.amnesia.boum.org/tails/stable/tails-amd64-5.3.1/tails-amd64-5.3.1.img`

step 5g: download signature of the usb image 
<br>`wget https://tails.boum.org/torrents/files/tails-amd64-5.3.1.img.sig` 

step 5h: verify the usb image is signed with the tails signing key 
<br>`TZ=UTC gpg --no-options --keyid-format long --verify tails-amd64-5.3.1.img.sig tails-amd64-5.3.1.img`

Output should be something like (Verify that the date of the signature is the same and the signature is marked as Good signature since you certified the Tails signing key with your own key.) 
<br>`gpg: Signature made Tue 02 Aug 2022 12:07:47 AM UTC gpg: using EDDSA key CD4D4351AFA6933F574A9AFB90B2B4BD7AED235F gpg: Good signature from "Tails developers (offline long-term identity key) <tails@boum.org>" [full] gpg: aka "Tails developers <tails@boum.org>" [full]`

step 5i: Select the device to be flashed (need to prompt user here and decide this dynamically eventually). For now...

 return a list of devices on the system 
 <br>`ls -1 /dev/sd?`


step 6 (arctica setup/4): insert SD/USB 1. 

step 6a: identify the newly inserted device
<br>`ls -1 /dev/sd?`

step 7: DD the appropriate storage device with the downloaded tails image (change the `of=/dev/sda` below to your target device)
<br>`sudo dd if=tails-amd64-5.3.1/tails-amd64-5.3.1.img of=/dev/sda bs=16M oflag=direct status=progress`

step 7a: run the `setup-persistence.sh` script
(i haven't tested this outside of tails yet, todo: should just automatically set password to 'a' instead of prompting)
<br>`sudo bash setup-persistence.sh`

step 7b: add virtual labels to the device
<br>`touch /media/user/TailsData/Persistent/sd1.txt && touch /media/user/TailsData/Persistent/setup1.txt`

step 7c: Download Bitcoin Core [if you dont have it already from an earlier step]
<br>`wget https://bitcoincore.org/bin/bitcoin-core-23.0/bitcoin-23.0-x86_64-linux-gnu.tar.gz`

step 7d: Extract the tarball to the persistent directory
<br>`tar -xvzf bitcoin-23.0-x86_64-linux-gnu.tar.gz -C /media/user/TailsData/Persistent`

step 7e: download and copy the fully compiled arctica application into Persistent directory
<br>`mkdir arctica /media/user/TailsData/Persistent/ ` [this is a place holder command since arctica isn't finished]

step 8 (arctica setup/5) once above process has completed, remove SD 1.
Repeat steps 5i-7e with SD 2
7b will be
<br>`touch /media/user/TailsData/Persistent/sd2.txt && touch /media/user/TailsData/Persistent/setup2.txt`

step 9 (arctica setup/6) once above process has completed, remove SD 2.
Repeat steps 5i-7e with SD 3
7b will be
<br>`touch /media/user/TailsData/Persistent/sd3.txt && touch /media/user/TailsData/Persistent/setup3.txt`

step 10 (arctica setup/7) once above process has completed, remove SD 3.
Repeat steps 5i-7e with SD 4
7b will be
<br>`touch /media/user/TailsData/Persistent/sd4.txt && touch /media/user/TailsData/Persistent/setup4.txt`

step 11 (arctica setup/8) once above process has completed remove SD 4.
Repeat steps 5i-7e with SD 5
7b will be
<br>`touch /media/user/TailsData/Persistent/sd5.txt && touch /media/user/TailsData/Persistent/setup5.txt`

step 12 (arctica setup/9) once above process has completed remove SD 5.
Repeat steps 5i-7e with SD 6
7b will be
<br>`touch /media/user/TailsData/Persistent/sd6.txt && touch /media/user/TailsData/Persistent/setup6.txt`

step 13 (arctica setup/10) once above process has completed remove SD 6.
Repeat steps 5i-7e with SD 7
7b will be
<br>`touch /media/user/TailsData/Persistent/sd7.txt && touch /media/user/TailsData/Persistent/setup7.txt`

step 14 (arctica setup/11) reboot into SD 1 
<br>You will need to configure your network (wifi if not connected to LAN)
<br>complete the TOR wizard
<br>Open Arctica which will direct you to step 12 of setup via virtual label created in step 7b 

Step 15 (arctica setup/12) insert the setup CD

step 15a
Make automount directories
<br>`mkdir --parents /live/persistence/TailsData_unlocked/dotfiles/.config/autostart`

step15b create the start up file
<br>`cd && cd /live/persistence/TailsData_unlocked/dotfiles/.config/autostart && echo "[Desktop Entry] X-GNOME-Autostart-enabled=true Exec=/udisksctl mount --block-device /dev/nvme0n1p2 Encoding=UTF-8 Version=1.0 Type=Application Name=autostart Terminal=false" > mount_internal.desktop`

step 15c give autostart directories the proper permissions:
<br>`sudo chmod -R 777 /live/persistence/TailsData_unlocked/dotfiles/.config/autostart && sudo chmod +x /live/persistence/TailsData_unlocked/dotfiles/.config/autostart/mount_internal.desktop`

step 15d mount the local disk since we haven't restarted yet
<br>`udisksctl mount --block-device /dev/nvme0n1p2`

step 15e create a .bitcoin dir inside the persistent dotfiles
<br>`mkdir .bitcoin /live/persistence/TailsData_unlocked/dotfiles && cd`

step 15f create symlink to chainstate folder on local disk [your device path will be different]
<br>`cd /live/persistence/TailsData_unlocked/dotfiles/.bitcoin && ln -s /media/amnesia/a988dd30-61b1-49d7-88f4-50b8c450e5c0/.bitcoin/chainstate chainstate`

step 15g create symlink to blocks folder on local disk [your device path will be different]
<br>`ln -s /media/amnesia/a988dd30-61b1-49d7-88f4-50b8c450e5c0/.bitcoin/blocks blocks`

step 15h: generate bitcoin xpriv into an 'encrypted' directory in a non persistent space and export xpub to setup CD (i THINK we can do this without a restart to get our blockdata folders synced with symlink)

step 15i: zip 'encrypted' directory into a tarball, encrypt with gpg (using setup CD ssh key as passphrase) and move the encrypted tarball to the SD card's persistent directory

step 15i remove old virtual label and replace with new virtual label
<br>`cd ~/Persistent && rm -r -f setup1.txt && touch setup8.txt`


step 16 (arctica setup/13) power off, remove SD 1, insert SD 2 and reboot into tails
<br>You will need to configure your network (wifi if not connected to LAN)
<br>complete the TOR wizard
<br>Open Arctica which will direct you to step 14 of setup via virtual label created in step 7b 
<br>repeat all of steps 15a-15i
<br>15i will be
<br>`cd ~/Persistent && rm -r -f setup2.txt && touch setup9.txt`

step 17 (arctica setup/14) power off, remove SD 2, insert SD 3 and reboot into tails
<br>You will need to configure your network (wifi if not connected to LAN)
<br>complete the TOR wizard
<br>Open Arctica which will direct you to step 15 of setup via virtual label created in step 7b 
<br>repeat all of steps 15a-15i
<br>15i will be
<br>`cd ~/Persistent && rm -r -f setup3.txt && touch setup10.txt`

step 18 (arctica setup/15) power off, remove SD 3, remove Setup CD, move to secondary machine, insert SD 4, boot into tails
<br>You will need to configure your network (wifi if not connected to LAN)
<br>complete the TOR wizard
<br>Open Arctica which will direct you to step 16 of setup via virtual label created in step 7b 
<br>repeat all of steps 15a-15i
<br>15i will be
<br>`cd ~/Persistent && rm -r -f setup4.txt && touch setup11.txt`

step 19 (arctica setup/16) insert the set up CD

step 20 (arctica setup/17) power off, remove SD 4, insert SD 5 and reboot into tails
<br>You will need to configure your network (wifi if not connected to LAN)
<br>complete the TOR wizard
<br>Open Arctica which will direct you to step 18 of setup via virtual label created in step 7b 
<br>repeat all of steps 15a-15i
<br>15i will be
<br>`cd ~/Persistent && rm -r -f setup5.txt && touch setup12.txt`

step 21 (arctica setup/18) power off, remove SD 5, insert SD 6 and reboot into tails
<br>You will need to configure your network (wifi if not connected to LAN)
<br>complete the TOR wizard
<br>Open Arctica which will direct you to step 19 of setup via virtual label created in step 7b 
<br>repeat all of steps 15a-15i
<br>15i will be
<br>`cd ~/Persistent && rm -r -f setup6.txt && touch setup13.txt`

step 22 (arctica setup/19) power off, remove SD 6, insert SD 7 and reboot into tails
<br>You will need to configure your network (wifi if not connected to LAN)
<br>complete the TOR wizard
<br>Open Arctica which will direct you to step 20 of setup via virtual label created in step 7b 
<br>repeat all of steps 15a-15i
<br>15i will be
<br>`cd ~/Persistent && rm -r -f setup7.txt && touch setup14.txt`

Step 23 (arctica setup/20) remove SD 7, remove the setup CD, return to primary machine and reboot into SD 1

step 24 (arctica setup/21) insert the set up CD
<br>At this point the Xpubs from all 7 xprivs will be loaded into memory so we can create the descriptor and start making backups of our persistent directory (maybe not the entire thing?)

step 24a remove the virtual label and add a new virtual label to SD 1
<br>`cd ~/Persistent && rm -r -f setup8.txt && touch setup15.txt`

step 25 (arctica setup/22) remove the setup CD

step 26 (arctica setup/23) insert CD 1
<br>creates backup of important stuff (decide what later)

step 27 (arctica setup/24) remove CD 1 and insert DVD 1
<br>creates backup of important stuff (decide what later)

step 28 (arctica setup/25) remove all devices from the machine and place SD 1, CD 1, and DVD 1 into envelope 1.

step 29 (arctica setup/26) power off, insert SD 2, and reboot [this screen may be redundant with previous screen]

step 30 (arctica setup/27a) insert the setup CD (to load the descriptor into memory)

step 31 (arctica setup/27b) remove the setup CD and insert CD 2
<br>creates backup of important stuff (decide what later)

step 32 (arctica setup/28) remove CD 2 and insert DVD 2
<br>creates backup of important stuff (decide what later)

step 32a remove the virtual label from SD 2
<br>`cd ~/Persistent && rm -r -f setup9.txt`

step 33 (arctica setup/29) remove all devices from the machine and place SD 2, CD 2, and DVD 2 into envelope 2. 

step 34 (arctica setup/30) power off, insert SD 3, and reboot [this screen may be redundant with previous screen unless we can load into RAM]

step 35 (arctica setup/31a) insert the setup CD (to load descriptor into memory)

step 36 (arctica setup/31b) remove the setup CD and insert CD 3
<br>creates backup of important stuff (decide what later)

step 37 (arctica setup/32) remove CD 3 and insert DVD 3
<br>creates backup of important stuff (decide what later)

step 37a remove the virtual label from SD 3
<br>`cd ~/Persistent && rm -r -f setup10.txt`

step 38 (arctica setup/33) remove all devices from the machine and place SD 3, CD 3, and DVD 3 into envelope 3. 

step 39 (arctica setup/34) power off, switch to secondary machine, insert SD 4, and reboot [this screen may be redundant with previous screen unless we can load into RAM]

step 40 (arctica setup/35a) insert the setup CD (to load descriptor into memory)

step 41 (arctica setup/35b) remove setup CD and insert CD 4
<br>creates backup of important stuff (decide what later)

step 42 (arctica setup/36) remove CD 4 and insert DVD 4
<br>creates backup of important stuff (decide what later)

step 42a remove the virtual label from SD 4
<br>`cd ~/Persistent && rm -r -f setup11.txt`

step 43 (arctica setup/37) remove all devices from the machine and place SD 4, CD 4, and DVD 4 into envelope 4. 

step 44 (arctica setup/38) power off, insert SD 5, and reboot [this screen may be redundant with previous screen unless we can load into RAM]

step 45 (arctica setup/39a) insert the setup CD (to load descriptor into memory)

step 46 (arctica setup/39b) remove the setup CD and insert CD 5
<br>creates backup of important stuff (decide what later)

step 47 (arctica setup/40) remove CD 5 and insert DVD 5

step47a remove the virtual label from SD 5
<br>`cd ~/Persistent && rm -r -f setup12.txt`

step 48 (arctica setup/41) remove all devices from the machine and place SD 5, CD 5, and DVD 5 into envelope 5. 

step 49 (arctica setup/42) power off, insert SD 6, and reboot [this screen may be redundant with previous screen unless we can load into RAM]

step 50 (arctica setup/43a) insert the setup CD (to load descriptor into memory)

step 51 (arctica setup/43b) remove the setup CD and insert CD 6
<br>creates backup of important stuff (decide what later)

step 52 (arctica setup/44) remove CD 6 and insert DVD 6

step 52a remove the virtual label from SD 6
<br>`cd ~/Persistent && rm -r -f setup13.txt`

step 53 (arctica setup/45) remove all devices from the machine and place SD 6, CD 6, and DVD 6 into envelope 6. 

step 54 (arctica setup/46) power off, insert SD 7, and reboot [this screen may be redundant with previous screen unless we can load into RAM]

step 55 (arctica setup/47a) insert the setup CD (to load descriptor into memory)

step 56 (arctica setup/47b) remove the setup CD and insert CD 7
<br>creates backup of important stuff (decide what later)

step 57 (arctica setup/48) remove CD 7 and insert DVD 7

step 57a remove the virtual label from SD 7
<br>`cd ~/Persistent && rm -r -f setup14.txt`

step 58 (arctica setup/49a) remove all devices from the machine and place SD 7, CD 7, and DVD 7 into envelope 7.

step 59 (arctica setup/49b) destroy your setup CD

step 60 (arctica setup/50a) return to primary machine, insert SD 1 and reboot into tails

step 61 (arctica setup/50b) finish blockchain sync

step 61a remove virtual label from SD 1
<br>`cd ~/Persistent && rm -r -f setup15.txt`

step 62 (arctica setup/51) initial sync complete, you can now log in



Notes: Shamir Secret Sharing
<br>`sudo apt-get update`
<br> `sudo apt-get install libgfshare-bin `

OR

create a 3 of 5 scheme with Shamir secret sharing
<br> `sudo apt install ssss`
<br>`ssss-split -t 3 -n 5`
enter the secret
<br>`********`
<br> `ssss-combine -t 3`


