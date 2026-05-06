# IBM_Docling_MacOS_Robust

A local Docling workspace with MacOS install/setup guidance and examples for converting PDFs using the `docling` package. Utilizes Python virtual environment isolation and local model artifacts.

## Files

- `Docling Local CPU.py` - Example Python script showing how to use `docling` in both online and local-only modes on MacOS.
- `Docling MacOS CPU.sh` - Shell setup instructions for creating a virtual environment and installing `docling` on MacOS.
- `requirements.txt` - Optional package list that can be generated from the virtual environment.
- `models/` - Local directory for downloaded Docling model artifacts.

## Setup (MacOS)

1. Change into the repository directory:
   ```bash
   cd ~
   cd "DataSci/PythonWorkArea/IBMDocling/IBMDoclingVenv"
   ```
2. Create and activate a Python virtual environment:
   ```bash
   python3 -m venv venv
   source "venv/bin/activate"
   ```
3. Install `docling`:
   ```bash
   venv/bin/python3 -m pip install docling
   ```
4. Download the local model artifacts to the repository `models/` folder:
   ```bash
   docling-tools models download --all -o "models"
   ```

## Running Locally

### Option 1: Internet-connected mode

Use `Docling Local CPU.py` with a URL input to convert an online PDF:

```python
from docling.document_converter import DocumentConverter

source = "https://arxiv.org/pdf/2408.09869"
converter = DocumentConverter()
doc = converter.convert(source).document
print(doc.export_to_markdown())
```

### Option 2: Local-only mode

When disconnected from the internet, use a downloaded PDF and the local `models/` artifacts folder:

```python
from docling.datamodel.base_models import InputFormat
from docling.datamodel.pipeline_options import PdfPipelineOptions
from docling.document_converter import DocumentConverter, PdfFormatOption
import os

strUserID = os.getlogin()
#  Note:  (change from my file structure)
os.chdir("/Users/"+strUserID+"/DataSci/PythonWorkArea/IBMDocling/IBMDoclingVenv")

# Use the local repository path on macOS:
artifacts_path = "models"
pipeline_options = PdfPipelineOptions(artifacts_path=artifacts_path)

doc_converter = DocumentConverter(
    format_options={
        InputFormat.PDF: PdfFormatOption(pipeline_options=pipeline_options)
    }
)

source = "2408.09869v5.pdf"
result = doc_converter.convert(source)
print(result.document.export_to_markdown())
```

## Generating `requirements.txt`

To export installed packages from the virtual environment:

```bash
venv/bin/python3 -m pip freeze > requirements.txt
```

Then install dependencies on another machine with:

```bash
venv/bin/python3 -m pip install -r requirements.txt
```

## Notes

- Optionally set the local model artifacts path with the `DOCLING_ARTIFACTS_PATH` environment variable on MacOS:
  ```bash
  export DOCLING_ARTIFACTS_PATH="/Users/<your-user>/DataSci/PythonWorkArea/IBMDocling/IBMDoclingVenv/models"
  ```
- This README is based on the actual MacOS files `Docling Local CPU.py` and `Docling MacOS CPU.sh` in this folder.
