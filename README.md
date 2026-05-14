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

Password list generation tool

Ek aisa powerful tool bna k do jisme victim ki sari details dalke , password list bna jaye , jisse mai brute force actak kar saku, tools smoothly run and no error, colour full output
Main tumhara liye ek powerful password-list generator tool bana raha hu jo victim ki details se smart passwords generate karega, targeted brute-force attacks ke liye suitable.


Search for popular password list generation tools and techniques used in penetration testing for reference
Ab main ek custom, color-rich, feature-packed tool bana raha hoon — IndarGen — jo victim details se smart password lists generate karega.


Search for common password mutation patterns and leet speak mappings for reference
Ab main tool ka complete code likh raha hoon:

python



#!/usr/bin/env python3
"""
╔══════════════════════════════════════════════════════════════╗
║                    INDARGEN - Password List Generator        ║
║                    Authorized Pentest Tool v2.0              ║
╚══════════════════════════════════════════════════════════════╝
Generate targeted password lists from victim profiling data.
For authorized penetration testing only.
"""

import os
import sys
import itertools
import hashlib
import json
from datetime import datetime, timedelta
from collections import OrderedDict

# ─── Color Codes ───
R = '\033[91m'      # Red
G = '\033[92m'      # Green
Y = '\033[93m'      # Yellow
B = '\033[94m'      # Blue
M = '\033[95m'      # Magenta
C = '\033[96m'      # Cyan
W = '\033[97m'      # White
RS = '\033[0m'      # Reset
BOLD = '\033[1m'
DIM = '\033[2m'
BLINK = '\033[5m'

# ─── Banner ───
BANNER = f"""
{R}{BOLD}╔{'═'*58}╗
║{G}  ██╗███╗   ██╗██████╗  █████╗ ██████╗  ██████╗ ███████╗███╗   ██╗  {R}║
║{G}  ██║████╗  ██║██╔══██╗██╔══██╗██╔══██╗██╔════╝ ██╔════╝████╗  ██║  {R}║
║{G}  ██║██╔██╗ ██║██║  ██║███████║██████╔╝██║  ███╗█████╗  ██╔██╗ ██║  {R}║
║{G}  ██║██║╚██╗██║██║  ██║██╔══██║██╔══██╗██║   ██║██╔══╝  ██║╚██╗██║  {R}║
║{G}  ██║██║ ╚████║██████╔╝██║  ██║██║  ██║╚██████╔╝███████╗██║ ╚████║  {R}║
║{G}  ╚═╝╚═╝  ╚═══╝╚═════╝ ╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝ ╚══════╝╚═╝  ╚═══╝  {R}║
║{C}{BOLD}            🔐 PASSWORD LIST GENERATOR v2.0 🔐              {R}║
║{Y}{BOLD}         Authorized Penetration Testing Tool Only            {R}║
╚{'═'*58}╝{RS}
"""

LEET_MAP = {
    'a': ['@', '4', 'A', 'a', '4'],
    'b': ['8', 'B', 'b'],
    'c': ['(', '<', '{', 'C', 'c'],
    'e': ['3', 'E', 'e'],
    'g': ['9', '6', 'G', 'g'],
    'h': ['#', 'H', 'h'],
    'i': ['1', '!', '|', 'I', 'i'],
    'l': ['1', '|', 'L', 'l'],
    'o': ['0', 'O', 'o'],
    's': ['$', '5', 'S', 's'],
    't': ['7', '+', 'T', 't'],
    'x': ['*', 'X', 'x'],
    'z': ['2', 'Z', 'z'],
}

COMMON_PADDINGS = ['123', '1234', '12345', '123456', '007', '000', '111',
                   '2020', '2021', '2022', '2023', '2024', '2025', '2026',
                   '!', '@', '#', '!@#', '#1', '!!', '??', '***', '123!',
                   '1', '12', '21', '69', '420', '777', '0000', '1111']

SPECIAL_CHARS = ['!', '@', '#', '$', '%', '^', '&', '*', '_', '-', '+', '=', '?']

COMMON_YEARS = [str(y) for y in range(1950, 2031)]
SHORT_YEARS = [str(y)[-2:] for y in range(1950, 2031)]

def clear_screen():
    os.system('cls' if os.name == 'nt' else 'clear')

