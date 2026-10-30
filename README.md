# Condenser

A lightweight Python desktop application built with *CustomTkinter* and the *OpenAI Python SDK*, configured to use Groq's API.

Condenser takes long blocks of text and converts them into short, readable bullet-point summaries. Every generated summary is automatically saved to a local history file with a timestamp.

---

## Features

- *One-click Condensing* — Paste text into the input box and click Condense to generate a concise bullet-point summary.
- *History Sidebar* — Every generated summary is saved with a timestamp and displayed in a scrollable history panel.
- *Preview Pane* — Select any history entry to display its complete summary in the preview panel.
- *Delete Entries* — Remove individual summaries using the ✕ button next to each entry.
- *Clear All* — Delete all saved history entries and update the local history file.
- *Dark Mode UI* — A CustomTkinter interface with a dark appearance and blue accent theme.
- *Persistent Storage* — Summaries are stored locally in history.json and restored when the application starts.
- *Background Processing* — API requests run in a separate thread, keeping the interface responsive.

---

## Technologies Used

| Technology | Purpose |
|---|---|
| Python | Application logic |
| CustomTkinter | Desktop GUI |
| OpenAI Python SDK | API communication |
| Groq API | Text summarisation |
| JSON | Local history persistence |
| Threading | Background API requests |
| Datetime | Summary timestamps |

## Concepts Used

### OpenAI-Compatible API Client

The application uses the OpenAI SDK with a custom base URL pointing to Groq's API.

python
client = OpenAI(
    base_url="https://api.groq.com/openai/v1",
    api_key=GROQ_API_KEY
)


This allows the application to communicate with Groq using the familiar OpenAI SDK interface.

### Threading for API Requests

API requests run in a background thread:

python
threading.Thread(
    target=worker,
    daemon=True
).start()


This prevents long-running API requests from blocking Tkinter's main event loop.

### Main-Thread UI Updates

Tkinter widgets should be updated from the main thread.

The application uses root.after() to schedule UI updates safely:

python
root.after(0, show_result, result)


This schedules show_result() to execute through Tkinter's event loop.

### JSON Persistence

The application uses two functions to manage local history:

- load_history() — Reads history.json and restores previously generated summaries on startup.
- save_history() — Writes the current summary history to history.json.

Each history entry contains a timestamp and its corresponding summary.

---

## Installation

### Requirements

- Python 3.10 or newer
- A Groq API key
- Internet connection

### 1. Clone the repository

bash
git clone <repository-url>
cd Condenser


### 2. Install dependencies

bash
pip install customtkinter openai


### 3. Configure your API key

Follow the instructions below to obtain and configure a Groq API key.

### 4. Run the application

bash
python main.py


---

## Getting a Groq API Key

The application uses Groq's API, which offers a free tier. Availability and usage limits depend on Groq's current policies.

1. Visit [Groq Console](https://console.groq.com/).
2. Sign up or sign in using your email or Google OAuth.
3. Open the [API Keys page](https://console.groq.com/keys).
4. Click *Create API Key*.
5. Give the key a name, such as Condenser, and submit.
6. Copy the generated key.

*Important:* The API key starts with gsk_ and is only displayed once when created. Store it securely.

---

## Configuring the API Key

Open main.py and locate:

python
GROQ_API_KEY = "YOUR API KEY HERE"


Replace the placeholder with your actual API key:

python
GROQ_API_KEY = "gsk_abc123..."


Keep the quotation marks around the key.

### Recommended: Environment Variable

If you intend to publish the project to a public repository, do not hardcode your API key.

Replace the original configuration with:

python
GROQ_API_KEY = os.environ.get("GROQ_API_KEY")


Set the environment variable before launching the application.

*Linux / macOS:*

bash
export GROQ_API_KEY="gsk_your_key_here"
python main.py


*Windows PowerShell:*

powershell
$env:GROQ_API_KEY="gsk_your_key_here"
python main.py


This keeps the API key out of the source code.

---

## Storage

Condenser automatically creates and updates:

text
history.json


Each entry contains:

- time — Date and time when the summary was generated.
- summary — The generated bullet-point summary.

Example:

json
[
  {
    "time": "2026-10-30 18:30",
    "summary": "- First key point\n- Second key point\n- Third key point"
  }
]


The file is stored locally in the application's working directory.

---

## AI Model

The application currently uses:

python
model="openai/gpt-oss-120b"


Requests are sent through Groq's OpenAI-compatible API endpoint:

text
https://api.groq.com/openai/v1


The summarisation prompt instructs the model to:

- Condense the supplied text.
- Produce short bullet points.
- Preserve important facts.
- Remove unnecessary detail.

The maximum response length is configured as:

python
max_tokens=800


---

## Suggested Improvements

The following improvements could make the application more robust and reliable.

### 1. Handle Invalid JSON on Load

Currently, json.load() can crash the application if history.json is corrupted or empty.

Wrap JSON loading in a try/except block and fall back to an empty list when the file cannot be parsed.

### 2. Close File Handles

Use context managers when reading and writing files:

python
with open(HISTORY_FILE, "r", encoding="utf-8") as file:
    history = json.load(file)


This ensures file handles are closed automatically.

### 3. Validate Input

The current condense() function only checks whether the input is empty.

Improvements could include:

- Stripping unnecessary whitespace.
- Rejecting inputs below a reasonable minimum length.
- Limiting maximum input size.
- Handling unsupported or excessively large requests.

### 4. Clean Shutdown

Register a window-close handler:

python
root.protocol("WM_DELETE_WINDOW", on_close)


Implement on_close() to save history and shut down cleanly, preventing incomplete writes or unfinished background operations.

---

## Project Structure

text
Condenser/
│
├── main.py
├── history.json
└── README.md


| File | Description |
|---|---|
| main.py | Main application, GUI, API integration and history management |
| history.json | Locally persisted summary history |
| README.md | Project documentation |

---
