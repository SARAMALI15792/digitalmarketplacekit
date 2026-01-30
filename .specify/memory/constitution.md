<!--
SYNC IMPACT REPORT
==================
Version change: 1.0.0 → 2.0.0 (MAJOR - production-grade standards overhaul)

Modified principles:
- I. Trust by Design: Added verification tiers, plagiarism detection mechanism
- II. User-First Experience: Added explicit responsive breakpoints, strengthened accessibility
- III. Security as Default: Added rate limiting, demo sandboxing, CSP requirements
- IV. Production Mindset: Strengthened backup requirements, added phased reliability targets
- V. Scalability & Performance: Added testable metrics, Lighthouse scores, connection context
- VI. Content Integrity & Moderation: Added automated verification checks

Added sections:
- Project Verification Tiers
- Responsive Design Contract
- Live Demo Sandbox Security
- Rate Limiting Requirements
- Data Portability Requirements
- Search & Discovery Standards
- Phased Reliability Targets

Removed sections: None

Templates requiring updates:
- ⚠️ plan-template.md - Review against new verification tiers
- ⚠️ spec-template.md - Update responsive breakpoint requirements
- ⚠️ tasks-template.md - Add demo sandbox security checklist

Follow-up TODOs:
- Implement automated plagiarism detection pipeline
- Configure CSP headers for demo iframe isolation
- Set up Lighthouse CI for performance monitoring
-->

# Digital Marketplace for Skills Constitution

## Core Principles

### I. Trust by Design

Every skill and project published on the platform MUST be authentic, verifiable, and free from misleading claims.

- Only functional projects with working live demos may be published
- Each project MUST include: problem statement, solution overview, tech stack, creator details, and expected outcomes
- GitHub or repository links MUST be provided when applicable
- Users MUST NOT exaggerate skill levels
- Verification tiers (Basic → Verified → Certified) MUST be implemented to incentivize quality
- Automated plagiarism detection REQUIRED: projects with >70% similarity to indexed content trigger manual review
- Demo link health-checks REQUIRED: broken links demoted after 72h warning

**Rationale**: Trust is the platform's core value proposition. Recruiters and visitors rely on authentic demonstrations to evaluate talent. Compromised trust destroys platform utility.

### II. User-First Experience

The platform MUST minimize friction and maximize clarity for all user roles: students, professionals, recruiters, and visitors.

- Navigation MUST remain simple and predictable
- Responsive design MANDATORY across all defined breakpoints (see Responsive Design Contract)
- Accessibility: WCAG 2.1 Level A REQUIRED for all pages; Level AA REQUIRED for core flows (signup, browse, view project)
- Automated accessibility scan (axe-core or equivalent) MUST report 0 critical violations
- Project uploads MUST be structured and guided
- Users MUST be able to discover skills and evaluate creators through live projects
- Search MUST return results in <1 second with filters for skill, tech stack, and recency

**Rationale**: A frictionless experience drives adoption and retention. Complex interfaces discourage usage and undermine the platform's mission to connect talent with opportunities.

### III. Security as Default

User data, authentication, and all platform interactions MUST follow secure engineering practices.

- Secure authentication REQUIRED (OAuth / JWT / session security)
- No plaintext password storage permitted; bcrypt or argon2 REQUIRED
- Protection against OWASP Top 10 vulnerabilities MANDATORY (XSS, injection, CSRF)
- Least-privilege access principles MUST be enforced
- Rate limiting REQUIRED: 100 req/min anonymous, 500 req/min authenticated
- Live demo sandboxing REQUIRED (see Live Demo Sandbox Security)
- CSP headers REQUIRED for all pages
- Moderation workflows MUST exist for reporting abuse
- Spam prevention mechanisms REQUIRED
- Logging REQUIRED for critical failures and security events

**Rationale**: Security breaches destroy user trust and platform credibility. Security is not optional—it is foundational to every feature shipped.

### IV. Production Mindset

Features MUST be built as if supporting real-world users at scale, not prototypes.

- Graceful error handling MUST be implemented throughout
- Graceful degradation REQUIRED: core content visible even if JavaScript fails
- System MUST meet phased reliability targets (see Phased Reliability Targets)
- Daily automated backups REQUIRED; restore procedure tested quarterly
- All features MUST support the defined user roles (Students, Professionals, Recruiters, Admins)

**Rationale**: Prototype-quality code accumulates technical debt and fails under real-world conditions. Building for production from the start prevents costly rewrites.

