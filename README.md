# 💣 ZIP BOMB — ⚠️ DO NOT EXTRACT ⚠️ 💣

> 📁 A collection of **zip bomb** archives designed for **educational and research purposes only**.
> These files will **expand to hundreds of terabytes or petabytes** and will **destroy your storage** if extracted carelessly.

---

## 🎯 Overview

A **zip bomb** (also called a **decompression bomb** or **zipper bomb**) is a malicious archive file that is **highly compressed** but decompresses to an **enormous size**, overwhelming the memory, CPU, or disk space of the system attempting to extract it.

This repository contains two zip bomb variants:

| 📄 File        | 🗜️ Compressed (on disk) | 📂 Entries    | 💨 Uncompressed (claimed) | ⚡ Ratio   |
|----------------|--------------------------|---------------|----------------------------|-----------|
| `zblg.zip`     | 9.44 MB                  | 65,534        | ~256 TB                    | ~28,000:1 |
| `zbxl.zip`     | 43.75 MB                | 190,023       | ~4.5 PB                    | ~98,000:1 |

> 🛑 **WARNING:** Opening either file — even viewing its contents in some archive managers — can **freeze your system** or **fill your disk to 100%**. **NEVER** attempt to extract these files.

---

## 📦 Repository Contents

```
ZIPBOMB/
├── 💣 zblg.zip    —  Zip Bomb (Large)   —  9.44 MB → 256 TB
├── 💣 zbxl.zip    —  Zip Bomb (XL)      —  43.75 MB → 4.5 PB
└── 📄 README.md   —  ⭐ Star this repo!
```

---

## 🔍 Detailed Technical Analysis

### 📁 zblg.zip — "Zip Bomb Large"

| Metric                  | Value                          |
|-------------------------|--------------------------------|
| File size on disk       | 9,893,525 bytes (~9.44 MB)     |
| Number of entries       | 65,534                         |
| Compression method      | Deflate (Type 8)               |
| Total uncompressed data | 281,395,456,244,934 bytes (~256 TB) |
| Each entry size         | ~4,294,967,240 bytes (~4.0 GB) |
| Each entry compressed   | 6,666,128 bytes (~6.35 MB)     |
| Construction technique  | **Overlapped entries** ⚠️     |

🔐 **Detection:** Python's `zipfile` module refuses to extract this file, raising:

```python
zipfile.BadZipFile: Overlapped entries: '0' (possible zip bomb)
```

This confirms the archive uses **overlapping compressed data** — multiple entries point to the *same* compressed bytes in the file, each claiming a ~4 GB decompression. The result: a tiny 9.44 MB file that claims to decompress to **256 TB**.

### 📁 zbxl.zip — "Zip Bomb Extra Large"

| Metric                  | Value                             |
|-------------------------|-----------------------------------|
| File size on disk       | 45,876,952 bytes (~43.75 MB)     |
| Number of entries       | 190,023                           |
| Compression method      | Deflate (Type 8)                 |
| Total uncompressed data | 4,507,981,427,706,459 bytes (~4.5 PB) |
| Each entry size         | ~23,728,433,572 bytes (~22 GB)   |
| Each entry compressed   | ~34,144,733 bytes (~32.5 MB)      |
| Construction technique  | **Overlapped entries** ⚠️         |

💀 This is one of the most dangerous zip bombs in existence. A mere **43.75 MB** download expands to claim **4.5 petabytes** — enough to **obliterate any consumer or many enterprise storage systems**.

---

## 💥 How Zip Bombs Work

Zip bombs exploit the **compression ratio gap** between compressed and uncompressed data. Here's the attack chain:

1. 📦 **Highly compressible data** — The archive stores data that compresses extremely well (e.g., repetitive bytes, sparse patterns).
2. 🔄 **Overlapped entries** — Multiple zip entries reference the *same* small compressed payload, but each reports a massive uncompressed size in its header.
3. 💨 **Exponential expansion** — When an extractor decompresses each entry, it produces the claimed (huge) output, consuming disk space, memory, and CPU.
4. 🧨 **Resource exhaustion** — The target system runs out of disk/memory/swap, causing crashes, freezes, or data corruption.

> 🔬 **Fun fact:** Python's standard library (`zipfile`) includes built-in zip bomb detection (since Python 3.6.2 via `ZIP_SMALLER` and entry overlap checks) and will **refuse** to extract these archives — proving they are genuine, dangerous bombs.

---

## 🧮 The 42.zip Mathematics

The classic **42.zip** bomb (42,374 bytes) inspired these variants. It uses **16 layers** of nested zips, each containing **16 files**, with the innermost layer holding a single **~4.3 GB** file:

```
Layer 5 (innermost):  1 file × 4,294,967,295 bytes              =        4.00 GB
Layer 4:              16 files × 4,294,967,295 bytes             =       68.72 GB
Layer 3:              16 files × 68,719,476,720 bytes            =        1.10 TB
Layer 2:              16 files × 1,099,511,627,520 bytes         =       17.59 TB
Layer 1:              16 files × 17,592,186,040,320 bytes        =      281.47 TB
Layer 0 (outermost):  16 files × 281,474,976,645,120 bytes       =    4,503.60 PB  (4.5 PB)
```

| 📐 Step | ✖️ Multiplication                     | 📊 Result                     |
|---------|----------------------------------------|-------------------------------|
| 1       | 16 × 4,294,967,295                    | 68,719,476,720 → **~68 GB**   |
| 2       | 16 × 68,719,476,720                   | 1,099,511,627,520 → **~1 TB** |
| 3       | 16 × 1,099,511,627,520                | 17,592,186,040,320 → **~17 TB** |
| 4       | 16 × 17,592,186,040,320               | 281,474,976,645,120 → **~281 TB** |
| 5       | 16 × 281,474,976,645,120              | 4,503,599,626,321,920 → **~4.5 PB** |

🔁 The **`zbxl.zip`** in this repo follows the same principle and decompresses to a staggering **4.5 petabytes** from just **43.75 MB**.

---

## ⚠️ Safety & Legal

- 🚫 **Never** extract these files on any production system, personal computer, or device with limited storage.
- 🛡️ Use **virtual machines** or **sandboxed environments** only.
- 🛑 Most antivirus and archive tools (7-Zip, WinRAR, etc.) will **warn** or **refuse** to extract — respect those warnings.
- 📚 Use these files **only** for:
  - ✅ Security research
  - ✅ Computer science education
  - ✅ Testing anti-virus / malware detection
  - ✅ Demonstrating compression vulnerabilities
- ❌ **Do not** use for any malicious purpose.

---

## 💖 Support

If you found this educational:

- ⭐ **Star** this repo on GitHub!
- 🔁 Share with fellow cybersecurity enthusiasts!

---

## 📜 License

This repository provides zip bomb archives for **educational and research purposes only**. The creators assume **no responsibility** for misuse. Use responsibly and ethically.
```
