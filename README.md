# CodeCraftHub

A tiny, beginner-friendly REST API to track courses you want to learn. Built with Node.js and Express and stores data in a simple JSON file (no database).

- No authentication or user management
- Focused on learning REST API basics
- Data stored in data/courses.json

---

## Tech stack

- Node.js
- Express
- JSON file storage (no DB)

---

## Getting started

Prerequisites:

- Node.js (LTS version recommended)

Steps:

1. Create a project directory and install dependencies
   - mkdir CodeCraftHub
   - cd CodeCraftHub
   - npm init -y
   - npm install express

2. Create the project structure (as described below) and add the files with the contents from the example (or your own variations).

3. Run the server
   - node server.js
   - Open http://localhost:3000/ in your browser or use curl to test the API

Optional for development:

- npm install --save-dev nodemon
- Add a script in package.json, e.g. "start:dev": "nodemon server.js" and run npm run start:dev

---

## Project structure

CodeCraftHub/

- package.json
- server.js (the Express app)
- data/
  - courses.json (start as an empty array [])
- routes/
  - courses.js (REST routes for courses)
- utils/
  - storage.js (helper to read/write the JSON file)
- README.md (this file)

Notes:

- The data file data/courses.json acts as a tiny database. It starts as an empty array [].
- Each course has a unique id (generated as a timestamp string in this simple setup).

---

## Data model

Course object example:
{
"id": "1697040000000",
"name": "Learn Node.js Basics",
"description": "Introduction to Node.js and Express",
"targetDate": "2026-04-30",
"status": "Not Started" // one of: "Not Started", "In Progress", "Completed"
}

Fields:

- id: string (unique identifier)
- name: string (course name)
- description: string
- targetDate: string (YYYY-MM-DD)
- status: string (Not Started | In Progress | Completed)

---

## API endpoints

Base path: /courses

- GET /courses
  - Description: List all courses
  - Response: 200 with an array of course objects

- GET /courses/:id
  - Description: Get a single course by id
  - Response: 200 with the course object, or 404 if not found

- POST /courses
  - Description: Create a new course
  - Required fields in body: name, description, targetDate
  - Optional: status (default: "Not Started")
  - Response: 201 with the created course

- PUT /courses/:id
  - Description: Replace a course entirely
  - Required fields in body: name, description, targetDate, status
  - Response: 200 with the updated course, or 400/404 on invalid input or not found

- PATCH /courses/:id
  - Description: Partially update a course
  - Body: any subset of fields (name, description, targetDate, status)
  - Response: 200 with the updated course, or 404 if not found

- DELETE /courses/:id
  - Description: Delete a course
  - Response: 200 with the deleted course, or 404 if not found

Notes:

- Status values are validated at creation/update time in the basic example (Not Started, In Progress, Completed).
- No authentication is involved.

---

## Data storage

- File path: data/courses.json
- Format: JSON array of course objects
- Initial content: []
- Data access pattern: read the array, mutate it in memory, then write it back to the file after each operation

---

## Basic usage examples (curl)

- List all courses
  - curl http://localhost:3000/courses

- Create a course
  - curl -X POST http://localhost:3000/courses \
    -H "Content-Type: application/json" \
    -d '{ "name": "Learn Node.js Basics", "description": "Intro to Node.js and Express", "targetDate": "2026-04-30" }'

- Get a course by id
  - curl http://localhost:3000/courses/1697040000000

- Update a course (full)
  - curl -X PUT http://localhost:3000/courses/1697040000000 \
    -H "Content-Type: application/json" \
    -d '{ "name": "Updated Name", "description": "Updated desc", "targetDate": "2026-05-01", "status": "In Progress" }'

- Patch a course (partial)
  - curl -X PATCH http://localhost:3000/courses/1697040000000 \
    -H "Content-Type: application/json" \
    -d '{ "status": "Completed" }'

- Delete a course
  - curl -X DELETE http://localhost:3000/courses/1697040000000

---

## How it works (high level)

- The server uses Express to handle routing.
- A small storage utility reads/writes data to data/courses.json on every request (sufficient for a learning project).
- Each operation reads the current array, mutates it, writes it back, and returns the result.

---

## Next steps / potential improvements

- Add input validation (e.g., validate date format, ensure status is one of the allowed values).
- Improve ID generation (use UUIDs).
- Add simple filtering (e.g., by status) or sorting (e.g., by targetDate).
- Introduce a lightweight in-memory cache with periodic persistence to reduce file I/O.
- Add unit tests for route handlers (e.g., with supertest).

---

## License

This project is a lightweight educational example. You can add a license if you plan to share publicly (e.g., MIT).