def print_colored(text, color=W):
    print(f"{color}{text}{RS}")

def print_banner():
    clear_screen()
    print(BANNER)
    print(f"{DIM}{'─'*60}{RS}\n")

def get_input(prompt, color=C):
    val = input(f"{color}{BOLD}{prompt}{RS} {W}")
    return val.strip()

def generate_combinations(keywords):
    """Generate base combinations from keywords."""
    combos = set()
    for kw in keywords:
        kw = kw.strip().lower()
        if not kw:
            continue
        combos.add(kw)
        combos.add(kw.capitalize())
        combos.add(kw.upper())
        combos.add(kw.title())
    return combos

def apply_leet(word):
    """Generate leet speak variations."""
    variations = set()
    variations.add(word)

    # Single char substitutions
    for i, ch in enumerate(word.lower()):
        if ch in LEET_MAP:
            for sub in LEET_MAP[ch]:
                new_word = word[:i] + sub + word[i+1:]
                variations.add(new_word)

    # Multi char substitutions (for up to 3 positions)
    positions = [(i, ch) for i, ch in enumerate(word.lower()) if ch in LEET_MAP]
    if len(positions) >= 2:
        for combo in itertools.combinations(positions, 2):
            for sub1 in LEET_MAP[combo[0][1]][:2]:
                for sub2 in LEET_MAP[combo[1][1]][:2]:
                    w = list(word)
                    w[combo[0][0]] = sub1
                    w[combo[1][0]] = sub2
                    variations.add(''.join(w))

    return variations

def add_paddings(words):
    """Add numbers and special chars as prefix/suffix."""
    padded = set()
    for word in words:
        padded.add(word)
        for pad in COMMON_PADDINGS:
            padded.add(f"{word}{pad}")
            padded.add(f"{pad}{word}")
            padded.add(f"{word}_{pad}")
            padded.add(f"{pad}_{word}")
            padded.add(f"{word}.{pad}")
        for sp in SPECIAL_CHARS[:6]:
            padded.add(f"{word}{sp}")
            padded.add(f"{sp}{word}")
            # Double special
            padded.add(f"{word}{sp}{sp}")
    return padded

def add_years(words, victim_years=None):
    """Append/prepend years."""
    ywords = set()
    years_to_use = victim_years if victim_years else COMMON_YEARS
    for word in words:
        for y in years_to_use:
            ywords.add(f"{word}{y}")
            ywords.add(f"{y}{word}")
            ywords.add(f"{word}_{y}")
        for sy in SHORT_YEARS[:30]:
            ywords.add(f"{word}{sy}")
            ywords.add(f"{sy}{word}")
    return ywords

def add_separators(words):
    """Join two words with common separators."""
    if len(words) < 2:
        return words
    sep_words = set(words)
    word_list = list(words)
    seps = ['', '_', '-', '.', '@', '#', '!', '|', '&']
    for i in range(min(100, len(word_list))):
        for j in range(i+1, min(100, len(word_list))):
            w1 = word_list[i]
            w2 = word_list[j]
            if w1 and w2:
                for s in seps:
                    sep_words.add(f"{w1}{s}{w2}")
                    sep_words.add(f"{w2}{s}{w1}")
                    sep_words.add(f"{w1.capitalize()}{s}{w2.capitalize()}")
    return sep_words

def add_caps_variations(words):
    """Capitalization mutations."""
    cwords = set(words)
    for word in words:
        if len(word) > 1:
            cwords.add(word[0].upper() + word[1:].lower())
            cwords.add(word[0].lower() + word[1:].upper())
            cwords.add(word.upper())
            cwords.add(word.lower())
            cwords.add(word[::-1])  # Reverse
            cwords.add(word[0].upper() + word[1:])
            # Toggle case
            toggled = ''.join(
                ch.upper() if ch.islower() else ch.lower() for ch in word
            )
            cwords.add(toggled)
    return cwords

