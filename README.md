P A R T  1 !!
Project Goal
Conduct a digital forensic investigation of the Jack_Mac.img disk image to identify signs of macOS system compromise, methods of persistence, and the actions carried out by the attacker.

Investigation Summary
Step 1. Working with the Disk Image
The disk image Jack_Mac.img (32 GB) was obtained.

GPT partition structure analyzed via fdisk:

sudo /sbin/fdisk -l Jack_Mac.img
Result:
Jack_Mac.img2  Start: 1024040, Size: 55599104 — this is the APFS partition
Attempted direct mounting:

sudo apfs-fuse -o offset=524300800 Jack_Mac.img /mnt/mac2
But: apfs-fuse does not support the -o offset option → switched to plan B.

Attempted to extract the APFS partition using dd:

sudo dd if=Jack_Mac.img of=MacSystem.apfs bs=512 skip=1024040 count=55599104 status=progress
Issue: No space left on device error — due to limited disk space on the TryHackMe VM.

Step 2. Insights Based on Errors
Despite accurate work with GPT structure and APFS mounting tools, the TryHackMe VM has strict disk space limits, which prevent a full analysis:

Extracting and mounting the ~26 GB APFS partition is not feasible

Even basic copying of the partition results in write errors

Skills & Tools Applied
Skill / Tool	Description
GPT Analysis (fdisk)	Identified correct partition boundaries
7z Archiver	Inspected image structure and contents
dd Command	Low-level extraction of partition data
apfs-fuse	Attempted mounting of APFS file system
Permissions Management	Used sudo to work around access limitations
System Error Handling	Handled disk space issues (ENOSPC) logically
