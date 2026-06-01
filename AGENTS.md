# Agent brief — sample todo app

## Stack

Rails 8.1 sample todo app on SQLite (`storage/*.sqlite3`). Hotwire: Turbo Rails and Stimulus via importmap; Propshaft for assets. JSON via Jbuilder where scaffolded. Tests: Minitest (`test/`) with Capybara system tests. Background work: Solid Queue (see `config/queue.yml`). No Devise or other auth gem in this repo.

## Commands

- Setup: `bin/setup` (bundle, `db:prepare`, optional `bin/dev`)
- Dev server: `bin/dev`
- Tests: `bin/rails test`
- Lint: `bin/rubocop`
- Security scan: `bin/brakeman`

## Conventions

- RESTful `TodosController` with `respond_to` for HTML and JSON; use Turbo Streams for in-place DOM updates on the index.
- Strong parameters via `params.expect(todo: [...])` in `todo_params`.
- Partials under `app/views/todos/`; list rows use `dom_id(todo)` for Turbo Stream targets.
- No separate authorization layer — all todos are visible to everyone using the app.

## Don'ts

- Do not add gems without explicit approval.
- Do not use inline JavaScript in ERB; use Stimulus or Turbo.
- Do not skip `verify_authenticity_token` or disable CSRF.
- Do not use `eval` on request parameters or `html_safe` / `raw` on user-supplied content.
- Do not seed real PII; use `db/seeds.rb` only for sample data.
- Keep changes scoped to this todo app — no imports from other projects.
