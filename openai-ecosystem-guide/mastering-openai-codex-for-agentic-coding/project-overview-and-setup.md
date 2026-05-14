---
icon: openai
---

# Project Overview and Setup

> Explore how to start a pixel art canvas project using Flask and SQLite by configuring the environment, installing dependencies, and writing an AGENTS.md file. Understand how setting project structure and conventions upfront guides Codex to delegate tasks effectively and maintain reliability throughout development.

Building software with Codex is best understood through practice. We are going to build a pixel art canvas app from scratch, delegating every implementation task to Codex and reviewing each change before it is accepted.

The app lets us draw on a 32×32 grid of coloured pixels, save artwork with a name, browse a gallery of saved pieces, and export any canvas as a PNG file. It covers a backend, a database, a JavaScript frontend, and file generation, which makes it a good fit for delegating to Codex piece by piece.

### A glimpse of the final product <a href="#a-glimpse-of-the-final-product" id="a-glimpse-of-the-final-product"></a>

Before we dive into implementation, let’s look at what we’re building. Having the end in mind makes the process clearer.

The finished app will be an interactive pixel art canvas app with a color pallete. You can create and save different drawings, see all the saved drawings in the gallery and export them as a PNG file.

> You can see the application in action by executing the code below:

```py
// app.py

from pathlib import Path
from flask import Flask
from models import db
from routes.canvas import canvas_bp

BASE_DIR = Path(__file__).resolve().parent

def create_app(test_config=None):
    app = Flask(__name__)
    app.config["SQLALCHEMY_DATABASE_URI"] = f"sqlite:///{BASE_DIR / 'pixel_art.db'}"
    app.config["SQLALCHEMY_TRACK_MODIFICATIONS"] = False
    if test_config:
        app.config.update(test_config)
    db.init_app(app)
    app.register_blueprint(canvas_bp)
    with app.app_context():
        db.create_all()

    return app

app = create_app()

if __name__ == "__main__":
    app.run(host='0.0.0.0', debug=True)
```

### Our technology stack <a href="#our-technology-stack" id="our-technology-stack"></a>

Here is everything we will use and what each piece is for:

* **Flask:** Python web framework for building the backend and serving routes.
* **SQLite:** Lightweight file-based database for storing canvas data.
* **Flask-SQLAlchemy:** ORM layer for interacting with SQLite without writing raw SQL.
* **HTML5 Canvas:** Browser API for rendering the 32×32 drawing grid.
* **Vanilla JavaScript:** Handles drawing interactions and communicates with the Flask API.
* **Pillow:** Python imaging library for generating PNG exports from canvas data.
* **pytest:** Test framework for verifying each feature Codex builds.

Before writing a single line of application code, we set up the project structure and write an `AGENTS.md`. Getting this foundation right means every Codex task from here on starts from the same shared understanding of the project’s conventions.

### Setting up the project <a href="#setting-up-the-project" id="setting-up-the-project"></a>

Let’s create the project folder and get everything in place before opening Codex. Open a terminal and run:

```
mkdir pixel-art-canvas
cd pixel-art-canvas
```

### Creating requirements.txt <a href="#creating-requirementstxt" id="creating-requirementstxt"></a>

Next, create a file named `requirements.txt` in the project root with the following dependencies:

```
flask
flask-sqlalchemy
pillow
pytest
```

Then install them:

```
pip install -r requirements.txt
```

The project now has everything installed.

### Writing `AGENTS.md` for the project <a href="#writing-agentsmd-for-the-project" id="writing-agentsmd-for-the-project"></a>

`AGENTS.md` is the instruction file Codex reads before every task. Writing it now, before any code exists, means every task Codex runs will follow the same conventions from the start. Here is what we will include:

* **Setup and test commands:** The install command and the pytest command Codex will use to verify its own work after each change.
* **Project structure:** A short map of where each part of the app will live once Codex builds it, giving it a reference before the files exist.
* **Do’s and don’ts:** The conventions this project follows: SQLAlchemy for database access, Pillow for export, no raw SQL, no print statements for logging.
* **Safety and permissions:** What Codex can do without asking and what requires confirmation first.
* **Review guidelines:** The checks Codex applies when reviewing its own changes.

Create a file named `AGENTS.md` at the project root with the following content:

```
# Pixel art canvas — agent instructions

## Setup
pip install -r requirements.txt

## Tests
pytest tests/ -v
# Single file: pytest tests/test_routes.py -v

## Project structure
- App entry point: app.py
- Database models: models.py
- Routes: routes/ (one file per feature area)
- Templates: templates/
- Static files: static/
- Tests: tests/

## Do
- Use Flask-SQLAlchemy for all database access
- Use Pillow for PNG export
- Use pytest for all tests
- Keep route handlers thin: business logic belongs in service functions

## Don't
- Do not use print statements for logging
- Do not write raw SQL: use the SQLAlchemy ORM
- Do not add new dependencies without updating requirements.txt

## Safety and permissions
Allowed without asking: read files, run single-file tests, lint
Ask first: deleting files, installing new packages, running the full test suite

## Review guidelines
- Flag any route that modifies data without validating the input
- Flag any database query that could return unbounded results
```

The folder structure at this point is minimal. Everything else will be built by Codex:

```
pixel-art-canvas/
├── requirements.txt
└── AGENTS.md
```

### Opening the project in Codex <a href="#opening-the-project-in-codex" id="opening-the-project-in-codex"></a>

Open the Codex Desktop App and open the `pixel-art-canvas` folder. Codex initialises a project for it automatically.

Before asking Codex to write any code, let’s confirm it has read `AGENTS.md` correctly. Open a new thread in the Codex Desktop App and send this prompt:

> **Prompt:** Read the AGENTS.md file and summarise the project conventions, test command, and the structure we expect. Tell me if anything is missing or unclear.

Codex should respond with an accurate summary of the conventions, the pytest command, and the folder structure we defined. If anything in its summary does not match what we wrote, this is the right moment to correct `AGENTS.md` before any code is generated. A small correction here prevents the same mistake appearing across every task downstream.

The project has the right dependencies installed, and an `AGENTS.md` that tells Codex exactly how this project is built. That foundation is what makes every subsequent task reliable: each prompt Codex receives from here on inherits these conventions automatically, without us repeating them.
