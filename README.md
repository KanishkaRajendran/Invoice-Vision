📄 Invoice Vision

Automated Invoice Data Extraction & Verification from Scanned PDFs

Extract structured data, validate it, and generate JSON, Excel, and confidence-based verification reports—all from image-based invoices.

📌 Overview

Invoice Vision is an intelligent document processing pipeline built to extract key fields from scanned invoices and detect the presence of signatures or stamps using a combination of OCR, computer vision, and rule-based validation.

🧠 Approach

This solution follows a modular pipeline approach:

1. Input Ingestion: Accepts PDF and image-based invoices.

2. Image Preprocessing: Enhances OCR accuracy through deskewing, denoising, and thresholding.

3. OCR (Optical Character Recognition): Extracts raw text using Tesseract OCR.

4. Key Field Extraction: Uses regex-based matching for fields like invoice number, date, and amount.

5. Signature/Stamp Detection: Employs contour analysis to identify probable signature or seal regions.

6. Output Structuring: Saves results in both JSON and CSV formats for downstream integration.

⚙️ Technologies & Models Used

| Task                 | Approach / Library                                     |
| -------------------- | ------------------------------------------------------ |
| OCR                  | `Tesseract OCR (pytesseract)`                          |
| PDF Parsing          | `PyMuPDF (fitz)`                                       |
| Image Preprocessing  | `OpenCV`, `scikit-image`                               |
| Text Parsing & Regex | Python `re` module                                     |
| Signature Detection  | Contour filtering with heuristics (area, aspect ratio) |
| Data Storage         | `pandas` (CSV), `json`                                 |

🧹 Preprocessing Pipeline

Each invoice (image or PDF page) undergoes the following steps before OCR:

* Grayscale Conversion: Simplifies pixel analysis

* Denoising: Non-Local Means denoising to reduce image noise

* Adaptive Thresholding: Binarization for OCR readiness

* Deskewing: Corrects rotation using the dominant angle of contours

These steps ensure higher accuracy in text extraction by cleaning and aligning the document.

🔍 Key Field Extraction
Using regular expressions on OCR’d text, the following fields are parsed:

📄 Invoice Number: Patterns like INV\d+ or variants

🗓️ Date: Multiple date format recognition (dd-mm-yyyy, yyyy/mm/dd, etc.)

💰 Total Amount: Currency-aware regex (₹\s?\d+[\d,]*)

🏷️ Vendor Name: Extracted using contextual text anchors (like "Vendor", "From")

🖋️ Signature / Seal Detection

Contours are extracted post-thresholding. Filters are applied:

* Area threshold to ignore noise

* Aspect ratio to detect typical signature layouts

* Bounding box cropping for identified regions

🔍 Fine-Tuning & Optimizations

1. Tesseract Fine-Tuning:

Custom datasets generated using makebox and lstmtraining

Improves recognition of invoice-specific fonts or layouts

2. Advanced Table Detection (Future-ready):

Potential integration with TableNet, CascadeTabNet, or DeepDeSRT


📤 Output

Each processed invoice results in:

* output/data.csv: Structured tabular data of extracted fields

* output/json/{filename}.json: Detailed JSON per invoice

* output/signatures/{filename}_sig.jpg: Cropped signature/seal image (if detected)

