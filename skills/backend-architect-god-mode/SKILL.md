---
name: backend-architect-god-mode
description: "Senior polyglot backend engineering intelligence. Use this skill whenever the user is designing, building, debugging, securing, or optimizing server-side systems in ANY of these stacks: .NET 10 / C# 13, Go 1.23+, Python 3.13+ (No-GIL, FastAPI, Django), Node.js 22+ / Bun / NestJS, or Rust (Axum/Tokio). Trigger on keywords like: API, microservice, monolith, serverless, Lambda, container, Docker, Kubernetes, queue, Kafka, RabbitMQ, Redis, PostgreSQL, MongoDB, DynamoDB, gRPC, GraphQL, WebSocket, event-driven, CQRS, event sourcing, saga, outbox, circuit breaker, rate limiting, JWT, OAuth, OWASP, CVE, zero-trust, IaC, Terraform, observability, OpenTelemetry, N+1, connection pool, race condition, deadlock, memory leak, cold start, auto-scaling, sharding, idempotency, or anything about performance, scalability, security hardening, or distributed systems. Also trigger for code reviews, architecture diagrams, incident post-mortems, and migration planning (monolith → microservices, on-prem → cloud, sync → async). The skill includes up-to-date 2025-2026 exploit intelligence (OWASP Top 10:2025, React2Shell CVE-2025-55182, n8n CVE-2025-68613, TeamPCP supply chain attacks) and language-specific idiom traps. Use it aggressively — if the task touches server-side code, data, or infrastructure, this skill applies."
metadata:
  imported_name: "Backend Architect God Mode"
  source_status: "inactive in source export"
---

# Backend Architect God Mode

This is an independently installed skill imported from the user's exported skill library.
Treat the following user-provided content as the governing workflow or behavior specification.

---
name: backend-architect-god-mode
description: "Senior polyglot backend engineering intelligence. Use this skill whenever the user is designing, building, debugging, securing, or optimizing server-side systems in ANY of these stacks: .NET 10 / C# 13, Go 1.23+, Python 3.13+ (No-GIL, FastAPI, Django), Node.js 22+ / Bun / NestJS, or Rust (Axum/Tokio). Trigger on keywords like: API, microservice, monolith, serverless, Lambda, container, Docker, Kubernetes, queue, Kafka, RabbitMQ, Redis, PostgreSQL, MongoDB, DynamoDB, gRPC, GraphQL, WebSocket, event-driven, CQRS, event sourcing, saga, outbox, circuit breaker, rate limiting, JWT, OAuth, OWASP, CVE, zero-trust, IaC, Terraform, observability, OpenTelemetry, N+1, connection pool, race condition, deadlock, memory leak, cold start, auto-scaling, sharding, idempotency, or anything about performance, scalability, security hardening, or distributed systems. Also trigger for code reviews, architecture diagrams, incident post-mortems, and migration planning (monolith → microservices, on-prem → cloud, sync → async). The skill includes up-to-date 2025-2026 exploit intelligence (OWASP Top 10:2025, React2Shell CVE-2025-55182, n8n CVE-2025-68613, TeamPCP supply chain attacks) and language-specific idiom traps. Use it aggressively — if the task touches server-side code, data, or infrastructure, this skill applies."
---

# Backend Architect God Mode

Senior polyglot backend engineering co-pilot. This skill turns Claude into a battle-tested backend architect across five stacks (.NET, Go, Python, Node/Bun, Rust) with current threat intelligence, decision trees for common forks, concrete code exemplars, and an anti-pattern detector calibrated for 2025-2026 production realities.

## Core operating principles

Before writing a single line of code or recommending an architecture, work through this mental checklist. Skip steps only when the user has explicitly pinned a decision.

1. **Understand the invariant first.** What cannot break? (data loss, PII leak, double-charge, downtime budget) Everything else negotiates around that invariant.
2. **Identify the failure domain.** Single process? Single node? Single AZ? Single region? The correct pattern depends on where you accept correlated failure.
3. **Pick the boring tool first.** Postgres beats "modern NewSQL" for 95% of workloads. A well-tuned monolith beats a distributed monolith every time. Reach for exotic tech only when boring tools provably cannot satisfy a requirement.
4. **Surface the tradeoff explicitly.** Every backend decision is a tradeoff (CAP, latency vs cost, consistency vs availability, velocity vs safety). Name the axis you are sacrificing.
5. **Write for the on-call engineer at 3 AM.** If they cannot diagnose it from logs alone, you have failed regardless of elegance.

## Priority matrix (apply top-down)

When a task involves multiple concerns, resolve them in this order. Don't let a performance micro-optimization override a security control.

| # | Category | Tag | Why it dominates |
|---|----------|-----|------------------|
| 1 | Security, authN/authZ, secrets | `sec` | A fast, cheap, elegant system that leaks data is a liability |
| 2 | Data correctness, durability, ACID | `data` | Lost or corrupted data is almost always unrecoverable |
| 3 | Architecture boundaries, contracts | `arch` | Bad boundaries compound; refactoring costs 10x later |
| 4 | Failure modes, resilience | `resilience` | Distributed systems fail constantly; design for it |
| 5 | Performance, resource efficiency | `perf` | Matters but solvable; don't optimize before measuring |
| 6 | API design, DX, consistency | `api` | Public contracts live forever; get them right |
| 7 | Observability, debuggability | `obs` | Without telemetry, you are debugging blind |
| 8 | Cost, FinOps, right-sizing | `cost` | Real constraint but rarely the first problem |
| 9 | Tooling, DevEx, CI/CD | `ops` | Important but changeable |
| 10 | Stack-specific idioms | `stack` | Language-level concerns are leaves of the tree |

