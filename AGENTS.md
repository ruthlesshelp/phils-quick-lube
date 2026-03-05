# GitHub Copilot Instructions for this project

## Project Overview

This is the **Phil's Quick Lube Management System (PQLMS)** - a comprehensive Python-based software application for managing operations at Phil's Quick Lube, a preventive vehicle maintenance service business. The system digitizes and streamlines operations, eliminates waste, and improves customer experience across Sales, Service, and Administration departments.

**Business Context:** Phil's Quick Lube provides preventive maintenance services including oil changes, battery services, transmission services, cooling system services, and Texas state vehicle inspections. The business currently experiences significant operational inefficiencies that this system aims to eliminate.

**Key Goals:**
- Eliminate waste identified in current manual processes (excessive motion, redundant data entry, manual handoffs)
- Reduce turnaround time for services
- Improve customer experience (especially for returning customers)
- Provide real-time visibility across all departments
- Automate inventory management and supplier ordering
- Enable data-driven business insights

**Reference:** See [PRD.md](PRD.md) and [CLARIFYING_QUESTIONS.md](CLARIFYING_QUESTIONS.md) for complete functional and non-functional requirements and strategic clarifications.

This project uses Test-Driven Development (TDD) with pytest for testing all business logic and features.

## Technology Stack & Strategic Decisions

### Backend Architecture
- **Backend Type:** Python-only solution (CLI/script-based for Phase 1)
- **Database:** SQLite with SQLAlchemy ORM
- **Schema Approach:** Code-first (schema evolved through TDD, not schema-first)
- **Hosting:** Google Cloud Platform (GCP) cloud-based SaaS
- **Real-Time:** Near-real-time polling every 5-10 seconds (WebSockets deferred to later phases)

### Integration Strategy (Phase 1 & Beyond)
- **Payment Processing:** Mock gateway bridge (never test real payments in development)
  - Use test cards for payment simulation
  - Gateway bridge handles mocking for development, real processor in production
- **Email/SMS:** Mocked through testing interface in Phase 1
  - Real integrations deferred to later phases
- **Accounting Software:** Mocked for testing, real integration in Phase 3+
- **VIN Decoder:** Optional NHTSA vPIC API for vehicle data lookup (reduces data entry)

### Testing Strategy
- **Test Fixtures:** Pre-populated for fast feedback (not dynamically generated)
- **Payment Testing:** Always mocked; never test real payments in development
- **Test Cards:** Use test card numbers for payment simulation
- **Boundary Conditions:** Choose sensible defaults; behave fault-tolerant
  - Service with $0 cost is acceptable complimentary service
  - Invoice with no items is acceptable as statement
  - Customer with null phone displays as (000) 000-0000
- **Inventory Validation:** Prevent service order creation if supplies insufficient (no negative inventory)

### Development Environment
- **Python Version:** 3.14.x (latest stable release)
- **Virtual Environment:** venv (built-in Python module)
  - Create with: `python3 -m venv .venv`
  - Activate with: `source .venv/bin/activate` (macOS/Linux) or `.venv\Scripts\activate` (Windows)
- **Testing Framework:** pytest with pytest-cov for 100% code coverage
- **Database:** SQLite with SQLAlchemy ORM
  - Database file location: `data/pqlms.db` (in .gitignore)
  - Schema migrations: Alembic for version-controlled schema management
- **Code Quality Tools:** Consolidated in pyproject.toml:
  - Linting: flake8, pylint
  - Formatting: black, isort
  - Static type checking: mypy
  - Security scanning: bandit
- **Local Testing:** Local pytest runs sufficient (no CI/CD required initially for Phase 1)
- **Staffing:** One AI-assisted developer with empirical process and plan-do-check-act cycles

## Project Structure

