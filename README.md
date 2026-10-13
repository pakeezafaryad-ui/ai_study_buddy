# AI Study Buddy — Phase 1 starter

A Streamlit study assistant for Computer Science, Mathematics, and Statistics. It supports Groq-powered tutoring, PDF/DOCX/TXT text extraction, image OCR, step-by-step solutions, MCQs, summaries, and flashcards.

## Files

- `app.py` — Streamlit application
- `requirements.txt` — Python dependencies
- `packages.txt` — system package needed for OCR on Streamlit Community Cloud
- `.gitignore` — ignores secret files and Python artifacts

## Run in Google Colab

1. Install dependencies:

   ```python
   !pip -q install -r requirements.txt
   !apt-get -qq update && apt-get -qq install -y tesseract-ocr
   ```

2. In Colab's left sidebar, open **Secrets** (key icon), add `GROQ_API_KEY`, enter the key value, and enable notebook access.
3. Load the key without printing it:

   ```python
   from google.colab import userdata
   import os
   os.environ["GROQ_API_KEY"] = userdata.get("GROQ_API_KEY")
   ```

4. Check syntax:

   ```python
   !python -m py_compile app.py
   print("Syntax check passed")
   ```

5. Run Streamlit inside Colab:

   ```python
   import subprocess, sys, time
   from google.colab import output

   log_file = open("/tmp/streamlit.log", "w")
   process = subprocess.Popen(
       [sys.executable, "-m", "streamlit", "run", "app.py",
        "--server.port", "8501", "--server.headless", "true"],
       stdout=log_file,
       stderr=subprocess.STDOUT,
   )
   time.sleep(5)
   print("If the app did not start, inspect /tmp/streamlit.log")
   output.serve_kernel_port_as_iframe(8501, height=800)
   ```

Keep the Colab notebook private. Do not create a public tunnel or deploy this prototype publicly until verified-email sign-in and the approved-email allowlist have been implemented.

## Run locally

```bash
pip install -r requirements.txt
streamlit run app.py
```

Set `GROQ_API_KEY` as an environment variable locally, or configure it in Streamlit Secrets when deploying. Never commit the key or a `.env`/`secrets.toml` file.

## Current prototype notes

- The default model is `llama-3.3-70b-versatile`; model availability can change, so check the provider console if a model error appears.
- Text is extracted from text-based PDFs. Scanned PDFs may need a later OCR enhancement; JPG/PNG photos are passed through Tesseract OCR.
- OCR may misread handwriting, equations, and special symbols. Verify mathematical working.
- Private email allowlisting is planned but not implemented in this starter. Do not publish it as a public app until that access control is added and tested.
- “Unlimited” access means the app itself will not add a subscription or artificial usage counter; the API provider and hosting platform may still impose quotas or costs.