### V. Scalability & Performance

Architecture MUST support growth without requiring major rewrites, and systems MUST be performant.

- Core pages MUST load ≤2 seconds on 4G/standard broadband connections
- Lighthouse performance score MUST be ≥85 for desktop, ≥80 for mobile
- API response time MUST be <500ms p95 (excluding cold starts; measured over 1000+ requests)
- Architecture MUST support 10x current load without re-architecture
- Database schema MUST use UUIDs, avoid cross-shard joins, and support read replicas
- Modular monolith architecture REQUIRED initially; service extraction only when justified by scale bottlenecks
- API-first design REQUIRED: all functionality accessible via well-documented APIs
- No hardcoded configurations—use environment variables and configuration management
- Performance budgets MUST be defined, monitored via Lighthouse CI or equivalent

**Rationale**: Fast systems improve engagement and credibility. Architecture that cannot scale becomes a liability as user base grows.

### VI. Content Integrity & Moderation

The platform MUST maintain high standards for published content and provide mechanisms for enforcement.

- Content moderation workflows MUST exist for reporting and handling abuse
- Spam prevention mechanisms REQUIRED
- Project verification tiers REQUIRED (see Project Verification Tiers)
- Admin tools MUST support content review and enforcement actions
- Project versioning REQUIRED: updates create new versions; previous versions remain viewable

**Rationale**: Without content integrity, the platform devolves into noise. Active moderation protects legitimate users and maintains platform quality.

---

## Key Standards

### Project Quality Requirements

| Requirement | Status | Description |
|-------------|--------|-------------|
| Live Demo | MANDATORY | Only functional projects with working demos may be published |
| Problem Statement | MANDATORY | Clear description of the problem being solved |
| Solution Overview | MANDATORY | Explanation of the approach taken |
| Tech Stack | MANDATORY | Technologies and frameworks used |
| Creator Details | MANDATORY | Attribution and contact information |
| Expected Outcomes | MANDATORY | What the project demonstrates or achieves |
| Repository OR Deploy Link | MANDATORY | At least one verification source required |
| Version History | MANDATORY | All updates create trackable versions |

### Project Verification Tiers

| Tier | Requirements | Badge | Benefits |
|------|--------------|-------|----------|
| **Basic** | Live demo link works, passes health check | None | Listed in search |
| **Verified** | + Public repo with matching code (≥60% similarity) | ✓ Verified | Priority in search, recruiter alerts |
| **Certified** | + Admin or peer code review passed | ⭐ Certified | Featured placement, certification badge |

**Verification Rules:**
- Demo link health-checked daily; broken links trigger 72h warning, then demotion
- Repo-to-demo code similarity checked automatically (threshold: ≥60%)
- Plagiarism scan against project index; >70% match triggers manual review queue
- Verified status revoked if repo becomes private or demo breaks for >7 days

### Security Requirements

| Threat | Mitigation | Status |
|--------|------------|--------|
| Authentication bypass | OAuth/JWT/secure sessions | MANDATORY |
| Password exposure | Hashed storage (bcrypt/argon2) | MANDATORY |
| XSS attacks | Input sanitization, CSP headers | MANDATORY |
| SQL injection | Parameterized queries, ORM usage | MANDATORY |
| CSRF attacks | Token validation | MANDATORY |
| Unauthorized access | Role-based access control | MANDATORY |
| API abuse | Rate limiting (100/500 req/min) | MANDATORY |
| Malicious demos | Sandboxed iframes with CSP | MANDATORY |

### Rate Limiting Requirements

| User Type | Limit | Scope |
|-----------|-------|-------|
| Anonymous | 100 requests/minute | Per IP |
| Authenticated | 500 requests/minute | Per user |
| Search endpoints | 30 requests/minute | Per user/IP |
| Upload endpoints | 10 requests/hour | Per user |

**Exceeded limit behavior**: Return HTTP 429 with `Retry-After` header; log for abuse pattern analysis.

### Performance Targets

| Metric | Target | Measurement | Phase |
|--------|--------|-------------|-------|
| Page Load Time | ≤2 seconds | 4G/broadband, warm cache | All |
| Lighthouse Desktop | ≥85 | Lighthouse CI | All |
| Lighthouse Mobile | ≥80 | Lighthouse CI | All |
| API Response Time | <500ms p95 | Excluding cold starts, 1000+ samples | All |
| Search Latency | <1 second | End-to-end with filters | All |
| Time to Interactive | ≤3.5 seconds | Mobile 4G | All |

