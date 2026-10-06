# 🎭 Review Manager – FastAPI Service

A lightweight **FastAPI application** for managing **theatre play reviews**, backed by a SQLite database, with full CRUD, filtering, pagination and per-play average ratings.

This project demonstrates **REST API design**, request validation with Pydantic/SQLModel, and database persistence.

---

## 🚀 Features

### ✍️ Review CRUD

- Create a review via `POST /review/`
- Fetch one review via `GET /review/{review_id}`
- Partially update a review's rating or comment via `PATCH /review/{review_id}`
- Delete a review via `DELETE /review/{review_id}`

### 📋 Listing & Filtering

- List reviews via `GET /review/`
- Filter by `play_name`
- Paginate with `skip` (default 0) and `limit` (default 10, max 50)

### ⭐ Average Rating

- `GET /review/average/{play_name}` returns the average rating (rounded to 2 decimals) and total review count for a play

### ✅ Validation & Error Handling

- Ratings must be an integer between 1 and 5
- `404` responses for unknown reviews or plays with no reviews
- Auto-generated interactive docs (Swagger UI / ReDoc)

---

## 🛠️ Tech Stack

- **Language:** Python 3
- **Framework:** FastAPI
- **ORM / Validation:** SQLModel (SQLAlchemy + Pydantic)
- **Server:** Uvicorn
- **Database:** SQLite (`reviewmanager.db`, created automatically on startup)

---

## 📂 Project Structure

```
review-manager/
├─ README.md
├─ requirements.txt
├─ main.py            # FastAPI app, lifespan (table creation) and root route
├─ database.py        # SQLite engine, table creation and session dependency
├─ models.py          # SQLModel table + create/read/update schemas
└─ routes/
   ├─ __init__.py
   └─ reviews.py      # /review endpoints
```

---

## ⚙️ Installation & Setup

### 1️⃣ Clone the repository

```bash
git clone https://github.com/mishraabhishek11/review-manager.git
```

### 2️⃣ Navigate to the project folder

```bash
cd review-manager
```

### 3️⃣ Create and activate a virtual environment

```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS / Linux
source .venv/bin/activate
```

### 4️⃣ Install dependencies

```bash
pip install -r requirements.txt
```

### 5️⃣ Start the server

```bash
uvicorn main:app --reload
```

### 6️⃣ Open in browser

- API: http://localhost:8000
- Swagger UI: http://localhost:8000/docs
- ReDoc: http://localhost:8000/redoc

---

## 🧑‍💻 Usage

### Endpoints

| Method | Path                          | Description                                          |
| ------ | ----------------------------- | ---------------------------------------------------- |
| GET    | `/`                           | Welcome message                                      |
| POST   | `/review/`                    | Create a review                                      |
| GET    | `/review/`                    | List reviews (optional `play_name`, `skip`, `limit`) |
| GET    | `/review/average/{play_name}` | Average rating and review count for a play           |
| GET    | `/review/{review_id}`         | Get a single review                                  |
| PATCH  | `/review/{review_id}`         | Update a review's `rating` and/or `comment`          |
| DELETE | `/review/{review_id}`         | Delete a review                                      |

### Create a review

```bash
curl -X POST http://localhost:8000/review/ \
  -H "Content-Type: application/json" \
  -d '{"play_name": "Hamlet", "reviewer_name": "Asha", "rating": 5, "comment": "Superb performance"}'
```

```json
{
  "id": 1,
  "play_name": "Hamlet",
  "reviewer_name": "Asha",
  "rating": 5,
  "comment": "Superb performance",
  "created_at": "2026-10-06T10:30:00.000000"
}
```

### List reviews

```bash
curl "http://localhost:8000/review/?play_name=Hamlet&skip=0&limit=10"
```

### Average rating

```bash
curl http://localhost:8000/review/average/Hamlet
```

```json
{
  "play_name": "Hamlet",
  "average_rating": 4.5,
  "total_reviews": 2
}
```

### Update a review

```bash
curl -X PATCH http://localhost:8000/review/1 \
  -H "Content-Type: application/json" \
  -d '{"rating": 4}'
```

### Delete a review

```bash
curl -X DELETE http://localhost:8000/review/1
```

```json
{ "message": "Review deleted" }
```

---

## ⚠️ Error Responses

| Status | When                                                                                                              |
| ------ | ----------------------------------------------------------------------------------------------------------------- |
| 404    | Review ID does not exist (get / update / delete), or no reviews exist for the play (average)                      |
| 422    | FastAPI validation error: missing fields, non-integer ID, rating outside 1–5, `skip` < 0, or `limit` outside 1–50 |

Example (`GET /review/999`):

```json
{
  "detail": "NO review"
}
```

---

## 🗄️ Data Model

| Field           | Type     | Notes                       |
| --------------- | -------- | --------------------------- |
| `id`            | int      | Primary key, auto-generated |
| `play_name`     | str      | Indexed                     |
| `reviewer_name` | str      |                             |
| `rating`        | int      | 1–5                         |
| `comment`       | str      |                             |
| `created_at`    | datetime | UTC, set automatically      |

Tables are created automatically on application startup. There is no migration tooling yet, so schema changes require recreating `reviewmanager.db`.

---

## 🎯 Learning Objectives

- Build REST endpoints with FastAPI
- Persist data with SQLModel and SQLite
- Use dependency injection for database sessions
- Implement partial updates, filtering and pagination
- Design consistent request/response shapes

---

## 🔮 Future Enhancements

- Database migrations (e.g. Alembic)
- Unit and integration tests
- Consistent error response shape
- Authentication
- Docker support

---

## 🤝 Contributing

1. Fork the repository
2. Create a new branch: `git checkout -b feat/your-feature`
3. Commit your changes: `git commit -m "feat(scope): add your message"`
4. Push to the branch: `git push origin feat/your-feature`
5. Open a Pull Request

---

## 👨‍💻 Author

Abhishek Mishra  
GitHub: https://github.com/mishraabhishek11

---

## ⭐ Support

If you like this project, give it a ⭐ on GitHub!
