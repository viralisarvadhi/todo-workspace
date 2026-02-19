---
name: todo-sync
description: When backend todo routes change, automatically sync frontend API calls 
to match
triggers:
  - type: file_saved
    pattern: "**/backend*/**/*.js"
  - type: manual
---

# Todo Sync Skill

When this skill is triggered:

1. Check if any backend route changed its:
   - Endpoint URL
   - Request body shape
   - Response shape
   - Todo object fields

2. If anything changed, update these frontend files:
   - /Users/sarvadhisolution/Downloads/frontend(t)/src/api/todos.js
   - Any component using that API call

3. Make sure todo shape always matches:
   { id, title, detail, status: "pending"|"done", createdAt }

4. Report what was changed and why.
