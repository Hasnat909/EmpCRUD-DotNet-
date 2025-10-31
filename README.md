Employee Management System — CRUD API & Angular Frontend

Full-stack CRUD Employee Management System: a .NET 9 Web API (EF Core + MediatR + SQLite) backend with an Angular 18 SPA frontend for creating, reading, updating and deleting employee records.
(Built as a learning project.)

Top summary

Backend: .NET 9 Web API with Entity Framework Core (SQLite), partial MediatR/CQRS usage, and Swagger for API exploration. (see: Employee-Management-System-main/EmployeeManagementAPI/Program.cs:1–36, Employee-Management-System-main/EmployeeManagementAPI/EmployeeManagementAPI.csproj:3–13, Employee-Management-System-main/EmployeeManagementAPI/CQRS/Handlers/CreateEmployeeHandler.cs:1–36)

Frontend: Angular 18 SPA using HttpClient to call the API; routes and components exist for list / detail / create / edit. (see: Employee-Management-System-main/employee-management-system/src/app/app.routes.ts:1–11, Employee-Management-System-main/employee-management-system/src/app/services/employee.service.ts:1–36)

Tech stack

Backend: .NET 9 Web API, ASP.NET Core Controllers, EF Core (SQLite), MediatR (partial). (see: Employee-Management-System-main/EmployeeManagementAPI/EmployeeManagementAPI.csproj:3–13, Employee-Management-System-main/EmployeeManagementAPI/Program.cs:5–16)

Frontend: Angular 18 (standalone components), TypeScript, Angular Router, HttpClient. (see: Employee-Management-System-main/employee-management-system/package.json:1–33, Employee-Management-System-main/employee-management-system/src/app/components/employee-list/employee-list.component.ts:6–14)

DB: SQLite via EF Core connection string Data Source=employees.db. (see: Employee-Management-System-main/EmployeeManagementAPI/appsettings.json:1–6, Employee-Management-System-main/EmployeeManagementAPI/Program.cs:11–14)

What the project actually does (features implemented — code-backed)

Create employee (API) — handled as a MediatR command (CreateEmployeeCommand → CreateEmployeeHandler) and persisted via EF Core. (see: Employee-Management-System-main/EmployeeManagementAPI/CQRS/Commands/CreateEmployeeCommand.cs:1–18, Employee-Management-System-main/EmployeeManagementAPI/CQRS/Handlers/CreateEmployeeHandler.cs:15–34, Employee-Management-System-main/EmployeeManagementAPI/Controllers/EmployeesController.cs:18–27)

Read all employees (API) — controller endpoint returning all employees from EF DbContext. (see: Employee-Management-System-main/EmployeeManagementAPI/Controllers/EmployeesController.cs:29–33)

Read employee by id (API) — controller endpoint returns a single employee by id. (see: Employee-Management-System-main/EmployeeManagementAPI/Controllers/EmployeesController.cs:35–45)

Update employee (API) — controller endpoint updates fields on an employee. (see: Employee-Management-System-main/EmployeeManagementAPI/Controllers/EmployeesController.cs:47–66)

Delete employee (API) — controller endpoint deletes an employee. (see: Employee-Management-System-main/EmployeeManagementAPI/Controllers/EmployeesController.cs:68–86)

Angular UI: list / detail / create / edit — components and routes exist to list employees, view details by route id, and create/edit employees. (see: Employee-Management-System-main/employee-management-system/src/app/components/employee-list/employee-list.component.ts:1–34, Employee-Management-System-main/employee-management-system/src/app/components/employee-detail/employee-detail.component.ts:1–30, Employee-Management-System-main/employee-management-system/src/app/components/employee-form/employee-form.component.ts:1–48,50–64, Employee-Management-System-main/employee-management-system/src/app/app.routes.ts:3–11)

Angular service points to backend API — base URL hardcoded to http://localhost:5037/api/employees. (see: Employee-Management-System-main/employee-management-system/src/app/services/employee.service.ts:9–11)

Swagger / OpenAPI enabled for the API (development). (see: Employee-Management-System-main/EmployeeManagementAPI/Program.cs:24–28, Employee-Management-System-main/EmployeeManagementAPI/EmployeeManagementAPI.csproj:19–21)

CORS configured to allow Angular dev server http://localhost:4200. (see: Employee-Management-System-main/EmployeeManagementAPI/Program.cs:18–23,31–31)

EF Core DbContext and Migrations present — project contains EmployeeDbContext and migration files. (see: Employee-Management-System-main/EmployeeManagementAPI/Data/EmployeeDbContext.cs:1–11, Employee-Management-System-main/EmployeeManagementAPI/Migrations/20240707083400_InitialCreate.cs:1–1)