### Phased Reliability Targets

| Phase | DAU Threshold | Uptime Target | Monitoring |
|-------|---------------|---------------|------------|
| MVP | <100 | 99.0% | Basic uptime checks |
| Growth | 100–1,000 | 99.5% | APM + alerting |
| Scale | 1,000–10,000 | 99.9% | Full observability stack |
| Enterprise | >10,000 | 99.95% | Multi-region, auto-failover |

---

## Responsive Design Contract

### Breakpoints (MANDATORY)

| Breakpoint | Width Range | Layout Requirements |
|------------|-------------|---------------------|
| Mobile | 360px–767px | Single column, touch targets ≥44px, no horizontal scroll |
| Tablet | 768px–1279px | Adaptive grid (1-2 columns), collapsible navigation |
| Desktop | ≥1280px | Full layout, sidebar navigation, multi-column grids |

### Testing Protocol

- All pages MUST be tested at: 360px, 768px, 1280px, 1920px
- No content truncation without expand/show-more option
- Images lazy-loaded with aspect-ratio preserved (no layout shift)
- Touch targets minimum 44x44px on mobile
- No horizontal scrolling on any breakpoint
- Lighthouse mobile score ≥80

### Responsive Checklist

| Component | Mobile | Tablet | Desktop |
|-----------|--------|--------|---------|
| Navigation | Hamburger menu | Collapsible sidebar | Full sidebar |
| Project cards | Single column stack | 2-column grid | 3-4 column grid |
| Demo viewer | Full-width, 300px height | 80% width, 450px height | 70% width, 600px height |
| Forms | Single column, large inputs | Single column | Two-column where appropriate |
| Tables | Card view or horizontal scroll | Horizontal scroll | Full table |

---

## Live Demo Sandbox Security

### Sandbox Policy

| Requirement | Implementation | Status |
|-------------|----------------|--------|
| Iframe isolation | `sandbox="allow-scripts allow-same-origin"` | MANDATORY |
| CSP frame-src | Restricted to approved hosts | MANDATORY |
| DOM isolation | No direct access between platform and demo | MANDATORY |
| Load timeout | 30 second limit with fallback | MANDATORY |

### Approved Demo Hosts

- Vercel (`*.vercel.app`)
- Netlify (`*.netlify.app`)
- GitHub Pages (`*.github.io`)
- Render (`*.onrender.com`)
- Railway (`*.railway.app`)
- Cloudflare Pages (`*.pages.dev`)
- Custom domains (user-verified, HTTPS required)

### Demo Requirements

| Requirement | Specification |
|-------------|---------------|
| Protocol | HTTPS mandatory |
| Authentication | No login prompts; must be public/demo mode |
| Health check | Weekly automated check; 3 consecutive failures = delisted with notification |
| Fallback | Screenshot + direct link shown if demo fails to load |

### Resource Limits

| Resource | Limit |
|----------|-------|
| Iframe width | 100% of container |
| Iframe height | 600px max (expandable on click) |
| Load timeout | 30 seconds |
| Autoplay media | Blocked without user interaction |

---

## Data & Privacy

### Data Portability (MANDATORY)

- Users can request full data export in JSON or CSV format
- Export request fulfilled within 48 hours
- Export includes: profile, projects, skills, activity history
- Delete account option removes all PII within 30 days

### Storage & Backup

| Requirement | Specification | Status |
|-------------|---------------|--------|
| Backup frequency | Daily automated | MANDATORY |
| Backup retention | 30 days rolling | MANDATORY |
| Restore testing | Quarterly verification | MANDATORY |
| Encryption at rest | AES-256 or equivalent | MANDATORY |
| Encryption in transit | TLS 1.2+ | MANDATORY |

### PII Handling

- Minimize collection: only collect data necessary for platform function
- No selling or sharing PII with third parties without explicit consent
- GDPR-compatible: right to access, rectify, delete
- Audit log for all PII access by admins

---

## Search & Discovery

### Performance Requirements

| Metric | Target |
|--------|--------|
| Search response time | <1 second |
| Autocomplete latency | <200ms |
| Filter application | <500ms |

### Filter Capabilities (MANDATORY)

- By skill/technology
- By verification tier
- By recency (last week, month, year)
- By project type
- By creator role (student, professional)

### Relevance Signals

