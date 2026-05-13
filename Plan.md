
---

## Verification

### Phase 1-3 Verification (Foundation)
- [ ] Repository can be added via CLI, cloned, and metadata stored in SQLite
- [ ] Run parsers on sample TypeScript/Java/Python code, extract endpoints correctly
- [ ] Extract parameters, methods, paths from detected APIs

### Phase 4-6 Verification (Standardization)
- [ ] Generated OpenAPI specs validate against OpenAPI 3.0 schema
- [ ] All CRUD operations work via FastAPI endpoints
- [ ] Search endpoint returns relevant results with pagination
- [ ] Database queries execute in <100ms (typical case)

### Phase 7-8 Verification (UI)
- [ ] React app loads without errors
- [ ] Can add repos and trigger sync from UI
- [ ] Search returns results matching backend queries
- [ ] API detail view displays all extracted metadata correctly
- [ ] OpenAPI spec can be viewed and exported

### Phase 9-12 Verification (Advanced)
- [ ] Incremental scan time is 70%+ faster than full scan
- [ ] New parsers can be added by implementing Plugin interface
- [ ] Search response time consistently <300ms even with 1000+ APIs
- [ ] System runs in Docker, CI/CD pipeline passes all tests
- [ ] User authentication works, RBAC enforced on endpoints

---

## Decisions & Assumptions

1. **Technology Stack**: Python FastAPI (chosen) + React for flexibility; SQLite for MVP simplicity, can migrate to PostgreSQL later
2. **Language Support**: Start with TypeScript (Babel), Java (tree-sitter), Python (ast); expandable via plugin system
3. **Scope**: Full 12-phase implementation across foundation, core, product, and maturity tiers
4. **Database**: SQLite with SQLAlchemy ORM + Alembic migrations for easy evolution
5. **API Design**: RESTful endpoints with JSON responses, searchable catalog
6. **UI**: React SPA with lazy-loaded components, Tailwind for styling
7. **Incremental Scanning**: Git diff-based to optimize repeated scans
8. **Parser Architecture**: Plugin-based to support future language additions without core changes

### Explicitly Excluded (Phase 1-8 MVP focus)
- Advanced analytics (API usage metrics)
- Service dependency graph visualization (Phase roadmap item)
- API versioning and change history tracking (defer to Phase 9+)
- Multi-user collaboration features (Phase 12)
- Webhook integrations (future)

---

## Further Considerations

1. **AST Parsing Complexity**: Each language parser has different AST structures
   - **Recommendation**: Start with TypeScript (Babel is most mature), then Java (tree-sitter has good Java support), then Python (std library ast)
   - Allocate time for testing edge cases (decorators, generics, async/await, lambda functions)

2. **Database Scaling**: SQLite works for MVP but will hit limits at ~100k APIs
   - **Recommendation**: Design ORM layer to be DB-agnostic (SQLAlchemy abstracts this); plan PostgreSQL migration for Phase 12

3. **CI/CD Pipeline Complexity**: Running parsers on large repos can be slow
   - **Recommendation**: Make parser timeouts configurable, implement async queue (Celery/RQ) in Phase 9-10 to prevent blocking HTTP requests

---

## Implementation Order (By Priority)

1. **Weeks 1-2**: Phase 1 (repo ingestion) + Phase 2 (TypeScript parser)
2. **Weeks 3-4**: Phase 3 (API detection) + Phase 4 (OpenAPI generation)
3. **Weeks 5-6**: Phase 5-6 (Database + Backend API), Phase 7 (Frontend setup in parallel)
4. **Weeks 7-8**: Phase 8 (UI features)
5. **Weeks 9-10**: Phase 9-10 (Incremental updates + plugins)
6. **Weeks 11+**: Phase 11-12 (Performance optimization + production hardening)