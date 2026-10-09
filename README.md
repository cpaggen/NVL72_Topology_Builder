# NVL72 Rail Planner

An interactive, browser-based cabling planner for NVL72 GPU rails. It models GPU-to-ToR links, ToR-to-spine uplinks, switch port counts, link speeds, bandwidth oversubscription, and a downloadable CSV cable schedule.

## Run with uv

Install [uv](https://docs.astral.sh/uv/getting-started/installation/) and Python 3.10 or newer. From this folder, run:

```powershell
uv sync
uv run python serve.py
```

Then open <http://127.0.0.1:8000>. Stop the server with `Ctrl+C`.

The project has no third-party Python dependencies. `requirements.txt` is included for environments that use pip instead of uv.

## Run with Python and pip

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python serve.py
```

Open <http://127.0.0.1:8000>.

## Options

The server binds to localhost by default. To choose a different local port:

```powershell
uv run python serve.py --port 8080
```

The app is static and keeps planner inputs in the browser; the server only serves the files and does not store user data.
