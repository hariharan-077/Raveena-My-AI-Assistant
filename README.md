# Raveena 🌱

**A personal AI assistant and companion that lives on your own computer.**

Raveena plans your day around your real routines, keeps track of your tasks and habits, and remembers what
matters to you. She is also someone you can talk to when you want to share how you're feeling. She learns about
you over time and talks to you accordingly.

This is my first project. It runs locally with [Streamlit](https://streamlit.io), stores everything in a single
SQLite file, and uses the **free Google Gemini API** as her brain.

> Raveena is a friendly companion, not a therapist. If you're in crisis she responds with care and points you
> to emergency services, a crisis line and someone you trust.

---

## ✨ What she can do

- 💬 **Chat like a person.** Say "remind me to send the report tomorrow" and it becomes a real task.
- 🗓️ **Plan your day** around your sleep, routines, energy and deadlines, and re-plan when things slip.
- ✅ **Manage tasks and goals** with plain-English capture ("report friday 2h high").
- 🔁 **Track habits** like water, study, exercise, sleep and meditation, with streaks that don't guilt-trip you.
- 🧠 **Remember you.** Everything she knows about you is visible, editable and deletable on the Memory page.
- 💛 **Emotional support.** Share how your day went, log your mood and energy, and she replies warmly and in
  your preferred tone (warm, coach or brief).
- 📝 **Weekly review** with gentle insights about your pace and your best working hours.
- ⏱️ **Focus timer** for sessions on your tasks.

## 🛡️ Built to be trusted

- **Local-first.** Your data lives in `data/raveena.db` on your machine. Export it as JSON or erase it any time
  in Settings.
- **Risky actions need your OK.** Deleting a task, goal, habit or memory never happens on its own. It waits as
  a card for your approval.
- **Everything is logged and undoable.** Every action she takes is written to an activity log with one-click undo.
- **Never stuck.** If Gemini is busy, rate-limited or you're offline, she switches to a simpler built-in
  **Lite mode** automatically.
- **You're in control.** Every automation can be switched off in Settings.

## 🚀 Quick start

You need **Python 3.10 or newer**.

```bash
git clone https://github.com/YOUR-USERNAME/raveena.git
cd raveena
```

**Windows:** double-click `run.bat`
**Mac / Linux:** `bash run.sh`

The first run sets up a private environment and installs what she needs (about a minute). Your browser opens at
`http://localhost:8501`, and she introduces herself with a few questions.

Prefer doing it by hand?

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
streamlit run app.py
```

## 🔑 Connect Gemini (free)

Without a key she runs in **Lite mode**: simple commands and a listening ear, but no real conversation. With a key
you can just talk to her.

1. Create a free key at [aistudio.google.com/apikey](https://aistudio.google.com/apikey).
2. Paste it on the onboarding screen or in **Settings**, or copy `.env.example` to `.env` and set
   `GEMINI_API_KEY=your_key`.

The default model is `gemini-flash-latest`. If you hit rate limits, try `gemini-flash-lite-latest`, which is
faster and has higher free limits.

**Privacy note:** each message sends your recent conversation and a compact summary of your day (tasks, schedule,
habits, goals, memories) to Google. On the free tier Google may use prompts to improve its products. If that
bothers you, switch AI off in Settings and keep using Lite mode.

## 🧩 How it works

Raveena runs a streaming **function-calling loop** with Gemini:

```
you speak → Gemini decides which tools to call → tools run through a safety layer
          → results go back to Gemini → Gemini replies in her own voice
```

Every ability is a tool in `raveena/tools.py` marked **low risk** (runs now, logged, undoable) or **high risk**
(waits for your approval). Automatic function calling is switched off on purpose, so nothing bypasses that layer.

| Folder / file | What's inside |
| --- | --- |
| `raveena/brain.py` | Gemini integration, tool loop, fallbacks and error handling |
| `raveena/tools.py` | Everything she can do, with risk levels and undo |
| `raveena/planner.py` | Scoring and day planning |
| `raveena/persona.py` | Her voice, tone and rules |
| `raveena/lite.py` | Offline Lite mode |
| `raveena/ui/` | Streamlit pages (Chat, My day, Tasks, Habits, Insights, Memory, Settings) |

## 🛠️ Make it yours

- **Her voice and rules:** `raveena/persona.py`
- **A new ability:** write a handler in `raveena/tools.py` and decorate it with `@tool(...)`. It then shows up for
  Gemini, goes through the activity log and undo, and asks for approval if you mark it `risk="high"`.
- **The look:** `raveena/ui/theme.py` and `.streamlit/config.toml`

## 🧪 Development

```bash
pip install -r requirements-dev.txt
pytest
```

The suite has 120+ tests covering the planner, the safety layer, Lite mode, every page, and the Gemini
integration (using a fake client built from the real SDK types).

Want to explore with realistic data first? Use a separate demo database that never touches your own:

```bash
RAVEENA_DATA_DIR=data-demo python scripts/seed_demo.py
RAVEENA_DATA_DIR=data-demo streamlit run app.py
```

## ⚠️ Limitations

- Streamlit only runs while the app is open, so there are no background notifications yet.
- No calendar sync, email access or voice. Text only for now.
- Lite mode is pattern matching, not understanding, and it tells you so.
- Gemini's free tier has per-minute and daily limits, and its servers are sometimes busy.

## 🗺️ Ideas for the future

- Gentle reminders via Telegram or ntfy
- Calendar sync
- Voice chat

## 🤝 Contributing

This is my first project, so feedback and ideas are very welcome. Open an issue or a pull request.

## 📄 License

Add a license before sharing widely. [MIT](https://choosealicense.com/licenses/mit/) is a common, simple
choice. On GitHub, use **Add file → Create new file → name it `LICENSE`** and pick a template.

---

Built with Python, Streamlit, SQLite and the Google Gemini API.
