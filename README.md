# Reverse Engineering Writeups

A collection of hands-on reverse engineering exercises, binary analysis experiments, and technical writeups documenting my journey into understanding compiled programs and low-level execution.

This repository focuses on analyzing binary behavior through static and dynamic analysis, understanding assembly instructions, and identifying how compiled applications process data and execute logic.

## Completed Challenges

### 1. REVERSE — PicoCTF

**Analysis:** Identified a hardcoded key stored in plaintext within the binary.

**Key concepts:**
- String analysis
- Static binary inspection
- Identifying hardcoded values

**Writeup:** [Read the analysis](https://github.com/drude087/REVERSE-ENGINEERING/blob/main/REVERSE/REVERSE.md)

### 2. Bbbbloat — PicoCTF

**Analysis:** Investigated a key represented in hexadecimal form and converted its numerical representation to identify the expected flag.

**Key concepts:**
- Integer representation
- Hexadecimal and decimal conversion
- Binary analysis

**Writeup:** [Read the analysis](https://github.com/drude087/REVERSE-ENGINEERING/blob/main/Bbbbloat/bbbbloat.md)

---

## Tools & Technologies

| Category | Tools / Concepts |
|---|---|
| Disassemblers | Ghidra, IDA Free |
| Debuggers | GDB, pwndbg |
| Binary utilities | strings, ltrace, strace |
| Architecture | x86, x64 Assembly |
| Binary formats | ELF |
| Languages | C |
| Methodology | Static and Dynamic Analysis |

## Research Interests

- Windows executable analysis
- Windows internals
- Driver reverse engineering
- Vulnerability research
- Malware analysis
- Binary exploitation fundamentals

## Repository Structure

```text
REVERSE-ENGINEERING/
├── REVERSE/
│   └── REVERSE.md
├── Bbbbloat/
│   └── bbbbloat.md
└── README.md
```

## Author

**Abishek**

- [GitHub](https://github.com/drude087)
- [LinkedIn](https://linkedin.com/in/abish3k/)
- [Medium](https://medium.com/@DANT3CH)

---

*Learning how software works beneath the surface, one binary at a time.*