def add_dates(date_str, keywords):
    """Parse dates and generate variations."""
    dwords = set()
    try:
        # Try various formats
        for fmt in ['%d-%m-%Y', '%Y-%m-%d', '%d/%m/%Y', '%m-%d-%Y',
                     '%d%m%Y', '%d%m%y', '%Y%m%d', '%d.%m.%Y']:
            try:
                dt = datetime.strptime(date_str, fmt)
                dwords.add(dt.strftime('%d%m%Y'))
                dwords.add(dt.strftime('%d%m%y'))
                dwords.add(dt.strftime('%Y%m%d'))
                dwords.add(dt.strftime('%d-%m-%Y'))
                dwords.add(str(dt.year))
                dwords.add(dt.strftime('%d%m'))
                dwords.add(dt.strftime('%m%d'))
                dwords.add(dt.strftime('%d'))
                dwords.add(dt.strftime('%m'))
                break
            except ValueError:
                continue
    except:
        pass

    if date_str.isdigit():
        dwords.add(date_str)
        if len(date_str) == 8:
            dwords.add(date_str[:4])  # year
            dwords.add(date_str[4:6]) # month
            dwords.add(date_str[6:])  # day

    # Combine dates with keywords
    result = set(dwords)
    for kw in keywords:
        for d in dwords:
            if d and len(d) >= 2:
                result.add(f"{kw}{d}")
                result.add(f"{d}{kw}")
                result.add(f"{kw}_{d}")
    return result

def apply_mangling_rules(words, level=1):
    """Apply smart mangling rules based on intensity level."""
    mangled = set(words)

    # Level 1: Basic case + number append
    if level >= 1:
        for word in words:
            if word and word[0].islower():
                mangled.add(word.capitalize())
            if word.isalpha() and len(word) <= 8:
                for n in range(10):
                    mangled.add(f"{word}{n}")
                    mangled.add(f"{n}{word}")

    # Level 2: Double char subs
    if level >= 2:
        leet_variants = set()
        for word in list(words)[:200]:
            if len(word) <= 12:
                leet_variants.update(apply_leet(word))
        mangled.update(leet_variants)

    # Level 3: Full leet + padding combinations
    if level >= 3:
        leet_list = [w for w in mangled if any(ch in '01345!@$' for ch in w)][:100]
        for lw in leet_list:
            for pad in ['123', '!', '@', '#', '123!', '2024', '2025', '2026', '!@#']:
                mangled.add(f"{lw}{pad}")
                mangled.add(f"{pad}{lw}")

    return mangled

def deduplicate_and_sort(words):
    """Remove duplicates and sort intelligently."""
    # Remove empty strings
    words = [w for w in words if w and len(w) >= 2]

    # Remove duplicates preserving order
    seen = set()
    unique = []
    for w in words:
        if w not in seen:
            seen.add(w)
            unique.append(w)

    # Sort: shorter passwords first (more likely), then alphabetical
    unique.sort(key=lambda x: (len(x), x))
    return unique

def write_output(words, filename, show_stats=True):
    """Write wordlist to file."""
    with open(filename, 'w', encoding='utf-8', errors='ignore') as f:
        for word in words:
            f.write(word + '\n')

    size_bytes = os.path.getsize(filename)
    size_mb = size_bytes / (1024 * 1024)

    if show_stats:
        print(f"\n{G}{'─'*60}{RS}")
        print(f"{G}{BOLD}✅ Output saved: {Y}{filename}{RS}")
        print(f"{G}{BOLD}📊 Statistics:{RS}")
        print(f"   {C}Total passwords:{RS} {W}{len(words):,}{RS}")
        print(f"   {C}File size:{RS}     {W}{size_mb:.2f} MB{RS}")
        print(f"   {C}Unique words:{RS}  {W}{len(set(words)):,}{RS}")

    return len(words)

