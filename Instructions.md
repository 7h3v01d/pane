# Instructions

These instructions will guide you through the process of setting up, testing, and building your frameless HTML window application, which can display local HTML files or web URLs.

## Prerequisites
- Python: Ensure you have Python 3.6 or newer installed.
- pip: Python's package installer, used to install the required libraries.
- Internet access: Required for web URLs.

### Step 1: Set Up the Project Directory
Create a single folder for your project. If using local HTML files, place your Python script (main.py), your HTML file (e.g., index.html), and any additional assets (e.g., images, CSS, JavaScript) in this directory. For web URLs, no local files are needed.

---

### Step 2: Install Required Libraries
Open your terminal or command prompt, navigate to your project directory, and install the necessary libraries using pip:

```bash
pip install PyQt6 PyQt6-WebEngine pyinstaller
```

---

### Step 3: Test the Application in Python Mode
This step is for testing your code before creating an executable (for local files) or directly accessing a web URL.

Run the script directly from your terminal, specifying either a local HTML file or a web URL:

- For a local file:
```bash
python main.py --html index.html
```
- For a web URL:
```bash
python main.py --html https://example.com
```
- To disable hardware acceleration (recommended for index.html to prevent flickering):
```bash
python main.py --html index.html --gpu disable
```
- To enable hardware acceleration (default):
```bash
python main.py --html index.html --gpu enable
```

This will launch your frameless window, displaying the specified content. If a local HTML file is missing, an error message will appear.

**Troubleshooting Flickering or Blank Screen**:
If the window content flickers or appears blank when resizing, maximizing, restoring, or hovering over HTML elements:
- Use `--gpu disable` to disable hardware acceleration, which has resolved flickering for index.html: `python main.py --html index.html --gpu disable`.
- Update your graphics drivers, as QWebEngineView relies on hardware acceleration.
- Right-click in the window and select "Inspect" to open Chromium DevTools. Check the console for JavaScript errors or excessive repaints.
- For web URLs, console warnings (e.g., "Unrecognized feature: 'web-share'") are normal and can be ignored unless they cause functionality issues.
- For local HTML, simplify hover effects, transitions, or animations in your HTML/CSS/JavaScript (e.g., reduce CSS `transition` durations or avoid complex `onmouseover` handlers).
- Resize the window slowly or toggle fullscreen (Ctrl+F) to force a repaint.
- Test the content in a browser to ensure it loads correctly without errors.

---

### Step 4: Validate Local HTML Content (if applicable)
If using a local HTML file, ensure it and its assets (e.g., images, CSS, JavaScript) are self-contained:
- Open the HTML file in a browser to verify it loads correctly.
- Ensure all resources are local (no external URLs) to avoid loading issues.
- Check the console for errors (e.g., missing files or JavaScript issues).
- Test hover effects and animations in the browser to ensure they don't cause excessive repaints.

---

### Step 5: Build the Executable (for local HTML files)
If using a local HTML file, use PyInstaller to convert your script into a standalone executable. This step is not necessary for web URLs unless you want to distribute the application with a hardcoded URL.

For **Windows**:
```bash
pyinstaller --onefile --windowed --add-data "index.html;." main.py
```

For **Unix-like systems** (Linux/MacOS):
```bash
pyinstaller --onefile --windowed --add-data "index.html:." main.py
```

To include additional assets (e.g., images, CSS), add them to the --add-data option, separated by commas. For example:
- Windows: `--add-data "assets/image.png;assets"`
- Unix-like: `--add-data "assets/image.png:assets"`

Alternatively, use the provided build.py script to automate the command:
```bash
python build.py
```

To include `--gpu disable` in the executable, modify main.py to hardcode `app_args.append("--disable-gpu")` before building.

After the command completes, find the executable in the dist folder within your project directory.

**Note**: For web URLs, building an executable is optional, as the application can run in Python mode to access online content. Ensure the target system has internet access. To create an executable for a specific URL, modify main.py to hardcode the URL (e.g., setUrl(QUrl("https://example.com"))).