The project follows a clean TDD architecture with strict separation of concerns:
- `src/` - Contains the main business logic organized by functional modules
  - Customer management (customers, vehicles, service history)
  - Service order management (order creation, routing, state tracking)
  - Service execution (bay management, checklists, quality checks)
  - Invoicing and payment processing
  - Inventory management (tracking, automated reordering, alerts)
  - Supplier management (database, purchase orders, invoicing)
  - Reporting and analytics (operational, financial, performance metrics)
  - User management and security (authentication, authorization, audit logging)
- `tests/test_*.py` - TDD test files that test the functionality of the code in `src/`
- `.gitignore` - Comprehensive ignore patterns for Python, IDE, and OS-specific files
- `pyproject.toml` - Modern Python project configuration and dependencies
- `PRD.md` - Complete Product Requirements Document with all functional and non-functional requirements
- `book/` - Reference documentation from "The Basics of Process Mapping, 2nd Edition"

**Note:** This project uses traditional TDD testing pattern with unit test files (`test_*.py`).

## Business Domain Model

### Core Entities

**Customer Management:**
- Customer (ID, name, contact info, preferences)
- Vehicle (ID, license plate, make/model/year, VIN, mileage, associated customer)
- Service History (date, service type, mileage, parts used, cost)

**Service Operations:**
- Service Order (order number, customer, vehicle, services requested, state, timestamps)
- Service Bay (Bay 1: oil changes, Bay 2: battery/fluid work, Bay 3: inspections)
- Service Checklist (service-type specific tasks)
- Quality Check (inspector, timestamp, pass/fail, rework reasons)

**Inventory & Suppliers:**
- Inventory Item (SKU, name, category, quantity, min/max thresholds, reorder point, cost)
- Supplier (ID, company name, contact info, payment terms, lead times)
- Purchase Order (PO number, supplier, line items, states, receipt tracking)
- Supplier Invoice (invoice number, PO matching, payment tracking)

**Financial:**
- Invoice (customer invoice for services, line items, taxes, total)
- Payment (amount, method, timestamp, associated invoice)

**Users & Security:**
- User (ID, name, role, department, credentials)
- Roles (Administrator, Sales Staff, Service Technician, Service Manager, Administration Staff, Owner/Manager)
- Audit Log (user, timestamp, action, old/new values)

### Key Business Rules

1. **Service Bay Routing:** Oil changes → Bay 1, Battery/fluid work → Bay 2, Inspections → Bay 3
   - Multiple services on one order route sequentially (service execution workflow determines order)
   - Service manager can manually override sequence for operational flexibility
   - Manual bay reassignment allowed after order creation

2. **Service Order States:** Created → Approved or Created → In Progress (can skip Approved under certain scenarios)
   - → Quality Check (Phase 2+; may not be in MVP) → Ready for Pickup → Completed
   - Quality checks cannot be skipped (once implemented in Phase 2)
   - Failed QC transitions back to In Progress with rework indicator
   - Resource deduction upon Approved state (implicit if skipping Approved)
  - Cancellation restores inventory, requires reason code, and orders cannot be reopened or deleted

3. **Returning Customers:** System auto-populates by phone number lookup
  - Returning customer = any customer with an existing record in the database
   - Auto-populate customer name, email, address with user confirmation
  - If multiple customers match a lookup value, show all matches for explicit selection
   - **Display dropdown of all customer vehicles; require explicit selection** (not auto-select even if single vehicle)
   - Optional NHTSA VIN decoder (vPIC) for vehicle data lookup (Phase 1D2+)
   - User must confirm all auto-populated data before proceeding

4. **Inventory Management - CRITICAL:** Supplies deducted when service is APPROVED (not at completion)
   - Low stock alerts BLOCK service order approval (prevent over-commitment)
   - Service manager CAN override low stock blocks with explicit confirmation
   - Tracked per service type templates
   - Design for single location now, build location awareness from start
   - **If order moves Created → In Progress (skips Approved), "Approved" state is implicit and inventory IS deducted**
   - Multi-service orders auto-split at approval if only some services have sufficient inventory
  - Services without inventory templates can proceed; deduction skipped
  - Inventory adjustments require reason codes and are not reversible

