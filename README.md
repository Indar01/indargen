<h1 align="center">
  <br>
  <img src="https://raw.githubusercontent.com/indar/indargen/main/assets/indargen_banner.png" alt="IndarGen" width="600">
  <br>
  INDARGEN - Smart Password List Generator 🔐
  <br>
</h1>

<h4 align="center">Targeted Password Wordlist Generator for Authorized Penetration Testing</h4>

<p align="center">
  <a href="https://www.python.org/downloads/">
    <img src="https://img.shields.io/badge/python-3.8+-blue.svg" alt="Python Version">
  </a>
  <a href="https://github.com/indar/indargen/blob/main/LICENSE">
    <img src="https://img.shields.io/badge/license-MIT-red.svg" alt="License">
  </a>
  <a href="https://github.com/indar/indargen/issues">
    <img src="https://img.shields.io/badge/contributions-welcome-brightgreen.svg" alt="Contributions Welcome">
  </a>
  <a href="https://www.kali.org/">
    <img src="https://img.shields.io/badge/Kali%20Linux-Ready-black.svg" alt="Kali Ready">
  </a>
  <a href="https://github.com/indar/indargen/stargazers">
    <img src="https://img.shields.io/github/stars/indar/indargen?style=social" alt="GitHub Stars">
  </a>
</p>

<p align="center">
  <a href="#-features">Features</a> •
  <a href="#-installation">Installation</a> •
  <a href="#-usage">Usage</a> •
  <a href="#-modes">Modes</a> •
  <a href="#-examples">Examples</a> •
  <a href="#-compatibility">Compatibility</a> •
  <a href="#-faq">FAQ</a> •
  <a href="#-license">License</a>
</p>

---

## 📌 Overview

**IndarGen** is a powerful, feature-rich password list generator designed for **authorized penetration testers** and **security professionals**. It creates highly targeted wordlists by analyzing victim profiling data (name, family, DOB, phone, location, interests, etc.) and applying intelligent mutation algorithms — including LeetSpeak, date analysis, case variations, separator combinations, and year padding.

Instead of relying on generic rockyou-style wordlists, IndarGen crafts passwords specifically tailored to the target, dramatically increasing brute-force success rates in authorized security assessments.

> ⚠️ **IMPORTANT**: This tool is for **authorized security testing only**. Using it against systems without explicit written permission is illegal. The author assumes no liability for misuse.

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🧠 **Smart Profiling** | 30+ data fields — name, family, pets, DOB, phone, location, work, education, hobbies |
| 🔄 **LeetSpeak Engine** | `a→@/4`, `s→$/5`, `o→0`, `e→3`, `t→7` with multi-position combinations |
| 📅 **Date Intelligence** | Auto-parses DD-MM-YYYY, YYYY-MM-DD, generates number combos |
| 🔢 **Phone Analysis** | Extracts patterns from full/partial phone numbers |
| 🔀 **Smart Mangling** | Case toggles, reverses, separators (`_`, `-`, `.`, `@`, `#`), padding |
| ⚡ **3 Operation Modes** | Interactive (full), Quick (fast), Batch (JSON multi-profile) |
| 🎨 **Color Output** | Full terminal color-coded progress indicators |
| 📊 **Statistics** | Password count, file size, dedup ratio after generation |
| 🧹 **Auto-Dedup** | Removes duplicates, sorts by length for efficiency |
| 📦 **Lightweight** | Pure Python3 — no external dependencies needed |

---

## 🔧 Installation

### Prerequisites
- Python 3.8 or higher
- Linux / macOS / Windows (WSL recommended on Windows)

### Method 1: Direct Download (Recommended)

```bash
# Clone the repository
git clone https://github.com/indar/indargen.git
cd indargen

# Make the script executable
chmod +x indargen.py

# Run it
python3 indargen.py

###╔═══════════════════════════════════════╗
║         🔥 SELECT MODE 🔥            ║
╚═══════════════════════════════════════╝

[1] 📋 INTERACTIVE MODE    - Full victim profiling (30+ fields)
[2] ⚡ QUICK MODE          - Fast generation (minimal inputs)
[3] 📂 BATCH MODE          - Load profiles from JSON file
[4] ❓ HELP                - View usage guide & examples
[5] 🚪 EXIT
