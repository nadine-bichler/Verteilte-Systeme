# Lab 01 - P2P Tron (Multiplayer Game)

A distributed peer-to-peer game in the style of **Tron** (light cycles). Multiple players connect directly to each other (no central server) over **UDP sockets** and exchange their game state as **JSON** messages.

## What the Project Does

- **Peer-to-peer architecture:** One player starts as the host (`Start`), other players join via `join Game`. There is no central broker.
- **Communication:** Message-based exchange over UDP sockets; messages are packed as JSON (`network/protocol.py`) and received on a background thread (`network/connection.py`).
- **Playing field:** Square grid (M x M, odd size, default 25), up to 4 players.
- **Gameplay:** Controlled with the arrow keys. Each player leaves a permanent trail behind. Collision with a wall, a trail, or another head eliminates the player.
- **Timeout:** A player who does not move for more than 100 seconds is eliminated automatically.
- **Network events:** `join_request`, `join_accept`, `join_reject`, `player_joined`, `move`, `dead`, `leave`, enabling dynamic joining and leaving at runtime.
- **GUI:** PySide6 interface with login/host menu, battlefield, player sidebar (score/alive status), and a final ranking dialog.

## Project Structure

```text
lab01/
├── lab01.iml                 # IntelliJ/PyCharm module definition
├── README.md
└── src/
    ├── main.py               # Entry point (starts QApplication and GameGUI)
    ├── main.spec             # PyInstaller spec for building an EXE
    ├── requirements.txt      # Dependencies (PySide6, ...)
    ├── controller/
    │   └── main_controller.py  # Mediates between GUI, game logic, and network
    ├── game/
    │   ├── entities.py         # Player and Battlefield (data models)
    │   └── game_engine.py      # Game logic: moves, collisions, ranking, grid matrix
    ├── gui/
    │   ├── main_window.py      # Main window, keyboard input, view switching
    │   └── widgets.py          # ConnectionWidget, BattlefieldWidget, PlayerListWidget
    └── network/
        ├── connection.py       # UDP socket, receive thread, signal/slot wiring
        └── protocol.py         # Builds JSON messages (move, dead, ...)
```

**Architecture (MVC-like):**

| Layer        | Folder        | Responsibility                                     |
| ------------ | ------------- | -------------------------------------------------- |
| GUI (View)   | `gui/`        | Rendering and input                                |
| Controller   | `controller/` | Connects view, game logic, and network; broadcasts |
| Model        | `game/`       | Game state, rules, ranking                         |
| Network      | `network/`    | UDP transport and JSON protocol                    |

## Requirements

- Python 3.11+ (tested with 3.11 - 3.14)
- Dependencies from `src/requirements.txt`

## Build & Run

### 1. Create a virtual environment and install dependencies

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r src\requirements.txt
```

### 2. Start

Run from the `src` folder so the packages are importable:

```powershell
python main.py
```

### 3. Play

- **Host:** choose nickname, own port, color, and grid size, then click `Start`.
- **Client(s):** choose nickname, own port, and color, enter the **host's IP and port** under *Join-Options*, then click `join Game`.
- In-game controls use the **arrow keys**. `Leave Game` exits the game.

> Note: The chosen local port must be free on each machine. The UDP port must be allowed through the firewall on the host.

### 4. (Optional) Build a standalone EXE

With [PyInstaller](https://pyinstaller.org/) using the provided spec:

```powershell
pip install pyinstaller
pyinstaller main.spec --distpath .\dist --workpath .\build
```

The result is placed under `dist/main/`.
