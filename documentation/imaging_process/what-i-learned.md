# Forensic Process Feedback Notes

# Forensic Imaging Lab (Educational)

> This repository documents my personal learning in digital forensics.
> No real evidence, devices, or sensitive data were used.
> All disk images and identifiers are simulated or educational.

> “Don’t give your opposition the rope they need to hang you.”

This feedback came from a professional forensic analyst in response to my student lab.  
The notes below summarize the lessons and principles that will guide my future digital forensics work.

---

## Key Lessons

### 1. Documentation Philosophy
- **Good notes are concise.**  
  Overly detailed documentation can introduce unnecessary risk — every detail is a potential point of contradiction.  
- **Rely on logs, not narration.**  
  Imaging tools already record times, sectors, and hashes. Don’t duplicate them in prose form.  
- Avoid recording irrelevant information such as when you entered the lab, when imaging began, or when checks were made — this is all present in `.log` or `.E01.txt` files.

---

### 2. Hashing and Verification
- **Never hash the original evidence after imaging.**  
  Drives may change state even when write-blocked (especially SSDs).  
- The correct process is **stream verification** — hashing the data stream as it’s imaged.  
- The built-in verification of tools like `dcfldd` or `Guymager` compares the stream used to create the image, ensuring integrity.
- Post-hash comparison (e.g., `sha256sum /dev/sda`) can produce false mismatches.

---

### 3. Write Blocking
- **Software write blockers** (e.g., `blockdev --setro`, `mount -o ro`) are *educational only*.  
  They can be overridden and lack verification logs.  
- **Hardware write blockers** are the professional standard. They physically intercept write signals and record all activity.  
- Always document which type was used.

---

### 4. Imaging Tools
- `dd` is not ideal for forensics — it provides no logging or verification.  
- `dcfldd` improves upon `dd` with:
  - Logging support (`hashlog`, `verifylog`)
  - Stream hashing and verification
  - Split output and progress display
- Professionals prefer `dcfldd`, `Guymager`, or `FTK Imager` for their verifiable audit trails.

---

### 5. Reporting and Risk Awareness
- Avoid recording failed attempts (e.g., “I messed up the image and retried”).  
  Just reimage properly — that’s the professional response.  
- Keep reports short, factual, and verifiable.
- Remember that your documentation may one day be scrutinized in court.  
  Minimal, accurate records reduce the chance of self-contradiction.

---

## Professional Habits to Adopt

| Practice | Rationale |
|-----------|------------|
| Rely on logs as the factual record | Prevents inconsistency |
| Avoid unnecessary timestamps | Reduces irrelevant noise |
| Record tool version numbers | Ensures repeatability |
| Keep documentation factual, not narrative | Maintains credibility |
| Reimage when errors occur | Protects integrity |
| Always hash during imaging | Prevents later discrepancies |

---

## Summary

This feedback reshaped how I approach forensic work:
- Focus on **forensic soundness**, not exhaustive description.  
- Trust the **tools and logs** as your objective witnesses.  
- Remember that **every extra sentence in a forensic report is potential rope** — so write with precision and restraint.

