# Course Tracker

Course Tracker is a full-stack web application for organizing and tracking courses, subjects, and listening groups. Candidates can register, join listening groups for courses, and track their progress, while instructors run groups within the subjects they teach.

## Technologies

**Backend**
- Java 17
- Spring Boot 4 (Web MVC, Data JPA, Validation, Mail, Actuator)
- MySQL
- Liquibase (database schema versioning)
- OpenPDF (PDF confirmation generation)
- Lombok
- Maven

**Frontend**
- React 19 + Vite
- React Router
- React Bootstrap / Bootstrap 5
- Recharts (charts)

## Features

- User registration and login (candidates and instructors) with e-mail verification code
- Browsing subjects, courses, and cities
- Creating, editing, and deleting listening groups
- Candidates joining a listening group
- Generating a PDF confirmation for a group registration
- Viewing candidates along with their listening groups and history
- Responsive user interface

## Project structure

```
njt/
├── backend/   # Spring Boot REST API
└── frontend/  # React (Vite) client application
```

### Backend packages

```
com.projekat.backend
├── config       # application configuration
├── controller   # REST controllers
├── dto          # data transfer objects
├── entity       # JPA entities (User, Candidate, Instructor, Course, Subject, City, ListeningGroup, LC, ...)
├── exception    # error handling
├── repository   # Spring Data JPA repositories
└── service      # business logic
```

## Running the application

### Prerequisites

- Java 17+
- Maven (or the bundled `mvnw` wrapper)
- Node.js 18+ and npm
- MySQL server

### Backend

1. Create the MySQL database, e.g.:
   ```sql
   CREATE DATABASE njtseminarski;
   ```
2. Configure the database connection as needed in `backend/src/main/resources/application.properties`:
   ```properties
   spring.datasource.url=jdbc:mysql://localhost:3306/njtseminarski
   spring.datasource.username=root
   spring.datasource.password=
   ```
3. The mail password is not stored in the file; it is read from the `MAIL_PASSWORD` environment variable (a Gmail app password for the account set in `spring.mail.username`). Set this variable before starting the backend, e.g.:
   ```bash
   export MAIL_PASSWORD=<app-password>   # Linux/macOS
   $env:MAIL_PASSWORD="<app-password>"   # Windows PowerShell
   ```
4. Run the application from the `backend` folder:
   ```bash
   ./mvnw spring-boot:run
   ```

The database schema is applied automatically via Liquibase migrations (`db/changelog`) on application startup. By default the backend listens on port `8080`.

### Frontend

```bash
cd frontend
npm install
npm run dev
```

The application will be available at the address printed by Vite (by default `http://localhost:5173`).

## API

The backend exposes a REST API under the `/api` prefix, including:

- `POST /api/register/candidate`, `POST /api/register/instructor` — registration with e-mail verification
- `POST /api/login` — user login
- `GET /api/subjects`, `GET /api/courses`, `GET /api/cities` — reference data
- `GET /api/listening-groups`, `POST /api/listening-groups`, `PUT /api/listening-groups/{id}`, `DELETE /api/listening-groups/{id}` — listening group management
- `POST /api/candidates/{candidateId}/listening-groups/{listeningGroupId}/join` — candidate joins a group
- `GET /api/candidates/{candidateId}/listening-groups/{listeningGroupId}/confirmation` — PDF registration confirmation
