# Windows Digital Forensics Investigation

## Overview

- This project demonstrates a digital forensic investigation conducted in a Windows 11 enviornment. The goal is to simulate the process of collecting, preserving, and analyzing digital evidence in a controlled setting.

## Environment Setup
- Host OS: Windows 11/ParrotOS(Linux)
- Target Device: ThinkPad T420s
- Guest OS(Target): Windows 11
- Investigator Tools:
    - FTK Imager / Autopsy / Paladin / hash utilities
- Storage for images & Logs: write-protected external drive or dedicated evidence server

## Project Idea / Scenario

- Use the ThinPad for several days while interntionall downloading various files(images, documents, installers)
- Deleted selcted files and perform actions that produce artifacts (open files, browse websites, install an app, plug USB devices)
- State a "crime scene", suspicious downloads, potential downloads, potential data exfiltration, or unauthorized software installation.
- Respond as an investigator arriving on scene, documenting, collecting, preserving, imaging, and analyzing evidence per LE procedures.

## Investigation Plan

##

- Define objectives: what you want to prove/find (deleted files, web history, USB usage, installed malware).
- Collect baseline info: device make/model, serial number, logged-on user, network connections (if live).
- Photograph the scene: laptop closed/open, ports, connected devices, visible stickers/serial numbers.
- Record chain of custody (CoC): who handled evidence, when, where, why.