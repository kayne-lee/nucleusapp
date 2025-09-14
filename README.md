# Nucleus — The All-in-One Student Productivity Hub

> **Built by students, for students.**
> Each semester, students waste hours juggling syllabi, deadlines, and grades across too many apps. We’ve been there—so we built **Nucleus**.

**Live demo:** [[https://nucleusapp.ca](https://nucleusapp.ca)](https://youtu.be/Z_G1My7CPzA)

---

## ✨ What Nucleus Offers

* **Syllabus Parsing** – Drop in a PDF/URL and Nucleus pulls key dates, weights, and deliverables.
* **Automated Task List & Weekly View** – Everything due this week, at a glance.
* **Custom Task Tiles** – Add one-offs (lab, study block, reminder) in seconds.
* **Calendar Integration** – Sync deadlines to your calendar.
* **Grade Tracking & Predictions** – Real-time current mark + “what-if” scenarios.

Nucleus is built to simplify academic life and help you focus on learning, not admin work.

---

## 🧭 Why We Built Nucleus

* **75%** of students identify as procrastinators.
* **78%** struggle with time management.
* **87%** rely on syllabi as their single source of truth.

There wasn’t a student-first solution that actually streamlines it all—so we made one.

---

## 🏗️ Monorepo Structure

```
nucleus/
├─ fe/            # React web app (Vite/CRA)
├─ be/            # Spring Boot API (Java 17)
├─ email/         # Node.js email microservice (SMTP)
└─ docs/          # Design notes, ERDs, API docs (optional)
```

---

## 🔌 Architecture (high level)

```
[ React FE ]  ── REST ──>  [ Spring Boot API ]
      │                         │
      └── webhooks/REST ──>     └── calls → [ email svc (Node/SMTP) ]
                               (DB: Postgres/MySQL/… via JPA)
```

* **FE** talks to **BE** for auth, tasks, syllabus parsing, grades.
* **BE** persists data and triggers email notifications by calling **email** service.
* **email** service sends transactional emails via SMTP (or a provider like SendGrid).

---

## 🚀 Quick Start

### 0) Prereqs

* Node 18+, npm or pnpm
* Java 17, Maven 3.8+
* Docker (optional but recommended)

**Email service (Node)**

```bash
cd email
npm install
npm run dev   # or: npm start
```

**Frontend (React)**

```bash
cd fe
npm install
npm run dev   # Vite (http://localhost:5173)
# CRA alternative: npm start (http://localhost:3000)
```

---

## 🐳 Docker & Compose

### Backend: Dockerfile (provided)

```dockerfile
# build
FROM maven:3.8.1-openjdk-17-slim AS build
WORKDIR /
COPY . .
RUN mvn clean package -DskipTests

# runtime
FROM openjdk:17-jdk-slim
COPY --from=build /target/be-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

## 🔐 Authentication

* **JWT Bearer tokens**
* Login returns `accessToken`; include in `Authorization: Bearer <token>`.

---

## 🤝 Contributing

1. Fork & branch: `feat/your-feature`
2. Add tests where relevant
3. `mvn test` (be), `npm test` (fe/email)
4. Open a PR with a clear description & screenshots

