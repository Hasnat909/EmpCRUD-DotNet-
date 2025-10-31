Employee Management System

Full-stack Employee Management System — Angular 18 single-page app (user interface) paired with a .NET 9 Web API backend. Implements end-to-end employee record management (create, read, update, delete) with a focus on clear architecture, modern patterns, and practical developer ergonomics.

Executive summary 

A compact, production-oriented demo combining an Angular 18 SPA with a .NET 9 Web API using EF Core (SQLite) and a measured CQRS pattern for command handling. Built as a hands-on learning/portfolio project demonstrating full-stack capabilities.

Why this repo is worth reviewing

Clear full-stack separation: frontend (Angular) and backend (.NET) are independent and easy to run locally.

Modern backend stack: .NET 9 Web API, EF Core migrations, MediatR used for command handling, and Swagger for fast API discovery.

Practical frontend: Angular 18 with standalone components, client routing for list/detail/create/edit flows, and SSR support.

Ready to demo: one command to run backend and one for frontend — minimal setup for live demos and interviews.

(If you want to run the app locally, Quickstart below has exact commands.)

Tech stack 

Backend: .NET 9, ASP.NET Core Web API, Entity Framework Core (SQLite), MediatR (partial CQRS), Swagger (OpenAPI).

Frontend: Angular 18, TypeScript, Angular Router, HttpClient; SSR scaffolding included.

Database: SQLite (file-based, easy for local demos).

Highlights / Implemented capabilities

Complete REST CRUD surface for Employees: create, list, fetch, update, delete (ready to exercise via Swagger or the Angular UI).

Command handling via MediatR for creates — shows CQRS awareness and pattern usage.

EF Core migrations present (database schema captured, easy to apply with EF tools).

CORS and developer tooling configured so the Angular dev server can interact with the API during local development.

Angular UI implements list, detail, create, and edit flows with appropriate navigation and basic client-side UX (forms, confirmation on delete).

Quickstart — run locally


Start backend API:

cd Employee-Management-System-main/EmployeeManagementAPI
dotnet restore
dotnet build
dotnet run


The API is configured for local development and is reachable at the default development URL (commonly http://localhost:5037). Swagger UI is available in dev mode for easy exploration.

Start frontend:

cd Employee-Management-System-main/employee-management-system
npm install
npm start


Open the Angular app at http://localhost:4200 and exercise the UI flows.

API at a glance (for interview demos)

Endpoints implemented for the Employees resource (suitable for quick demo via Swagger or curl):

POST /api/employees — create employee

GET /api/employees — list employees

GET /api/employees/{id} — get employee detail

PUT /api/employees/{id} — update employee

DELETE /api/employees/{id} — delete employee

(These routes are implemented in the backend controllers and are wired to EF Core persistence and command handlers for create operations.)

What reviewers will find in the code

Clear controller wiring and request routing on the backend.

EF Core DbContext, migration files, and an approachable SQLite setup for demos.

MediatR command and handler examples (illustrating domain command flow).

Angular routes, services, and component structures that align with modern enterprise patterns.

A minimal but functional test scaffold on the frontend (unit specs present for core components).
