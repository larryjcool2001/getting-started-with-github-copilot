# AI Copilot Instructions for Mergington High School Activities API

## Project Overview

This is a **GitHub Copilot learning exercise** featuring a minimal FastAPI web application for Mergington High School's extracurricular activities management. The project demonstrates full-stack development patterns: Python backend (FastAPI), vanilla JavaScript frontend, and in-memory data storage.

## Architecture & Key Components

### Backend: FastAPI Application (`src/app.py`)

- **Framework**: FastAPI with Uvicorn server
- **Data Storage**: In-memory dictionary (reset on server restart)
- **Core Resource**: `activities` dict with activity name as key, details object as value
- **API Pattern**: RESTful endpoints for fetching activities and signing up

**Structure**:
```
activities = {
  "Activity Name": {
    "description": str,
    "schedule": str,
    "max_participants": int,
    "participants": [emails]  # List of signed-up student emails
  }
}
```

### Frontend: Vanilla JavaScript + HTML/CSS (`src/static/`)

- **app.js**: Async fetch calls to API, DOM manipulation, form handling
- **index.html**: Structure with sections for activities list and signup form
- **styles.css**: Styling (referenced but specific styles discoverable in file)

**Key Pattern**: Load activities from API on DOMContentLoaded, populate dropdown + activity cards, handle POST signup with real-time feedback messages.

### Data Flow

1. Browser loads `/` → redirects to `/static/index.html`
2. Frontend fetches `/activities` on page load
3. JS populates activity list (cards) and dropdown
4. User submits email + activity → POST `/activities/{activity_name}/signup?email=X`
5. API appends email to participants list, returns success/error message

## Development & Testing

### Running the Application

```bash
# Install dependencies
pip install -r requirements.txt

# Start server (runs on http://localhost:8000)
python src/app.py
```

- API docs available at `http://localhost:8000/docs` (Swagger UI)
- Alternative docs at `http://localhost:8000/redoc`

### Testing Setup

- **Test Framework**: pytest (configured in `pytest.ini` with pythonpath = .)
- Tests should import from `src.app` module
- Run with: `pytest`

## Code Patterns & Conventions

### FastAPI Patterns

1. **Endpoint Parameter Handling**: Use URL path parameters for resource identifiers (activity name) and query strings for optional data (email in signup)
2. **Error Handling**: Raise `HTTPException` for validation failures (404 for missing activity)
3. **Static File Mounting**: Use `StaticFiles` middleware for serving frontend assets

### JavaScript Patterns

1. **Error Handling**: Wrap fetch calls in try-catch, display user-friendly messages
2. **DOM References**: Query elements once on load, store in variables
3. **Message Display**: Use CSS classes (`.hidden`, `.success`, `.error`) for state management
4. **Auto-hide Messages**: Dismiss feedback messages after 5 seconds with setTimeout

### Data & Validation

- **Activity Identifiers**: Use activity name (string key) — case-sensitive
- **Participant Tracking**: Simple list of emails (no deduplication or validation in current version)
- **Availability Calculation**: `max_participants - len(participants)` to display spots left

## Key Files Reference

| File | Purpose |
|------|---------|
| `src/app.py` | Core FastAPI application with endpoints |
| `src/static/app.js` | Frontend logic, API integration, DOM manipulation |
| `src/static/index.html` | HTML structure with form and activities container |
| `requirements.txt` | Python dependencies (fastapi, uvicorn) |
| `pytest.ini` | pytest configuration |

## Common Tasks

- **Add new activity**: Add entry to `activities` dict in `app.py`
- **Modify API response**: Edit endpoint function and return statement
- **Update frontend display**: Modify innerHTML template in `app.js` (see activity card creation)
- **Add form validation**: Add checks in signup form submit handler before fetch call

---

**Note**: This is an educational project. All data is in-memory and resets on server restart. No persistence layer or advanced validation is implemented.
