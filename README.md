# The Book Club

> Project developed for the **Object-Oriented Programming (OOP)** course.  
> A Java platform designed to engage readers through clubs, challenges, progress tracking, streaks, and gamified missions.

---

## About the Project

**The Book Club** is an application focused on turning reading into a collective, interactive experience. Users can create or join reading groups, log the number of pages read each day, discuss chapters through spoiler-protected comments, and climb the group ranking by hitting goals and keeping up consistent habits (streaks).

---

## MVP Features

Already implemented in the backend core:

* **Groups and Challenges:** Support for different types of reading groups (ongoing Clubs, ranked by pages read, and time-limited Challenges, ranked by streak).
* **Reading Management:** Registration of members, books, and chapters.
* **Daily Logging and Streaks:** Daily logging of pages read and tracking of consecutive reading days.
* **Gamified Ranking:** Automatically sorted member rankings, with different criteria for Clubs and Challenges.
* **Anti-Spoiler System:** Per-chapter comments that are automatically hidden from members who haven't reached that point in the book yet.
* **Book Review:** Final rating (0 to 10) and character picks (favorite, most hated, and most relatable).
* **HTTP API:** `ApiServer` (the JDK's built-in HttpServer, no framework) exposes `GET /api/ranking` and `POST /api/progresso`, and serves the visual prototype (`mockups` folder) already wired to these two endpoints.

Still to be implemented in this MVP delivery:

* **Meetups and Missions:** Scheduling of discussion meetups and a mission system based on completed goals.
* **Rest of the graphical interface:** logging reading progress and the ranking already use the real backend; creating/joining a group, commenting, and reviewing the book are still static visual prototypes only.

---

## Architecture & OOP Concepts

The application applies the core principles of Object-Oriented Programming to keep the code maintainable and scalable:

* **Abstraction and Inheritance:** The base class `Grupo` is extended by `Clube` and `Desafio`, reusing shared structures and specializing behavior.
* **Polymorphism:** Ranking and scoring calculations vary by group type (`Clube` focuses on ongoing habits; `Desafio` focuses on goals and deadlines).
* **Encapsulation:** The internal state of entities (`Membro`, `Livro`, `Capitulo`, etc.) is protected with private attributes and controlled accessor methods.
* **Custom Exception Handling:** Specific business errors are caught and handled (`CapituloInvalidoException`, `PrazoDesafioExpiradoException`, `NotaInvalidaException`, `CodigoGrupoInvalidoException`).
* **Collections:** Strategic use of `Collections` (`List`, `Set`) to manage members and reading logs and to calculate streaks.

---

## How to Run

Prerequisite: JDK 17+ installed.

Compile:
```bash
find src/main/java -name "*.java" | xargs javac -d out
```

Run:
```bash
java -cp out clubeleitura.Main
```

`Main` runs a fixed demo scenario: it creates members, a book, a club, and a challenge, logs reading sessions, calculates streaks and rankings, tests spoiler comments and reviews, and triggers the four business errors (`CapituloInvalidoException`, `PrazoDesafioExpiradoException`, `NotaInvalidaException`, `CodigoGrupoInvalidoException`) to show exception handling in action.

Besides `Main`, the project includes two verification classes that also run from the command line:

```bash
java -cp out clubeleitura.FluxoCompletoTest
```
Verifies the full end-to-end flow (create a group, join it, log progress, comment, view the ranking, finish the book, and review it), printing PASS or FAIL for each check.

```bash
java -ea -cp out clubeleitura.modelo.TesteRanking
```
Covers ranking, ties, streaks, and expired-deadline cases using `assert`, which is why it needs the `-ea` flag for the checks to actually run.

To view the interface in the browser, with ranking and reading logging connected to the real backend:

```bash
java -cp out clubeleitura.web.ApiServer
```

Starts a server at `http://localhost:8080` that serves the files in `mockups/` and exposes `GET /api/ranking` and `POST /api/progresso`.
