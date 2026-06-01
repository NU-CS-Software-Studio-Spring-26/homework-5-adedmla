# Homework 5 — submission

**Repository / branch:** https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-adedmla/tree/hw5

---

## Part 1 — Setup

| Artifact | Link |
|----------|------|
| `.cursorignore` | https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-adedmla/blob/hw5/.cursorignore |

_Part 1 IDE steps (models, indexing, @-context, Tab, “Read my .env” blocked) completed in Cursor._

---

## Part 2 — Teach Cursor your codebase

| Artifact | Link |
|----------|------|
| `AGENTS.md` | https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-adedmla/blob/hw5/AGENTS.md |
| `rails-conventions.mdc` | https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-adedmla/blob/hw5/.cursor/rules/rails-conventions.mdc |
| `security.mdc` | https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-adedmla/blob/hw5/.cursor/rules/security.mdc |

---

## Part 3 — Ask / Plan / Agent

### Ask mode

**Prompt:**

```
Where in this codebase is the todos index list populated and rendered? Cite the exact files and line numbers. Do not propose changes.
```

**Citations returned:**

```
app/controllers/todos_controller.rb — index action sets @todos = Todo.all (lines 5–7)
app/views/todos/index.html.erb — iterates @todos and renders each todo partial (lines 7–10)
app/views/todos/_todo.html.erb — row partial for a single todo (lines 1–13)
config/routes.rb — resources :todos maps GET /todos to index (lines 2–7)
```

**Verification (real or hallucinated?):**

```
Opened all three files in the editor. Line numbers match the repo on hw5. Routes file correctly points resources :todos to TodosController#index. No hallucinated paths.
```

### Plan mode

**Prompt:**

```
I want to change the todos index so that high-priority todos appear first, then the rest sorted by created_at descending. Propose a plan as a numbered list of changes, including files to edit, new tests to add, and any migration. Do not write code.
```

**Plan from AI (+ your edits):**

```
1. In TodosController#index, replace Todo.all with an ordered scope (e.g. order(high_priority: :desc, created_at: :desc)).
2. Add a model scope on Todo for default ordering (optional, keeps controller thin).
3. Add a controller test: create two todos with different high_priority values, GET index, assert response order in assigns or response body.
4. No migration needed — high_priority column already exists.

Edit: Removed any mention of per-user ownership — this app has no authentication.
```

### Agent mode (smallest slice)

**Prompt:**

```
Add only the migration for a high_priority boolean on todos with default false and null false. Do not add routes, views, or controller actions yet.
```

**Commit:** https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-adedmla/commit/80ccc0d

### Bad → good prompt

**Bad:**

```
fix the bug in todos
```

**Good:**

```
Context: app/controllers/todos_controller.rb (toggle_priority), test/controllers/todos_controller_test.rb, app/views/todos/toggle_priority.turbo_stream.erb

Task: Ensure PATCH toggle_priority returns a Turbo Stream when the client sends Accept: text/vnd.turbo-stream.html.

Expected: response.media_type is text/vnd.turbo-stream.html and the index row updates without a full page reload.
Actual: (if broken) response is text/html or the whole page reloads.

Constraints: No new gems. Use format.turbo_stream and a *.turbo_stream.erb template. Follow existing dom_id(todo) partial pattern in app/views/todos/_todo.html.erb.

Done when: bin/rails test test/controllers/todos_controller_test.rb passes the test "toggle_priority responds with turbo stream" (assert_equal on response.media_type).
```

---

## Part 4 — Turbo Streams & feature PR

### Turbo Streams (your explanation)

A Turbo Stream response is a small HTML document containing instructions (e.g. replace, update, append) for Turbo to apply to the current page DOM. Unlike a normal HTML response that replaces the entire document, the MIME type is `text/vnd.turbo-stream.html`, so Turbo fetches via XHR/fetch and patches only the targeted element. In this app, clicking the priority button PATCHes `toggle_priority`, the controller renders `toggle_priority.turbo_stream.erb`, and Turbo replaces the single todo row identified by `dom_id(@todo)` without reloading `/todos`.

**Verified against handbook/source:**

Claim: Turbo Stream responses use MIME type `text/vnd.turbo-stream.html`. Checked the Turbo Streams reference (turbo.hotwired.dev) and Rails guides — confirmed. Our integration test sends `Accept: text/vnd.turbo-stream.html` and asserts `response.media_type` matches (test/controllers/todos_controller_test.rb lines 41–44).

### Pull request

**PR URL:** _(paste after you create the PR, e.g. https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-adedmla/pull/1)_

---

## Submission checklist

- [x] Repo / `hw5` branch link
- [x] `.cursorignore` link
- [x] `AGENTS.md` + both `.mdc` rule links
- [x] Part 3 complete
- [ ] Part 4 PR URL filled in after PR is created
- [x] Part 4 Turbo explanation + verification
