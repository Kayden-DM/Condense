## Text Condenser
A desktop text summariser built with CustomTkinter and the OpenAI Python SDK, configured to use Groq's API. It takes long blocks of text and converts them into short bullet-point summaries, saving every result to a local history file.

## Features
One-Click Condensing — Paste text into the input box and click Condense to generate a bullet-point summary.

History Sidebar — Every summary is saved with a timestamp and shown in a scrollable history panel.

Preview Pane — Select any history entry to display its full summary in the preview box.

Delete Entries — Remove individual summaries with the ✕ button next to each row.

Clear All — Delete every saved entry and update the history file.

Input Validation — Rejects empty input and text over 10,000 characters.

Rate Limiting — Enforces a 2-second gap between API requests with time.sleep().

Prompt Injection Guard — The system prompt instructs the model to treat user text as data only and never follow instructions inside it.

Background Processing — API requests run in a separate thread so the interface stays responsive.

Main-Thread Updates — UI changes are scheduled with root.after(0, ...) to stay safe.

Persistent Storage — Summaries are stored in history.json and restored on startup, with corrupted files handled gracefully.

Dark Mode UI — CustomTkinter dark appearance with a blue accent theme.

## Technologies Used
text
| Technology | Purpose |
|---|---|
| Python | Application logic |
| `customtkinter` | Desktop GUI |
| `openai` | API client for Groq's OpenAI-compatible endpoint |
| Groq API | Text summarisation (`openai/gpt-oss-120b`) |
| `json` | Local history persistence |
| `os` | Checks whether the history file exists |
| `datetime` | Summary timestamps |
| `threading` | Background API requests |
| `time` | Rate limiting between requests |
<!-- -->
## How to Use It
Install Python 3.6 or newer from python.org and make sure it's added to your PATH.

Install the required packages by running pip install customtkinter openai.

Get a free API key from Groq Console and replace YOUR API KEY HERE in the code with your key (it starts with gsk_).

Save the code as condenser.py.

Open a terminal in the folder containing the file.

Run python condenser.py.

Paste text, click Condense, then browse, preview, or delete entries from the history panel.

## Improvements
Move the API key out of the source — GROQ_API_KEY is hardcoded, so publishing the file would leak it; using os.environ.get("GROQ_API_KEY") keeps the key out of the code.

Validate the API key at startup — if the placeholder is left unchanged, the app only fails when Condense is pressed; checking for "YOUR API KEY HERE" on launch and showing a clear warning would make setup mistakes obvious.
