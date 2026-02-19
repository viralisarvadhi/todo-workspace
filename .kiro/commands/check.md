---
name: check
description: Tests all todo API endpoints and reports if they work
---

When this command is triggered, do the following steps one by one:

1. CHECK if backend server is running at http://localhost:3000
   - If not running, say "❌ Backend server is not running. Start it with: node 
server.js"
   - If running, continue below

2. TEST each endpoint and report result:

   GET /todos
   - Send GET request to http://localhost:3000/todos
   - Expected: returns an array (even if empty)
   - Pass ✅ or Fail ❌ with reason

   POST /todos
   - Send POST to http://localhost:3000/todos
   - Body: { "title": "Test Todo", "detail": "This is a test" }
   - Expected: returns new todo with id, title, detail, status: "pending", createdAt
   - Pass ✅ or Fail ❌ with reason

   PUT /todos/:id
   - Use the id from the POST result above
   - Send PUT to http://localhost:3000/todos/{id}
   - Body: { "status": "done" }
   - Expected: returns updated todo with status: "done"
   - Pass ✅ or Fail ❌ with reason

   DELETE /todos/:id
   - Use same id
   - Send DELETE to http://localhost:3000/todos/{id}
   - Expected: todo removed, returns success message
   - Pass ✅ or Fail ❌ with reason

3. PRINT a final summary like this:
   ─────────────────────────────
   API Health Check Results:
   ─────────────────────────────
   GET    /todos        ✅ Pass
   POST   /todos        ✅ Pass
   PUT    /todos/:id    ✅ Pass
   DELETE /todos/:id    ✅ Pass
   ─────────────────────────────
   All endpoints working! 🎉
   ─────────────────────────────

4. If any endpoint fails, suggest exactly what to fix in server.js
