# Chatting

Ollama chatting app — a single-file, dependency-free local AI chat GUI for Ollama.


A single-file, dependency-free chat app for your local Ollama instance. Modern dark theme by default, soft light theme with one click. No Node, no npm, no build step, no internet required — everything is inside `index.html`.

## Requirements

- [Ollama](https://ollama.com) installed and running locally
- Any modern browser (Chrome, Edge, Firefox)

## Run it

1. Start Ollama (if it isn't already running as a service):
   - **Linux / macOS:** `ollama serve` (skip if Ollama is already running in the menu bar / tray)
   - **Windows:** Ollama usually runs automatically after install
2. **Important (one-time):** allow the page to talk to Ollama by setting:
   - **Linux (systemd):** `sudo systemctl edit ollama` → add:
     ```
     [Service]
     Environment="OLLAMA_ORIGINS=*"
     ```
     then `sudo systemctl restart ollama`
   - **Linux / macOS (manual run):** `OLLAMA_ORIGINS=* ollama serve`
   - **Windows (PowerShell):**
     ```powershell
     setx OLLAMA_ORIGINS "*"
     ```
     then restart Ollama from the Start menu
3. Open `index.html` in your browser (double-click it, or `xdg-open index.html` / `open index.html`).

## Features

- **Model dropdown** — on launch the app calls Ollama's model list API (`/api/tags`, the HTTP equivalent of `ollama list`) and shows all installed models with their sizes. Manual refresh button included.
- **Connect / Disconnect**
  - *Connect* loads the selected model into memory (preloads it and keeps it resident).
  - *Disconnect* unloads the model from memory immediately (`keep_alive: 0`) and clears the chat history.
- **Chat window** with user / AI bubbles, streaming token-by-token output, and full message history sent as context.
- **Send** (or press `Enter`; `Shift+Enter` adds a newline) and **■ Stop** — aborts generation mid-stream; partial output and its stats are kept.
- **Tokens/sec stats** right below every AI bubble: **min · avg · max** tokens per second plus the token count. The average uses Ollama's own `eval_count` / `eval_duration` report for accuracy; min/max come from live streaming samples.
- **Dark / light theme** toggle (remembered between sessions).

## Configuration

- Ollama URL defaults to `http://localhost:11434` (the standard Ollama port). To change it, edit the `DEFAULT_URL` constant at the top of the `<script>` section in `index.html`.

## Troubleshooting

| Symptom | Fix |
|---|---|
| "Cannot reach Ollama" | Is Ollama running? Try `curl http://localhost:11434` — it should reply "Ollama is running". |
| Browser console shows a CORS / "blocked by CORS" error | Set `OLLAMA_ORIGINS=*` and restart Ollama (see step 2 above). |
| "No models installed" | Pull one: `ollama pull llama3.2` |
| Model answers seem to forget earlier messages | History resets when you press Disconnect (that also frees the model's memory). |