5. **Quality Checks Required (Phase 2+):** All services must pass quality check before completion
   - Service technicians can complete checklist, service manager performs QC action
   - **Service manager CAN QC their own work**
   - Failed checks transition back to In Progress with rework indicator
   - **If service fails quality check twice (any issue on same service order), service manager personally completes rework steps**
  - Rework resets checklist; all items must be completed before re-check
   - Quality metrics tracked for every service
   - **Phase 1 MVP may not include quality checks (defer to Phase 2)**
   - Phase 2 focus: Quality checks ONLY for oil change service (not all services)

6. **Payment Methods:** Support cash, credit/debit cards, checks, digital wallets
   - **Payment BLOCKED until entire multi-service order complete**
   - Only service managers can approve invoice adjustments (with audit trail)
   - Zero-cost complimentary services do NOT require payment processing
   - Single payment attempt; declined payments must retry with different method immediately
   - Service cannot be released without successful payment
  - Auto-split services generate separate invoices when each is Ready for Pickup

7. **Pricing:** Fixed per service type (no variable pricing, no bundles, no customer-specific discounts)
   - Fixed service pricing per service type
   - Service managers can apply flat dollar discounts or line-item adjustments only
   - No percentage-based adjustments allowed
   - Adjustments applied to subtotal before tax recalculation

8. **No Repairs:** System is for preventive maintenance only, NOT vehicle repairs

9. **Technician Assignment:** Assigned automatically by bay to first available technician
  - Service manager can reassign at any time; reassignments are audited

## Tools and Dependencies

This project uses pytest for testing and consolidates all tool configurations in `pyproject.toml`. 

**Core Testing Dependencies:**
- `pytest` - Testing framework
- `pytest-cov` - Code coverage plugin for pytest to measure test coverage
- `sqlalchemy` - ORM for database access with SQLite
- `alembic` - Database schema migrations and version control

**Code Quality & Development Dependencies (configure in pyproject.toml):**
- `black` - Code formatter (enforces consistent style)
- `flake8` - Linter (identifies style/error violations)
- `pylint` - Advanced linter (catches more issues)
- `mypy` - Static type checker (validates type hints)
- `isort` - Import sorter (organizes imports)
- `bandit` - Security scanner (identifies security issues)

**Tool Configuration Location:**
- ALL tool configurations consolidated in `pyproject.toml`
- Eliminates need for separate .flake8, setup.cfg, .mypy.ini files
- Example configuration:
  ```toml
  [tool.black]
  line-length = 88
  
  [tool.isort]
  profile = "black"
  
  [tool.mypy]
  python_version = "3.14"
  
  [tool.pytest.ini_options]
  testpaths = ["tests"]
  addopts = "--cov=src --cov-report=html --cov-report=term-missing"
  ```

**Python Version:** 3.14.x for modern language features

**Future Dependencies (as features are implemented):**
- Payment gateway bridge (mocked initially for testing)
- Email/SMS notification services (mocked initially)
- Accounting software SDKs (deferred to Phase 3)
- Web framework (Flask, FastAPI, or Django - deferred to Phase 2+)
- Authentication libraries (JWT, OAuth)

## Code Coverage

The project should eventually achieve 100% code coverage using `pytest-cov`:
- All business logic in the `src/` directory is fully tested
- Coverage includes both statement and branch coverage
- HTML coverage reports are generated in the `htmlcov/` directory
- XML coverage reports are generated as `coverage.xml`
- Terminal coverage reports show complete coverage status

To run tests with coverage:
```bash
pytest tests/ -v --cov=src --cov-report=html --cov-report=term-missing
```

**Coverage Analysis:**
- HTML report location: Open `htmlcov/index.html` in browser for detailed line-by-line coverage
- Terminal report shows immediately which lines lack coverage
- Target: 100% coverage on all src/ code (test files excluded)

