# README_for_AI.md (machine-first)
# Version: 2025-10-31
# Purpose: precise, evidence-cited description of repository contents and runtime behavior.
# Note: all paths are relative to repository root. Some scan outputs omit the outer folder
# (e.g., "EmployeeManagementAPI/..." vs "Employee-Management-System-main/EmployeeManagementAPI/...").
# Both refer to the same files when repo root contains Employee-Management-System-main/.

project:
  name: "Employee Management System"
  purpose: "Learning project: Angular 18 frontend + .NET 9 Web API backend (EF Core + SQLite)."
  confidence: "High (claims derived from code citations supplied)."

repo_structure_summary:
  backend_root: "EmployeeManagementAPI/ (aka Employee-Management-System-main/EmployeeManagementAPI/)"
  frontend_root: "employee-management-system/ (aka Employee-Management-System-main/employee-management-system/)"
  db_file: "EmployeeManagementAPI/employees.db (committed)"
  migrations_dir: "EmployeeManagementAPI/Migrations/"

run_commands:
  backend:
    working_directory: "Employee-Management-System-main/EmployeeManagementAPI (or EmployeeManagementAPI/)"
    steps:
      - "dotnet restore"
        evidence:
          - "EmployeeManagementAPI/EmployeeManagementAPI.csproj:1 (csproj exists)"
      - "dotnet build"
      - "dotnet run"
        evidence:
          - "EmployeeManagementAPI/Properties/launchSettings.json:12-21 (profiles present)"
          - "EmployeeManagementAPI/Properties/launchSettings.json:17 (applicationUrl: \"http://localhost:5037\")"
    default_listen_urls:
      http: "http://localhost:5037"
      https: "https://localhost:7029"
      evidence:
        - "EmployeeManagementAPI/Properties/launchSettings.json:17"
        - "EmployeeManagementAPI/Properties/launchSettings.json:27"
    https_redirection:
      present: true
      evidence:
        - "EmployeeManagementAPI/Program.cs:36 (app.UseHttpsRedirection())"

  frontend:
    working_directory: "Employee-Management-System-main/employee-management-system (or employee-management-system/)"
    steps:
      - "npm install"
        evidence:
          - "employee-management-system/package.json:1-44 (package.json exists)"
      - "npm start"
        evidence:
          - "employee-management-system/package.json:5 (start -> 'ng serve')"
    dev_server_default_port: 4200
      evidence:
        - "employee-management-system/package.json:5 (ng serve default)"
        - "implicitly: angular dev server defaults to 4200; angular.json also shows serve target"

api:
  base_route:
    controller: "EmployeesController"
    route_attribute: "[Route(\"api/[controller]\")]"
    evidence:
      - "EmployeeManagementAPI/Controllers/EmployeesController.cs:9-10"
    runtime_route: "/api/employees"
      rationale: "[controller] token + controller class name -> 'employees' (lowercase). Evidence: class name and route attribute."
      evidence:
        - "EmployeeManagementAPI/Controllers/EmployeesController.cs:9-10"
        - "employee-management-system/src/app/services/employee.service.ts:10 (client uses /api/employees)"

  endpoints:
    - method: POST
      route: "/api/employees"
      purpose: "Create new employee (uses MediatR command CreateEmployeeCommand)"
      evidence:
        - "EmployeeManagementAPI/Controllers/EmployeesController.cs:22-27"
        - "EmployeeManagementAPI/CQRS/Commands/CreateEmployeeCommand.cs:6-14"
        - "EmployeeManagementAPI/Controllers/EmployeesController.cs:25-26 (mediator.Send(...))"
    - method: GET
      route: "/api/employees"
      purpose: "Get all employees"
      evidence:
        - "EmployeeManagementAPI/Controllers/EmployeesController.cs:29-34"
        - "EmployeeManagementAPI/Controllers/EmployeesController.cs:31-33 (uses _context.Employees.ToListAsync())"
    - method: GET
      route: "/api/employees/{id}"
      purpose: "Get employee by id"
      evidence:
        - "EmployeeManagementAPI/Controllers/EmployeesController.cs:36-45"
        - "EmployeeManagementAPI/Controllers/EmployeesController.cs:39-40 (FindAsync(id))"
    - method: PUT
      route: "/api/employees/{id}"
      purpose: "Update employee (controller accepts CreateEmployeeCommand as request body)"
      evidence:
        - "EmployeeManagementAPI/Controllers/EmployeesController.cs:47-65"
        - "EmployeeManagementAPI/Controllers/EmployeesController.cs:48 (signature: Update(Guid id, CreateEmployeeCommand command))"
    - method: DELETE
      route: "/api/employees/{id}"
      purpose: "Delete employee"
      evidence:
        - "EmployeeManagementAPI/Controllers/EmployeesController.cs:67-79"
        - "EmployeeManagementAPI/Controllers/EmployeesController.cs:71-76 (deletes via _context)"

