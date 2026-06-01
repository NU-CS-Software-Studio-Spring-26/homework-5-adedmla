## Story

As a user managing my todo list, I want to mark items as high priority from the index without a full page reload, so that I can quickly scan and update priority.

- `Todo` has a `high_priority` boolean (default false).
- Each row on `/todos` shows a toggle (★ / ☆) for current priority.
- Clicking sends a PATCH that flips priority and returns a Turbo Stream updating only that row.
- Network: Accept and Content-Type include `text/vnd.turbo-stream.html`; no full document reload.
- Automated test asserts Turbo Stream response.

## Plan

1. Migration: add `high_priority` to `todos` (default false, null false).
2. Route `patch :toggle_priority` on todos member; `TodosController#toggle_priority` with `format.turbo_stream` and HTML fallback redirect.
3. `app/views/todos/toggle_priority.turbo_stream.erb` uses `turbo_stream.replace` on `dom_id(@todo)`; `button_to` on index partial with `form: { data: { turbo_stream: true } }`.
4. Controller tests patch with `Accept: text/vnd.turbo-stream.html` and assert `response.media_type`.

## Tests

```bash
bin/rails test test/controllers/todos_controller_test.rb
```

```
Running 9 tests in a single process
.........

Finished in 0.191941s, 46.8894 runs/s, 72.9391 assertions/s.
9 runs, 14 assertions, 0 failures, 0 errors, 0 skips
```

## Things I rejected from the AI

- Using a full-page HTML redirect as the only response for the priority toggle (assignment requires Turbo Stream in-place update).
- Adding custom JavaScript fetch handlers instead of Rails `button_to` + `data-turbo-stream` (kept Hotwire-native pattern).