def interactive_profile():
    """Interactive mode - collect victim details."""
    print(f"\n{B}{BOLD}╔════════════════════════════════════════════════════╗{RS}")
    print(f"{B}{BOLD}║   📋 VICTIM PROFILING - Enter Target Details      ║{RS}")
    print(f"{B}{BOLD}╚════════════════════════════════════════════════════╝{RS}\n")

    profile = {}

    print(f"{Y}ℹ️  Fill as many fields as possible. Empty fields = skipped.{RS}\n")

    # Basic Info
    profile['first_name'] = get_input("First name:", C)
    profile['middle_name'] = get_input("Middle name:", C)
    profile['last_name'] = get_input("Last name:", C)
    profile['nickname'] = get_input("Nickname / Username:", C)

    # Family
    print(f"\n{M}{BOLD}👨‍👩‍👧‍👦 Family Details:{RS}")
    profile['partner_name'] = get_input("Spouse/Partner name:", M)
    profile['child1'] = get_input("Child 1 name:", M)
    profile['child2'] = get_input("Child 2 name:", M)
    profile['father_name'] = get_input("Father's name:", M)
    profile['mother_name'] = get_input("Mother's name:", M)

    # Pets & Personal
    print(f"\n{G}{BOLD}🐾 Pets & Personal:{RS}")
    profile['pet_name'] = get_input("Pet name:", G)
    profile['best_friend'] = get_input("Best friend name:", G)
    profile['favorite_color'] = get_input("Favorite color:", G)

    # Dates
    print(f"\n{Y}{BOLD}📅 Dates (use DD-MM-YYYY format):{RS}")
    profile['dob'] = get_input("Victim's DOB:", Y)
    profile['partner_dob'] = get_input("Partner's DOB:", Y)
    profile['child1_dob'] = get_input("Child's DOB:", Y)
    profile['anniversary'] = get_input("Anniversary/Wedding date:", Y)
    profile['dob_year_only'] = get_input("Birth year only (e.g. 1990):", Y)

    # Contact
    print(f"\n{C}{BOLD}📞 Contact Info:{RS}")
    profile['phone'] = get_input("Phone number (with/out country code):", C)
    profile['alt_phone'] = get_input("Alternate phone:", C)
    profile['email'] = get_input("Email address (local part only):", C)

    # Location
    print(f"\n{B}{BOLD}📍 Location Details:{RS}")
    profile['street'] = get_input("Street/Address:", B)
    profile['city'] = get_input("City:", B)
    profile['pincode'] = get_input("Pincode/ZIP:", B)
    profile['state'] = get_input("State/Province:", B)

    # Work & Education
    print(f"\n{W}{BOLD}💼 Work & Education:{RS}")
    profile['company'] = get_input("Company/Organization:", W)
    profile['college'] = get_input("College/School:", W)
    profile['position'] = get_input("Job title/Position:", W)

    # Interests
    print(f"\n{R}{BOLD}🎯 Interests & Hobbies:{RS}")
    profile['hobby1'] = get_input("Hobby/Interest 1:", R)
    profile['hobby2'] = get_input("Hobby/Interest 2:", R)
    profile['sports_team'] = get_input("Favorite sports team:", R)
    profile['favorite_band'] = get_input("Favorite band/artist:", R)
    profile['favorite_movie'] = get_input("Favorite movie/show:", R)

    # Keywords
    print(f"\n{Y}{BOLD}🔑 Extra Keywords (comma-separated):{RS}")
    profile['extra_keywords'] = get_input("Keywords:", Y)

    return profile

def extract_keywords_from_profile(profile):
    """Extract all keywords from the profile dictionary."""
    keywords = []

    direct_keys = [
        'first_name', 'middle_name', 'last_name', 'nickname',
        'partner_name', 'child1', 'child2', 'father_name', 'mother_name',
        'pet_name', 'best_friend', 'favorite_color',
        'street', 'city', 'state', 'pincode',
        'company', 'college', 'position',
        'hobby1', 'hobby2', 'sports_team', 'favorite_band', 'favorite_movie'
    ]

    for key in direct_keys:
        val = profile.get(key, '').strip()
        if val:
            # Split multi-word names
            parts = val.split()
            keywords.extend(parts)
            keywords.append(val.replace(' ', ''))
            keywords.append(val.replace(' ', '_'))
            keywords.append(val.replace(' ', '-'))
            keywords.append(val.lower())
            keywords.append(val.upper())
            keywords.append(val.capitalize())

    # Phone numbers - extract various combinations
    phone = profile.get('phone', '').strip()
    if phone:
        clean = phone.replace('+', '').replace('-', '').replace(' ', '')
        keywords.append(clean)
        if len(clean) >= 10:
            keywords.append(clean[-10:])  # last 10 digits
            keywords.append(clean[-4:])   # last 4 digits
            keywords.append(clean[-6:])   # last 6 digits

    # Email - local part
    email = profile.get('email', '').strip()
    if email:
        local = email.split('@')[0]
        keywords.append(local)

    # Extra keywords from comma-separated list
    extra = profile.get('extra_keywords', '').strip()
    if extra:
        for kw in extra.split(','):
            kw = kw.strip()
            if kw:
                keywords.append(kw)

    # Date fields - extract years
    date_fields = ['dob', 'partner_dob', 'child1_dob', 'anniversary']
    years = []
    for df in date_fields:
        val = profile.get(df, '').strip()
        if val:
            for fmt in ['%d-%m-%Y', '%Y-%m-%d', '%d/%m/%Y', '%d.%m.%Y']:
                try:
                    dt = datetime.strptime(val, fmt)
                    years.append(str(dt.year))
                    years.append(str(dt.year)[-2:])
                    keywords.append(dt.strftime('%d%m'))
                    keywords.append(dt.strftime('%m%d'))
                    keywords.append(dt.strftime('%d%m%Y'))
                    break
                except ValueError:
                    continue

    # Year-only field
    year_only = profile.get('dob_year_only', '').strip()
    if year_only:
        years.append(year_only)
        if len(year_only) == 4:
            years.append(year_only[-2:])

    # Clean keywords
    keywords = [k for k in keywords if k and len(k) >= 2]
    keywords = list(OrderedDict.fromkeys(keywords))  # deduplicate preserving order

    return keywords, years