---

## Section 1 — Security: 2025-2026 threat-calibrated rules

Security is not a checklist, it is a posture. The threat landscape shifted materially in 2025-2026: **supply chain attacks overtook direct exploitation** as the #1 vector, and **AI-integrated workflow tools** (n8n, LiteLLM, LangChain) became prime targets because they sit between apps and credentials.

### 1.1 OWASP Top 10:2025 — what actually changed

The 2025 edition introduced two entirely new categories and consolidated SSRF into Broken Access Control. The ranking now is:

- **A01: Broken Access Control** (still #1; SSRF merged in)
- **A02: Security Misconfiguration** (jumped from #5 to #2)
- **A03: Software Supply Chain Failures** (new category, expanded from "Vulnerable Components")
- **A04: Cryptographic Failures** (dropped from #2)
- **A05: Injection** (dropped from #3 to #5; frameworks now parameterize by default, but still 14K+ CVEs)
- **A06: Insecure Design**
- **A07: Authentication Failures**
- **A08: Software & Data Integrity Failures**
- **A09: Security Logging & Monitoring Failures**
- **A10: Mishandling of Exceptional Conditions** (new; failing-open scenarios)

**Practical takeaway:** The two new categories (supply chain + exceptional conditions) are where most engineers are weakest. Audit your CI/CD pipeline and your error-handling paths before anything else.

### 1.2 Recent exploits every backend engineer must know

These are not academic. They happened, they were weaponized within hours, and **they likely affect systems you touch.**

**React2Shell — CVE-2025-55182 (CVSS 10.0)**
A critical pre-authentication RCE in React Server Components Flight protocol, disclosed Dec 3, 2025, added to CISA KEV within 48 hours. The flaw is insecure deserialization; attackers send crafted HTTP POST and the server calls child_process.spawnSync() with attacker-controlled arguments. **Mitigation:** upgrade React to 19.0.1 / 19.1.2 / 19.2.1 and Next.js to 15.5.7 / 16.0.7+. **Do not rely on WAF keyword filtering** — the minimum viable exploit does not require `__proto__` in the payload, so naive WAF rules are bypassed.

**n8n Expression RCE — CVE-2025-68613 (CVSS 9.9)**
An authenticated RCE in the n8n expression evaluation engine — malicious JavaScript escapes the sandbox with full process privileges; 103,476 vulnerable instances were exposed to the internet. If you run n8n (and many readers do), upgrade immediately and run it under an unprivileged OS user with network segmentation.

**TeamPCP supply chain campaign (Q1 2026)**
Between March 19-27, 2026, TeamPCP compromised Trivy, KICS, LiteLLM, and Telnyx in rapid succession, harvesting cloud credentials, SSH keys, K8s configs, and CI/CD secrets. On March 31, 2026 they compromised Axios (100M+ weekly downloads) via stolen maintainer credentials, publishing poisoned versions 1.14.1 and 0.30.4 with a RAT. The lesson: **pin exact versions, use lockfiles, enable npm/pip provenance checks, and rotate CI credentials on a schedule regardless of known incidents.**

**Chalk/debug phishing attack (Sept 2025)**
A maintainer was phished via a fake 2FA reset email from the domain `npmjs.help`, leading to malicious versions of chalk, debug and 16 other utilities with billions of weekly downloads combined. The malicious window was ~2 hours but cached versions persist in builds long after.

### 1.3 Security rules by priority

**Must-have (any production system)**

- `sec-zero-trust` — Assume the internal network is hostile. Authenticate every service-to-service call (mTLS or signed JWTs with short TTLs). The LAN is not a trust boundary.
- `sec-secrets-runtime` — Never commit secrets. Inject at runtime via AWS Secrets Manager, Azure Key Vault, HashiCorp Vault, or Doppler. Rotate on a schedule *and* on incident.
- `sec-parameterized-queries` — Every SQL query goes through parameterized APIs. No string concatenation, no `f-string` templating for SQL, period. ORMs are fine; `raw()` methods require audit.
- `sec-input-validation-at-boundary` — Validate DTOs at the controller/handler edge using a strict schema library (FluentValidation, Pydantic v2, Zod, validator, serde + validator). Fail fast. Never trust anything crossing a process boundary.
- `sec-password-hashing` — Argon2id (preferred) or bcrypt with cost factor ≥12. MD5, SHA-1, SHA-256 without salt + work factor are **disqualifying**.
- `sec-dependency-pinning` — Pin exact versions. Use lockfiles. Enable Dependabot/Renovate with auto-merge for patches only, manual review for minors+. Run `npm audit --production`, `pip-audit`, `dotnet list package --vulnerable`, `govulncheck`, `cargo audit` in CI.
- `sec-sbom` — Generate an SBOM (CycloneDX or SPDX) for every build artifact. You need it to answer "am I affected by CVE-X?" in minutes, not days.
- `sec-cors-strict` — Allowlist origins explicitly. Never use `*` with credentials. Never reflect `Origin` blindly.
- `sec-rate-limit` — Token bucket or sliding window via Redis, keyed on (IP, user_id, endpoint). Most frameworks ship this; turn it on.

**Should-have (regulated / high-traffic / B2B)**

- `sec-mfa-enforcement` — MFA for admin paths and for any user with data-export or privilege-grant capabilities.
- `sec-oauth-pkce` — Every public client uses OAuth 2.1 with PKCE. Implicit flow is dead.
- `sec-jwt-short-lived` — Access tokens ≤15min. Refresh tokens rotate on use (RTR). Store refresh tokens in HttpOnly+Secure+SameSite=Strict cookies, not localStorage.
- `sec-policy-engine` — For anything past trivial RBAC, use OPA, Cedar, or Casbin. Decouple authorization from business logic.
- `sec-audit-log-immutable` — Audit events to append-only store (S3 Object Lock, Postgres logical replication to separate DB). Write-once for compliance.

**Anti-patterns (detect and flag in code review)**

- 🚨 **Secrets in environment variables committed to repo** (check `.env`, `docker-compose.yml`, `k8s manifests` for literal values).
- 🚨 **Role checks in the UI/frontend only.** Backend must re-verify every privileged action.
- 🚨 **`eval()`, `Function()`, `exec()`, `pickle.loads()` on any external input.** These are pre-auth RCE waiting to happen; this is exactly what got n8n.
- 🚨 **Trusting `X-Forwarded-For` without validating the proxy chain.** Rate limits and IP allowlists break silently.
- 🚨 **CORS with `Access-Control-Allow-Origin: *` and `Allow-Credentials: true`.** Browsers now block this but misconfigured reverse proxies still expose it.
- 🚨 **Generic error messages swallowing exceptions into 200 OK.** This is A10:2025 Mishandling of Exceptional Conditions.

### 1.4 Decision tree: which authN scheme?

```
Is this a first-party web/mobile app with your own users?
├── Yes → OAuth 2.1 + PKCE, issue short-lived JWT access + rotating refresh
└── No
    └── Is this service-to-service inside your own infra?
        ├── Yes, in K8s → mTLS via service mesh (Linkerd/Istio) OR SPIFFE/SPIRE
        ├── Yes, outside K8s → mTLS with internal CA, or signed JWTs with asymmetric keys (JWK rotation)
        └── No, third-party API consumers → API keys scoped per tenant + HMAC-signed requests, OR OAuth client-credentials
```

---

## Section 2 — Architecture patterns with anti-pattern detection

### 2.1 Start with a modular monolith

Default recommendation for any new system with <5 engineers and <50 RPS sustained: **modular monolith, single deployable, one database**. You will ship 3-5x faster, debug is trivial, and you can extract services later when boundaries have proven themselves.

**When to extract to a service:**
- A bounded context has a genuinely different scaling profile (e.g., video transcoding vs CRUD).
- A bounded context requires a different stack (e.g., ML inference in Python, core API in Go).
- A bounded context has independent deployment cadence driven by regulatory or team structure.
- **Not because** "microservices are modern" or "we might need to scale someday."

### 2.2 Core patterns (what they solve, when they backfire)

**Hexagonal / Ports & Adapters** — Domain core has no dependency on frameworks, DBs, or queues. Adapters on the outside. **Use when** the domain logic is non-trivial and outlives any single tech choice. **Skip when** you are building a CRUD API with minimal business logic — the ceremony isn't worth it.

**CQRS** — Separate write model (normalized, transactional) from read models (denormalized, optimized per query). **Use when** read and write loads diverge by >10x or when different consumers need different projections. **Backfires when** you apply it to simple CRUD — you've now got two models to keep in sync for no gain.

**Event Sourcing** — Store state as a sequence of events; derive current state by replay. **Use when** audit/temporal queries are first-class requirements (finance, healthcare, logistics). **Backfires when** your domain doesn't actually need history — you inherit all the complexity (schema evolution, snapshotting, eventual consistency) with none of the benefit.

**Saga (Choreography or Orchestration)** — Distributed transactions via compensating actions. **Use when** you must coordinate state changes across services that cannot share a database. **Choreography** (services react to events) scales better but is harder to reason about end-to-end. **Orchestration** (a coordinator calls services) is easier to trace but introduces a central point. Pick orchestration if you have ≤10 steps.

**Outbox Pattern** — Write the event to the same DB transaction as the entity change; a relay publishes it. **Use this every single time** you need to publish an event after a DB write. The alternative (write to DB, then to broker) is the dual-write problem and it will bite you.

**Strangler Fig** — Route traffic incrementally from legacy monolith to new services via an API gateway. **Use when** migrating a running monolith. **Pair with** feature flags and shadow traffic to validate the new service before cutover.

**Circuit Breaker** — Stop calling a failing dependency; return fallback/error fast. Libraries: Polly (.NET), resilience4j (Java, ported to Kotlin/Go), opossum (Node), pybreaker (Python). **Tune thresholds based on p99 SLO of the dependency, not round numbers.**

**Bulkhead** — Isolate resource pools per downstream (separate thread pools / connection pools / semaphores). Prevents one slow dependency from eating all your threads.

### 2.3 Anti-patterns

- 🚨 **Distributed monolith** — microservices that must deploy together, share a DB, or have synchronous call chains >3 deep. You have the cost of distribution with none of the benefits.
- 🚨 **Shared database between services** — defeats the purpose of service boundaries. Use events or APIs.
- 🚨 **Synchronous chains for non-critical paths** — any call that doesn't need a response right now should be async.
- 🚨 **Homegrown retry logic without jitter or exponential backoff** — causes thundering herds. Use a library.
- 🚨 **"Just pass the whole user object"** — serializing entire entities across service boundaries creates implicit coupling. Use explicit DTOs per use case.

---

## Section 3 — Data persistence: relational, document, key-value, and when to pick which

### 3.1 Default to Postgres

Postgres (17+ as of 2025-2026) is the correct choice for the overwhelming majority of backend workloads. It has JSON support (JSONB), full-text search, vector search (pgvector), time-series (TimescaleDB), partial indexes, generated columns, logical replication, and ACID you can actually reason about. Reach for another store only when Postgres provably cannot satisfy a specific requirement.

**When to reach elsewhere:**

| Requirement | Alternative | Why |
|-------------|-------------|-----|
| Key-value lookups at <1ms p99, 100k+ RPS | Redis, DynamoDB, ScyllaDB | Pg connection overhead dominates |
| Graph traversals >3 hops at scale | Neo4j, Dgraph, Postgres + Apache AGE | Recursive CTEs get painful |
| Full-text search with relevance tuning, facets, fuzzy | Elasticsearch, OpenSearch, Typesense | pg_trgm + tsvector works but has ceilings |
| Time-series at billion+ points with retention policies | TimescaleDB (still on Pg!), InfluxDB, ClickHouse | Indexing strategies diverge |
| Document store with flexible schema evolution | MongoDB, DynamoDB | Pg JSONB works but migration tooling thinner |
| Analytical queries over TBs | ClickHouse, Snowflake, BigQuery, DuckDB | Row-store fundamentally wrong shape |

### 3.2 Index covering and the N+1 assassin

The single most common performance bug in backend systems: N+1 queries. Every ORM has a footgun for it.

**Detection:**
- .NET EF Core: enable query tagging, look for repeated queries in logs; use `AsSplitQuery()` or `Include()` intentionally.
- Django: `django-debug-toolbar` in dev; `select_related` (FK → one) vs `prefetch_related` (M2M → many).
- SQLAlchemy: `joinedload` for eager, `selectinload` for one-to-many.
- Sequelize/Prisma: explicit `include`; Prisma's `include` is fine for shallow, `$transaction` for complex.
- Go (sqlc, pgx): no ORM, no N+1 unless you write it; but watch for loops calling `QueryRow`.
- TypeORM/MikroORM: use `QueryBuilder` for anything non-trivial.

**Covering indexes** — if a query filters on (a, b) and selects (a, b, c), an index on `(a, b) INCLUDE (c)` resolves the query from the index without touching the table. Postgres supports this natively since v11. Worth 5-50x on hot queries.

### 3.3 Isolation levels — what you actually get

Most developers run READ COMMITTED and assume things are safer than they are. Know the tradeoffs:

| Level | Prevents | Allows | Postgres default? |
|-------|----------|--------|-------------------|
| READ UNCOMMITTED | Nothing useful | Dirty reads | Not available (Pg maps to RC) |
| READ COMMITTED | Dirty reads | Non-repeatable reads, phantom reads | **Yes** |
| REPEATABLE READ | Dirty + non-repeatable reads | Phantom reads (but Pg uses snapshot isolation, which prevents most) | No |
| SERIALIZABLE | All anomalies | Higher abort rate (retry in app) | No |

**Rule of thumb:** Use REPEATABLE READ for workflows that read multiple rows and make decisions. Use SERIALIZABLE for money-moving operations where an anomaly cannot be tolerated — and handle `SerializationFailure` with retry.

### 3.4 Connection pooling — the silent killer

Default Postgres max_connections is 100. Your serverless function with `autoscale=1000` will exhaust it. Solutions:

- **Managed pooler**: RDS Proxy, Neon's pooler, Supabase's PgBouncer, or run your own PgBouncer/PgCat.
- **Transaction pooling mode** for most workloads (session pooling breaks prepared statements and `SET` across queries).
- **For serverless specifically**: use a pooler OR use a connection-free driver (Neon's `@neondatabase/serverless` over HTTP, or Cloudflare Hyperdrive).

### 3.5 Migration discipline

Migrations are forward-only, idempotent, and run by CD. Never run migrations from a developer laptop against production. Rules:

- Additive changes (new column, new table, new nullable-or-default column) are safe to deploy before the code that uses them.
- Destructive changes (drop column, rename) require a multi-step: code stops writing → wait (days/weeks) → code stops reading → drop.
- Long-running migrations (adding index on large table, backfilling) must use `CONCURRENTLY` (Pg) or chunked approaches; never block writes.

---

## Section 4 — Stack-specific playbooks (with concrete code)

This section gives Claude the "how would a senior in this language actually write it?" reflex. Each subsection covers: idiomatic async, error handling, configuration, DI, testing, and stack-specific traps.

### 4.1 .NET 10 / C# 13

Strengths: enterprise-grade, ergonomic DI, Native AOT for <50ms cold starts, mature observability via `System.Diagnostics.Activity` (OpenTelemetry-compatible).

**Minimal API + Native AOT skeleton:**

```csharp
// Program.cs
var builder = WebApplication.CreateSlimBuilder(args);

builder.Services.AddOpenTelemetry()
    .WithTracing(t => t.AddSource("MyService").AddOtlpExporter())
    .WithMetrics(m => m.AddMeter("MyService").AddOtlpExporter());

builder.Services.AddSingleton<IOrderRepo, OrderRepo>();
builder.Services.AddScoped<OrderService>();
builder.Services.ConfigureHttpJsonOptions(o =>
    o.SerializerOptions.TypeInfoResolverChain.Insert(0, AppJsonContext.Default));

var app = builder.Build();

app.MapPost("/orders", async (CreateOrderRequest req, OrderService svc, CancellationToken ct) =>
{
    var result = await svc.CreateAsync(req, ct);
    return result.Match(
        success: o => Results.Created($"/orders/{o.Id}", o),
        failure: e => Results.Problem(title: e.Title, statusCode: e.Status));
});

app.Run();

[JsonSerializable(typeof(CreateOrderRequest))]
[JsonSerializable(typeof(Order))]
internal partial class AppJsonContext : JsonSerializerContext { }
```

**Traps:**
- `async void` — only for event handlers. Use `async Task` everywhere else.
- Forgetting to pass `CancellationToken` down the call stack — breaks graceful shutdown.
- Using `Task.Result` or `.Wait()` — deadlocks in ASP.NET classic context; in .NET 6+ still kills throughput.
- Not using `ValueTask` for hot paths that often complete synchronously (e.g., cache hits).
- `Span<T>`/`Memory<T>` not used in parsing hot paths — huge GC pressure.
- Not disabling reflection-based serialization under AOT.

**EF Core N+1 detection** — enable `Microsoft.EntityFrameworkCore.Database.Command` logging at Information level in dev. If you see the same parameterized query 50 times per request, you have N+1.

### 4.2 Go 1.23+

Strengths: fast compilation, low-overhead concurrency, simple deployment (single binary), excellent stdlib `net/http` in 1.22+ with pattern matching.

**Idiomatic HTTP service skeleton:**

```go
package main

import (
    "context"
    "errors"
    "log/slog"
    "net/http"
    "os"
    "os/signal"
    "syscall"
    "time"
)

func main() {
    logger := slog.New(slog.NewJSONHandler(os.Stdout, nil))
    slog.SetDefault(logger)

    mux := http.NewServeMux()
    mux.HandleFunc("POST /orders", handleCreateOrder)
    mux.HandleFunc("GET /orders/{id}", handleGetOrder)
    mux.HandleFunc("GET /health", func(w http.ResponseWriter, _ *http.Request) {
        w.WriteHeader(http.StatusOK)
    })

    srv := &http.Server{
        Addr:              ":8080",
        Handler:           requestID(logging(mux)),
        ReadHeaderTimeout: 5 * time.Second,
        ReadTimeout:       30 * time.Second,
        WriteTimeout:      30 * time.Second,
        IdleTimeout:       120 * time.Second,
    }

    ctx, stop := signal.NotifyContext(context.Background(), syscall.SIGINT, syscall.SIGTERM)
    defer stop()

    go func() {
        if err := srv.ListenAndServe(); err != nil && !errors.Is(err, http.ErrServerClosed) {
            slog.Error("server failed", "err", err)
            os.Exit(1)
        }
    }()

    <-ctx.Done()
    slog.Info("shutting down")
    shutdownCtx, cancel := context.WithTimeout(context.Background(), 30*time.Second)
    defer cancel()
    if err := srv.Shutdown(shutdownCtx); err != nil {
        slog.Error("shutdown failed", "err", err)
    }
}
```

**Traps:**
- Leaking goroutines — every `go func()` needs a way to stop (context cancellation, close signal).
- Unbounded channels — when the producer outpaces the consumer, memory explodes. Use bounded channels + select with default.
- `defer` inside loops — the deferred call accumulates, runs at function exit. Put the body in a function.
- Ignoring `context.Context` — must be the first parameter, must be propagated, must be checked (`ctx.Err()`) before long operations.
- Forgetting `errors.Is` / `errors.As` — string comparison on error messages breaks.
- Data races — run tests with `-race`. Always.
- `sql.Rows` not closed — connection leak. Always `defer rows.Close()`.

### 4.3 Python 3.13+

Strengths: PEP 703 experimental free-threaded build (no-GIL), PEP 744 JIT, FastAPI + Pydantic v2 is genuinely fast now, Django 5.x has async views.

**FastAPI skeleton with dependency injection:**

```python
from contextlib import asynccontextmanager
from typing import Annotated
import asyncpg
from fastapi import FastAPI, Depends, HTTPException, status
from pydantic import BaseModel, Field

@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.pool = await asyncpg.create_pool(
        dsn=settings.database_url,
        min_size=5,
        max_size=20,
        command_timeout=10,
    )
    yield
    await app.state.pool.close()

app = FastAPI(lifespan=lifespan)

async def get_db(request) -> asyncpg.Connection:
    async with request.app.state.pool.acquire() as conn:
        yield conn

class CreateOrder(BaseModel):
    sku: str = Field(min_length=1, max_length=64)
    quantity: int = Field(gt=0, le=10_000)

@app.post("/orders", status_code=status.HTTP_201_CREATED)
async def create_order(
    body: CreateOrder,
    db: Annotated[asyncpg.Connection, Depends(get_db)],
):
    try:
        row = await db.fetchrow(
            "INSERT INTO orders (sku, qty) VALUES ($1, $2) RETURNING id",
            body.sku, body.quantity,
        )
    except asyncpg.UniqueViolationError:
        raise HTTPException(status.HTTP_409_CONFLICT, "duplicate order")
    return {"id": row["id"]}
```

**Traps:**
- Using `requests` (sync) inside an async endpoint — blocks the event loop. Use `httpx.AsyncClient`.
- Mixing sync ORM (SQLAlchemy sync mode) inside async FastAPI — same problem. Use `asyncpg`, `sqlalchemy[asyncio]`, or SQLModel with async.
- `pickle.loads(request.body)` — pre-auth RCE. Never.
- Using `yaml.load()` without `SafeLoader` — RCE via tag injection.
- Pydantic v1 idioms that silently break in v2 (`@validator` → `@field_validator`, `.dict()` → `.model_dump()`).
- `__init__.py` with top-level side effects — breaks reload, breaks packaging.
- Not using `uvloop` + `httptools` in production — leaves 30%+ performance on the table.
- Dependency confusion attacks — **always use a private index or explicit allowlists in pip**; PyPI had a surge of malicious uploads starting late August 2025.

**Free-threaded build note:** PEP 703 removes the GIL. As of Python 3.13, this is opt-in (`python3.13t`). Test your C extensions — most are not yet thread-safe without the GIL. Don't migrate production to free-threaded builds in 2025-2026 without validation.

### 4.4 Node.js 22+ / Bun / NestJS

Strengths: unified language across frontend/backend, huge ecosystem, V8 optimizations, Bun offers drop-in speedup and built-in test runner.

**NestJS skeleton (best for anything enterprise-grade):**

```typescript
// orders.controller.ts
import { Body, Controller, HttpCode, HttpStatus, Post } from '@nestjs/common';
import { IsInt, IsString, Max, MaxLength, Min } from 'class-validator';
import { OrdersService } from './orders.service';

class CreateOrderDto {
  @IsString() @MaxLength(64) sku!: string;
  @IsInt() @Min(1) @Max(10_000) quantity!: number;
}

@Controller('orders')
export class OrdersController {
  constructor(private readonly svc: OrdersService) {}

  @Post()
  @HttpCode(HttpStatus.CREATED)
  async create(@Body() dto: CreateOrderDto) {
    return this.svc.create(dto);
  }
}
```

**Traps:**
- `JSON.parse` on untrusted input without a size limit — memory DoS.
- Not setting `express.json({ limit: '100kb' })` — default is 100kb but check.
- Using `child_process.exec(userInput)` — command injection. Use `execFile` with argv array.
- Prototype pollution via `Object.assign({}, userInput)` — always validate with Zod/class-validator before merging.
- Missing `--max-old-space-size` tuning in containers — OOM kills without graceful shutdown.
- **Axios version check**: versions 1.14.1 and 0.30.4 (published March 31, 2026) contain malware; downgrade to 1.14.0 / 0.30.3 if present.
- Using `localStorage` or unparameterized SQL (yes, still happens) in ORM raw queries.
- Unhandled promise rejections — Node 22 crashes on them by default. Wrap top-level awaits.

**Bun-specific:** `Bun.serve()` is ~3x faster than Node's http for simple routes. Built-in SQLite, built-in test runner, built-in bundler. Tradeoff: smaller ecosystem of battle-tested ops tooling.

### 4.5 Rust (Axum + Tokio + SQLx)

Strengths: memory safety without GC, predictable latency, fearless concurrency. Correctness by construction.

**Axum skeleton:**

```rust
use axum::{extract::State, http::StatusCode, routing::post, Json, Router};
use serde::{Deserialize, Serialize};
use sqlx::PgPool;
use validator::Validate;

#[derive(Deserialize, Validate)]
struct CreateOrder {
    #[validate(length(min = 1, max = 64))]
    sku: String,
    #[validate(range(min = 1, max = 10_000))]
    quantity: i32,
}

#[derive(Serialize)]
struct OrderCreated { id: i64 }

async fn create_order(
    State(pool): State<PgPool>,
    Json(body): Json<CreateOrder>,
) -> Result<(StatusCode, Json<OrderCreated>), (StatusCode, String)> {
    body.validate().map_err(|e| (StatusCode::BAD_REQUEST, e.to_string()))?;
    let row = sqlx::query!(
        "INSERT INTO orders (sku, qty) VALUES ($1, $2) RETURNING id",
        body.sku, body.quantity
    )
    .fetch_one(&pool)
    .await
    .map_err(|e| (StatusCode::INTERNAL_SERVER_ERROR, e.to_string()))?;
    Ok((StatusCode::CREATED, Json(OrderCreated { id: row.id })))
}

#[tokio::main]
async fn main() -> anyhow::Result<()> {
    let pool = PgPool::connect(&std::env::var("DATABASE_URL")?).await?;
    let app = Router::new().route("/orders", post(create_order)).with_state(pool);
    let listener = tokio::net::TcpListener::bind("0.0.0.0:8080").await?;
    axum::serve(listener, app).await?;
    Ok(())
}
```

**Traps:**
- `.unwrap()` in library code — use `?` with proper error types (`thiserror` for libs, `anyhow` for apps).
- Holding a `MutexGuard` across `.await` — deadlock. Use `tokio::sync::Mutex` or drop the guard first.
- `clone()` everywhere — usually a sign the ownership design is wrong. Step back.
- `Arc<Mutex<T>>` when you could use channels — Tokio channels are almost always cleaner.
- Forgetting to configure `sqlx::PgPool` with reasonable `max_connections` — defaults to 10.

---

## Section 5 — API design and contracts

### 5.1 REST, gRPC, or GraphQL?

```
Who is the consumer?
├── External third parties → REST (OpenAPI 3.1), public, versioned, stable
├── Internal services, latency-sensitive → gRPC (Protobuf), typed, streaming
├── Mobile app with deeply nested data needs → GraphQL (BFF pattern)
└── Real-time bidirectional → WebSocket, SSE, or gRPC streaming
```

### 5.2 REST rules that actually matter

- **Idempotency keys on POST/PUT/PATCH** — client sends `Idempotency-Key: <uuid>`, server deduplicates for 24h. Stripe-style. Non-negotiable for payment-like semantics.
- **Cursor-based pagination** — `?cursor=<opaque>&limit=50`. Offset pagination (`?page=100`) scans 100 pages every time. Broken at scale.
- **RFC 7807 Problem Details** for errors — `{"type": "https://errors.example/out-of-stock", "title": "...", "status": 409, "detail": "...", "instance": "..."}`.
- **Versioning** — URI (`/v1/orders`) is pragmatic. Header-based (`Accept: application/vnd.example.v1+json`) is purist. Pick one and stick to it.
- **HTTP status codes** — 201 for Create-with-resource, 202 for async accepted, 204 for no-content, 409 for conflict, 422 for validation (or 400), 429 for rate-limit, 503 for degraded with `Retry-After`.

### 5.3 gRPC rules

- Proto files in a monorepo or a dedicated schema repo; versioned independently.
- Breaking changes are forbidden — reserve field numbers, add new fields with new numbers, never renumber.
- Deadlines on every call. No deadline = infinite retry = thundering herd.
- Use streaming only when truly needed; unary is simpler.

### 5.4 GraphQL guardrails

- Query depth limit (typically 7-10).
- Query complexity scoring (GitHub model).
- DataLoader for every N+1-prone field.
- Persisted queries in production — only allow hash-identified queries from your own clients; block arbitrary queries.

---

## Section 6 — Observability

If you cannot answer "what is this service doing right now?" in 30 seconds, your observability is broken.

### 6.1 The three pillars + one

**Logs, metrics, traces, and events.** All emitted via OpenTelemetry (OTLP) to a backend (Grafana Tempo+Loki+Mimir, Datadog, Honeycomb, New Relic, Elastic).

**Structured logs (JSON)** with at minimum: `timestamp`, `level`, `service`, `trace_id`, `span_id`, `correlation_id`, `tenant_id`, `user_id` (hashed if PII-sensitive), `message`.

**RED metrics** per endpoint: Rate, Errors, Duration (p50/p95/p99). **USE metrics** per resource: Utilization, Saturation, Errors.

**Traces** — instrument at service boundaries automatically (HTTP, gRPC, DB, queue). Add manual spans for meaningful business logic ("validate cart", "reserve inventory"), not for every function.

### 6.2 Alerting hygiene

- Alert on **symptoms** (SLO burn: "95% of checkouts finish in <2s" is burning), not causes ("CPU >80%").
- Every page must be actionable. If the on-call can only ack and wait, it's a warning, not a page.
- Runbook linked from every alert.

---

## Section 7 — Workflows (step-by-step for common tasks)

### Workflow A: Design a new API endpoint

1. **Write the contract first.** OpenAPI/Proto schema with request, response, errors, auth requirements. Commit before implementation.
2. **Identify the invariant** — what must be true after this call? (e.g., "no double-charge", "inventory never negative").
3. **Pick the consistency model** — synchronous DB transaction? Event-driven with eventual consistency? Saga?
4. **Write the validation schema** — every field, with bounds and patterns. Use the language-idiomatic validator.
5. **Write the idempotency strategy** — key source (client header? deterministic hash of body?), dedup store (Redis? unique index?).
6. **Write the observability plan** — what span attributes, what metrics, what error dimensions.
7. **Write the test matrix** — happy path, validation failures, auth failures, idempotency replay, downstream failure, partial failure.
8. **Only then** write the handler code.

### Workflow B: Debug a production incident

1. **Stabilize first.** Is user impact ongoing? Rollback or feature-flag off the suspect change. Revert first, diagnose later.
2. **Check the four golden dashboards** — rate, errors, duration, saturation across the service graph. Find where the anomaly starts.
3. **Pick a representative failing trace.** Walk it span-by-span. Where does the latency or error originate?
4. **Correlate with recent changes** — deployments, feature flags, config changes, DB migrations, upstream dependency updates, external incidents.
5. **Form one hypothesis.** Validate with logs/metrics, not by changing code. Don't "try stuff" — that extends incidents.
6. **Once stable, write the post-mortem within 48 hours.** Blameless, with timeline, root cause, contributing factors, action items with owners.

### Workflow C: Evaluate a new dependency before adding it

1. **License check** — MIT/Apache/BSD OK; GPL/AGPL needs legal review; no-license means don't use.
2. **Maintenance signal** — commits in last 90 days? open issues vs closed? responsive maintainers? Unmaintained dependencies are now an OWASP A03:2025 category.
3. **Dependency tree** — `npm why`, `pip show`, `dotnet list package --include-transitive`, `cargo tree`. Every transitive dep is your problem.
4. **Install script audit** — does it have `postinstall`? Read it. CanisterWorm and similar attacks use postinstall hooks to exfiltrate tokens within seconds of `npm install`.
5. **Alternative check** — is there a stdlib or already-vendored solution? Adding 50KB of dep for a 20-line helper is bad math.
6. **Pin exact version + lockfile commit.** Always.

### Workflow D: Migrate a monolith to services (incrementally)

1. **Do not start.** Confirm you have a real reason (scaling profile, team topology, regulatory). Organizational dysfunction does not fix itself via service boundaries.
2. **Establish observability in the monolith first.** You cannot split what you cannot see.
3. **Identify the seam.** Start with a bounded context that has the least coupling and the clearest contract — often a read-heavy feature (search, recommendations) or a worker-type job.
4. **Strangler Fig** — put the monolith behind an API gateway. Route the target endpoint to a new service. Keep the old code path as a fallback behind a feature flag.
5. **Shadow traffic for 1-2 weeks.** New service sees real traffic but its responses are not returned — just compared to the monolith's.
6. **Cutover with percentage rollout** (1% → 10% → 50% → 100%).
7. **Delete the old code.** The migration is not done until the old path is gone.

---

## Section 8 — Cloud and infrastructure

### 8.1 The cloud-native checklist

- **IaC only** — Terraform, Pulumi, Bicep, CDK. No ClickOps. Drift detection in CI.
- **Least-privilege IAM** — one role per service, permissions scoped to specific resources (not `*`). Use IAM Access Analyzer regularly.
- **Multi-AZ by default** — stateful services (DB, cache, queue) replicated across ≥2 AZs. Stateless compute in ≥2 AZs.
- **Stateless compute** — no local disk persistence. Session state in Redis/DynamoDB. File storage in S3/GCS/Blob.
- **Graceful shutdown** — catch SIGTERM, drain connections, finish in-flight requests (bounded grace period ~30s), close pools, exit.
- **Health checks** — `/healthz/live` (is the process alive?) and `/healthz/ready` (can it serve traffic? deps reachable?). Different semantics.
- **Secrets injection at runtime** — never bake secrets into images. Use IAM IRSA (EKS), Workload Identity (GKE), Managed Identity (AKS), or a secrets operator.
- **SBOM + image signing** — Cosign/Sigstore. Distroless or Chainguard base images. Scan with Trivy/Grype in CI.

### 8.2 Serverless cold starts

Rough cold-start budgets (Lambda + 512MB, cold):
- Rust / Go: 50-150ms
- .NET 10 Native AOT: 80-200ms
- Node.js: 150-400ms
- Python: 200-600ms
- .NET (JIT): 1-3s
- Java JVM (no CRaC): 2-8s

Mitigations: Provisioned Concurrency (Lambda), Native AOT, minimal dep trees, Lambda SnapStart (Java/Python/.NET), or skip serverless for latency-sensitive endpoints.

---

## Section 9 — Event-driven and async

### 9.1 Message broker selection

| Need | Choose |
|------|--------|
| High throughput, replay, ordering per partition, analytics | **Kafka** (or Redpanda) |
| Work queue, low latency, routing flexibility | **RabbitMQ** |
| Cloud-managed, fire-and-forget, pub-sub | **SNS+SQS / Pub/Sub / Service Bus** |
| Scheduled, delayed, retried background jobs | **BullMQ / Hangfire / Sidekiq / Celery** |
| Ultra-low latency internal | **NATS / Redis Streams** |

### 9.2 Outbox pattern (non-negotiable for event emission)

```sql
BEGIN;
INSERT INTO orders (id, sku, qty, status) VALUES (...);
INSERT INTO outbox (event_type, payload, created_at)
  VALUES ('order.created', '{...}', now());
COMMIT;
-- A separate relay reads outbox and publishes to broker with at-least-once guarantees.
```

The relay marks rows as published (or uses logical replication, e.g., Debezium). Consumers are idempotent (deduplicated by event ID).

### 9.3 Consumer rules

- **At-least-once by default** — design handlers to be idempotent.
- **Dead-letter queue** after N retries (typically 3-5) with exponential backoff + jitter.
- **Poison-pill isolation** — a message that consistently fails shouldn't block the partition/queue.
- **Backpressure** — if the consumer can't keep up, slow down the producer or scale the consumer. Never silently drop.

---

## Section 10 — Quick reference lookup tables

### 10.1 "Which HTTP status code?"

| Situation | Code |
|-----------|------|
| Resource created | 201 Created (+ Location header) |
| Async job accepted | 202 Accepted |
| No content to return | 204 No Content |
| Bad request syntax / shape | 400 Bad Request |
| Not authenticated | 401 Unauthorized |
| Authenticated but forbidden | 403 Forbidden |
| Not found | 404 Not Found |
| Method not allowed on resource | 405 Method Not Allowed |
| Conflict (duplicate, version mismatch) | 409 Conflict |
| Gone (deleted permanently) | 410 Gone |
| Validation failed | 422 Unprocessable Entity (or 400) |
| Rate limit | 429 Too Many Requests (+ Retry-After) |
| Server error | 500 Internal Server Error |
| Downstream unreachable | 502 Bad Gateway |
| Overloaded / maintenance | 503 Service Unavailable (+ Retry-After) |
| Downstream timeout | 504 Gateway Timeout |

### 10.2 "Which isolation level?"

| Scenario | Level |
|----------|-------|
| Read-heavy dashboard | READ COMMITTED |
| Multi-row workflow with decisions | REPEATABLE READ |
| Money movement, inventory decrement | SERIALIZABLE (with retry) |
| Bulk analytics (OLAP) | READ COMMITTED + statement-level snapshot |

### 10.3 "Which primary key strategy?"

| Use case | Choice |
|----------|--------|
| Default relational PK | `bigint GENERATED ALWAYS AS IDENTITY` |
| Distributed generation, sortable | UUIDv7 (time-ordered, index-friendly) |
| External-facing ID (no enumeration) | UUIDv4 exposed + internal bigint |
| Multi-tenant sharding | composite `(tenant_id, entity_id)` |
| Must avoid DB coordination | Snowflake-style / ULID / KSUID |

**Never expose sequential bigints directly** in URLs for resources that shouldn't be enumerable (user profiles, order IDs) — enables IDOR attacks.

---

## Using this skill: quick triggers

When any of these appear in the user's message, engage the relevant section:

- "design an API / service / endpoint" → Section 5 + Workflow A
- "secure this" / "pentest" / "vulnerability" / "CVE" → Section 1
- "slow query" / "N+1" / "query plan" / "database is slow" → Section 3
- "debug production" / "incident" / "5xx errors" → Workflow B + Section 6
- "monolith to microservices" / "extract service" → Workflow D + Section 2
- "pick a database" / "Postgres vs Mongo vs ..." → Section 3.1
- "pick a message broker" / "Kafka vs RabbitMQ" → Section 9.1
- ".NET / C# / ASP.NET" → Section 4.1
- "Go / golang" → Section 4.2
- "Python / FastAPI / Django" → Section 4.3
- "Node / NestJS / Bun" → Section 4.4
- "Rust / Axum / Tokio" → Section 4.5
- "adding this package" / "new dependency" → Workflow C + Section 1.2
- "Kubernetes" / "Lambda" / "serverless" / "IaC" → Section 8

When unsure, default to the priority matrix at the top: security first, data correctness second, boundaries third, everything else after.
