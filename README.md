# showcase-angular-ngrx-store

> An enterprise Angular 17 course management application showcasing predictable state management with NgRx Store, Effects, Entity adapters, and time-travel devtools.

**Inception (First Commit):** `2024-04-02` • **Batch / Horizon:** `batch-2024-h1` • **Architecture Domain:** `Predictable State Management / Angular NgRx Store`

---

## 📌 Architecture & Overview

`angular-ngRx` demonstrates the implementation of the Redux pattern in modern Angular (v17) using the official `@ngrx` platform. It provides an enterprise-scale course and curriculum management interface backed by an embedded mock Node.js/Express REST server.

The application strictly enforces unidirectional data flow: user interactions dispatch strongly typed Actions, asynchronous side effects (HTTP calls, localStorage caching) are isolated within Effects, and state mutations are performed exclusively through pure Reducer functions. Entity normalization is streamlined via `@ngrx/entity`.

```
+-------------------------------------------------------------------------+
|                               angular-ngRx                              |
|                                                                         |
|  [ Angular Component UI ]                                               |
|         |                                                               |
|         | 1. Dispatches Action: courseActions.loadAllCourses()          |
|         v                                                               |
|  +--------------------+                                                 |
|  | NgRx Store         |                                                 |
|  +---------+----------+                                                 |
|            |                                                            |
|            +-----------------------+                                    |
|            | 2. Intercepts Action  | 2. Synchronous State Mutation      |
|            v                       v                                    |
|  +--------------------+  +--------------------+                         |
|  | NgRx Effects       |  | Pure Reducers      |                         |
|  | (courses.effects)  |  | (EntityAdapter)    |                         |
|  +---------+----------+  +---------+----------+                         |
|            |                       |                                    |
|            | 3. HTTP GET /courses  | 4. Emits New Immutable State       |
|            v                       v                                    |
|  +--------------------+  +--------------------+                         |
|  | Express Mock API   |  | Memoized Selectors | (createSelector)        |
|  | (server/server.ts) |  +---------+----------+                         |
|  +--------------------+            |                                    |
|                                    | 5. Streamed via async pipe         |
|                                    v                                    |
|                          [ UI View Updates ]                            |
+-------------------------------------------------------------------------+
```

* **Context:** Enterprise state architecture reference demonstrating NgRx Entity collection normalization, side-effect isolation, and reactive UI data binding.
* **Key Patterns:**
  * **Unidirectional Data Flow:** Strict separation between action dispatchers, pure state reducers, and observable side-effects.
  * **Normalized Entity Storage:** Using `createEntityAdapter<Course>` to manage items as an indexed dictionary (`ids: string[], entities: Record<string, Course>`), yielding $O(1)$ lookup performance.
  * **Memoized Selector Graphs:** Composing efficient selectors via `createFeatureSelector` and `createSelector` to compute derived state with automatic caching.
  * **Time-Travel Observability:** Integrating `@ngrx/store-devtools` for inspecting dispatched actions, diffing state trees, and rolling back state transitions.

---

## 🛠 Tech Stack

* **Client Framework:** Angular 17.x (`@angular/core`, `@angular/forms`, `@angular/router`)
* **State Management Suite:** NgRx 17 (`@ngrx/store`, `@ngrx/effects`, `@ngrx/entity`, `@ngrx/router-store`, `@ngrx/store-devtools`)
* **UI Components & Styling:** Angular Material 17, Angular CDK, SCSS
* **Backend Mock Server:** Node.js, Express, `ts-node`
* **Testing:** Karma, Jasmine, Protractor (legacy)

---

## ✨ Features & State Slices

* **Authentication Feature Slice (`auth`):** Login actions, JWT session state, and persistent authentication effects.
* **Course Catalog Slice (`courses`):** Normalized entity collection, category filtering (Beginner, Advanced), and live course editing dialogs.
* **Lessons Pagination Slice (`lessons`):** Search queries and paginated lesson browsing.
* **Router Integration:** `@ngrx/router-store` synchronizing Angular router navigation with the global state tree.

### Core NgRx Store Structure

| State Slice | Key Actions | Entity Adapter | Associated Effects |
|---|---|---|---|
| `auth` | `login`, `logout` | No | Persists session credentials to storage |
| `courses` | `loadAllCourses`, `allCoursesLoaded`, `courseUpdated` | Yes (`Course`) | Fetches catalog via HTTP, syncs updates |
| `router` | `ROUTER_NAVIGATION`, `ROUTER_CANCELLED` | No | Serializes active route params into state |

---

## 🚀 Getting Started

### Prerequisites

* Node.js `>= 18.0.0`
* Angular CLI 17 (`npm install -g @angular/cli@17`)

### Installation

```bash
# Clone the repository
git clone https://github.com/fabogit/showcase-angular-ngrx-store.git
cd showcase-angular-ngrx-store

# Install dependencies
npm install
```

### Running the Application

To ensure proper data fetching, run the backend mock API and the Angular client simultaneously:

```bash
# Terminal 1: Launch Express Mock REST Server (Port 9000)
npm run server

# Terminal 2: Launch Angular Development Server (Port 4200)
npm start
```

Open [http://localhost:4200](http://localhost:4200) in your browser. Install the Redux DevTools extension to inspect time-travel debugging.

### Running Tests

```bash
# Unit & reducer tests via Karma
npm test
```

---

## 📄 License & Author

Developed by [@fabogit](https://github.com/fabogit).