models:
  Employee:
    path: "EmployeeManagementAPI/Models/Employee.cs"
    properties:
      - "Id: Guid"
      - "FirstName: string"
      - "LastName: string"
      - "Email: string"
      - "PhoneNumber: string"
      - "Position: string"
      - "Department: string"
    evidence:
      - "EmployeeManagementAPI/Models/Employee.cs:7-13"

cqrs_mediatR:
  registration:
    line: "EmployeeManagementAPI/Program.cs:15 (builder.Services.AddMediatR(...))"
  usage_scope:
    - "Create: implemented via MediatR (CreateEmployeeCommand -> CreateEmployeeHandler). Evidence: EmployeeManagementAPI/CQRS/Commands/CreateEmployeeCommand.cs:6-14; EmployeeManagementAPI/CQRS/Handlers/CreateEmployeeHandler.cs:9-36"
    - "Other endpoints (GET/PUT/DELETE): operate directly on EmployeeDbContext. Evidence: EmployeeManagementAPI/Controllers/EmployeesController.cs:31-33,50-63,71-76"
  note: "Partial/hybrid CQRS: only Create uses MediatR."

db_and_migrations:
  provider: "SQLite"
  connection_string:
    source: "EmployeeManagementAPI/appsettings.json:1-6"
    value: "Data Source=employees.db"
    evidence:
      - "EmployeeManagementAPI/appsettings.json:1-6"
      - "EmployeeManagementAPI/Program.cs:12-13 (UseSqlite(builder.Configuration.GetConnectionString(\"DefaultConnection\")))"
  db_file:
    path: "EmployeeManagementAPI/employees.db"
    status: "present_in_repo (committed)"
    evidence:
      - "EmployeeManagementAPI/employees.db (glob/list result)"
  migrations:
    list:
      - filename: "20240707083400_InitialCreate.cs"
        evidence: "EmployeeManagementAPI/Migrations/20240707083400_InitialCreate.cs:1-1"
      - filename: "EmployeeDbContextModelSnapshot.cs"
        evidence: "EmployeeManagementAPI/Migrations/EmployeeDbContextModelSnapshot.cs:12-14"
    auto_apply_on_startup: false
    evidence:
      - "Program.cs: no Database.Migrate() found (searched Program.cs, Data/*, Migrations/*)"

cors:
  policy_name: "AllowAngularApp"
  allowed_origins:
    - "http://localhost:4200"
  evidence:
    - "EmployeeManagementAPI/Program.cs:17-25 (AddCors with WithOrigins(\"http://localhost:4200\"))"
    - "EmployeeManagementAPI/Program.cs:37 (app.UseCors(\"AllowAngularApp\"))"

swagger:
  enabled: true
  environment: "Development-only"
  evidence:
    - "EmployeeManagementAPI/Program.cs:30-34 (if (app.Environment.IsDevelopment()) { app.UseSwagger(); app.UseSwaggerUI(); })"
    - "EmployeeManagementAPI/EmployeeManagementAPI.csproj:19-21 (Swashbuckle package references)"

auth_and_security:
  authentication_registered: false
    evidence_search: "EmployeeManagementAPI/Program.cs, EmployeeManagementAPI/Controllers/*"
    details: "UseAuthorization() present but no UseAuthentication() or AddAuthentication() registration and no [Authorize] attributes."
    evidence:
      - "EmployeeManagementAPI/Program.cs:36-38 (app.UseCors(...); app.UseAuthorization())"
      - "EmployeeManagementAPI/Program.cs:27-40 (no UseAuthentication())"
  authorization_attributes: none_found
    evidence: "EmployeeManagementAPI/Controllers/* (no [Authorize] occurrences)"

logging:
  framework: "Default ASP.NET Core logging (no Serilog detected)"
  evidence:
    - "EmployeeManagementAPI/appsettings.json:6-9 (Logging -> LogLevel)"
    - "EmployeeManagementAPI/Program.cs: no custom Serilog or logging registrations found"

