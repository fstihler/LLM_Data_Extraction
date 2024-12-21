# LLMs for Pathology Reports
This repo contains code for a project undertaken in the Spring 2024 iteration of STAT 222 at UC Berkeley.
Contributors include Shane Hogan, Isabel Moreno, Matthew Dworkin, and Frederik Stihler (names listed in no particular order)

## Required file structure
In order to run the notebooks provided in this repo, it is important to match this file structure:
```
-- assets/
    -- 100annotations.csv
    -- prompts/
        -- prompt1.txt
        -- prompt2.txt
        ...
    -- secrets.json
-- data/
    -- clean_ocr_txt/
    -- extracted_txt/
        -- prompt1/
            -- prompt1_041580F0-700A-4A47-83A6-207ED267E844.txt
            ...
        -- prompt2/
            -- prompt2_041580F0-700A-4A47-83A6-207ED267E844.txt
            ...
    -- pdfs/
    -- raw_ocr_txt/
```

`assets/secrets.json` contains the API key for LLAMA
`assets/100annotations.csv` contains the tabular gold standard manual annotations for 100 documents.
`assets/prompts/` contains the prompts used to generate the extracted outputs.



## Downloading the Data
Data for this project comes from The Cancer Genome Atlas (TCGA). Below are the steps taken to download the pdf files.
- [Download TCGA Command Line tool](https://gdc.cancer.gov/access-data/gdc-data-transfer-tool)
- Filter by Data Type "Pathology Reports" on [TCGA Data Repository](https://portal.gdc.cancer.gov/analysis_page?app=Downloads), and download the associated manifest file. 
- Download the data using the following command:
```bash
gdc-client download -m path/manifest.txt
```

## Preprocessing the Data
We first use Optical Character Recognition (OCR) to convert the pdf scans into text files. Below are the dependencies required for our conversion.
- Download [tesseract](https://github.com/tesseract-ocr/tesseract).
- Pip install the following packages:
  (1) `pytesseract`
  (2) `PIL` (Python Image Library)
  (3) `pdf2image`
```bash
python -m pip install pytesseract PIL pdf2image
```
- If you are running on Windows or Mac, [poppler](https://pypi.org/project/pdf2image/) will need to be downloaded for pdf2image.
- If necessary (i.e. running on SCF), add the relevant location to PATH:
```bash
  export PATH="$PATH:/accounts/grad/{ACCOUNT_ID}/.local/bin"
```
Next, we put the output of OCR into an LLM to fix spelling.

## API to LLM
See https://deepinfra.com/docs/getting-started for how to set up deepinfra API.

- Pip install openai

More information can be found here: https://deepinfra.com/codellama/CodeLlama-70b-Instruct-hf/api?example=openai-python