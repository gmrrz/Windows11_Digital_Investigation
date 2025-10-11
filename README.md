# Windows Digital Forensics Investigation

## Overview

- This project demonstrates a digital forensic investigation conducted in a Windows 11 environment. The goal is to simulate the process of collecting, preserving, and analyzing digital evidence in a controlled setting.

## Environment Setup
- Host OS: Windows 11/ParrotOS(Linux)
- Target Device: ThinkPad T420s
- Guest OS(Target): Windows 11
- Investigator Tools:
    - FTK Imager, Autopsy, Paladin, hash utilities
- Storage for images & Logs: write-protected external drive or dedicated evidence server

## Project Idea / Scenario

- Use the ThinkPad for several days while intentionally downloading various files(images, documents, installers)
- Deleted selected files and perform actions that produce artifacts (open files, browse websites, install an app, plug USB devices)
- Stage a "crime scene", suspicious downloads, potential downloads, potential data exfiltration, or unauthorized software installation.
- Respond as an investigator arriving on scene, documenting, collecting, preserving, imaging, and analyzing evidence per LE procedures.

## Investigation Plan

### Phase 1 - Preparation & Documentation

- Define objectives: what you want to prove/find (deleted files, web history, USB usage, installed malware).
- Collect baseline info: device make/model, serial number, logged-on user, network connections (if live).
- Photograph the scene: laptop closed/open, ports, connected devices, visible stickers/serial numbers.
- Record chain of custody (CoC): who handled evidence, when, where, why.

### Phase 2 - Evidence Acquisition (Imaging)

- Goal: create a forensically sound bit‑stream image and verify integrity with hashes.

- Preferred workflow (forensic best practice)
    - Remove the target drive if available and image it offline. If not possible, use a write‑blocker or live imaging with caution.
    - Use FTK Imager (GUI) or dd/dcfldd (Linux) to create an image.
    - Calculate hashes (MD5 and SHA256) before and after imaging to prove integrity.

    - Important: store original device (if removed) in evidence bag, label with date/time/initials. Never write to the original device.

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
