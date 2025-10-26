# Windows Digital Forensics Investigation

<img src="thumbnail.png"/>

## Overview

- This project demonstrates a digital forensic investigation conducted in a Windows 11 environment. The goal is to simulate the process of collecting, preserving, and analyzing digital evidence in a controlled setting.

> Note: This lab reflects my early learning process in forensic imaging and verification.
> Feedback was incorporated to improve documentation and methodology.
> All experiments were performed on educational/test devices only.

## Environment Setup

- **Host OS:** Windows 11 / Linux  
- **Target Device (Evidence):** ThinkPad T420s  
- **Guest OS (Target VM):** Windows 11  

- **Investigator Tools:**  
    - Autopsy  
    - Paladin  
    - Hash utilities (`sha256sum`)  
    - [Forensic Duplicator](https://github.com/gmrrz/Rasp-Pi-Writer-Blocker)  
    - 2× SSD to USB-A Male converters  
    - Camera (for evidence documentation)  
    - Lenovo ThinkPad E16 Gen 2 (investigator workstation / analysis notes / CoC)  
    - Destination SSD for image storage  
    - `dd` (for imaging)  

- **Storage for Images & Logs:**  
    - Write-protected external drive  
    - Dedicated evidence server (if available)  

## Project Idea / Scenario

- Use the ThinkPad for several days while intentionally downloading various files(images, documents, installers)
- Deleted selected files and perform actions that produce artifacts (open files, browse websites, install an app, plug USB devices)
- Stage a "crime scene", suspicious downloads, potential downloads, potential data exfiltration, or unauthorized software installation.
- Respond as an investigator arriving on scene, documenting, collecting, preserving, imaging, and analyzing evidence per LE procedures.
- Laptop usage from (10/12/2025)

## Investigation Plan

### Phase 1 - Preparation & Documentation

- Define objectives: what you want to prove/find (deleted files, web history, USB usage, installed malware).
- Collect baseline info: device make/model, serial number, logged-on user, network connections (if live).
- Photograph the scene: laptop closed/open, ports, connected devices, visible stickers/serial numbers.
- Record chain of custody (CoC): who handled evidence, when, where, why.

### Phase 2 – Evidence Acquisition (Imaging)

- **Goal:** Create a forensically sound bit‑stream image of the evidence drive and verify integrity with SHA256 hashing.

- **Workflow followed:**
  - Connected the evidence drive (/dev/sda) to a Raspberry Pi 4 via a write-blocker, keeping it read-only at all times.
  - Connected the destination drive (/dev/sdb) as the copy; initially read-only, later made writable to receive the image.
  - Verified read-only status of both drives using blockdev.
  - Performed imaging using dd, **hashed both the original evidence and the image using SHA256**.
  - Monitored progress and logged start/end times.
  - Stored the original evidence drive safely in its evidence bag when finished; it remained read-only throughout.

- **Integrity verification:**
  - SHA256 hashes match between the original and image.
  - Destination copy is now available for analysis without altering the original evidence.
  - **Important:** Only one image was created for this educational exercise due to storage limitations.

### Phase 3 - Verification & Preservation

- Verify image hashes match source hashes (if you hashed the source).
- Make at least two copies of the image and store separately (evidence redundancy).
- Maintain CoC log entries for each transfer/handling event.

### Phase 4 - Analysis (Autopsy / Autopsy modules)
- Open forensic image in Autopsy.
- Run modules: File Type Identification, Timeline Analysis, Web History, Recycle Bin/Deleted Files, USB/Registry Artifacts.
- Extract relevant artifacts for the report.

### Phase 5 - Reporting & Presentation
- Document methodology, tools, and findings.
- Include screenshots of recovered artifacts and hash verification.
- Prepare chain-of-custody and timeline for final submission.
