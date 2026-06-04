# Groq Receipt / Invoice Extractor

A ready-to-run FastAPI project that lets you upload a receipt/invoice image in the browser and extracts structured JSON using Groq vision directly.

## Features

- Browser UI for image upload and JSON preview
- Direct image input to Groq vision model
- No separate OCR engine required
- Saves output JSON in `outputs/`
- Download JSON from UI
- Validates image type and size
- Compresses/resizes uploaded image before sending to Groq base64 vision request
- Adds image quality metadata: width, height, DPI metadata, format, quality warnings
- Supports Indian receipt fields: GSTIN, FSSAI, CGST, SGST, IGST, UPI, cash/card/mixed payment

## Project structure

```text
.
├── app/
│   ├── main.py
│   ├── config.py
│   ├── services/
│   │   ├── groq_service.py
│   │   ├── image_utils.py
│   │   ├── json_utils.py
│   │   └── prompt.py
│   ├── static/
│   │   ├── app.js
│   │   └── style.css
│   └── templates/
│       └── index.html
├── outputs/
├── uploads/
├── .env.example
├── requirements.txt
├── run.py
└── README.md
```

## Setup

### 1. Create virtual environment

Windows PowerShell:

```powershell
python -m venv venv
.\venv\Scripts\activate
```

macOS/Linux:

```bash
python3 -m venv venv
source venv/bin/activate
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Add Groq API key

Copy `.env.example` to `.env`:

Windows PowerShell:

```powershell
copy .env.example .env
```

macOS/Linux:

```bash
cp .env.example .env
```

Then edit `.env`:

```env
GROQ_API_KEY=your_real_groq_key_here
GROQ_MODEL=meta-llama/llama-4-scout-17b-16e-instruct
```

### 4. Run project

```bash
python run.py
```

Open:

```text
http://127.0.0.1:8000
```

## API endpoints

### UI

```http
GET /
```

### Health

```http
GET /health
```

### Extract JSON

```http
POST /api/extract
Content-Type: multipart/form-data
file=<image>
```

Example using curl:

```bash
curl -X POST "http://127.0.0.1:8000/api/extract" \
  -F "file=@receipt.jpg"
```

## Notes

- For best results, use a clear image with the full receipt visible.
- If your image is very large, the app compresses it before sending to Groq.
- If `needs_review=true`, check `validation_issues` in the JSON.
- Do not commit `.env` to GitHub.
