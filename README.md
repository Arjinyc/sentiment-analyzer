# AI Sentiment Analyzer

A beginner-friendly web app built with Python, Streamlit, and the OpenAI API.
Paste a customer review, click **Analyze**, and the app shows:

- the overall sentiment: **Positive**, **Negative**, **Neutral**, or **Mixed** (color-coded)
- a short explanation
- up to three specific things the customer liked or disliked
- one actionable improvement for the business

## What you need

- **Python 3.11 or newer.** Check with `python3 --version` (macOS) or `py --version` (Windows).
- **Your own OpenAI API key.** Create one at <https://platform.openai.com/api-keys>.
  This repository does not include a key; each person uses their own.

## Get the code

With Git (macOS or Windows):

```bash
git clone https://github.com/Arjinyc/sentiment-analyzer.git
cd sentiment-analyzer
```

Without Git: on the GitHub page, click the green **Code** button, choose **Download ZIP**, unzip it,
and open a terminal in the unzipped `sentiment-analyzer-main` folder.

## Setup and run on macOS

Open Terminal in the `sentiment-analyzer` folder, then run:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
cp .env.example .env
```

Open `.env` in a text editor and paste your key after `OPENAI_API_KEY=`. Then start the app:

```bash
python -m streamlit run app.py
```

## Setup and run on Windows

Open PowerShell in the `sentiment-analyzer` folder, then run:

```powershell
py -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
copy .env.example .env
```

Open `.env` in Notepad and paste your key after `OPENAI_API_KEY=`. Then start the app:

```powershell
python -m streamlit run app.py
```

If PowerShell refuses to run `Activate.ps1`, use Command Prompt instead and replace that line with
`.venv\Scripts\activate.bat`.

Streamlit opens the app in your browser at <http://localhost:8501>. Press **Ctrl+C** in the
terminal to stop it. Next time, you only need to activate the virtual environment and run
`python -m streamlit run app.py` again.

## Troubleshooting

**`TypeError: ... unexpected keyword argument 'width'`** means the app was started with an older
Streamlit installed somewhere else on your computer (for example, Anaconda), not the one in
`.venv`. Make sure your terminal prompt starts with `(.venv)` (run the activate command above if
it doesn't), then start the app with `python -m streamlit run app.py`. Using `python -m` makes
sure the virtual environment's Streamlit is the one that runs.

## How it works

Everything lives in `app.py`, which is split into four commented sections:

1. **Settings.** Loads your key from the environment or `.env` and sets the model, example
   reviews, colors, and the instructions for the AI.
2. **Pydantic models.** `ReviewAnalysis` and `Aspect` describe the exact JSON the AI must return.
   OpenAI's Structured Outputs feature enforces that shape, so the app always gets valid data
   to display.
3. **Helper functions.** `analyze_review()` calls `client.responses.parse()` and returns a
   `ReviewAnalysis` object. `show_results()` draws the color-coded result sections.
4. **Page layout.** The title, example buttons, text box, and **Analyze** button. Errors (missing
   key, rejected key, rate limits, network problems) are shown as clear messages on the page.

## Privacy

- The app has no database and never writes reviews to disk.
- Requests are sent with `store=False`, so OpenAI does not keep the response for later retrieval.
  OpenAI's standard [data policies](https://openai.com/policies/api-data-usage-policies) still apply.
- Your API key is read from the environment and never shown on the page. `.env` is listed in
  `.gitignore` so it isn't committed by accident.

## Changing the model

The app uses `gpt-5.4-mini` by default. To use a different model, add a line like
`OPENAI_MODEL=gpt-5-mini` to your `.env` file and restart the app.
