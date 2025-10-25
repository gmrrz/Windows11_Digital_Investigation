# Forensic Imaging Process

## Imaging System Setup

- Laptop/Workstation: Lenovo ThinkPad E16 Gen 2
- Imager: Raspberry Pi 4, 4GB RAM
- OS: Raspberry Pi OS 64-bit
- Write blocker: Raspberry Pi 4, 4GB RAM - Open-source write-blocker (https://github.com/gmrrz/Rasp-Pi-Writer-Blocker)
- Imaging tools: dcfldd
- Hashing tools: SHA256 via dcfldd
- Network: System was offline during imaging for security purposes

## Lab Notes

| Time (UTC) | Action | Notes | Command Ref |
|------------|--------|-------|-------------|
| 10:30 PM | Arrived at lab | Took pictures of the SSDs | - |
| 10:45 PM | Chain of custody | Wrote the CoC file | - |
| 10:56 PM | Connect drives | Evidence drive (/dev/sdb) and copy drive (/dev/sda) connected | - |
| 10:58 PM | Check read-only status | Verified both drives read-only | C1 |
| 11:01 PM | Ready to start imaging | Both drives confirmed read-only | - |
| 11:27 PM | Troubleshooting | Destination drive (/dev/sda) needed to be writable; evidence drive remains read-only | - |
| 11:30 PM | Make destination writable | Destination now writable | C2 |
| 11:35 PM | Start imaging | Begin cloning and hashing | C3 |

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

### Status Table

| Drive    | Role             | Status                     |
| -------- | ---------------- | -------------------------- |
| /dev/sdb | Evidence         | Read-only (write-blocked)  |
| /dev/sda | Destination copy | Writable (receiving image) |

---

**Note:** Gloves were worn while handling the evidence. Briefly removed for typing commands; evidence itself was not touched with bare hands.
