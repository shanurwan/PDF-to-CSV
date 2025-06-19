# 🧾 PDF Table Extractor to CSV (with pdfplumber)
Extract tables from multiple PDF files in a folder and convert them into a single CSV file using Python.

## 📌 Overview
This script automates the extraction of tabular data from all PDF files in a specified folder. It uses pdfplumber to identify and extract tables, and saves the results in a combined CSV file for further analysis.

## 🚀 pdfplumber Installation
Make sure you have Python 3.x installed.

Install the required libraries:

```python
pip install pdfplumber pandas
``` 
If you're using Jupyter Notebook:

```python
!pip install pdfplumber pandas
```

## 🧠 How It Works
- Reads all .pdf files inside the Bank Transaction folder.

- Extracts all tables from each page.

- Adds the source filename to each table.

- Combines all tables into a single CSV file.

## 💻 Usage
1 Place all your PDF files into a folder.

2 Run the following Python script:

```python
import pdfplumber
import pandas as pd
import os

# Folder containing all PDFs
pdf_folder = "folder_name"
output_csv = "all_pdf.csv"

all_data = []

for filename in os.listdir(pdf_folder):
    if filename.lower().endswith(".pdf"):
        pdf_path = os.path.join(pdf_folder, filename)
        print(f"Processing: {filename}")
        with pdfplumber.open(pdf_path) as pdf:
            for page_num, page in enumerate(pdf.pages):
                tables = page.extract_tables()
                for table in tables:
                    df = pd.DataFrame(table)
                    df['source_file'] = filename  # Track source file
                    all_data.append(df)

if all_data:
    combined_df = pd.concat(all_data, ignore_index=True)
    combined_df.to_csv(output_csv, index=False)
    print(f"✅ Done! Combined CSV saved as: {output_csv}")
else:
    print("⚠️ No tables found in any PDFs.")

```

## 📈 Sample Output
CSV Columns Example:

| Date       | Description      | Amount   | Balance  | source_file    |
|------------|------------------|----------|----------|----------------|
| 01/06/2024 | ATM Withdrawal   | -500.00  | 2500.00  | June2024.pdf   |
| 02/06/2024 | Salary Credit    | 3000.00  | 5500.00  | June2024.pdf   |

Actual output depends on the format of your PDF tables.

## ⚠️ Limitations
- Only works on PDFs with extractable tables (not scanned images).

- Table structure consistency improves accuracy.

## 🛠️ To-Do / Ideas
- Add support for OCR with Tesseract for scanned PDFs.

- Add CLI or GUI for easier use.

- Export to Excel format.
