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

**Reference:** See [KATA_DESCRIPTION.md](KATA_DESCRIPTION.md) and [README.md](README.md) for reference information.

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

For domain-specific errors, consider creating custom exception classes.

## Testing Conventions

**Important**: This project exclusively uses TDD testing. All testing should go through the TDD workflow with corresponding `test_*.py` files.

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
