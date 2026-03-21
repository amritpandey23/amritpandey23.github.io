# Amrit's Blog

This is a static website and blog built using [Nikola](https://getnikola.com/).

## 🤖 Agent-Friendly Information

### Project Structure
- `conf.py`: Main configuration for Nikola.
- `posts/`: Source Markdown files for blog posts.
- `pages/`: Source files for static pages.
- `files/`: Static assets (images, CSS, etc.) copied directly to `output/`.
- `output/`: The generated static website (do not edit manually).
- `venv/`: Local Python virtual environment.
- `memory-bank/`: Project context and history (see below).

### Key Commands
Run these from the project root after activating the virtual environment.

- **Build Site:** `nikola build`
- **Preview Site:** `nikola serve -b`
- **Auto-rebuild on change:** `nikola auto`
- **Create new post:** `nikola new_post`
- **Create new page:** `nikola new_page`

---

## 🚀 Getting Started

### 1. Prerequisites
- Python 3.x installed.

### 2. Setup Development Environment
```bash
# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On Unix or MacOS:
source venv/bin/activate

# Install dependencies (if not already done)
pip install Nikola[extras]
```

### 3. Build and Serve
```bash
nikola build
nikola serve -b
```

---

## 🧠 Memory Bank
This project uses a Memory Bank to store context for AI agents. Please update these files after significant changes.

Location: `memory-bank/`
- `activeContext.md`: Current focus and recent changes.
- `projectBrief.md`: High-level goals and project overview.
- `productContext.md`: Why this project exists and how it's used.
- `progress.md`: What's built and what's next.
- `systemPatterns.md`: Architectural decisions and patterns.
- `decisionLog.md`: Log of important technical decisions.
