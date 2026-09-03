# AI Personal Assistant

A lightweight Flask web app powered by Google's Gemini API. It provides a simple browser interface for asking questions and turning long emails into short summaries.

## Features

- Ask a general question with a helpful-assistant system prompt.
- Summarize pasted email text in two or three sentences.
- JSON responses from both backend actions.
- Responsive static interface served by Flask.

## Tech stack

- Python 3.11+
- Flask
- Google Gen AI Python SDK
- Gunicorn for production serving
- HTML, CSS, and browser JavaScript

## Project structure

```text
.
├── main.py                 # Flask application and API routes
├── requirements.txt        # Python dependencies
├── render.yaml             # Render web-service configuration
├── static/
│   └── style.css           # Application styles
└── templates/
    └── index.html          # Browser interface
```

## Requirements

- Python 3.11 or newer
- A Google Gemini API key

## Run locally

1. Create and activate a virtual environment:

   ```powershell
   python -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```

2. Install dependencies:

   ```powershell
   python -m pip install -r requirements.txt
   ```

3. Create a `.env` file in the project root:

   ```env
   GEMINI_API_KEY=your_api_key_here
   ```

4. Start the development server:

   ```powershell
   python main.py
   ```

5. Open <http://127.0.0.1:5000>.

The `.env` file is ignored by git. Never commit the API key.

## API endpoints

### `GET /`

Returns the web interface.

### `POST /ask`

Form field:

```text
question=What is the capital of France?
```

Response:

```json
{"response": "Paris is the capital of France."}
```

### `POST /summarize`

Form field:

```text
email=Paste the email content here
```

Response:

```json
{"response": "A concise summary of the email."}
```

## Deploy to Render

This repository includes `render.yaml`, so it can be deployed as a Render Blueprint:

1. Push the project to a GitHub or GitLab repository.
2. In Render, choose **New +** and select **Blueprint**.
3. Connect the repository and select the branch containing `render.yaml`.
4. When prompted, enter the `GEMINI_API_KEY` secret.
5. Create the service and wait for the deploy to finish.

The configuration uses:

```text
Build command: pip install -r requirements.txt
Start command: gunicorn main:app
```

Render should expose the service over its generated `.onrender.com` URL. `GEMINI_API_KEY` must be configured as a secret environment variable before using `/ask` or `/summarize`.

## Troubleshooting deploy logs

- `ModuleNotFoundError`: confirm the package is listed in `requirements.txt` and the build command runs from the repository root.
- `Failed to find attribute 'app'`: confirm the start command is exactly `gunicorn main:app`.
- Gemini authentication errors: check that `GEMINI_API_KEY` is present in the Render environment and has not been pasted with extra spaces.
- A successful deploy should include dependency installation followed by Gunicorn starting the `main:app` WSGI application.

## Notes

The app currently uses the Gemini model configured in `main.py`. Model availability and API pricing are controlled by Google and may change independently of this project.