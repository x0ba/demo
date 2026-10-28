# Demo API

A tiny in-memory Express API. Three books are seeded on startup; data resets when the process restarts.

`npm run dev` restarts the server when you save a file.

## Endpoints

| Method | Path | Notes |
| --- | --- | --- |
| `GET` | `/health` | `{ "ok": true }` |
| `GET` | `/books` | Optional `?q=` and `?genre=` |
| `GET` | `/books/:id` | |
| `POST` | `/books` | `{ "title", "author", "genre?", "year?" }` |
| `PATCH` | `/books/:id` | Any of those fields |
| `DELETE` | `/books/:id` | `204` |

```bash
curl http://localhost:3000/books
curl http://localhost:3000/books?q=butler
curl -X POST http://localhost:3000/books \
  -H 'content-type: application/json' \
  -d '{"title":"Piranesi","author":"Susanna Clarke","genre":"fantasy","year":2020}'
```

## Intended Inputs + Outputs

GET /health
200
{"ok":true}

GET /books
200
{"books":[{"id":"1","title":"The Left Hand of Darkness","author":"Ursula K. Le Guin","genre":"science fiction","year":1969},{"id":"2","title":"Kindred","author":"Octavia E. Butler","genre":"science fiction","year":1979},{"id":"3","title":"The Name of the Rose","author":"Umberto Eco","genre":"mystery","year":1980}]}

GET /books?q=butler
200
{"books":[{"id":"2","title":"Kindred","author":"Octavia E. Butler","genre":"science fiction","year":1979}]}

GET /books?genre=mystery
200
{"books":[{"id":"3","title":"The Name of the Rose","author":"Umberto Eco","genre":"mystery","year":1980}]}

GET /books?q=rose&genre=mystery
200
{"books":[{"id":"3","title":"The Name of the Rose","author":"Umberto Eco","genre":"mystery","year":1980}]}

GET /books?q=nope
200
{"books":[]}

GET /books/1
200
{"book":{"id":"1","title":"The Left Hand of Darkness","author":"Ursula K. Le Guin","genre":"science fiction","year":1969}}

GET /books/99
404
{"error":"Book not found"}

POST /books
Content-Type: application/json
{"title":"Piranesi","author":"Susanna Clarke","genre":"fantasy","year":2020}
201
{"book":{"id":"4","title":"Piranesi","author":"Susanna Clarke","genre":"fantasy","year":2020}}

POST /books
Content-Type: application/json
{"title":"Nope"}
400
{"error":"title and author are required"}

POST /books
Content-Type: application/json
{"title":"Nope","author":"Someone","year":"2020"}
400
{"error":"year must be an integer"}

PATCH /books/4
Content-Type: application/json
{"genre":"literary fantasy"}
200
{"book":{"id":"4","title":"Piranesi","author":"Susanna Clarke","genre":"literary fantasy","year":2020}}

PATCH /books/4
Content-Type: application/json
{"title":"  "}
400
{"error":"title cannot be empty"}

PATCH /books/4
Content-Type: application/json
{"author":""}
400
{"error":"author cannot be empty"}

PATCH /books/99
Content-Type: application/json
{"year":2021}
404
{"error":"Book not found"}

DELETE /books/4
204

DELETE /books/99
404
{"error":"Book not found"}

GET /nope
404
{"error":"Not found"}