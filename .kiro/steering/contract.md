# Shared Contract — Todo App

## Todo Shape
{ id, title, detail, status: "pending"|"done", createdAt }

## API
- Base URL: http://localhost:3000
- GET    /todos
- POST   /todos      → { title, detail }
- PUT    /todos/:id  → { status }
- DELETE /todos/:id

## Ports
- Backend:  http://localhost:3000
- Frontend: http://localhost:5173
