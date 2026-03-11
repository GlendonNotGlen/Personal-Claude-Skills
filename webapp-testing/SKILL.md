---
name: webapp-testing
description: Test and interact with local web applications using Playwright. Take screenshots, verify UI behavior, capture console logs, automate form submissions, and discover page elements.
license: Complete terms in LICENSE.txt
---

# Web Application Testing

Write native Python Playwright scripts to test and interact with local web applications.

**Helper Scripts Available**:
- `scripts/with_server.py` - Manages server lifecycle (start, health-check, teardown). Supports multiple servers.

**Always run scripts with `--help` first** to see usage. DO NOT read the source until you try running the script first and find that a customized solution is absolutely necessary. These scripts can be very large and thus pollute your context window. They exist to be called directly as black-box scripts rather than ingested into your context window.

## Decision Tree: Choosing Your Approach

```
User task --> Is it a static HTML file (no server needed)?
    |
    +-- Yes --> Use file:// URL directly
    |           1. Read the HTML file to identify element selectors
    |           2. Write Playwright script using file:// URL (no networkidle wait needed)
    |           3. See examples/static_html_automation.py
    |
    +-- No (dynamic webapp) --> Is the server already running?
        |
        +-- No --> Start it with the helper:
        |          1. Run: python scripts/with_server.py --help
        |          2. Use the helper to start server + run your Playwright script
        |
        +-- Yes --> Reconnaissance-then-action:
            1. Navigate and wait for networkidle
            2. Take screenshot or inspect DOM to discover selectors
            3. Execute actions with discovered selectors
```

## Starting Servers with with_server.py

Run `--help` first, then invoke the helper. Your Playwright script only needs browser logic -- the helper manages server start/stop.

**Single server:**
```bash
python scripts/with_server.py --server "npm run dev" --port 5173 -- python your_automation.py
```

**Multiple servers (e.g., backend + frontend):**
```bash
python scripts/with_server.py \
  --server "cd backend && python server.py" --port 3000 \
  --server "cd frontend && npm run dev" --port 5173 \
  -- python your_automation.py
```

## Writing Playwright Scripts

Every script follows this skeleton. Key rules:
- Always use `chromium` in `headless=True` mode.
- Always call `page.wait_for_load_state('networkidle')` before inspecting or acting on dynamic pages (not needed for static `file://` URLs).
- Always close the browser in a `finally` block or context manager.
- Set viewport when you need consistent screenshot dimensions.

```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch(headless=True)
    page = browser.new_page(viewport={'width': 1920, 'height': 1080})
    page.goto('http://localhost:5173')
    page.wait_for_load_state('networkidle')

    # --- Your logic here ---

    # Save screenshots to /tmp/ for inspection
    page.screenshot(path='/tmp/result.png', full_page=True)

    browser.close()
```

## Reconnaissance-Then-Action Pattern

Use this when you do not know the page structure in advance.

**Step 1 -- Discover elements:**
```python
page.wait_for_load_state('networkidle')
page.screenshot(path='/tmp/inspect.png', full_page=True)

# Enumerate interactive elements
buttons = page.locator('button').all()
for i, btn in enumerate(buttons):
    print(f"Button [{i}]: {btn.inner_text() if btn.is_visible() else '[hidden]'}")

links = page.locator('a[href]').all()
inputs = page.locator('input, textarea, select').all()
```

**Step 2 -- Act on discovered selectors:**
```python
page.click('text=Submit')
page.fill('#username', 'testuser')
page.select_option('select#role', 'admin')
```

See `examples/element_discovery.py` for a full working example.

## Capturing Console Logs

Attach a listener **before** navigation to capture all output:

```python
console_logs = []

def handle_console(msg):
    console_logs.append(f"[{msg.type}] {msg.text}")

page.on("console", handle_console)
page.goto(url)
page.wait_for_load_state('networkidle')
```

See `examples/console_logging.py` for the full pattern.

## Common Pitfalls

- **Inspecting DOM too early:** On dynamic apps, always call `page.wait_for_load_state('networkidle')` before reading elements or taking screenshots. Without this, JS-rendered content will be missing.
- **Selectors not found:** Use `page.wait_for_selector('.my-element', timeout=5000)` instead of assuming elements exist immediately. Print the page content with `page.content()` to debug missing elements.
- **Flaky clicks:** If a click does nothing, the target may be covered by an overlay or not yet interactive. Use `page.locator('.btn').wait_for(state='visible')` before clicking.

## Selector Priority

Prefer selectors in this order (most robust to least):
1. **Test IDs:** `page.get_by_test_id('submit-btn')` (if data-testid attributes exist)
2. **Role:** `page.get_by_role('button', name='Submit')`
3. **Text:** `page.locator('text=Submit')`
4. **CSS with IDs:** `page.locator('#submit-button')`
5. **CSS classes:** `page.locator('.btn-primary')` (fragile -- classes change often)

## Best Practices

- **Use bundled scripts as black boxes.** Check `scripts/` for helpers before writing custom solutions. Run `--help` first, invoke directly.
- **Save screenshots to `/tmp/`** so you can read them with the Read tool for visual inspection.
- **Print actionable output** from scripts (element counts, text content, error messages) so results are visible in the terminal.
- **Use `sync_playwright()`** -- async is unnecessary for single-script automation.
- **Add targeted waits** (`wait_for_selector`, `wait_for_load_state`) rather than arbitrary `wait_for_timeout` calls. Use timeouts only as a last resort.

## Reference Files

- **examples/** - Working examples showing common patterns:
  - `element_discovery.py` - Discovering buttons, links, and inputs on a page
  - `static_html_automation.py` - Using file:// URLs for local HTML files
  - `console_logging.py` - Capturing browser console logs during automation