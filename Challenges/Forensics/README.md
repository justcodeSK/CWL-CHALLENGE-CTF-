# Digital Forensics Report – Live Streaming File Analysis

## Table of Contents
1. Scope & Objective  
2. Identify the File  
3. Safe Extraction  
4. PDF Metadata Analysis  
5. Extract Visible Text  
6. Check for Hidden URLs  
7. Conclusion  

---

# 1. Scope & Objective

This challenge involves the **forensic analysis of an archived file discovered during routine workstation maintenance**.  
The objective is to analyze the file safely and determine whether it contains:

- Malware
- Suspicious artifacts
- Hidden information
- Embedded data or external links

All analysis was performed using **standard Linux forensic tools in a controlled environment**.

---

# 2. Identify the File

The investigation began by identifying the file type and verifying its integrity.

Commands used:

```
ls -la
file Live_streaming.rar
```

### Findings

| Property | Value |
|--------|--------|
| File Name | Live_streaming.rar |
| File Size | 289,760 bytes (~283 KB) |
| Extension | .rar |
| Actual File Type | RAR archive data (version 5) |

The file extension matched the actual file type, confirming that it is a **valid RAR archive**.

---

## Security Precautions

Extracting files from unknown sources can introduce **malware risks**, therefore several precautions were taken.

### 1. Isolated Environment

The file was analyzed in:

- A **virtual machine**
- An **isolated analysis environment**

---

### 2. Hash Verification

The file hash was generated to maintain integrity tracking.

```
sha256sum Live_streaming.rar
```

This ensures the file remains unchanged during analysis.

---

### 3. Inspect Archive Before Extraction

The archive contents were inspected without extraction.

```
unrar l Live_streaming.rar
```

This step verifies the archive structure before interacting with the files.

---

### 4. Malware Scan

The archive was scanned using **ClamAV**.

```
clamscan Live_streaming.rar
```

### Safety Checks

Results showed:

- No suspicious filenames
- Archive contained **only one file**

```
Live_streaming.pdf
```

ClamAV reported **no malware detected**.

---

# 3. Safe Extraction

To maintain safe analysis practices, the archive was extracted into a dedicated directory.

```
unrar x Live_streaming.rar safe_extract/
```

Directory verification:

```
ls -la safe_extract/
```

File verification:

```
file safe_extract/Live_streaming.pdf
```

Malware scan of extracted file:

```
clamscan safe_extract/Live_streaming.pdf
```

---

### Extraction Details

| Property | Value |
|--------|--------|
| Extraction Folder | safe_extract/ |
| Extracted File | Live_streaming.pdf |
| File Size | 422,769 bytes (~412 KB) |
| File Type | PDF document |
| PDF Version | 1.4 |
| Page Count | 8 pages |

### Findings

The extracted file is a **standard PDF document** and the malware scan again reported:

```
No viruses detected
```

Both the archive and the extracted PDF appear **clean and safe**.

---

# 4. PDF Metadata Analysis

Metadata often contains useful forensic artifacts such as:

- Author information
- Software used
- Creation timestamps
- Embedded tools

Tool used:

```
exiftool safe_extract/Live_streaming.pdf
```

### Findings

The metadata revealed several key insights:

- The document appears to be a **Wikipedia page exported as PDF**.
- The PDF was generated using:

```
Headless Chrome
```

Rendering engine:

```
Skia/PDF
```

Creation date:

```
October 5, 2025
```

### Interpretation

The use of **Headless Chrome** suggests that:

- The document may have been **automatically generated**
- Possibly **scraped or downloaded programmatically**

This behavior is commonly seen in **automation scripts or data collection tools**.

---

# 5. Extract Visible Text

All visible text from the PDF was extracted for analysis.

Tool used:

```
pdftotext safe_extract/Live_streaming.pdf output.txt
```

### Findings

The extracted content confirmed that the document is a **standard Wikipedia article related to live streaming platforms**.

The content appeared normal and **showed no visible modifications**.

However, considering the context of the challenge, it was assumed that **hidden data might still exist within the document structure**.

---

# 6. Check for Hidden URLs

To identify hidden links or embedded references within the PDF, raw strings were extracted.

Command used:

```
strings safe_extract/Live_streaming.pdf | grep -i http
```

### Findings

Several URLs were discovered, including references to well-known platforms:

- YouTube
- TikTok
- Twitch
- Facebook
- Amazon
- The New York Times
- CNN
- The Guardian

All detected URLs corresponded to **normal reference links typically found in Wikipedia articles**.

No suspicious domains or hidden external connections were identified.

---

# 7. Conclusion

The forensic investigation involved multiple stages of analysis including:

- Archive verification
- Malware scanning
- Safe extraction
- Metadata analysis
- Text extraction
- URL inspection

Key findings:

- The archive contains **only one file (Live_streaming.pdf)**.
- Both the archive and the PDF are **clean and free from malware**.
- Metadata indicates the document was **generated using Headless Chrome**, likely from an automated process.
- Extracted text confirms the document is **a Wikipedia article about live streaming platforms**.
- URL analysis revealed **only legitimate reference links**.

Final assessment:

The analyzed PDF is **clean and contains no malicious payloads, hidden executables, or suspicious artifacts**.

The file appears to be a **standard exported document with no evidence of embedded malware or covert data**.
