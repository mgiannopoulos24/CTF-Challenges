![img](https://i.imgur.com/Xw2DVAT.png)

<img src='https://i.imgur.com/heKOInX.png' style='zoom: 80%;' align=left /> <font size='10'>Gawk</font>

29<sup>th</sup> November 2025

Prepared By: [deathwish24](https://app.hackthebox.com/users/2024290)

Challenge Author(s): [MrR3boot](https://app.hackthebox.com/users/13531)

Difficulty: <font color='lightgreen'>Very Easy</font> 
<br><br><br><br><br><br>


# Synopsis

- The user must interact with an exposed printer service using the Printer Exploitation Toolkit (PRET). By inspecting the printer's job queue or file system, the user retrieves a raw print job containing a Base64 encoded string. This string must be decoded into a corrupted PDF and subsequently repaired using Ghostscript to reveal the flag.

## Description
- I lost access to my computer and need a document urgently which got stuck in a printer. Can you get me the document ?

## Skills Required

- Network Enumeration
- Printer Exploitation (PJL/PostScript)
- Python Scripting
- PDF Analysis and Repair

## Skills Learned

- Using PRET to interact with printers via port 9100.
- Extracting raw data from printer memory/filesystem.
- Repairing malformed PDF streams using Ghostscript.

# Enumeration

## Interacting with the Printer

We start by identifying port 9100 (JetDirect) open on the target. To interact with the printer, we use [**PRET** (Printer Exploitation Toolkit)](https://github.com/RUB-NDS/PRET).

We connect to the target IP:

```bash
python2 pret.py <TARGET_IP> pjl
```

Once connected, we can enumerate the file system and current print jobs. The goal is to find temporary files or jobs in progress.

```bash
> ls
> jobs
```

We discover a job or file containing a massive block of text. Upon closer inspection, the content matches the header of a PDF file but is encoded in Base64. We extract this string for local processing.

# Solution

## The Script: Decoding and Repairing

The extracted Base64 string decodes into a PDF file, but simply decoding it results in a corrupted file that PDF viewers cannot open. To fix this, we write a Python script that:
1.  Decodes the Base64 string.
2.  Saves the raw bytes to a file.
3.  Uses **Ghostscript** (`gs`) to interpret the corrupted PDF stream and rewrite it into a valid, compliant PDF.

### The Solve Script

```python
import base64
import shutil
import subprocess

# The extracted Base64 data from the printer
base64_data = """
JVBERi0xLjQKJdPr6eEKMSAwIG9iago8PC9UaXRsZSAoQm9vayByZXBvcnQpCi9Qcm9kdWNlciAo
U2tpYS9QREYgbTk0IEdvb2dsZSBEb2NzIFJlbmRlcmVyKT4+CmVuZG9iagozIDAgb2JqCjw8L2Nh
... [Redacted for brevity] ...
404 [Redacted] ...
"""

def repair_pdf(in_file, out_file):
    # Check for Ghostscript
    gs = shutil.which("gs")
    if not gs:
        raise RuntimeError("Ghostscript not found")
    
    # Use Ghostscript to process the PDF and fix structural errors
    subprocess.check_call([
        gs,
        "-dSAFER",
        "-dNOPAUSE",
        "-dBATCH",
        "-sDEVICE=pdfwrite",
        "-o",
        out_file,
        in_file,
    ])

def main():
    print("[*] Decoding Base64 data...")
    # Decode and save corrupted PDF
    pdf_data = base64.b64decode(base64_data)
    with open("corrupted.pdf", "wb") as f:
        f.write(pdf_data)
    print("[*] Saved as 'corrupted.pdf'")
    
    # Repair the PDF
    print("[*] Attempting to repair PDF with Ghostscript...")
    repair_pdf("corrupted.pdf", "repaired.pdf")
    print("[*] Success! Flag should be in 'repaired.pdf'")

if __name__ == "__main__":
    main()
```

## Exploitation

We run the solution script:

```bash
python3 solve.py
```

The script produces a file named `repaired.pdf`.

### Getting the Flag

We open `repaired.pdf` using any standard PDF viewer. The document, now rendered correctly, displays the hidden text clearly.