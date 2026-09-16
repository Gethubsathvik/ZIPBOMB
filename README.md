# 💣 ZIP BOMB — ⚠️ DO NOT EXTRACT ⚠️ 💣

> 📁 A collection of **zip bomb** archives designed for **educational and research purposes only**.
> These files will **expand from kilobytes to petabytes** and will **destroy your storage** if extracted carelessly.

---

## 🎯 Overview

A **zip bomb** (also called a **decompression bomb** or **zipper bomb**) is a malicious archive file that is **highly compressed** but decompresses to an **enormous size**, overwhelming the memory, CPU, or disk space of the system attempting to extract it.

This repository contains **4 zip bomb variants** using two different construction techniques:

| 📄 File       | 🗜️ Compressed | 🛠️ Technique         | 💨 Expands To  | ⚡ Ratio        |
|---------------|----------------|----------------------|-----------------|-----------------|
| `zbsm.zip`     | 42 kB          | Non-recursive overlap | ~5.5 GB         | ~129,000:1      |
| `zblg.zip`     | 9.4 MB         | Non-recursive overlap | ~281 TB         | ~28,400,000:1   |
| `zbxl.zip`     | 43.75 MB       | Non-recursive overlap (Zip64) | ~4.5 PB   | ~98,000:1       |
| `42.zip`       | 41.8 KB        | Recursive nesting (password: `42`) | ~4.5 PB | ~107,000,000:1  |

> 🛑 **WARNING:** Opening any of these files — even just viewing their contents in some archive managers — can **freeze your system** or **fill your disk to 100%**. **NEVER** attempt to extract these files on a real machine.

---

## 📦 Repository Contents

```
ZIPBOMB/
├── 💣 zbsm.zip    —  Zip Bomb (Small) —  42 kB → 5.5 GB   (non-recursive)
├── 💣 zblg.zip    —  Zip Bomb (Large)  —  9.4 MB → 281 TB (non-recursive)
├── 💣 zbxl.zip    —  Zip Bomb (XL)     —  43.75 MB → 4.5 PB (non-recursive, Zip64)
├── 💣 42.zip      —  Zip Bomb (Classic) — 41.8 KB → 4.5 PB (recursive, password: 42)
├── 📄 README.md   —  ⭐ Star this repo!
└── 📄 .gitignore  —  🚫 Ignore extracted output
```

---

## 📱 QR Code — Download

> ⚠️ **WARNING:** QR codes are placed below in each file's detailed analysis section. Scanning a QR code with your phone or device will open a **direct download link** for that specific zip bomb file. If the downloaded file is opened or extracted, it will **fill your storage to 100%** and may cause system instability.

---

## 🧬 Construction Techniques

### 🔬 Non-Recursive Zip Bombs (zbsm, zblg, zbxl)

> **Summary:** This construction shows how to build a non-recursive zip bomb that achieves an extremely high compression ratio by **overlapping files inside the zip container**. "Non-recursive" means it does **not** rely on a decompressor's recursively unpacking zip files nested within zip files — it expands fully after a **single round of decompression**. The output size increases **quadratically** in the input size, reaching a compression ratio of over **28 million** (10 MB → 281 TB) at the limits of the zip format. Even greater expansion is possible using **64-bit extensions** (Zip64).

**Key properties:**
- ✅ Uses only the most common compression algorithm — **DEFLATE** (Type 8)
- ✅ Compatible with most zip parsers
- ✅ Single decompression pass (no recursive nesting)
- ⚠️ Detects as "Overlapped entries" by Python's `zipfile` module
- 📈 Expands **quadratically** with input size

**How it works:**
1. 📦 The compressed data is highly compressible (repetitive byte patterns).
2. 🔄 Multiple zip entries are constructed so their compressed data **overlaps** — they point to the **same** small compressed payload in the file.
3. 💨 Each entry reports a massive `uncompressed_size` in its header (up to 4 GB per entry for non-Zip64, or much more with Zip64).
4. 🧨 When an extractor decompresses each entry, it writes out the full claimed size — producing hundreds of TB or PB from a tiny archive.

> 🔬 **Detection:** Python's standard library `zipfile` module refuses to extract these archives and raises:
> ```python
> zipfile.BadZipFile: Overlapped entries: '0' (possible zip bomb)
> ```

### 🔁 Recursive Zip Bomb (42.zip — Classic)

The legendary **`42.zip`** uses a completely different technique — **recursive nesting**:
- 🔐 Password-protected with AES encryption (password: **`42`**)
- 📚 Contains **16 nested layers** of zip files
- 📂 Each layer holds **16 zip files** that decompress to the next layer
- 🏁 The innermost layer contains a single file of **~4.3 GB** of highly compressible data
- 💥 Final expansion: **4.5 petabytes** from a 42 KB archive