Quickstart — exact commands to run locally

Backend (API) — run from repository root (this is the exact flow used for development):

# from repo root
cd Employee-Management-System-main/EmployeeManagementAPI && dotnet restore
cd Employee-Management-System-main/EmployeeManagementAPI && dotnet build
cd Employee-Management-System-main/EmployeeManagementAPI && dotnet run


The API project file exists and targets net9.0. (see: Employee-Management-System-main/EmployeeManagementAPI/EmployeeManagementAPI.csproj:4–4)

The API uses SQLite and creates/uses employees.db by default per appsettings.json. (see: Employee-Management-System-main/EmployeeManagementAPI/appsettings.json:1–6)

Frontend (Angular) — run from repo root:

cd Employee-Management-System-main/employee-management-system
npm install
npm start   # runs "ng serve" per package.json


npm start is defined in package.json and will run the Angular dev server. (see: Employee-Management-System-main/employee-management-system/package.json:3–12)

Notes:

API CORS allows the Angular dev server at http://localhost:4200. (see: Employee-Management-System-main/EmployeeManagementAPI/Program.cs:18–23,31–31)

No environment .env files are used for backend configuration — connection string comes from appsettings.json. (see: Employee-Management-System-main/EmployeeManagementAPI/appsettings.json:1–6)

Angular service currently contains a hardcoded API base URL (no environment switching in code). (see: Employee-Management-System-main/employee-management-system/src/app/services/employee.service.ts:9–11)

API summary (routes implemented — exact controller citations)

Controller base route: [Route("api/[controller]")] in EmployeesController → routes resolve under /api/Employees. (see: Employee-Management-System-main/EmployeeManagementAPI/Controllers/EmployeesController.cs:10–14)

POST /api/Employees — Create employee (MediatR command). (see: Employee-Management-System-main/EmployeeManagementAPI/Controllers/EmployeesController.cs:18–27)

GET /api/Employees — Get all employees. (see: Employee-Management-System-main/EmployeeManagementAPI/Controllers/EmployeesController.cs:29–33)

GET /api/Employees/{id} — Get employee by id. (see: Employee-Management-System-main/EmployeeManagementAPI/Controllers/EmployeesController.cs:35–45)

PUT /api/Employees/{id} — Update employee fields. (see: Employee-Management-System-main/EmployeeManagementAPI/Controllers/EmployeesController.cs:47–66)

DELETE /api/Employees/{id} — Delete employee. (see: Employee-Management-System-main/EmployeeManagementAPI/Controllers/EmployeesController.cs:68–86)

Sample curl commands (use after starting API on default port)

The Angular service targets http://localhost:5037/api/employees, so examples use that base URL.

# Create
curl -X POST http://localhost:5037/api/Employees \
  -H "Content-Type: application/json" \
  -d '{"name":"Alice","position":"Engineer","salary":60000}'

# Get all
curl http://localhost:5037/api/Employees

# Get by id
curl http://localhost:5037/api/Employees/1

# Update
curl -X PUT http://localhost:5037/api/Employees/1 \
  -H "Content-Type: application/json" \
  -d '{"name":"Alice M","position":"Senior Engineer","salary":75000}'

# Delete
curl -X DELETE http://localhost:5037/api/Employees/1


(Cite: controller endpoints and Angular service base URL). (see: Employee-Management-System-main/EmployeeManagementAPI/Controllers/EmployeesController.cs:18–86, Employee-Management-System-main/employee-management-system/src/app/services/employee.service.ts:9–11)

Real-time features

None found. No SignalR / WebSocket / Server-Sent Events code present in the API or frontend. Searched these locations: EmployeeManagementAPI controllers, program, models, data, CQRS; and employee-management-system/src. (see: repository tree references above and absence of SignalR bits in Program.cs and Angular code).

Auth & security

No authentication/authorization scheme configured. Program.cs calls UseAuthorization() but there is no JWT/Identity scheme registration or [Authorize] attributes present on controllers. (see: Employee-Management-System-main/EmployeeManagementAPI/Program.cs:29–34, Employee-Management-System-main/EmployeeManagementAPI/Controllers/EmployeesController.cs:1–86)

All API endpoints are public based on current code. (see: same files above)

Database & migrations

ORM: Entity Framework Core with EmployeeDbContext (DbSet) present. (see: Employee-Management-System-main/EmployeeManagementAPI/Data/EmployeeDbContext.cs:1–11)

Provider: SQLite via connection string Data Source=employees.db. (see: Employee-Management-System-main/EmployeeManagementAPI/appsettings.json:1–6, Employee-Management-System-main/EmployeeManagementAPI/Program.cs:11–14)