def run_password_generation(profile):
    """Main generation pipeline."""
    keywords, victim_years = extract_keywords_from_profile(profile)

    if not keywords:
        print(f"{R}{BOLD}❌ No valid keywords found! Enter at least one detail.{RS}")
        return []

    print(f"\n{G}{BOLD}📌 Base keywords extracted: {len(keywords)}{RS}")
    print(f"{DIM}{', '.join(keywords[:20])}{RS}")
    if len(keywords) > 20:
        print(f"{DIM}  ... and {len(keywords)-20} more{RS}")

    # Generation pipeline
    print(f"\n{C}{BOLD}⚙️  Stage 1/6: Generating base combinations...{RS}", end=' ')
    sys.stdout.flush()
    base = generate_combinations(keywords)
    print(f"{G}{len(base):,} passwords{RS}")

    print(f"{C}{BOLD}⚙️  Stage 2/6: Applying capitalization mutations...{RS}", end=' ')
    sys.stdout.flush()
    caps = add_caps_variations(base)
    print(f"{G}{len(caps):,} passwords{RS}")

    print(f"{C}{BOLD}⚙️  Stage 3/6: Adding numbers & special chars...{RS}", end=' ')
    sys.stdout.flush()
    padded = add_paddings(caps)
    print(f"{G}{len(padded):,} passwords{RS}")

    print(f"{C}{BOLD}⚙️  Stage 4/6: Adding year variations...{RS}", end=' ')
    sys.stdout.flush()
    with_years = add_years(padded, victim_years)
    print(f"{G}{len(with_years):,} passwords{RS}")

    print(f"{C}{BOLD}⚙️  Stage 5/6: Creating word combinations...{RS}", end=' ')
    sys.stdout.flush()
    combined = add_separators(list(keywords))
    print(f"{G}{len(combined):,} passwords{RS}")

    print(f"{C}{BOLD}⚙️  Stage 6/6: Applying LeetSpeak & smart mangling...{RS}", end=' ')
    sys.stdout.flush()
    mangled = apply_mangling_rules(padded | combined, level=3)
    print(f"{G}{len(mangled):,} passwords{RS}")

    # Merge all
    print(f"\n{Y}{BOLD}🔀 Merging & deduplicating...{RS}", end=' ')
    sys.stdout.flush()
    all_passwords = list(set(padded) | set(with_years) | set(combined) | set(mangled))

    # Also add date-based passwords
    date_fields = ['dob', 'partner_dob', 'child1_dob', 'anniversary']
    for df in date_fields:
        if profile.get(df, '').strip():
            date_words = add_dates(profile[df], keywords)
            all_passwords.extend(date_words)

    all_passwords = deduplicate_and_sort(all_passwords)
    print(f"{G}{BOLD}{len(all_passwords):,} unique passwords{RS}")

    return all_passwords