---

## 🔍 Detailed Technical Analysis

### 📁 zbsm.zip — "Zip Bomb Small"

| Metric                  | Value                              |
|-------------------------|------------------------------------|
| File size on disk       | 42,374 bytes (~42 kB)             |
| Number of entries       | 250                                |
| Compression method      | Deflate (Type 8)                   |
| Total uncompressed data | 5,461,307,620 bytes (~5.5 GB)     |
| Per-entry uncompressed  | ~21,849,182 bytes (~21 MB)        |
| Per-entry compressed    | ~30,273 bytes (~30 kB)            |
| Construction technique  | **Overlapped entries** ⚠️          |
| Compression ratio       | ~129,000:1                         |

Python's `zipfile` detects this as: `Overlapped entries: '0' (possible zip bomb)`.

#### 📲 Download QR
<img src="qr-zbsm.png" alt="QR Code — zbsm.zip Download" width="150">
**Raw URL:** `https://github.com/Gethubsathvik/ZIPBOMB/raw/master/zbsm.zip`

### 📁 zblg.zip — "Zip Bomb Large"

| Metric                  | Value                                  |
|-------------------------|----------------------------------------|
| File size on disk       | 9,893,525 bytes (~9.44 MB)            |
| Number of entries       | 65,534                                 |
| Compression method      | Deflate (Type 8)                      |
| Total uncompressed data | 281,395,456,244,934 bytes (~281 TB)   |
| Per-entry uncompressed  | ~4,294,967,240 bytes (~4.0 GB)        |
| Per-entry compressed    | ~6,666,128 bytes (~6.35 MB)           |
| Construction technique  | **Overlapped entries** ⚠️              |
| Compression ratio       | ~28,400,000:1 (28.4 million : 1)      |

💀 This archive achieves one of the highest compression ratios possible in the standard ZIP format: **9.44 MB → 281 TB**. Python refuses extraction with an "Overlapped entries" error.

#### 📲 Download QR
<img src="qr-zblg.png" alt="QR Code — zblg.zip Download" width="150">
**Raw URL:** `https://github.com/Gethubsathvik/ZIPBOMB/raw/master/zblg.zip`

### 📁 zbxl.zip — "Zip Bomb Extra Large"

| Metric                  | Value                                      |
|-------------------------|--------------------------------------------|
| File size on disk       | 45,876,952 bytes (~43.75 MB)              |
| Number of entries       | 190,023                                    |
| Compression method      | Deflate (Type 8, Zip64 extensions)        |
| Total uncompressed data | 4,507,981,427,706,459 bytes (~4.5 PB)     |
| Per-entry uncompressed  | ~23,728,433,572 bytes (~22 GB)            |
| Per-entry compressed    | ~34,144,733 bytes (~32.5 MB)              |
| Construction technique  | **Overlapped entries (Zip64)** ⚠️          |
| Compression ratio       | ~98,000:1                                  |

💀 This is one of the most dangerous zip bombs in existence. A mere **43.75 MB** download expands to claim **4.5 petabytes** — enough to **obliterate any consumer or many enterprise storage systems**. Uses **Zip64** extensions to exceed the 4 GB per-file limit of standard ZIP, which reduces compatibility with older extractors.

#### 📲 Download QR
<img src="qr-zbxl.png" alt="QR Code — zbxl.zip Download" width="150">
**Raw URL:** `https://github.com/Gethubsathvik/ZIPBOMB/raw/master/zbxl.zip`
> 💣 `zbxl.zip` (43.75 MB) will expand to **4.5 petabytes** if extracted!

### 📁 42.zip — "Classic 42 Zip Bomb"

| Metric                  | Value                              |
|-------------------------|------------------------------------|
| File size on disk       | 42,838 bytes (~41.8 kB)           |
| Number of entries       | 16 (at outermost layer)           |
| Compression method      | Deflate (Type 8)                   |
| Encryption              | AES (password: **`42`**)          |
| Nesting depth           | 5 layers (16 files per layer)     |
| Innermost file size     | 4,294,967,295 bytes (~4.0 GB)     |
| Total uncompressed data | ~4.5 PB                            |
| Construction technique  | **Recursive nesting**              |
| Compression ratio       | ~107,000,000:1 (107 million : 1) |

The entries at each layer are named `lib 0.zip` through `lib 15.zip`, each being a password-protected nested zip that unpacks to the next layer.

