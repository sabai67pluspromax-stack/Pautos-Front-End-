# PaUtos Frontend

UI for the PaUtos errand app: Jinja templates + CSS/JS (soft charcoal & orange theme, light/dark mode, mobile-first).
The backend lives in its own repo and renders these templates, so this repo has no Python code.

```
templates/   Jinja pages (base.html, _ui.html, auth/, customer/, rider/, admin/, profile/)
static/      css/style.css, js/app.js
```

## Plugging it into the backend

The backend loads this repo as its template and static folder. Backend side, one time:

1. Add this repo as a submodule inside the backend repo:
   `git submodule add <this-repo-url> app/frontend`
2. In `app/__init__.py`, change line 13 to:
   ```python
   app = Flask(__name__, template_folder="frontend/templates", static_folder="frontend/static")
   ```
3. Delete the old `app/templates/` and `app/static/` from the backend repo.

Cloning the backend afterwards: `git clone --recurse-submodules <backend-url>`.
Updating the UI later: `git submodule update --remote app/frontend`, then commit.

## Contract with the backend

The templates depend on these from the backend. If one changes, tell the frontend dev:
- Endpoint names used in `url_for(...)` (e.g. `customer.new_request`, `rider.accept`, `admin.riders`)
- Template variables passed by each route (e.g. `requests`, `req`, `form`, `stats`, `earnings`)
- Filters `status_label` and `ph_time`; form field names and ids
- Static JS hooks: element ids `feed`, `presence`, `presence-status`, `map`, `map-picker`, `job-map`, `eta`, `job-eta` and the pin field names
- Template file names (e.g. `admin/category_form.html`)

## Heads-up for the backend dev
- The JS files (`rider_feed.js`, `rider_presence.js`, `location_picker.js`, `route_line.js`) live in `static/js/` here, because the templates load them from `js/...`. In the backend zip they were inside `static/css/js/`, which would 404. Delete that copy.
- `admin/cateory_form.html` (typo) is not needed. This repo has `admin/category_form.html`.

## Notes
- Theme choice is saved in the browser (`localStorage`, key `pautos-theme`); dark is the default.
- Fonts (Google Fonts) and the live map (Leaflet/OpenStreetMap) load from the internet.
