# Gauss Jordan Flask

Short web app that demonstrates and solves 3×3 linear systems using the Gauss Jordan method.

**Contents**
- Purpose
- Architecture overview
- Quick start (local)
- Optional numeric validation (NumPy)
- Testing
- Deployment (Vercel)
- Next steps

---

## Purpose

This repository provides an educational web application that:

- Lets users enter a 3×3 linear system via a form.
- Runs a pure-Python Gauss Jordan solver server-side and returns the solution plus a step-by-step reduction log rendered with MathJax.
- Optionally validates results using NumPy (guarded and optional).

## Architecture Overview

- **Frontend (Templates)**: [templates/index.html](templates/index.html) and [templates/base.html](templates/base.html) render UI, examples, and the MathJax equations.
- **Static**: [static/css/styles.css](static/css/styles.css) contains site styling.
- **Backend**: [api/index.py](api/index.py) is the Flask app handling GET/POST, input parsing, `gauss_jordan_solve(A,b)`, optional NumPy validation, and rendering.
- **Optional numeric stack**: `requirements-optional.txt` lists `numpy` and `scipy` for validation and cross-checking.
- **Deployment config**: [requirements.txt](requirements.txt) and [vercel.json](vercel.json) at repository root for Vercel deployment.

### Component Diagram

```mermaid
flowchart LR
  Browser -->|GET /, POST /| Frontend[Templates + Static]
  Frontend -->|form data| Flask[Flask app - api/index.py]
  Flask -->|compute| Solver[gauss_jordan_solve (pure Python)]
  Flask -->|optional| NumPy[NumPy validation (optional)]
  Flask -->|render| Frontend
  subgraph Deploy
    Vercel[Hosting: Vercel / @vercel/python]
  end
  Flask --> Vercel
```

## Quick start (local)

Run these Windows commands from the repo root.

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
# Optional validation libs
pip install -r requirements-optional.txt
python api\index.py
# open http://127.0.0.1:5000
```

Notes:
- The app is intentionally small and stateless: each request computes and returns results without persisting server-side.
- To run without optional validation, skip installing `requirements-optional.txt`.

## Optional numeric validation

If `numpy` is installed, the server will attempt to compute a reference solution with `numpy.linalg.solve` and present a small diff. This is useful for correctness checks but not required for the core learning experience.

## Testing

Add unit tests for `gauss_jordan_solve` (recommended). Example with `pytest`:

```python
def test_simple_system():
    A = [[2,1],[1,-1]]  # for a 2x2 example you might adapt solver
    b = [7,2]
    sol, _ = gauss_jordan_solve(A,b)
    assert pytest.approx(sol) == [3,1]
```

Run tests:

```powershell
pip install pytest
pytest
```

## Deployment (Vercel)

This project includes a `vercel.json` configured to run `api/index.py` with `@vercel/python`. Before deploying:

- Ensure `requirements.txt` lists any runtime libraries (Flask). Avoid bundling heavy optional libs unless necessary.
- Set environment variable `FLASK_ENV=production` (or run app with debug disabled).

Deploy steps:

1. Install Vercel CLI and login: `npm i -g vercel` then `vercel login`.
2. From repo root run `vercel` and follow prompts.

## Production notes & security

- Turn off debug mode in production. Do not leave `debug=True` in `api/index.py`.
- Validate inputs carefully. Even numeric form inputs should be parsed defensively.
- Consider rate limiting or adding CAPTCHA if public-facing.

## Next steps (recommended)

- Add `README` (this file). (done)
- Add unit tests for solver correctness and CI (GitHub Actions).
- Add a short CONTRIBUTING.md describing how to run tests and propose changes.
- Add small integration test that posts sample inputs to the Flask test client.

---

If you want, I can:

1. Add a `tests/` folder with `pytest` tests for `gauss_jordan_solve()` and wire up a GitHub Actions CI workflow.
2. Create a minimal `CONTRIBUTING.md` and `CHANGELOG.md`.
3. Prepare the Vercel deployment steps as a checklist and perform a dry-run locally.

Tell me which of the above you'd like next.
