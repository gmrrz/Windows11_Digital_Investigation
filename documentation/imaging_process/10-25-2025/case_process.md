# Forensic Imaging Process

## Imaging System Setup

- **Laptop/Workstation:** Lenovo ThinkPad E16 Gen 2  
- **Imager:** Raspberry Pi 4 (4GB RAM)  
- **Operating System:** Raspberry Pi OS 64-bit  
- **Forensic Duplicator:** Raspberry Pi Write Blocker (Open-source: [gmrrz/Rasp-Pi-Writer-Blocker](https://github.com/gmrrz/Rasp-Pi-Writer-Blocker))  
- **Imaging Tool:** `dd`  
- **Hashing Tool:** `sha256sum`  
- **Network Status:** Offline during imaging for data integrity and security  

---

## Lab Notes

| Time (UTC) | Action | Notes | Command Ref |
|-------------|--------|--------|--------------|
| 6:35 PM (10/25/2025) | Arrived at the lab and reviewed previous imaging issue. Realized the previous process created a bit-for-bit clone instead of a forensic image. Planned to create a `.dd` image file and verify integrity through hashing. | - |
| 6:44 PM (10/25/2025) | Verified both disks were correctly identified and confirmed read-only status. Made `/dev/sdb` writable to mount it as the destination. | C1 |
| 6:54 PM (10/25/2025) | Began imaging the evidence disk to the mounted destination using `dd`. Hashing will be performed afterward for verification. | C2 |
| 9:06 PM (10/25/2025) | Imaging completed successfully. Proceeded to compare the hashes of the original evidence disk and the created image. | C3 |
| 11:02 PM (10/25/2025) | Finished verifying hashes — both matched, confirming the image’s integrity. Noted that additional verification could be done with alternate imaging tools if storage allowed. | C4 |
| 11:10 PM (10/25/2025) | Imaging process concluded. Evidence was re-bagged in an anti-static evidence bag for secure storage. Analysis will be performed on a separate system. | - |

---

## Commands / Outputs

### C1: Checking writability & changing permissions
```bash
sudo blockdev --getro /dev/sda
# Output: 1
sudo blockdev --getro /dev/sdb
# Output: 1

sudo blockdev --setrw /dev/sdb
# Destination drive is now writable

sudo mkdir /mnt/dest
sudo mount /dev/sdb2 /mnt/dest
# Mounted destination disk for image storage
```
### C2: Imaging evidence to mounted destination
```bash
sudo dd if=/dev/sda of=/mnt/dest/evidence_image.dd bs=4M status=progress
```
### C3: Comparing hashes for verification
```bash
sha256sum /dev/sda
sha256sum /mnt/dest/evidence_image.dd
```
### C4: Unmount and restore read-only status
```bash
sudo umount /mnt/dest
sudo blockdev --setro /dev/sdb
# Destination drive returned to read-only state
```

---

### Drive Status Summary

| Drive    | Role             | Status                    |
| -------- | ---------------- | ------------------------- |
| /dev/sda | Evidence         | Read-only (write-blocked) |
| /dev/sdb | Destination copy | Writable (during imaging) |


Note: Gloves were worn during evidence handling. The evidence drive was not directly touched with bare hands. The entire imaging process was conducted on an isolated system to maintain forensic integrity.