---
icon: openai
---

# Completing the App: Gallery and PNG Export

> Explore how to complete a pixel art canvas application by adding a gallery page to browse saved artworks and a download feature to export pixel art as PNG files. Learn to integrate these features using Flask, SQLite, and Codex prompts, including automated test updates for robust functionality.

The drawing and saving feature is working. We can paint on a 32x32 grid, choose a colour from the palette, give our canvas a name, and save it to the database. What we cannot do yet is view any of those saved canvases or get them out of the app as image files. In this lesson we complete the build with two more feature:

1. Adding a gallery page where every saved canvas appears with a pixel preview, its name, and the date it was saved.
2. Adding a Download button to each gallery card that generates and downloads a PNG file of the artwork.

Once both features are implemented, the app is fully working end to end.

### Adding a gallery page <a href="#adding-a-gallery-page" id="adding-a-gallery-page"></a>

Now we will ask Codex to add the gallery page. The drawing page already has a way to save canvases, so this task is about giving us somewhere to browse them. We describe the page from the user’s perspective and let Codex handle the structure. Here is the prompt:

> **Prompt:** Add a gallery page that shows all saved canvases. Each canvas should display its name, the date it was saved, and a visual preview of the pixel art. The gallery should be accessible from the main drawing page and link back to it.

Codex reads the prompt, checks the existing routes and templates, and builds the full gallery feature. Here is the code:

[**https://github.com/prince-chhirolya/openai-codex-projects-for-agentic-coding**](https://github.com/prince-chhirolya/openai-codex-projects-for-agentic-coding) (Complete Project Source Code on GitHub)

Let’s see what Codex did. It created two new files and updated four existing ones:

* `templates/gallery.html`**:** A new page with a navigation bar at the top, a hero section with a heading and description, and a responsive grid of canvas cards. Each card shows a pixel preview of the artwork, the canvas name, and the formatted save date. An empty state message is shown when no canvases have been saved yet.
* `static/gallery.js`**:** A new JavaScript file that reads the pixel data stored in each card and renders it onto a small canvas element as a pixel preview.
* `routes/canvas.py` **(updated):** A new `GET /gallery` route was added that fetches all saved canvases and passes them to the gallery template.
* `services/canvas_service.py` **(updated):** A new `list_canvases()` function was added that queries all canvas rows from the database ordered by save date, newest first.
* `templates/index.html` **(updated):** A navigation bar was added to the drawing page so we can switch between the editor and the gallery.
* `static/styles.css` **(updated):** New styles were added for the gallery layout, canvas cards, pixel previews, and the shared navigation bar.

> **Note: Codex updated the tests again**
>
> Looking at the project code, we can see that `tests/test_canvas_routes.py` now has a fourth test: `test_gallery_route_shows_saved_canvases`. Codex added it without being asked, inserting a canvas directly into the test database and asserting that it appears on the gallery page.

The gallery page is live. We can navigate from the drawing page to the gallery and see all our saved canvases with their previews.

### Adding download button for PNG export <a href="#adding-download-button-for-png-export" id="adding-download-button-for-png-export"></a>

The gallery shows our saved canvases, but we have no way to get them out of the app yet. Now we will ask Codex to add a download feature. We want each gallery card to have a button that generates a PNG file of the canvas artwork. We specify Pillow here because it is already in our `requirements.txt` and it is the library we chose for this task. Here is the prompt:

> **Prompt:** Add a download feature. Each canvas in the gallery should have a Download button that exports the pixel art as a PNG file. Use Pillow to generate the image.

Codex reads the prompt, locates the existing gallery template and service layer, and adds the export feature. Here is the code:

[**https://github.com/prince-chhirolya/openai-codex-projects-for-agentic-coding**](https://github.com/prince-chhirolya/openai-codex-projects-for-agentic-coding) (Complete Project Source Code on GitHub)

Let’s see what Codex did. It updated four existing files:

* `services/canvas_service.py` **(updated):** Three new functions were added. `get_canvas()` fetches a single canvas by id and returns a 404 if it does not exist. `build_canvas_png()` creates a 32x32 Pillow image, sets each pixel using the stored hex colour values, scales it up to 512x512 using nearest-neighbour resizing to keep the pixels sharp, and returns the image as a byte buffer ready to send. `build_download_filename()` generates a clean filename from the canvas name, for example `ocean-breeze.png`.
* `routes/canvas.py` **(updated):** A new `GET /canvases/<id>/download` route was added. It fetches the canvas, calls `build_canvas_png()`, and returns the buffer as a file attachment using Flask’s `send_file()`.
* `templates/gallery.html` **(updated):** A Download PNG link was added to each canvas card pointing to the download route for that canvas.
* `static/styles.css` **(updated):** Styling was added for the download link button.

> **Note: The test suite now covers the export route**
>
> Codex added a fifth test to `tests/test_canvas_routes.py` without being asked. `test_download_canvas_returns_png` creates a canvas with specific coloured pixels, hits the download route, checks the response is a PNG attachment with the correct filename, then opens the image with Pillow and asserts the exact pixel colours at the expected positions. All five tests pass. Codex has been verifying its own work after every task, and the test suite has grown alongside the app.

The app is now complete. We can draw pixel art, save it, browse the gallery, and download any canvas as a PNG file.

Four tasks, four prompts, and the app is complete. Every structural decision came from `AGENTS.md` and every feature was delivered by describing the outcome and letting Codex handle the implementation. The test suite grew alongside the code at each step, entirely unprompted. This is the workflow that makes Codex a genuine collaborator rather than just a code generator: clear conventions in `AGENTS.md`, focused prompts, and consistent review of every change before it is accepted.

### Do it yourself <a href="#do-it-yourself" id="do-it-yourself"></a>

The app works end to end, but there is plenty of room to extend it. Here are some ideas to explore with Codex using the same approach we used throughout this build: write a short prompt describing what we want, and let Codex implement it.

* **New canvas button:** Right now, reloading the drawing page is the only way to start a fresh canvas. A “New canvas” button that clears the grid without a full page reload would make the editor more usable.
* **Eraser tool:** Currently, selecting white from the palette acts as an eraser since it paints white over existing pixels. A dedicated eraser tool with a clear label would make this behaviour explicit and easier to discover.
* **Colour picker:** The palette only has eight fixed colours. Adding a colour picker input would let us paint with any hex colour without being limited to the preset swatches.
* **Edit a saved canvas:** Add a way to open a canvas from the gallery back in the editor so we can continue drawing on it. This would require loading the saved pixel data into the drawing grid on the editor page.
* **Delete a canvas:** Add a Delete button to each gallery card that removes the canvas from the database. A confirmation step before deletion would be a good addition.
* **Gallery search and filtering.** The gallery shows all canvases sorted by newest first but has no way to search or filter. A name search input at the top of the gallery page would help once the collection grows.

Each of these is a self-contained feature that can be delegated to Codex with a single prompt. Download the project code and begin implementing.

> [**https://github.com/prince-chhirolya/openai-codex-projects-for-agentic-coding**](https://github.com/prince-chhirolya/openai-codex-projects-for-agentic-coding) (Fork & Download)
