# arctica-install-companion

Note: lettered steps are to be manually completed in the terminal but will eventually be scripts executed as user interacts with arctica GUI

step 1: Start on a fresh ubuntu installation on a dedicated laptop (this laptop will be your full node and ideally will have a fairly new 500gb internal SATA SSD

step 1a: [may remove or move this step] install bitcoin core to the internal disk and start syncing, make sure that ~/.bitcoin is where blockdata and chainstate are being stored, this will be important later

step 1b: create your .bitcoin directory inside of root
<br>`mkdir .bitcoin `

step 1c: create chainstate and blocks directories inside of .bitcoin, these will be symlinked later
<br>`mkdir chainstate ~/.bitcoin && mkdir blocks ~/.bitcoin`

step 2 (arctica setup/2): set up password

step 3 (arctica setup/2): confirm password

step 4 (arctica setup/3): label your 8 CDs, 7 DVDs, 7 SD cards (or usbs) and 7 envelopes

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

step 15h generate bitcoin xpriv, save to persistent and export xpub to setup CD (i THINK we can do this without a restart to get our blockdata folders synced with symlink)

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