Migrations: initial migration and snapshot files are present. (see: Employee-Management-System-main/EmployeeManagementAPI/Migrations/20240707083400_InitialCreate.cs:1–1, Employee-Management-System-main/EmployeeManagementAPI/Migrations/20240707083400_InitialCreate.Designer.cs:1–1, Employee-Management-System-main/EmployeeManagementAPI/Migrations/EmployeeDbContextModelSnapshot.cs:1–1)

Tests

Angular unit test scaffolds exist (Karma/Jasmine devDependencies and .spec.ts files for components/services). Run with:

cd Employee-Management-System-main/employee-management-system
npm test


(see: Employee-Management-System-main/employee-management-system/package.json:20–33, test specs: employee-list.component.spec.ts, employee-form.component.spec.ts, employee-detail.component.spec.ts, employee.service.spec.ts — see their files under src/app/...)

No .NET test projects found in EmployeeManagementAPI or solution root. (searched: EmployeeManagementAPI folder & solution root)

Docker & CI/CD

None found. No Dockerfile, docker-compose.yml, or .github/workflows/* files present in repository tree. (searched repo root and EmployeeManagementAPI/frontend folders)

Known limitations (evidence-backed)

No authentication/authorization; endpoints are public. (see: Program.cs:29–34)

Partial CQRS: Create uses MediatR, but read/update/delete are performed directly in controllers via DbContext — inconsistent pattern. (see: CQRS/Handlers/CreateEmployeeHandler.cs:15–34 vs Controllers/EmployeesController.cs:29–86)

No DTO mapping: entities appear to be exposed directly through API (no AutoMapper or DTO layer found). (see: Controllers/EmployeesController.cs:29–45)

No server-side validation for create/update payloads (no DataAnnotations or FluentValidation found in controller code). (see: Controllers/EmployeesController.cs:18–27,47–66)

No global exception handling middleware (no explicit error-handling middleware registration). (see: Program.cs:1–36)

Angular service hardcodes API base URL (no environment-based switching). (see: employee-management-system/src/app/services/employee.service.ts:9–11)

No pagination/filtering/sorting for GET list endpoint. (see: Controllers/EmployeesController.cs:29–33)

Limited logging beyond default (no structured/audit logging implemented). (see: appsettings.json:6–12)

Suggested next steps (PR-level, prioritized)

Add authentication (JWT bearer) and protect write endpoints with [Authorize]. (addresses public endpoints)

Standardize CQRS: either expand MediatR usage for read/update/delete or remove MediatR and keep a consistent controller/service approach. (addresses pattern inconsistency)

Introduce DTOs + AutoMapper to decouple EF entities from API contracts.

Add request validation (DataAnnotations or FluentValidation) and return standardized ProblemDetails for validation errors.

Add global exception handling middleware and structured logging (Serilog).

Externalize frontend API URL into environment.ts files and make builds target the correct API. (replace hardcoded base URL)

Add integration tests for API (in-memory SQLite or TestServer) and expand Angular unit tests to meaningful assertions.

Add Dockerfiles + docker-compose for local dev orchestration and a GitHub Actions CI to run builds & tests.

Implement pagination/filtering for employee list endpoints.

Add OpenAPI examples and improve Swagger descriptions for clearer API docs.

Minimal contribution guide

Fork and create a feature branch (feature/<short-desc>).

Run backend and frontend locally using the Quickstart commands above.

Add tests for any new behavior and update README.md with design notes for larger changes.

Open a PR and include a short description & test plan.

Architecture (ASCII diagram — simple overview)
[Angular 18 SPA] --> HTTP --> [ .NET 9 Web API (Controllers) ]
                                     |
                                   EF Core
                                     |
                                  [ SQLite DB ]



Where to look in code (quick pointers)

API program & wiring: Employee-Management-System-main/EmployeeManagementAPI/Program.cs:1–36

API controllers: Employee-Management-System-main/EmployeeManagementAPI/Controllers/EmployeesController.cs:1–86

EF DbContext: Employee-Management-System-main/EmployeeManagementAPI/Data/EmployeeDbContext.cs:1–11

Migrations: Employee-Management-System-main/EmployeeManagementAPI/Migrations/* (initial migration files present)

MediatR command/handler (Create): Employee-Management-System-main/EmployeeManagementAPI/CQRS/Commands/CreateEmployeeCommand.cs:1–18, .../Handlers/CreateEmployeeHandler.cs:1–36

Angular routes: Employee-Management-System-main/employee-management-system/src/app/app.routes.ts:1–11

Angular service: Employee-Management-System-main/employee-management-system/src/app/services/employee.service.ts:1–36

Angular components: employee-list, employee-detail, employee-form under src/app/components/*.
