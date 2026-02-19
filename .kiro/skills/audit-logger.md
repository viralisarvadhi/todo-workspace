---
name: audit-logger
description: Adds file-based audit logging to backend routes. Use when adding or 
modifying any CRUD endpoints that need to track add, update, delete events.
triggers:
  - type: manual
  - type: file_saved
    pattern: "**/backend*/**/server.js"
---

# Audit Logger Skill

## What This Skill Does
Automatically adds audit logging to backend routes.
Every ADD, UPDATE, DELETE action gets written to audit.log file.

## When Triggered
- Manually: when developer asks to add auditing
- Automatically: when server.js is saved/modified

## Steps to Follow

### Step 1: Create logger.js
Create this file in the backend root folder:
```javascript
const fs = require('fs');
const path = require('path');

const logFile = path.join(__dirname, 'audit.log');

function writeLog(action, todo) {
  const timestamp = new Date().toISOString()
    .replace('T', ' ')
    .substring(0, 19);

  let message = '';

  if (action === 'ADD') {
    message = `[${timestamp}] ADD    | id:${todo.id} | title:"${todo.title}" | 
status:${todo.status}`;
  } else if (action === 'UPDATE') {
    message = `[${timestamp}] UPDATE | id:${todo.id} | title:"${todo.title}" | 
${todo.oldStatus} → ${todo.newStatus}`;
  } else if (action === 'DELETE') {
    message = `[${timestamp}] DELETE | id:${todo.id} | title:"${todo.title}"`;
  }

  fs.appendFileSync(logFile, message + '\n');
  console.log(message);
}

module.exports = { writeLog };
```

### Step 2: Update server.js
- Add at top: const { writeLog } = require('./logger');
- After POST /todos success: writeLog('ADD', newTodo)
- After PUT /todos/:id success: writeLog('UPDATE', { ...todo, oldStatus, newStatus: 
todo.status })
- After DELETE /todos/:id success: writeLog('DELETE', deletedTodo)

### Step 3: Add /audit Endpoint
Add this route to server.js:
- GET /audit → reads audit.log and returns { logs: [...] }
- If file does not exist return { logs: [] }

### Step 4: Update .gitignore
Add audit.log to backend .gitignore so it never gets pushed to GitHub

### Step 5: Report
Tell the developer:
- Which files were created or modified
- What events are now being tracked
- How to read logs: GET http://localhost:3000/audit
- Where the log file is saved
```
