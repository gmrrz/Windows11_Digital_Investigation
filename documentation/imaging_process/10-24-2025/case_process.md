# Forensic Imaging Process

## Imaging System Setup

- **Laptop/Workstation:** Lenovo ThinkPad E16 Gen 2  
- **Imager:** Raspberry Pi 4 (4GB RAM)  
- **Operating System:** Raspberry Pi OS 64-bit  
- **Forensic Duplicator:** Raspberry Pi Write Blocker (Open-source: [gmrrz/Rasp-Pi-Forensic-Duplicator](https://github.com/gmrrz/Rasp-Pi-Writer-Blocker))  
- **Imaging Tool:** `dcfldd`  
- **Hashing Tool:** SHA256 via `dcfldd`  
- **Network Status:** Offline during imaging for data integrity and security  

---

## Lab Notes

| Time (UTC) | Action | Notes | Command Ref |
|-------------|--------|--------|--------------|
| 10:30 PM (10/24/2025) | Arrived at lab | Photographed the SSDs for documentation. | - |
| 10:45 PM (10/24/2025) | Chain of custody | Completed and signed chain of custody form. | - |
| 10:56 PM (10/24/2025) | Connect drives | Connected evidence drive `/dev/sdb` and destination drive `/dev/sda` to the Raspberry Pi. | - |
| 10:58 PM (10/24/2025) | Check read-only status | Verified both drives were initially set to read-only. | C1 |
| 11:01 PM (10/24/2025) | Ready to start imaging | Confirmed both drives’ status; prepared for acquisition. | - |
| 11:27 PM (10/24/2025) | Troubleshooting | Made destination drive `/dev/sda` writable to allow imaging; evidence `/dev/sdb` remained read-only. | - |
| 11:30 PM (10/24/2025) | Make destination writable | Changed write permissions on `/dev/sda`. | C2 |
| 11:35 PM (10/24/2025) | Start imaging | Began cloning and hashing process using `dcfldd`. | C3 |
| 1:26 AM (10/25/2025) | Finished imaging | Initial imaging process completed successfully. | - |
| 1:57 AM (10/25/2025) | Hash verification | Performed SHA256 hash checks and re-enabled read-only mode on `/dev/sda`. | C4 |
| 4:46 AM (10/25/2025) | Hash result | Hash values between evidence and copy did not match. This is likely due to destination drive’s larger capacity and additional unused space. | - |
| 5:02 AM (10/25/2025) | End of current session | Imaging and verification completed for initial attempt. Plan to reimage evidence drive on 10/26/2025 using a destination of equal capacity to confirm matching hashes. | - |
| 6:35 PM (10/25/2025) | I got to the lab and I realized what the issue was. I was doing a bit for bit clone of the disk drive and not a image. Now I will mount the source and put a dd image there - compare the hashes - then unmount it and start analyzing | - |



---

## Commands / Outputs

### C1: Check read-only status
```bash
sudo blockdev --getro /dev/sda
# Output: 1
sudo blockdev --getro /dev/sdb
# Output: 1
```
### C2: Make destination writable
```bash
sudo blockdev --setrw /dev/sda
sudo blockdev --getro /dev/sda
# Output: 0
```
### C3: Start imaging with hash
```bash
sudo dcfldd if=/dev/sdb of=/dev/sda hash=sha256 hashlog=/home/pi/hash.log
```
### C4: Turn read-only mode back on and verify hashes
```bash
sudo blockdev --setro /dev/sda
sudo blockdev --setro /dev/sdb

# Verify hash
sudo sha256sum /dev/sdb > /home/pi/verified_hash.log
sudo sha256sum /dev/sda >> /home/pi/verified_hash.log
cat /home/pi/verified_hash.log

# Result: Hashes differ due to drive size discrepancy. Reimage scheduled.
```

### Status Table
| Drive    | Role             | Status                    |
| -------- | ---------------- | ------------------------- |
| /dev/sdb | Evidence         | Read-only (write-blocked) |
| /dev/sda | Destination copy | Writable (for imaging)    |

### Summary

- Imaging was performed from /dev/sdb (evidence) to /dev/sda (destination) using dcfldd.

- The evidence drive remained write-protected during the entire process.

- The destination drive was temporarily made writable for imaging, then returned to read-only.

- SHA256 hashing was used for verification; hash values did not match due to differing disk capacities (destination drive larger than source).

- No indications of data corruption or alteration were observed.

- The imaging process was conducted offline to maintain forensic integrity.

Next Steps:

- Reimage the evidence drive on 10/25/2025 using a destination drive of equal capacity.

- Store both drives in secure evidence storage until reimaging is performed.

- Update chain of custody and process documentation following reimaging.
