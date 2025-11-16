# Pawsitive Impact Constitution

## 1. Core Principles

### 1. Library-First Modularity
Favor proven libraries over custom code to maximize reliability and speed.
- **Why:** Contest time is limited; trusted libraries reduce bugs and speed up development.
- **Examples:**
  - Use SQLAlchemy ORM for all DB access, not raw SQL.
  - Use Flask-Login for authentication, not custom session logic.
  - Use React Router for navigation, not manual URL parsing.
- **Violation:** Writing custom DB queries or authentication logic.

### 2. Stable Public Interfaces
Define contracts (API, DB schema, component props) that do not change after initial release.
- **Why:** Prevents breakage and confusion during rapid development.
- **Examples:**
  - Lock API endpoint paths and request/response formats.
  - Keep DB schema fields and types fixed.
  - Maintain React prop types for key components.
- **Violation:** Renaming API endpoints or DB fields mid-sprint.

### 3. Test-Before-Commit
No code is committed without passing tests.
- **Why:** Ensures reliability and contest scoring for validation.
- **Examples:**
  - Run pytest for every backend change.
  - Manually test frontend features before pushing.
  - End-to-end test before presentation.
- **Violation:** Committing code with failing or missing tests.

### 4. Document Everything
Log all AI prompts and document code, especially public interfaces.
- **Why:** 20% of contest score; helps future contributors and judges.
- **Examples:**
  - Save every prompt in `docs/prompts/` with timestamp and tool.
  - Write docstrings for Python functions and comments for JS.
  - Document API endpoints and DB schema in README.
- **Violation:** Missing prompt logs or undocumented code changes.

### 5. Security By Default
Protect sensitive pet/family data at all times.
- **Why:** Ethical responsibility and contest requirement.
- **Examples:**
  - Hash passwords with werkzeug.security.
  - Use @login_required for all API endpoints.
  - Validate and sanitize all user input.
- **Violation:** Storing plain text passwords or exposing sensitive data.

## 2. Stable Public Interfaces (The Contracts)

### Backend API Endpoints
- `/api/auth/*` (login, logout, me)
- `/api/cases/*` (search, CRUD, notes, recover)
- `/api/upload/*` (parse, commit)
- `/api/reports/*` (dashboard, outcomes, species, export)
- `/api/admin/*` (user management)

### Database Schema
- **users:** id, email (unique), password_hash, full_name, role, is_active, created_at, last_login
- **cases:** id, case_id (unique), contact_name, phone_number, email, pet_name, pet_species, pet_breed, source_system, initial_request, status, outcome, notes (JSON), is_deleted, created_at, updated_at

### React Component Props
- `CaseSearchProps`: `{ onCaseSelect: (caseId: string) => void }`
- `CaseDetailProps`: `{ caseId: string; onClose: () => void; onUpdate: () => void }`

## 3. Development Workflow

- **Git Commit Convention:** `type(scope): short description` + bullets + why + files
- **Branch Strategy:** Main branch only, frequent commits, push every 2 hours
- **Testing:** Pytest for backend, manual for frontend, integration before presentation
- **AI Prompt Documentation:** Log every prompt in `docs/prompts/` with details
- **Code Review:** Self-review before commit

## 4. Constraints & Security

- **Cannot:** Use paid APIs, store plain passwords, use external uploads, commit secrets, use raw SQL, trust unsanitized input, expose admin endpoints to staff, permanently delete data
- **Security:** Hash passwords, require authentication, check admin role, sanitize input, configure CORS, session timeout
- **Data Protection:** Only authenticated access, audit trail, soft delete

## 5. Quality Standards

- **Code:** PEP 8 (Python), standard React (JS), max 50 lines per function, docstrings/comments, no magic numbers
- **Performance:** Search <500ms/10k cases, dashboard <2s, no N+1 queries, proper indexes
- **Usability:** Form validation, loading states, feedback, mobile responsive
- **Accessibility:** WCAG 2.1 AA contrast, keyboard accessible, labeled forms

## 6. Templates

### Git Commit Example
```
feat(backend): add case search by phone number
- Implemented exact match search on phone_number field
- Added database index for performance
- Returns all cases for matching phone
- Includes test coverage for search endpoint
Addresses user story US-001
Files modified: backend/routes/cases.py, backend/tests/test_cases.py
```

### API Endpoint Doc Example
```
## POST /api/cases
Purpose: Create a new pet case
Auth: Staff/Admin
Request: { contact_name, phone_number, ... }
Success: { case_id, contact_name, ... }
Errors: 400, 401, 500
Example: api.post('/api/cases', caseData)
```

### Prompt Log Example
```
2025-11-15 10:30 AM - Create Case Model
Tool: GitHub Copilot
Context: SQLAlchemy model for case
Prompt: [full text]
Quality: 5/5
Iterations: 1
Key Learning: Specifying soft delete upfront prevented refactor
Files: backend/models/case.py
```

## 7. Collaboration Guidelines

- **Communication:** In-person (Alejandro, Juan A), remote (Juan M), sync at 12/6/9 PM, GitHub issues, commit messages
- **Task Ownership:** Backend (Alejandro), Frontend (Juan A), Docs/QA (Juan M), Integration (Alejandro + Juan A)
- **Conflict Resolution:** Follow principles, choose simplest solution, document decision

## 8. Success Criteria

- **MVP:** File upload, case search, CRUD, soft delete/recovery, dashboard, report, staff login
- **Stretch:** More reports, export, admin management, charts, mobile responsive
- **Demo:** Show login, upload, search, edit, dashboard, report
- **Docs:** README, API docs, prompt logs, reflection

---
This constitution is the guiding document for all work on Pawsitive Impact during the contest. Follow it strictly for every decision and commit.