**Coverage Tools:**
- run `python analyze_coverage.py` for detailed coverage analysis (if analysis script created)
- View HTML report: `htmlcov/index.html`

**MVP Database Approach:**
- **MVP Database:** Uses `:memory:` SQLite (in-memory, not file-based)
  - Test fixtures reconstructed fresh for each test class
  - Delivers clean state for each test run
  - Deliverable 2 transitions to persistent `data/pqlms.db` file-based SQLite

## TDD Testing Patterns

When working with TDD tests, follow these patterns:

1. Write a failing test that defines an incremental functional requirement.
2. Implement the minimum code required to make the test pass.
3. Refactor the code while keeping the tests green.
4. Stop to allow for code review and feedback.

Use test files to organize tests logically and avoid clutter.

Organize test files by functional module matching the PRD structure:
- `test_customer_management.py` - Customer and vehicle entity tests
- `test_service_order.py` - Service order creation, routing, state management
- `test_service_execution.py` - Bay management, checklists, quality checks
- `test_invoicing_payment.py` - Invoice generation, payment processing
- `test_inventory.py` - Inventory tracking, alerts, automated reordering
- `test_supplier.py` - Supplier database, purchase orders, invoice matching
- `test_reporting.py` - Report generation, analytics, metrics
- `test_user_security.py` - Authentication, authorization, audit logging

Use Arrange, Act, Assert comments within the test methods to clarify the test structure.

Keep the test methods short and focused on a single behavior.

Name the test methods descriptively to indicate the behavior being tested, for example:
- `test_create_service_order_for_returning_customer_expect_auto_populated_data`
- `test_route_oil_change_service_expect_bay_1_assignment`
- `test_inventory_below_minimum_threshold_expect_low_stock_alert`
- `test_service_order_without_quality_check_expect_cannot_mark_complete`

## Exception Handling

In the code under test, use specific exceptions to indicate different error conditions:

Use `NotImplementedError` for unimplemented features or functionality.

Use `ValueError` built-in exception when an operation or function receives an argument that has the correct type but is an inappropriate or invalid value.

Use `RuntimeError` for business rule violations or calculations that cannot be performed with the given parameters (e.g., attempting to assign inspection work to Bay 1, or marking service complete without quality check).

For domain-specific errors, consider creating custom exception classes:
- `ServiceOrderStateError` - Invalid state transitions
- `InsufficientInventoryError` - Not enough supplies for service
- `UnauthorizedAccessError` - User lacks permission for operation
- `PaymentProcessingError` - Payment gateway failures

## Testing Conventions

**Important**: This project exclusively uses TDD testing. All testing should go through the TDD workflow with corresponding `test_*.py` files.

## Implementation Priority

Follow the phased implementation approach from the PRD:

**Phase 1 (Core Operations - Two Deliverables):**

*Deliverable 1: Minimum Marketable Product (MVP)*
- Simplified core functionality demonstrating key system aspects
- Foundation for Deliverable 2 iteration

*Deliverable 2: Full Phase 1 Feature Set*
- Customer/vehicle management with returning customer auto-population
- Service order creation/tracking with state management
- Basic invoicing/payment (payment gateway mocked)
- User management and security with role-based access control
- Walk-in customer model (appointment scheduling deferred to Phase 5)
- Inventory tracking (deduction on approval, low stock alerts)

**Phase 2 (Service Execution):** Digital checklists (focus on oil change), bay management, quality checks (service manager oversight with rework escalation), real-time status

**Phase 3 (Back Office):** Inventory management, supplier management, purchase orders, alerts, accounting software integration

**Phase 4 (Analytics):** Reporting dashboard, performance analytics, customer analytics, forecasting

**Phase 5 (Advanced Features):** Appointment scheduling, customer portal, automated reordering, integrations