| Signal | Weight | Notes |
|--------|--------|-------|
| Verification tier | High | Certified > Verified > Basic |
| Demo health | High | Working demos ranked higher |
| Recency | Medium | Recent updates boost ranking |
| Completeness | Medium | Full metadata ranks higher |
| Engagement | Low | Views, saves (avoid gaming) |

---

## Constraints & Architecture

### Supported User Roles

1. **Students**: Create profiles, upload projects, showcase skills
2. **Professionals**: Enhanced profiles, project portfolios, skill verification
3. **Recruiters**: Search talent, evaluate projects, contact creators
4. **Admins**: Content moderation, user management, platform configuration

### Architecture Requirements

| Principle | Requirement | Rationale |
|-----------|-------------|-----------|
| **Modular Monolith** | Start with well-bounded modules in single deployable; extract to services only when scale justifies | Avoids premature complexity |
| **API-First** | All functionality accessible via documented REST/GraphQL APIs | Enables future clients, integrations |
| **Cloud-Native** | Containerized, horizontally scalable, infrastructure-as-code | Deployment flexibility |
| **Configuration** | No hardcoded secrets; environment variables + secrets manager | Security, environment parity |

### Service Extraction Criteria

Extract a module to a separate service ONLY when:
- Module has fundamentally different scaling requirements (>10x difference)
- Module requires different deployment cadence (daily vs. weekly)
- Module has distinct failure domain that shouldn't affect core platform
- Team ownership boundaries clearly separate the module

### Database Design Requirements

| Requirement | Specification |
|-------------|---------------|
| Primary keys | UUIDs (not auto-increment integers) |
| Foreign keys | Avoid cross-shard join patterns |
| Read scaling | Schema supports read replicas |
| Migrations | Forward-compatible; no breaking changes without versioned API |
| Indexing | All filtered/sorted columns indexed; query performance monitored |

---

## Internationalization Readiness

### Requirements (RECOMMENDED for Phase 2)

| Requirement | Specification |
|-------------|---------------|
| UI text | Externalized to translation files (JSON/YAML) |
| Date/time | Locale-aware formatting |
| Numbers/currency | Locale-aware formatting |
| RTL support | Layout structure compatible with RTL languages |
| Character encoding | UTF-8 throughout |

---

## Governance

### Amendment Process

1. Proposed changes MUST be documented with rationale
2. Changes affecting security or trust principles require explicit approval
3. All amendments MUST include a migration plan for existing features
4. Version number MUST be updated according to semantic versioning
5. Breaking changes require 30-day notice to dependent teams

### Versioning Policy

- **MAJOR**: Backward-incompatible changes to principles or removal of requirements
- **MINOR**: New principles, sections, or materially expanded guidance
- **PATCH**: Clarifications, wording improvements, typo fixes

### Compliance Review

- All pull requests MUST verify compliance with constitution principles
- Security-related changes require security review
- Performance-impacting changes require performance validation (Lighthouse CI)
- New features MUST map to at least one core principle
- Responsive design changes require multi-breakpoint testing

### Non-Negotiable Rule

**If a feature compromises trust, security, or platform reliability—it MUST NOT ship.**

This rule supersedes all other considerations including deadlines, feature requests, or business pressure.

---

## Appendix: Testing Checklists

### Pre-Launch Checklist

- [ ] All core pages load ≤2s on 4G
- [ ] Lighthouse desktop ≥85, mobile ≥80
- [ ] axe-core reports 0 critical accessibility violations
- [ ] All OWASP Top 10 mitigations verified
- [ ] Rate limiting tested and functional
- [ ] Demo sandbox isolation verified (no DOM leakage)
- [ ] Backup restore tested successfully
- [ ] All breakpoints tested (360px, 768px, 1280px, 1920px)
- [ ] Search returns results <1s with filters
- [ ] Project verification flow functional

### Security Checklist

- [ ] Authentication bypass attempts blocked
- [ ] SQL injection attempts blocked
- [ ] XSS payloads sanitized
- [ ] CSRF tokens validated
- [ ] Rate limits enforced
- [ ] CSP headers present on all pages
- [ ] Demo iframes properly sandboxed
- [ ] No secrets in codebase or logs

---

**Version**: 2.0.0 | **Ratified**: 2026-01-30 | **Last Amended**: 2026-01-30

**Changelog**:
- 2.0.0 (2026-01-30): Production-grade overhaul - added verification tiers, responsive breakpoints, demo sandboxing, rate limiting, phased reliability, testable metrics
- 1.0.0 (2026-01-30): Initial constitution ratification
