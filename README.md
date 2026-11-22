# AWS Student Records Web App

A full-stack student records management system built and deployed on AWS as part of a cloud computing module. The app runs on an EC2 instance with a MariaDB/MySQL database, uses AWS Secrets Manager for secure credential storage, and is served over HTTPS with a Node.js/Express backend.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Runtime | Node.js |
| Framework | Express.js |
| Templating | Mustache (via mustache-express) |
| Database | MariaDB / MySQL (mysql2) |
| Cloud | AWS EC2, AWS Secrets Manager |
| Frontend | Bootstrap 4, jQuery |
| Validation | express-validator |

---

## Features

- **View all students** — lists every record from the database
- **Add a student** — form with server-side validation (name, address, city, state, phone)
- **Update a student** — pre-filled edit form loaded by ID
- **Delete a student** — removes a record by ID
- **AWS Secrets Manager integration** — DB credentials are pulled from a named secret at startup; falls back to localhost defaults if not found (useful for local dev)
- **CORS enabled** — API endpoints accessible cross-origin
- **Custom 404 page** — handled gracefully

---

## Project Structure

```
├── index.js                        # App entry point, routes
├── package.json
├── app/
│   ├── config/
│   │   └── config.js               # DB config — reads from AWS Secrets Manager
│   ├── controller/
│   │   └── supplier.controller.js  # Route handlers (create, read, update, delete)
│   └── models/
│       └── supplier.model.js       # MySQL queries
├── views/                          # Mustache HTML templates
│   ├── home.html
│   ├── supplier-add.html
│   ├── supplier-update.html
│   ├── supplier-list-all.html
│   ├── 404.html
│   └── 500.html
└── public/
    ├── css/
    │   ├── base.css
    │   └── bootstrap.min.css
    ├── js/
    │   ├── bootstrap.min.js
    │   └── jquery-3.6.0.min.js
    └── img/
        └── favicon.ico
```

---

## AWS Architecture

- **EC2** — Linux instance hosting the Node.js app
- **MariaDB** — relational database running on the EC2 instance (or RDS)
- **AWS Secrets Manager** — stores DB credentials securely under the secret name `Mydbsecret`
- **HTTPS / SSL** — app served securely via a configured certificate on the EC2 instance
- **Security Groups** — configured to allow HTTP/HTTPS traffic on appropriate ports

---

## How to Run Locally

1. **Clone the repo**
   ```bash
   git clone https://github.com/aradsgit/aws-student-records.git
   cd aws-student-records
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Set up the database**

   Create a MySQL/MariaDB database called `STUDENTS` with a `students` table:
   ```sql
   CREATE DATABASE STUDENTS;
   USE STUDENTS;
   CREATE TABLE students (
     id INT AUTO_INCREMENT PRIMARY KEY,
     name VARCHAR(255),
     address VARCHAR(255),
     city VARCHAR(255),
     state VARCHAR(255),
     email VARCHAR(255),
     phone VARCHAR(50)
   );
   ```

4. **Configure environment**

   The app reads DB credentials from AWS Secrets Manager by default. For local development, it falls back to:
   - Host: `localhost`
   - DB: `STUDENTS`
   - User: `nodeapp`

   You can also set environment variables directly:
   ```bash
   export APP_DB_HOST=localhost
   export APP_DB_USER=your_user
   export APP_DB_PASSWORD=your_password
   export APP_DB_NAME=STUDENTS
   ```

5. **Start the server**
   ```bash
   npm start
   # or
   node index.js
   ```

6. **Open in browser**
   ```
   http://localhost:3000
   ```

---

## Routes

| Method | Route | Description |
|---|---|---|
| GET | `/` | Home page |
| GET | `/students/` | List all students |
| GET | `/supplier-add` | Add student form |
| POST | `/supplier-add` | Submit new student |
| GET | `/supplier-update/:id` | Edit student form |
| POST | `/supplier-update` | Submit updated student |
| POST | `/supplier-remove/:id` | Delete a student |

---

## Security Notes

- Database credentials are **not hardcoded** — stored in AWS Secrets Manager
- Input validation and sanitisation applied on all POST routes via `express-validator`
- `.gitignore` excludes `node_modules/`, `.pem` keys, and any files with real credentials
- Repo is **private** as the base project is course material from TU Dublin

---

## Author

**Arad Soltanian**  
BSc Cloud Computing, TU Dublin  
GitHub: [@aradsgit](https://github.com/aradsgit)
