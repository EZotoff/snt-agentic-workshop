# Project Context

## Purpose

**Brief description of your project's goals and objectives.**

Example: "This is a web application for tracking customer orders with real-time inventory management and analytics dashboards."

### Core Goals
- Goal 1: Describe primary objective
- Goal 2: Describe secondary objective
- Goal 3: Describe tertiary objective

## Tech Stack

### Core Technologies
- **Language**: Python 3.10+ / Node.js 18+ / etc.
- **Framework**: Django / React / FastAPI / etc.
- **Database**: PostgreSQL / MongoDB / etc.

### Key Libraries
List your main dependencies with brief purpose:
- **library-name**: What it's used for
- **another-library**: What it's used for
- **testing-framework**: pytest / jest / etc.

Example:
```
- **pandas**: Data manipulation and analysis
- **fastapi**: REST API framework
- **sqlalchemy**: Database ORM
```

## Project Conventions

### Code Style

**Language Standards:**
- Language version requirements (e.g., "Python 3.10+", "ES2020+")
- Import style (e.g., "Use absolute imports", "Prefer named exports")
- Type hints / TypeScript usage
- Documentation format (docstrings, JSDoc, etc.)

**Naming Conventions:**
- Files: `snake_case.py`, `kebab-case.ts`, `PascalCase.tsx`
- Functions/methods: `snake_case`, `camelCase`
- Classes: `PascalCase`
- Constants: `UPPER_CASE`

**File Organization:**
```
project/
├── src/           # Source code
├── tests/         # Test files
├── docs/          # Documentation
└── config/        # Configuration files
```

### Architecture Patterns

**Design Patterns:**
Describe your architectural approach. Examples:
- MVC / MVVM / Clean Architecture
- Repository pattern for data access
- Service layer for business logic
- API client with retry logic

**Code Organization:**
- How modules are structured
- Dependency injection approach
- Error handling patterns
- Async/await conventions

Example:
```
- Use dependency injection for services
- Implement repository pattern for database access
- Handle errors with custom exception classes
- Use async/await for I/O operations
```

### Testing Strategy

**Test Framework:** pytest / jest / mocha / etc.

**Testing Approach:**
- Unit tests for business logic
- Integration tests for API endpoints
- E2E tests for critical user flows
- Test coverage target: 80%+

**Running Tests:**
```bash
# Example commands
npm test                  # Run all tests
pytest tests/ -v         # Verbose test output
npm run test:coverage    # Generate coverage report
```

### Git Workflow

**Repository:** https://github.com/your-org/your-repo
- **Owner:** your-username
- **Main branch:** main / master / develop

**Commit Conventions:**
- Use conventional commits: `feat:`, `fix:`, `docs:`, `refactor:`
- Reference issue numbers: `feat: add user auth (#123)`
- Keep commits atomic and focused

**Branching Strategy:**
- `main` - Production-ready code
- `develop` - Integration branch
- `feature/*` - Feature branches
- `hotfix/*` - Emergency fixes

## Domain Context

**Domain-specific terminology and concepts that assistants should understand.**

Example for e-commerce:
- **Order States**: pending, confirmed, shipped, delivered, cancelled
- **Inventory**: real-time stock tracking with reservation system
- **Pricing**: base price + dynamic discounts + tax calculation

Example for healthcare:
- **Patient Records**: HIPAA-compliant data handling
- **Appointment Scheduling**: timezone-aware booking system
- **Billing Codes**: ICD-10 / CPT code validation

### Key Concepts
- **Concept 1**: Brief explanation
- **Concept 2**: Brief explanation
- **Concept 3**: Brief explanation

### File Formats
List data formats used in your project:
- CSV: User data exports
- JSON: API request/response format
- XML: Legacy system integration
- Parquet: Analytics data storage

## Important Constraints

### Performance Considerations
- API response time: < 200ms for 95th percentile
- Database query optimization: use indexes on frequently queried fields
- Caching strategy: Redis for session data, CDN for static assets
- Rate limiting: 100 requests/minute per user

### API Configuration
List API-specific requirements:
- Authentication: JWT tokens with 1-hour expiration
- Rate limits: Respect third-party API limits (e.g., 1000 requests/day)
- Retry logic: Exponential backoff for failed requests
- Timeout settings: 30s for external API calls

### Known Issues
Document current limitations and workarounds:
- **Issue 1**: Description of problem and temporary workaround
- **Issue 2**: Known bug with planned fix date
- **Issue 3**: Performance bottleneck under investigation

Example:
```
- **Database connection pool**: Under high load (>1000 concurrent users), 
  connection pool exhaustion occurs. Workaround: increase pool size to 50.
- **Safari date parsing**: Date inputs fail on Safari < 14. 
  Workaround: use custom date picker component.
```

### Manual Interaction Required
List steps that require human intervention:
- Deployment approval for production releases
- Database migration review before applying
- Manual testing of payment flows
- User acceptance testing for UI changes

### Data Privacy & Security
Security requirements and sensitive data handling:
- Never log API keys, passwords, or tokens
- Encrypt PII (Personally Identifiable Information) at rest
- Use HTTPS for all API communications
- Follow GDPR/CCPA compliance requirements
- Sanitize user inputs to prevent XSS/SQL injection

## External Dependencies

### Required API Keys (environment variables)
List required credentials:
- **`API_KEY_NAME`**: Description of service (required)
- **`DATABASE_URL`**: Database connection string (required)
- **`SECRET_KEY`**: Application secret for sessions (required)

Example:
```
- **`STRIPE_API_KEY`**: Payment processing (required)
- **`SENDGRID_API_KEY`**: Email delivery (required)
- **`AWS_ACCESS_KEY_ID`**: Cloud storage access (optional)
```

### External Services
Third-party services your project depends on:
- **Service Name**: What it's used for, pricing tier
- **Another Service**: What it's used for, SLA requirements

Example:
```
- **Stripe**: Payment processing, Standard tier
- **SendGrid**: Transactional emails, 100k emails/month plan
- **AWS S3**: File storage, pay-as-you-go
```

### System Dependencies
Runtime requirements:
- Operating system: Linux / macOS / Windows
- Runtime version: Python 3.10+ / Node.js 18+
- Package manager: pip / npm / yarn
- Database: PostgreSQL 14+ / MongoDB 5+
- Other tools: Redis, Docker, etc.

### Environment Setup

**Quick Start:**
```bash
# 1. Clone repository
git clone https://github.com/your-org/your-repo.git
cd your-repo

# 2. Install dependencies
npm install
# or
pip install -r requirements.txt

# 3. Configure environment
cp .env.example .env
# Edit .env with your API keys

# 4. Run development server
npm run dev
# or
python manage.py runserver
```

**Environment Variables:**
Create `.env` file based on `.env.example`:
```
API_KEY=your_key_here
DATABASE_URL=postgresql://localhost/dbname
SECRET_KEY=generate_random_string
```

### Documentation Structure
Links to other important documentation:
- **README.md**: Project overview and quick start
- **docs/API.md**: API endpoint documentation
- **docs/ARCHITECTURE.md**: System design and architecture decisions
- **docs/DEPLOYMENT.md**: Deployment procedures and infrastructure
- **CONTRIBUTING.md**: Development guidelines for contributors