When implementing features, always refer to the requirement IDs in [PRD.md](PRD.md) (e.g., REQ-CM-001, REQ-SO-007, REQ-IM-003).

## Code Organization

Keep business logic separate from test code.

Organize code by functional module (customer, service_order, inventory, etc.) matching the PRD module structure.

Use a layered architecture:
- **Domain Layer:** Core business entities and rules
- **Service Layer:** Business logic and workflows
- **Repository Layer:** Data access abstractions
- **API Layer:** External interfaces (REST API, CLI, etc.)

## Development Process & Tools

### Git Workflow

**Decision: Trunk-Based Development**
- Single main branch (no feature branches)
- Suitable for single AI-assisted developer
- More frequent, smaller commits
- Fewer merge conflicts

### Commit Message Format

**Decision: Conventional Commits**
- Format: `<type>: <description>`
- Types: feat, fix, test, docs, refactor, chore, style
- Examples:
  - `feat: add customer phone number lookup`
  - `test: add service order routing tests for multiple services`
  - `fix: correct inventory deduction timing to approval state`
  - `docs: update AGENTS.md with Python 3.14 requirements`

### Code Quality & Review Process

**Decision: Automated Quality Checks**
- Use linting, formatting, static type checking, and security scanning
- Consolidate ALL tool configurations into `pyproject.toml`
- Tools included:
  - **black:** Code formatting (enforces consistent style)
  - **flake8:** Linting (catches PEP 8 violations)
  - **pylint:** Advanced linting (catches more complex issues)
  - **mypy:** Static type checking (validates type hints)
  - **isort:** Import organization (consistent import ordering)
  - **bandit:** Security scanning (identifies security vulnerabilities)

**Decision: AI Pair Programming Review**
- Include AI pair programming review in workflow
- Focus on code quality and business logic correctness
- Self-review by developer before commit
- Example: Run linters/formatters → Review output → Make corrections → Commit with conventional message

## Strategic Clarifications & Key Decisions

**Important:** Review [CLARIFYING_QUESTIONS.md](CLARIFYING_QUESTIONS.md) for comprehensive strategic clarifications from the planning phase, including:

- **MVP Scope:** Single happy path (returning customer → order → bay → complete → invoice → payment)
- **MVP Database:** In-memory SQLite (transitions to persistent in Deliverable 2)
- **MVP Users:** Single admin user (full RBAC in Deliverable 2)
- **Inventory Deduction Timing:** Deduction occurs when service is APPROVED (not at completion) to maintain accuracy
- **Service Order Flexibility:** Orders can skip "Approved" state to move Created → In Progress under certain scenarios
- **Multiple Services:** Route to lowest bay number first (e.g., Bay 1 before Bay 2)
- **Returning Customer Auto-Population:** Triggered by phone/name/email lookup; returning customer = any existing record; show all matches for explicit selection
- **Customer Data Confirmation:** Explicit confirmation required for any changes to returning customer data
- **Vehicle Selection:** Display dropdown of ALL vehicles; REQUIRE explicit selection (not auto-select)
- **VIN Decoder:** Deferred to Phase 1 Deliverable 2
- **Quality Check Rework:** Service manager takeover on ANY second failure (not limited to same issue); SM personally does rework
- **Quality Checks Phase 2:** Focus on oil change service only (Phase 2); other services added in Phase 3+
- **Payment Processing:** All testing uses mock gateway bridge with test cards (Square selected); real integration for production only
- **Mock Payment Behavior:** Phase 1 mock processor always succeeds for valid test cards
- **Fixed Service Pricing:** Oil change $79.99, Battery $149.99, Transmission $249.99, Cooling $199.99, Inspection $25.50
- **Tax Rate:** 3% Texas state sales tax; no partial payments; no voids/refunds after payment
- **Inventory Items:** Phase 1 focus on oil change (5 qts synthetic oil, 1 filter, 1 sticker)
- **Auto-Split Orders:** Multi-service orders split at approval if only some services have sufficient inventory
- **Invoice Splits:** Separate invoices generated per service when Ready for Pickup; payment blocked until entire order complete
- **Inventory Adjustments:** Service Manager and Administration Staff can adjust (add stock, correct counts) with reason codes; not reversible
- **Missing Inventory Template:** Services without a template can proceed; deduction skipped
- **Role Restrictions:** Service Manager CAN do tech work; Tech CANNOT see other tech's orders; no multiple roles per user
- **Session Timeout:** 15 minutes of inactivity
- **Password Requirements:** Min 8 chars, 1 uppercase, 1 lowercase, 1 number
- **Audit Logging:** Data modifications plus login/logout events; views not logged
- **Audit Log Retention:** 7 years (same as transaction data)
- **Business Hours:** 6am-6pm, Monday-Saturday
- **Service Volumes:** ~10 oil changes, 6 battery, 9 inspections daily
- **Staff Structure:** 3 sales, 5 techs, 2 managers, 3 admin, 1 owner = 14 total
- **Customer Base:** ~500 existing customers
- **Service Durations:** Oil 30min, Battery 30min, Transmission 120min, Cooling 45min, Inspection 30min
- **Test Customers:** 7 returning customers with edge cases (no phone, no email)
- **Test Cards:** Visa 4242424242424242, Mastercard 5555555555554444, Discover 6011111111111117
- **Email Notifications:** Order created, service complete, payment receipt, 90-day reminder
- **Email Mocking:** Mock email processor captures sent messages in memory for tests
- **Data Validation:** US phone numbers (raw digits, optional +1), basic email (@), mileage 0-999999 (can decrease), flexible license plate, optional VIN
- **Git:** Trunk-based development with conventional commits
- **Phase 1 Two-Deliverable Approach:** MVP followed by full feature set delivery
- **Walk-In Only:** Appointment scheduling deferred to Phase 5
- **Staffing:** One AI-assisted developer using plan-do-check-act cycles with flexible timeline
- **License Plate Uniqueness:** License plates may NOT be unique; License Plate + State Registration enforces uniqueness
- **Technician Work Visibility:** Can see own assigned orders and current bay status ONLY; cannot see other techs' orders
- **Technician Assignment:** First available technician in bay; service manager can reassign at any time with audit trail
- **Service Order State Transitions:** Technicians CAN move through states; managers approval NOT required for state transitions
- **Business Hours Enforcement:** System available 24/7; service orders can only be CREATED/APPROVED during business hours
- **Service Manager Inventory Override:** CAN override low-stock blocks with explicit confirmation and audit logging
- **Soft Deletes:** Records use deleted_at timestamps; default queries exclude deleted records
- **Reporting Scope:** No reporting in Phase 1; reporting deferred to Phase 4

## Project Maintenance

The project includes a comprehensive `.gitignore` file that excludes:
- Python bytecode and cache files (`__pycache__/`, `*.pyc`)
- Virtual environments (`.venv/`, `venv/`)
- IDE-specific files (VS Code, PyCharm)
- OS-specific files (macOS `.DS_Store`, Windows `Thumbs.db`)
- Testing artifacts (`.pytest_cache/`, coverage reports)

When adding new functionality, ensure:
1. TDD tests are created as `tests/test_*.py` files
2. Requirements from PRD.md are referenced in code comments
3. Business rules are validated with appropriate tests
4. Empty or unused test files are removed to keep the project clean
5. All new code paths are tested to maintain 100% coverage

## Success Metrics from PRD

Keep these target metrics in mind when implementing features:
- Reduce service order creation time by 60% for returning customers
- Reduce average turnaround time by 15%
- Eliminate manual job packet handoffs
- Achieve 95% on-time service completion
- Reduce inventory stockouts to zero
- Achieve 85%+ bay utilization during business hours

These metrics should guide implementation decisions and optimization efforts.
