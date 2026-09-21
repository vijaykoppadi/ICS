https://haveibeenpwned.com/PwnedWebsites

https://meet.google.com/ejt-yjiw-zkr

https://github.com/CoderBlee/Configuring-a-Honeypot-with-PentBox-on-Kali-Linux

https://drive.google.com/drive/folders/17YVbjFna_79fDBigOMd37J2oi-HABjNS?usp=sharing


1. Cancel/leave this Autopsy error page

Open a new Terminal in Kali.

2. Create a new 100 MB raw disk image

Run:

cd ~/Downloads
dd if=/dev/zero of=email_evidence.dd bs=1M count=100
3. Create a partition table

Run:

sudo fdisk email_evidence.dd

Inside fdisk, enter these keys one by one:

o
n
p
1
<Enter>
<Enter>
w

Meaning:

o → create DOS partition table
n → new partition
p → primary
1 → partition number 1
First <Enter> → accept default first sector
Second <Enter> → accept default last sector
w → write changes
4. Create a loop device

Run:

sudo losetup --find --show --partscan ~/Downloads/email_evidence.dd

It should return something like:

/dev/loop0

Your number may be different, so use the number that Kali actually displays.

Then check:

lsblk

You should see something similar to:

loop0
└─loop0p1
5. Format the partition

If your loop device is /dev/loop0, run:

sudo mkfs.ext4 /dev/loop0p1

If Kali gave you /dev/loop1, use:

sudo mkfs.ext4 /dev/loop1p1
6. Mount it
sudo mkdir -p /mnt/email_evidence

Then, using your actual loop number:

sudo mount /dev/loop0p1 /mnt/email_evidence
7. Copy the email into the forensic image

Your existing email is:

~/Downloads/sample_email.eml

Copy it:

sudo cp ~/Downloads/sample_email.eml /mnt/email_evidence/

Check:

ls -l /mnt/email_evidence/

You should see:

sample_email.eml
8. Unmount everything

First:

sudo umount /mnt/email_evidence

Then:

sudo losetup -d /dev/loop0

Again, use your actual loop number.

9. Add the new image to Autopsy

Go back to the Autopsy Add New Image screen.

For Location, enter:

/home/kali/Downloads/email_evidence.dd

Select:

Type → Disk

Select:

Import Method → Symlink

Then click Next.