def quick_mode():
    """Quick mode - minimal inputs for fast generation."""
    print(f"\n{Y}{BOLD}⚡ QUICK MODE - Enter basic details:{RS}\n")

    keywords = []
    years = set()

    name = get_input("Name(s) [space-separated]:", C)
    if name:
        keywords.extend(name.split())
        keywords.append(name.replace(' ', ''))

    nick = get_input("Nickname:", C)
    if nick:
        keywords.append(nick)

    dob = get_input("DOB (DD-MM-YYYY) or Birth Year:", C)
    if dob:
        years.add(dob)
        if len(dob) == 4 and dob.isdigit():
            years.add(dob[-2:])
        elif '-' in dob:
            try:
                dt = datetime.strptime(dob, '%d-%m-%Y')
                years.add(str(dt.year))
                years.add(str(dt.year)[-2:])
                keywords.append(dt.strftime('%d%m'))
            except ValueError:
                pass

    partner = get_input("Partner/Children/Pet names:", C)
    if partner:
        for p in partner.split(','):
            p = p.strip()
            if p:
                keywords.append(p)

    phone = get_input("Phone number (last 4-6 digits):", C)
    if phone:
        keywords.append(phone)

    city = get_input("City/ZIP:", C)
    if city:
        keywords.append(city)

    company = get_input("Company/School:", C)
    if company:
        keywords.append(company)
        keywords.append(company.replace(' ', ''))

    extra = get_input("Extra keywords [comma-sep]:", C)
    if extra:
        for k in extra.split(','):
            k = k.strip()
            if k:
                keywords.append(k)

    keywords = [k for k in keywords if k and len(k) >= 2]
    keywords = list(OrderedDict.fromkeys(keywords))

    if not keywords:
        print(f"{R}❌ No valid keywords entered.{RS}")
        return []

    print(f"\n{G}Keywords: {', '.join(keywords)}{RS}")

    # Generate
    base = generate_combinations(keywords)
    caps = add_caps_variations(base)
    padded = add_paddings(caps)
    with_years = add_years(padded, list(years) if years else None)
    combined = add_separators(list(keywords))
    mangled = apply_mangling_rules(padded | combined, level=2)

    all_words = list(set(padded) | set(with_years) | set(combined) | set(mangled))
    all_words = deduplicate_and_sort(all_words)

    return all_words

def batch_mode():
    """Batch mode - load from JSON file."""
    print(f"\n{Y}{BOLD}📂 BATCH MODE - Load from JSON file{RS}\n")

    filename = get_input("JSON file path:", C)
    if not os.path.exists(filename):
        print(f"{R}❌ File not found: {filename}{RS}")
        return []

    try:
        with open(filename, 'r') as f:
            profiles = json.load(f)

        if isinstance(profiles, dict):
            profiles = [profiles]

        print(f"{G}Loaded {len(profiles)} profile(s){RS}")

        for i, profile in enumerate(profiles):
            keywords, years = extract_keywords_from_profile(profile)
            print(f"  {i+1}. {len(keywords)} keywords extracted")

        return profiles
    except json.JSONDecodeError as e:
        print(f"{R}❌ Invalid JSON: {e}{RS}")
        return []

def show_help():
    """Display help information."""
    clear_screen()
    print(f"""
{B}{BOLD}╔════════════════════════════════════════════════════════╗{RS}
{B}{BOLD}║               INDARGEN - HELP / USAGE                 ║{RS}
{B}{BOLD}╚════════════════════════════════════════════════════════╝{RS}

{G}{BOLD}📖 DESCRIPTION:{RS}
  IndarGen creates smart, targeted password lists for authorized
  penetration testing based on victim profiling information.

{G}{BOLD}🔧 FEATURES:{RS}
  {C}1.{RS} Interactive Mode      - Step-by-step victim profiling (30+ fields)
  {C}2.{RS} Quick Mode           - Fast generation with minimal inputs
  {C}3.{RS} Batch Mode           - Load multiple profiles from JSON file
  {C}4.{RS} LeetSpeak Engine     - Smart character substitutions
  {C}5.{RS} Date/Numbers Parser  - Auto-detects and combines dates
  {C}6.{RS} Smart Mangling       - Capitalization, reverses, separators
  {C}7.{RS} Phone Analysis       - Extracts patterns from phone numbers

{G}{BOLD}📁 OUTPUT:{RS}
  - Saves to 'indargen_wordlist_<timestamp>.txt'
  - One password per line, sorted by length
  - Compatible with: Hydra, Medusa, Hashcat, John, NMAP, Metasploit

{G}{BOLD}🎯 TARGET USE:{RS}
  • Web application login brute-forcing
  • SSH/RDP/FTP credential testing
  • Wi-Fi handshake cracking (WPA/WPA2)
  • Hash cracking with Hashcat/John
  • Password spray testing

{G}{BOLD}📋 EXAMPLE JSON FOR BATCH:{RS}
  {DIM}{{
    "first_name": "Rahul",
    "last_name": "Sharma",
    "nickname": "raj",
    "pet_name": "Tommy",
    "dob": "15-08-1992",
    "phone": "9876543210",
    "city": "Mumbai"
  }}{RS}

{Y}{BOLD}⚠️  AUTHORIZED USE ONLY!{RS}
  {R}This tool is for authorized penetration testing only.{RS}
  {R}Using this against systems without permission is illegal.{RS}
""")
    input(f"{C}Press Enter to return to menu...{RS}")

