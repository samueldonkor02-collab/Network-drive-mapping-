# Network-drive-mapping-
Reviewed a mapped SMB network share on Windows 11 and found sensitive files like certificates and personal documents mixed in with casual photo storage, no separation or visible access controls.

# Network Share Review (Windows SMB Share Exposure Check)

`Windows 11` `File Explorer` `Mapped Network Drives` `SMB Shares` `Data Exposure`

## Overview
This section covers reviewing a mapped network share on a Windows 11 machine to see what it exposes and whether that exposure is appropriate. The share in question is a folder called family photos, reachable over SMB at \\localhost and mapped locally as the Z: drive.

## Objective
Get a feel for how easy it can be to browse into a network share and immediately see what is sitting on it, and think through what that means from a security standpoint when the share holds more than just photos.

## Environment
* Machine: Windows 11 desktop
* Share: family photos (\\localhost), mapped as Z:
* Drive shows 369 GB free of 465 GB total

## What I Did

### Checking This PC
Opened File Explorer and looked at This PC to see everything currently attached to the machine. Alongside the local OS drive, two optical drives, and a backup drive, there was a network location called family photos (\\localhost) (Z:) sitting right there in the same view as the local disks, with no visual indicator that it is remote rather than local storage.

### Browsing Into the Share
Opened the Z: drive and found it was not just photos. The listing had 191 items total, including several subfolders and a long list of loose files sitting at the root. Alongside expected things like a family photos folder and a Zoom folder, the root also contained items like PDF certificates, a car sale photo, an OpenDocument presentation, and a one page summary document, all mixed together with no separation between personal, financial, or otherwise sensitive material and everyday photo files.

### Thinking Through the Exposure
Because the share is mapped and mounted the same way a local drive would be, anyone with access to that machine, or anyone else on the network who can reach \\localhost with the right share name, gets the same flat view. There was nothing in the interface itself that hinted at folder level permissions or that separated the more sensitive files like certificates from the casual ones like vacation photos.

## What's in This Section

```
screenshots/
01 this pc drives view.png (This PC, showing the mapped Z drive alongside local disks)
02 family photos share contents.png (Z drive root, 191 items including personal and sensitive files)
```

## Skills I Picked Up
Recognizing a mapped network share in the This PC view and telling it apart from local storage.
Thinking about data exposure from the perspective of what a share reveals the moment someone opens it, rather than just whether it required a password to connect.
Noticing how mixing sensitive files like certificates and financial documents into a general purpose folder like family photos increases the blast radius if that share is ever reached by the wrong person.

## How This Applies in the Real World
This is close to what a security analyst looks for during an internal assessment or a home network review. Open or loosely permissioned SMB shares are a common finding, and the risk usually is not that the share was hacked, it is that it was simply left reachable and full of files that were never meant to be grouped together. Separating sensitive documents from general file storage and tightening share level permissions are both quick, practical fixes that come out of an exercise like this one.

## Limitations
This was a visual review from the client side rather than an actual permissions audit. It does not confirm who else on the network can reach the share, what NTFS or share level permissions are actually set, or whether the share requires authentication for other users.

## References
Microsoft SMB File Sharing Overview: https://learn.microsoft.com/windows-server/storage/file-server/smb-overview
Windows File and Printer Sharing Documentation: https://support.microsoft.com/windows
