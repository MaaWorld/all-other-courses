---
icon: openai
---

# Completing the App: Pixel Art Canvas

> Explore how to build a pixel art canvas application using OpenAI Codex with Flask and SQLite. Learn to scaffold the base app, implement a drawing grid with color palette, and add saving functionality. This lesson teaches effective prompt writing for Codex to create well-structured code and demonstrates how Codex supports testing automatically.

Our project folder is ready with `AGENTS.md` in place. Now we will begin implementing the pixel art canvas app using Codex. To keep things manageable, we have broken the implementation into four tasks. Each task is a single Codex prompt that adds one working feature to the app:

1. **Scaffolding the base app:** This sets up the Flask project with a database, a `Canvas` model, and an index route. By the end of this task, the app starts and the database is created automatically.
2. **Drawing and saving feature:** This adds a 32x32 drawing grid and a colour palette so we can paint pixel art. It also adds the ability to give our canvas a name and save it to the database.
3. **Adding gallery:** This creates a second page that shows all saved canvases with pixel previews and lets us navigate between the drawing page and the gallery.
4. **Adding PNG export feature:** This adds a download button to each gallery card that exports the canvas as a PNG file using Pillow.

Let’s begin implementing the app. In this lesson we will scaffold the base project and implement the drawing and saving feature.

### Writing prompts for Codex <a href="#writing-prompts-for-codex" id="writing-prompts-for-codex"></a>

Before we start writing code, it is worth understanding how to communicate effectively with Codex. **Prompt engineering** is the practice of writing clear, well-structured instructions that guide an AI model to produce the output we need. With Codex, good prompts directly affect the quality, structure, and correctness of the code it generates.

Here are some best practices to follow when writing prompts for Codex:

* **Provide context upfront for the first task:** If Codex does not know what kind of project it is working on, it makes assumptions. The first prompt should describe the full scope of the app and the tech stack so Codex has the complete picture before writing a single file.
* **Keep each task focused on one feature:** A prompt that asks for multiple unrelated things at once produces code that is harder to review. One feature per prompt makes it easy to read the diff and verify the change.
* **Reference specific files when it matters:** If we want a change in a specific file or function, naming it removes ambiguity. If we want Codex to decide the structure, we leave it out.
* **Let** `AGENTS.md` **handle conventions:** Codex reads `AGENTS.md` before every task. There is no need to repeat project conventions in every prompt. Write the convention once in `AGENTS.md` and Codex applies it automatically.

> If you want to learn writing effective prompts, explore our course All You Need to Know About Prompt Engineering.

Now that we know how to write effective prompts, let's start building.

### Scaffolding the base app <a href="#scaffolding-the-base-app" id="scaffolding-the-base-app"></a>

We will begin by giving Codex the full picture of the app we are building and then asking it to implement the foundation. Providing the full context upfront means Codex understands the shape of the project before writing a single file, which leads to better structural decisions from the start. Here is the prompt we give to Codex:

> **Prompt:** We are building a Flask pixel art canvas app. The app will let users draw on a 32×32 grid, save their artwork with a name, browse a gallery of saved canvases, and export any canvas as a PNG file. The stack is Flask, SQLite, Flask-SQLAlchemy, HTML5 Canvas, vanilla JavaScript, and Pillow.
>
> For this first task, scaffold the base app:
>
> * `app.py`**:** Flask app with SQLAlchemy configured and the database initialised on startup
> * `models.py`**:** A `Canvas` model with fields: `id` (integer, primary key), `name` (string, required), `pixel_data` (JSON, stores the 32×32 grid as a 2D array of hex colour strings), `created_at` (timestamp, auto-set)
> * `templates/index.html`**:** A minimal base HTML page that the index route serves
> * An index route in `app.py` that returns the index template
>
> The app should start without errors when we run `python3 app.py`.

Codex reads the prompt, explores the project folder, and generates the basic structure. Here is the code:

[**https://github.com/prince-chhirolya/openai-codex-projects-for-agentic-coding**](https://github.com/prince-chhirolya/openai-codex-projects-for-agentic-coding) (Complete Project Source Code on GitHub)

Let’s see what Codex did. It created three files:

* `app.py`**:** Sets up the Flask app with a SQLite database, adds the index route, and creates the database tables automatically on startup.
* `models.py`**:** Defines the `Canvas` model with fields for the canvas name, the pixel grid stored as JSON, and a timestamp that is set automatically on every save.
* `templates/index.html`**:** A minimal HTML page that the index route serves, confirming the app is running.

Our base foundation is ready. Now, let’s implement the drawing and saving feature.

### Implementing the drawing and saving feature <a href="#implementing-the-drawing-and-saving-feature" id="implementing-the-drawing-and-saving-feature"></a>

Now we will ask Codex to add the drawing and saving feature. We describe what the user should be able to do and let Codex figure out how to build it. Here is the prompt:

> **Prompt:** The base app is running. Now add the drawing and saving feature. The user should be able to draw on a 32×32 pixel grid, pick a colour from a palette, give their canvas a name, and save it. Saving should store the canvas in the database and confirm success on screen.

Codex reads the prompt, reviews the existing files, and implements the full feature. Here is the code:

[**https://github.com/prince-chhirolya/openai-codex-projects-for-agentic-coding**](https://github.com/prince-chhirolya/openai-codex-projects-for-agentic-coding) (Complete Project Source Code on GitHub)

Let’s see what Codex did. It created six new files and updated two existing ones:

* `app.py` **(updated):** Refactored into an app factory with a `create_app()` function. This keeps the route handlers and configuration separate and makes the app easier to test.
* `routes/canvas.py`**:** A new routes file with two endpoints. One serves the drawing page and one handles saving a canvas to the database.
* `services/canvas_service.py`**:** A new service file that handles all the business logic: defining the colour palette, validating the pixel grid, and writing to the database.
* `templates/index.html` **(updated):** Replaced with a two-panel layout showing the colour palette and Save form on the left and the drawing grid on the right.
* `static/app.js`**:** A new JavaScript file that handles drawing on the grid, colour selection, and sending the saved canvas to the API.
* `static/styles.css`**:** A new stylesheet for the two-panel layout.

**Codex wrote tests without being asked**

> If we look at the project code, we can see a `tests/test_canvas_routes.py` file. We never asked for test cases in our prompt, but Codex generated them anyway and ran them before reporting back. This is one of the most valuable behaviours Codex exhibits: it reads `AGENTS.md`, sees that `pytest tests/ -v` is listed as the test command, and treats testing as part of completing the task rather than an optional extra. The result is a test suite that covers the index route, a successful save, and a failed save with a missing name — all verified before we even reviewed the output.

The drawing interface is now live. We can paint on the grid, pick a colour, enter a name, and save our canvas to the database.

Both tasks followed the same pattern: a short, outcome-focused prompt produced a complete, well-structured implementation. The prompts described what to build and `AGENTS.md` handled the conventions behind the scenes. Codex applied the project structure, kept business logic in service functions, and even wrote and ran tests without being asked. Writing good prompts and a well-defined `AGENTS.md` is what makes this possible.