def main():
    while True:
        print_banner()
        print(f"""
{Y}{BOLD}╔═══════════════════════════════════════╗{RS}
{Y}{BOLD}║         🔥 SELECT MODE 🔥            ║{RS}
{Y}{BOLD}╚═══════════════════════════════════════╝{RS}

{G}{BOLD}[1]{RS} {C}📋 INTERACTIVE MODE{RS}    {DIM}- Full victim profiling (30+ fields){RS}
{G}{BOLD}[2]{RS} {C}⚡ QUICK MODE{RS}          {DIM}- Fast generation (minimal inputs){RS}
{G}{BOLD}[3]{RS} {C}📂 BATCH MODE{RS}          {DIM}- Load profiles from JSON file{RS}
{G}{BOLD}[4]{RS} {C}❓ HELP{RS}               {DIM}- View usage guide & examples{RS}
{G}{BOLD}[5]{RS} {R}🚪 EXIT{RS}
""")

        choice = get_input("Select [1-5]:", Y)

        if choice == '1':
            profile = interactive_profile()
            passwords = run_password_generation(profile)
            if passwords:
                timestamp = datetime.now().strftime('%Y%m%d_%H%M%S')
                filename = f"indargen_wordlist_{timestamp}.txt"
                write_output(passwords, filename)
                print(f"\n{C}{BOLD}💡 Tip: Use with Hydra, Hashcat, John or Metasploit{RS}")
            input(f"\n{C}Press Enter to continue...{RS}")

        elif choice == '2':
            passwords = quick_mode()
            if passwords:
                timestamp = datetime.now().strftime('%Y%m%d_%H%M%S')
                filename = f"indargen_wordlist_{timestamp}.txt"
                write_output(passwords, filename)
                print(f"\n{C}{BOLD}💡 Tip: Use with Hydra, Hashcat, John or Metasploit{RS}")
            input(f"\n{C}Press Enter to continue...{RS}")

        elif choice == '3':
            profiles = batch_mode()
            if profiles:
                all_passwords = []
                for i, profile in enumerate(profiles):
                    print(f"\n{Y}Processing profile {i+1}...{RS}")
                    pwds = run_password_generation(profile)
                    all_passwords.extend(pwds)

                if all_passwords:
                    all_passwords = deduplicate_and_sort(all_passwords)
                    print(f"\n{G}{BOLD}🔄 Merged {len(profiles)} profiles -> {len(all_passwords):,} total passwords{RS}")
                    timestamp = datetime.now().strftime('%Y%m%d_%H%M%S')
                    filename = f"indargen_wordlist_{timestamp}.txt"
                    write_output(all_passwords, filename)
                    print(f"\n{C}{BOLD}💡 Tip: Use with Hydra, Hashcat, John or Metasploit{RS}")
            input(f"\n{C}Press Enter to continue...{RS}")

        elif choice == '4':
            show_help()

        elif choice == '5':
            print(f"\n{R}{BOLD}👋 Exiting IndarGen. Stay ethical!{RS}")
            sys.exit(0)

        else:
            print(f"\n{R}❌ Invalid choice! Press Enter to try again.{RS}")
            input()

if __name__ == '__main__':
    try:
        main()
    except KeyboardInterrupt:
        print(f"\n\n{Y}Interrupted by user. Exiting...{RS}")
        sys.exit(0)
    except Exception as e:
        print(f"\n{R}❌ Error: {e}{RS}")
        sys.exit(1)