#### 📲 Download QR
<img src="qr-42.png" alt="QR Code — 42.zip Download" width="150">
**Raw URL:** `https://github.com/Gethubsathvik/ZIPBOMB/raw/master/42.zip`
> 🔐 Password: **`42`** · 💣 `42.zip` (41.8 KB) will expand to **4.5 petabytes** if extracted!

---

## 💥 How Zip Bombs Work

Zip bombs exploit the **compression ratio gap** between compressed and uncompressed data. Here are the two attack vectors:

### 🔄 Recursive Nesting (42.zip)
```
Layer 0:  16 zips → 41.8 KB total
  ↓ extract
Layer 1:  16 × 16 = 256 zips → ~558 KB total
  ↓ extract
Layer 2:  16 × 16 × 16 = 4,096 zips → ~8.9 MB total
  ↓ extract
Layer 3:  16 × 16 × 16 × 16 = 65,536 zips → ~143 MB total
  ↓ extract
Layer 4:  16 × 16 × 16 × 16 × 16 = 1,048,576 files → ~4.3 GB total
  ↓ = final decompression = ~4.5 PB of data
```

### 🔄 Overlapped Entries (zbsm, zblg, zbxl)
1. 📦 **Highly compressible data** — The archive stores data with extreme repetition (e.g., runs of identical bytes).
2. 🔄 **Overlapped entries** — Multiple zip file entries reference the **same** small compressed data block, but each header claims a massive uncompressed size.
3. 💨 **Single-pass expansion** — One decompression triggers all entries simultaneously, writing out terabytes/petabytes.
4. 🧨 **Resource exhaustion** — The target system exhausts disk space, memory, or swap, causing crashes, freezes, or data corruption.

---

## 🧮 Compression Ratio Mathematics

### Non-Recursive Bombs (Overlap Technique)

| 📄 File     | 🗜️ Input Size  | 💨 Output Size | ⚡ Ratio           |
|-------------|----------------|----------------|---------------------|
| `zbsm.zip`  | 42 kB          | 5.5 GB         | ~129,000:1         |
| `zblg.zip`  | 9.4 MB         | 281 TB         | ~28,400,000:1 ✅   |
| `zbxl.zip`  | 43.75 MB       | 4.5 PB         | ~98,000:1          |
| `42.zip`    | 41.8 KB 🔐 (pw: `42`) | 4.5 PB  | ~107,000,000:1     |

💡 **`zblg.zip` achieves the record**: a compression ratio of over **28 million** (28,400,000:1) — the theoretical maximum in the ZIP format using quadratic overlap and DEFLATE compression.

### Recursive Bomb (42.zip)

```
Step | Calculation                          | Result
-----|--------------------------------------|------------------
1    | 16 files × 4,294,967,295 bytes       | 68,719,476,720    → ~68 GB   (Layer 4)
2    | 16 × 68,719,476,720                  | 1,099,511,627,520 → ~1 TB    (Layer 3)
3    | 16 × 1,099,511,627,520               | 17,592,186,040,320 → ~17 TB  (Layer 2)
4    | 16 × 17,592,186,040,320              | 281,474,976,645,120 → ~281 TB (Layer 1)
5    | 16 × 281,474,976,645,120             | 4,503,599,626,321,920 → ~4.5 PB (Layer 0)
```

---

## 💾 Source Code & Construction

The non-recursive zip bombs (zbsm, zblg, zbxl) are constructed using a technique that places **overlapping entries inside the zip container** — the compressed data of many entries points to the same bytes, while the uncompressed size fields are set to maximum values. The construction:

- Uses **DEFLATE** (the most common compression algorithm in ZIP)
- Achieves **quadratic expansion** of output vs. input size
- Reaches a maximum ratio of **over 28 million** (10 MB → 281 TB) in standard ZIP
- Can exceed this using **Zip64 (64-bit) extensions** for even larger claims

---

## ⚠️ Safety & Legal

- 🚫 **Never** extract these files on any production system, personal computer, or device with limited storage.
- 🛡️ Use **virtual machines** or **sandboxed disposable environments** only — and monitor disk usage closely.
- 🛑 Most antivirus and archive tools (7-Zip, WinRAR, etc.) will **warn** or **refuse** to extract — respect those warnings.
- 🔐 **`42.zip`** is password-protected (password: `42`) — do not enter the password unless you understand the consequences.
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
- 📖 Cite this for any academic or research presentations.

---

## 📜 License

This repository provides zip bomb archives for **educational and research purposes only**. The creators assume **no responsibility** for misuse. Use responsibly and ethically.
