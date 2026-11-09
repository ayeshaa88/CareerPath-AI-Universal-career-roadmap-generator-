# CareerPath AI – Universal Career Roadmap Generator

CareerPath AI creates a personal, step-by-step career roadmap for **any career in any field**:
medicine, nursing, law, teaching, engineering, accounting, arts, media, aviation,
agriculture, hospitality, sports, fashion, skilled trades, public service,
environmental work, technology, and any career you type yourself.

You describe your starting point (education, subjects, skills, available time,
language and goal) and Google Gemini writes a roadmap that fits you. It is honest
about careers that need degrees, entrance tests, licences or supervised training,
and it warns you when your chosen timeline is too short.

## Features

- Works for every profession. Pick from 23 career fields with suggestions, or type any career.
- Personal input form: name, age group, country, education level, subjects, status,
  skill level, existing skills, daily time, days per week, duration, language,
  learning style and career goal.
- Time check: the app calculates your weekly and total study hours and Gemini builds
  the plan around them, including a "Reality Check" at the top.
- Roadmap with 16 sections: introduction, scope, qualifications, skills, learning path,
  month-by-month plan, weekly schedule, books, free courses, tools, projects,
  certifications, experience, milestones, job preparation and a readiness checklist.
- Clear difference between what you can learn yourself and what needs formal
  education, accredited institutions or a licence.
- Any language: English, Urdu, Hindi, Arabic, or type another language.
- Right-to-left display for Urdu and Arabic inside the app (Nastaliq style for Urdu).
- Downloads: PDF, TXT and HTML (web page).
- API key kept safely in Streamlit Secrets.

## Technologies

| Part | Tool |
|---|---|
| Language | Python |
| Web app | Streamlit |
| AI | Google Gemini via the official `google-genai` SDK |
| PDF | ReportLab + free Google Noto fonts |
| Urdu/Arabic in PDF | `arabic-reshaper` + `python-bidi` |
| Hindi in PDF | `uharfbuzz` (text shaping) |
| HTML export | `markdown` |

## About multilingual PDFs (please read)

- The app downloads free **Noto** fonts the first time a PDF is made, so non-English
  text does not show as empty boxes.
- **Urdu and Arabic** are printed right-to-left with joined letters. Urdu uses the
  Naskh style in the PDF (the Nastaliq style used in newspapers cannot be drawn
  reliably by ReportLab).
- **Hindi** is shaped correctly when `uharfbuzz` is installed (it is in
  `requirements.txt`). Without it, some vowel signs may look out of place.
- If anything looks wrong in a PDF, download the **HTML** version, open it in your
  browser, then choose **Print → Save as PDF**. Browsers render every language
  perfectly, including Nastaliq Urdu.
- Optional: if your server cannot download fonts, create a folder named `fonts`
  next to `app.py` and put the `.ttf` files there (for example
  `NotoSans-Regular.ttf`, `NotoSans-Bold.ttf`, `NotoNaskhArabic-Regular.ttf`,
  `NotoSansDevanagari-Regular.ttf`). The app checks this folder first.

## Project files

```
careerpath-ai/
├── app.py            # the whole application
├── requirements.txt  # Python libraries
└── README.md         # this guide
```

## 1. Get a free Gemini API key

1. Go to **https://aistudio.google.com** and sign in with a Google account.
2. Click **Get API key** → **Create API key**.
3. Copy the key (it is a long string). Keep it private. Never put it in `app.py`
   or upload it to GitHub.

## 2. Run on your own computer (optional)

You need Python 3.10–3.12 installed.

```bash
# 1. Go to the project folder
cd careerpath-ai

# 2. Create and activate a virtual environment
python -m venv venv
# Windows:
venv\Scripts\activate
# Mac / Linux:
source venv/bin/activate

# 3. Install libraries
pip install -r requirements.txt
```

Create a folder `.streamlit` inside the project and, inside it, a file
`secrets.toml` with this content:

```toml
GEMINI_API_KEY = "paste-your-key-here"
```

Then start the app:

```bash
streamlit run app.py
```

Your browser opens at `http://localhost:8501`.

> Add `.streamlit/secrets.toml` to a `.gitignore` file so the key is never uploaded.

## 3. Deploy on Streamlit Community Cloud

1. Create a **public GitHub repository** (for example `careerpath-ai`) and upload
   `app.py`, `requirements.txt` and `README.md`.
2. Go to **https://share.streamlit.io** and sign in with GitHub.
3. Click **Create app** → **Deploy a public app from GitHub**.
4. Choose your repository, branch `main`, and main file `app.py`.
5. Open **Advanced settings**:
   - Python version: **3.12** (recommended).
   - Secrets: paste
     ```toml
     GEMINI_API_KEY = "paste-your-key-here"
     ```
6. Click **Deploy** and wait a few minutes while the libraries install.

To change the key later: open your app on share.streamlit.io → **⋮** → **Settings** → **Secrets**.

## Changing the Gemini model

The model name is set once at the top of `app.py`:

```python
GEMINI_MODEL = "gemini-3.8-flash"
```

Google retires older models from time to time. If you see "model unavailable",
check the current model list at https://ai.google.dev/gemini-api/docs/models and
either edit that line, or (no code change needed) add this to your Secrets:

```toml
GEMINI_MODEL = "new-model-name"
```

The app also tries the backup models in `BACKUP_MODELS` automatically.

## How to use the app

1. Fill in **About you** (all optional).
2. Choose your **education level** and **subjects**.
3. Under **The career you want**, browse a field and pick a career, or choose
   **Type my own career** and write any profession.
4. Set your **daily time**, **days per week** and **duration**. The app shows your
   total study hours.
5. Choose the **roadmap language** and **learning style**.
6. Click **Generate my roadmap** and wait 30–90 seconds.
7. Read the roadmap section by section and download it as **PDF**, **TXT** or **HTML**.

## Troubleshooting

| Message | What to do |
|---|---|
| "API key is missing" | Add `GEMINI_API_KEY` to Secrets and reboot the app. |
| "Gemini rejected the API key" | Copy the key again from Google AI Studio. |
| "usage limit has been reached" | Free tier limit. Wait a minute or try tomorrow. |
| "model is unavailable" | Change `GEMINI_MODEL` (see above). |
| PDF letters look wrong | Use the HTML download and print it to PDF. |
| Deployment fails while installing | Make sure Python 3.12 is selected in Advanced settings. |

## Disclaimer

Roadmaps are generated by AI and are for guidance only. Always confirm admission
rules, fees, licensing requirements and course details with official institutions.
