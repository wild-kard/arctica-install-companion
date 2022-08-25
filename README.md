# arctica-install-companion

Note: lettered steps are to be manually completed in the terminal but will eventually be scripts executed as user interacts with arctica GUI

step 1: Start on a fresh ubuntu installation on a dedicated laptop (this laptop will be your full node and ideally will have a fairly new 500gb internal SATA SSD

step 1a: install bitcoin core to the internal disk

step 2 (arctica setup/2): set up password

step 3 (arctica setup/2): confirm password

step 4 (arctica setup/3): label your 8 CDs, 7 DVDs, 7 SD cards (or usbs) and 7 envelopes

step 5a: import the tails signing key and add to the GnuPGP keyring 
`wget https://tails.boum.org/tails-signing.key gpg --import < tails-signing.key`

step 5b: install the debian keyring 
`sudo apt update && sudo apt install debian-keyring`

step 5c: import the openPGP key of Chris Lamb (former Debian lead) 
`gpg --keyring=/usr/share/keyrings/debian-keyring.gpg --export chris@chris-lamb.co.uk | gpg --import`

step 5d: verify certifications made on tails signing key 
`gpg --keyid-format 0xlong --check-sigs A490D0F4D311A4153E2BB7CADBB802B258ACD84F`

You are looking for something like 
`sig! 0x1E953E27D4311E58 2020-03-19 Chris Lamb <chris@chris-lamb.co.uk>`

step 5e: certify the tails signing key 
`gpg --lsign-key A490D0F4D311A4153E2BB7CADBB802B258ACD84F`

step 5f: download tails 
`wget --continue http://dl.amnesia.boum.org/tails/stable/tails-amd64-5.3.1/tails-amd64-5.3.1.img`

step 5g: download signature of the usb image 
`wget https://tails.boum.org/torrents/files/tails-amd64-5.3.1.img.sig` 

step 5h: verify the usb image is signed with the tails signing key 
`TZ=UTC gpg --no-options --keyid-format long --verify tails-amd64-5.3.1.img.sig tails-amd64-5.3.1.img`

Output should be something like (Verify that the date of the signature is the same and the signature is marked as Good signature since you certified the Tails signing key with your own key.) 
`gpg: Signature made Tue 02 Aug 2022 12:07:47 AM UTC gpg: using EDDSA key CD4D4351AFA6933F574A9AFB90B2B4BD7AED235F gpg: Good signature from "Tails developers (offline long-term identity key) <tails@boum.org>" [full] gpg: aka "Tails developers <tails@boum.org>" [full]`

step 5i: Select the device to be flashed (need to prompt user here and decide this dynamically eventually). For now...

 return a list of devices on the system 
 `ls -1 /dev/sd?`


step 6 (arctica setup/4): insert SD/USB 1. 

step 6a: identify the newly inserted device
`ls -1 /dev/sd?`

step 7: DD the appropriate storage device with the downloaded tails image (change the `of=/dev/sda` below to your target device)
`sudo dd if=tails-amd64-5.3.1/tails-amd64-5.3.1.img of=/dev/sda bs=16M oflag=direct status=progress`

step 7a: run the `setup-persistence.sh` script
(i haven't tested this outside of tails yet)
`sudo bash setup-persistence.sh`
