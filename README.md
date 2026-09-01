# Course Tracker

Course Tracker je full-stack web aplikacija za organizaciju i praćenje kurseva, predmeta i grupa za slušanje (listening groups). Aplikacija omogućava kandidatima da se registruju, prijave na grupe za slušanje kurseva i prate svoj napredak, dok instruktori vode grupe u okviru predmeta koje predaju.

## Tehnologije

**Backend**
- Java 17
- Spring Boot 4 (Web MVC, Data JPA, Validation, Mail, Actuator)
- MySQL
- Liquibase (verzionisanje baze podataka)
- OpenPDF (generisanje PDF potvrda)
- Lombok
- Maven

**Frontend**
- React 19 + Vite
- React Router
- React Bootstrap / Bootstrap 5
- Recharts (grafici)

## Funkcionalnosti

- Registracija i prijava korisnika (kandidati i instruktori) uz verifikaciju putem e-mail koda
- Pregled predmeta, kurseva i gradova
- Kreiranje, izmena i brisanje grupa za slušanje kursa
- Prijava kandidata na grupu za slušanje (join group)
- Generisanje PDF potvrde o prijavi na grupu
- Pregled kandidata sa njihovim grupama za slušanje i istorijom
- Responsivan korisnički interfejs

## Struktura projekta

```
njt/
├── backend/   # Spring Boot REST API
└── frontend/  # React (Vite) klijentska aplikacija
```

### Backend paketi

```
com.projekat.backend
├── config       # konfiguracija aplikacije
├── controller   # REST kontroleri
├── dto          # data transfer objekti
├── entity       # JPA entiteti (User, Candidate, Instructor, Course, Subject, City, ListeningGroup, LC, ...)
├── exception    # rukovanje greškama
├── repository   # Spring Data JPA repozitorijumi
└── service      # poslovna logika
```

## Pokretanje aplikacije

### Preduslovi

- Java 17+
- Maven (ili korišćenje priloženog `mvnw` wrapper-a)
- Node.js 18+ i npm
- MySQL server

### Backend

1. Kreirati MySQL bazu, npr:
   ```sql
   CREATE DATABASE njtseminarski;
   ```
2. Podesiti konekciju na bazu po potrebi u `backend/src/main/resources/application.properties`:
   ```properties
   spring.datasource.url=jdbc:mysql://localhost:3306/njtseminarski
   spring.datasource.username=root
   spring.datasource.password=
   ```
3. Mail lozinka se ne čuva u fajlu, već se čita iz environment varijable `MAIL_PASSWORD` (Gmail app password za nalog naveden u `spring.mail.username`). Pre pokretanja backend-a je potrebno postaviti tu varijablu, npr:
   ```bash
   export MAIL_PASSWORD=<app-lozinka>   # Linux/macOS
   $env:MAIL_PASSWORD="<app-lozinka>"   # Windows PowerShell
   ```
4. Pokrenuti aplikaciju iz `backend` foldera:
   ```bash
   ./mvnw spring-boot:run
   ```

Šema baze se automatski primenjuje pomoću Liquibase migracija (`db/changelog`) prilikom pokretanja aplikacije. Backend po podrazumevanim podešavanjima sluša na portu `8080`.

### Frontend

```bash
cd frontend
npm install
npm run dev
```

Aplikacija će biti dostupna na adresi koju ispiše Vite (podrazumevano `http://localhost:5173`).

## API

Backend izlaže REST API pod prefiksom `/api`, između ostalog:

- `POST /api/register/candidate`, `POST /api/register/instructor` — registracija uz e-mail verifikaciju
- `POST /api/login` — prijava korisnika
- `GET /api/subjects`, `GET /api/courses`, `GET /api/cities` — osnovni podaci
- `GET /api/listening-groups`, `POST /api/listening-groups`, `PUT /api/listening-groups/{id}`, `DELETE /api/listening-groups/{id}` — upravljanje grupama za slušanje
- `POST /api/candidates/{candidateId}/listening-groups/{listeningGroupId}/join` — prijava kandidata na grupu
- `GET /api/candidates/{candidateId}/listening-groups/{listeningGroupId}/confirmation` — PDF potvrda o prijavi