frontend_details:
  framework: "Angular 18"
  scripts_and_deps:
    evidence:
      - "employee-management-system/package.json:5-11 (scripts: start -> ng serve; test -> ng test; serve:ssr -> node ...server.mjs)"
      - "employee-management-system/package.json:14-43 (dependencies list)"
  routes:
    file: "employee-management-system/src/app/app.routes.ts"
    routes_list:
      - path: ""
        redirectTo: "/employees"
      - path: "employees"
        component: "EmployeeListComponent"
      - path: "employees/new"
        component: "EmployeeFormComponent"
      - path: "employees/:id"
        component: "EmployeeDetailComponent"
      - path: "employees/:id/edit"
        component: "EmployeeFormComponent"
    evidence:
      - "employee-management-system/src/app/app.routes.ts:6-12"
  api_service:
    file: "employee-management-system/src/app/services/employee.service.ts"
    base_url: "http://localhost:5037/api/employees"
    evidence:
      - "employee-management-system/src/app/services/employee.service.ts:10 (private apiUrl = 'http://localhost:5037/api/employees')"
  components_behaviour:
    EmployeeListComponent:
      fetch_on_init: true
      delete_confirmation: true
      evidence:
        - "employee-management-system/src/app/components/employee-list/employee-list.component.ts:20-28"
        - "employee-management-system/src/app/components/employee-list/employee-list.component.ts:30-36"
    EmployeeFormComponent:
      create_and_update: true
      navigate_on_success: true
      evidence:
        - "employee-management-system/src/app/components/employee-form/employee-form.component.ts:44-53"
    EmployeeDetailComponent:
      fetch_by_route_id: true
      evidence:
        - "employee-management-system/src/app/components/employee-detail/employee-detail.component.ts:23-29"

frontend_ssr:
  present: true
  evidence:
    - "employee-management-system/angular.json:33-37 (ssr/server entry configured)"
    - "employee-management-system/server.ts:1-6 (SSR bootstrap)"
    - "employee-management-system/package.json:11 (serve:ssr script)"

tests:
  frontend:
    has_unit_tests: true
    runner: "Karma/Jasmine"
    evidence:
      - "employee-management-system/src/app/components/employee-list/employee-list.component.spec.ts:1-10"
      - "employee-management-system/package.json:9 (test script)"
  backend:
    has_tests: false
    evidence_search: "root and EmployeeManagementAPI"
    evidence: "No .csproj with <IsTestProject> found; no Tests folder detected"

docker_and_ci:
  present: false
  evidence_search: "repo root and subdirs for Dockerfile/docker-compose/.github/workflows"
  evidence: "none found (scan result)"

security_and_secrets:
  plaintext_secrets_found: false
  evidence:
    - "EmployeeManagementAPI/appsettings.json:1-12 (connection string is 'Data Source=employees.db', no passwords/keys)"
  recommended_action:
    - "Remove employees.db from repo history and add to .gitignore"
    - "Externalize secrets via environment variables or secret store"

files_present_of_interest:
  - "EmployeeManagementAPI/Program.cs: lines 7-40 (service registrations + middleware pipeline)"
  - "EmployeeManagementAPI/Controllers/EmployeesController.cs: lines 9-86 (all endpoints)"
  - "EmployeeManagementAPI/Data/EmployeeDbContext.cs: lines 7-11"
  - "EmployeeManagementAPI/Models/Employee.cs: lines 7-13"
  - "EmployeeManagementAPI/Migrations/* (InitialCreate and snapshot)"
  - "EmployeeManagementAPI/employees.db (committed)"
  - "employee-management-system/src/app/services/employee.service.ts: line 10 (hardcoded base URL)"
  - "employee-management-system/src/app/app.routes.ts: lines 6-12"
  - "employee-management-system/server.ts (SSR entry)"
  - "employee-management-system/package.json (scripts and deps)"

missing_or_not_detected:
  - "No authentication registration (AddAuthentication / UseAuthentication) found. Evidence: Program.cs scan."
  - "No global exception middleware / error handling detected."
  - "No backend unit tests detected."
  - "No Dockerfile / CI workflows detected."

recommended_minimal_changes (for repo hygiene & clarity):
  - "git rm --cached EmployeeManagementAPI/employees.db; add employees.db to .gitignore. Evidence: employees.db present in repo."
  - "Add environment-based API base URL for Angular (use src/environments/environment.ts). Evidence: apiUrl hardcoded in service."
  - "Document how to apply migrations (dotnet ef database update) and/or call Database.Migrate() on startup in dev."
  - "Add authentication (JWT) registration and protect write endpoints."

final_notes_for_consuming_AIs:
  - "All claims above are evidence-cited (file:path:line-range). Use those citations for verification."
  - "If a consuming AI needs absolute file paths on disk, prefix the relative path with the repo root folder name if present (e.g., Employee-Management-System-main/EmployeeManagementAPI/...)."
  - "This document is authoritative for what exists in the repo as of the last scan; re-scan if repo changes."