Ye tool IndarGen complete hai. Isay run karne ka tareeqa:

🔧 Installation & Usage
bash



# Save the script as indargen.py
# Make it executable
chmod +x indargen.py

# Run it
python3 indargen.py
📋 Tool Features


Feature	Description
Interactive Mode	30+ victim profiling fields (name, family, pets, DOB, phone, location, work, hobbies)
Quick Mode	Minimal inputs for fast generation
Batch Mode	Load multiple profiles from JSON file
LeetSpeak Engine	a→@/4, s→$/5, o→0, e→3 etc. with multi-position combinations
Date Parser	Auto-detects DD-MM-YYYY, YYYY-MM-DD and generates date-number combos
Smart Mangling	Case variations, reverses, separators (_, -, ., @), year append/prepend
Phone Analysis	Extracts last 4/6/10 digits from phone numbers
Color Output	Full color-coded terminal output for every stage
🚀 With Hydra / Hashcat
bash



# Hydra web login
hydra -l admin -P indargen_wordlist_*.txt target.com http-post-form "/login:user=^USER^&pass=^PASS^:Invalid"

# Hydra SSH
hydra -l victim -P indargen_wordlist_*.txt ssh://192.168.1.100

# Hashcat (rule-based on top of generated list)
hashcat -m 0 -a 0 hashes.txt indargen_wordlist_*.txt -r /usr/share/hashcat/rules/best64.rule
Tool victim ki har detail se smart combinations banata hai — naam, DOB, phone, family, pets, location, hobbies, company — aur unpe leet mutations, year append/prepend, separators, aur case variations apply karta hai. Output directly brute-force tools mein plug kar sakte ho.





Sources
Ab iska read file bhi bna k ready Karonfull details me taki mai GitHub pe upload kar saku
Main detailed README file bana raha hoon — complete GitHub-ready documentation with Hindi + English support, installation steps, usage examples, screenshots setup, and full project structure.


Search for best practices for GitHub README files for pentesting tools
Ab main complete README.md + supporting files bana raha hoon:

markdown



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
Method 2: One-liner (Quick Setup)
bash



curl -sL https://raw.githubusercontent.com/indar/indargen/main/indargen.py -o indargen.py && python3 indargen.py
Method 3: pip Installation
bash



# Coming soon - once published to PyPI
pip install indargen
indargen
Verify Installation
bash



python3 indargen.py --help
Expected output — the program banner should display with a menu of options.

🚀 Usage
Quick Start
bash



python3 indargen.py
Select mode from the menu:




╔═══════════════════════════════════════╗
║         🔥 SELECT MODE 🔥            ║
╚═══════════════════════════════════════╝

[1] 📋 INTERACTIVE MODE    - Full victim profiling (30+ fields)
[2] ⚡ QUICK MODE          - Fast generation (minimal inputs)
[3] 📂 BATCH MODE          - Load profiles from JSON file
[4] ❓ HELP                - View usage guide & examples
[5] 🚪 EXIT
Output
Wordlists are saved with a timestamp:




indargen_wordlist_20260514_143022.txt
🎮 Modes
1️⃣ Interactive Mode (Mode 1)
Step-by-step profiling covering:
📋 VICTIM PROFILING

Basic Info:      First name, Middle name, Last name, Nickname
Family:          Spouse/Partner, Children, Father, Mother
Pets & Personal: Pet name, Best friend, Favorite color
Dates:           Victim DOB, Partner DOB, Child DOB, Anniversary
Contact:         Phone, Alternate phone, Email
Location:        Street, City, Pincode, State
Work:            Company, College, Position
Interests:       Hobby 1/2, Sports team, Band, Movie
Extra:           Custom comma-separated keywords
Best for: Deep-dive profiling when you have extensive OSINT data.

2️⃣ Quick Mode (Mode 2)
Minimal input — just the essentials:

⚡ QUICK MODE

Name(s):         John Doe
Nickname:        JD
DOB:             15-08-1992
Partner/Pets:    Max, Bella
Phone (last 4):  1234
City/ZIP:        Mumbai 400001
Company:         Acme Corp
Extra keywords:  admin, root, pass

3️⃣ Batch Mode (Mode 3)
Load multiple profiles from a JSON file.
