# AIDLC Terminology Glossary

## Core Terminology

### Phase vs Stage

**Phase**: One of the three high-level lifecycle phases
- 🔵 **INCEPTION PHASE** — Planning & Architecture (WHAT and WHY)
- 🟢 **CONSTRUCTION PHASE** — Design, Implementation & Test (HOW)
- 🟡 **OPERATIONS PHASE** — Deployment & Monitoring

**Stage**: An individual workflow activity within a phase
- Examples: Workspace Detection, Requirements Analysis, Code Generation
- Stages can be ALWAYS-EXECUTE or CONDITIONAL

**Usage Examples**:
- ✅ "The CONSTRUCTION phase contains 7 stages"
- ✅ "The Code Generation stage is always executed"
- ❌ "The Requirements Assessment phase" (should be "stage")
- ❌ "The CONSTRUCTION stage" (should be "phase")

---

## Three-Phase Lifecycle

### INCEPTION PHASE

**Purpose**: Planning and architectural decisions  
**Focus**: Determine WHAT to build and WHY  
**Location**: `aidlc-docs/inception/`

**Stages**:
- Workspace Detection (ALWAYS)
- Reverse Engineering (CONDITIONAL — Brownfield only)
- Requirements Analysis (ALWAYS — Adaptive depth)
- Cross-Team Alignment (WORKSHOP-SPECIFIC)
- User Stories (CONDITIONAL)
- Workflow Planning (ALWAYS)
- Application Design (CONDITIONAL)
- Units Generation (CONDITIONAL)

### CONSTRUCTION PHASE

**Purpose**: Detailed design and implementation  
**Focus**: Determine HOW to build it  
**Location**: `aidlc-docs/construction/`

**Stages** (per-unit loop):
- Functional Design (CONDITIONAL)
- NFR Requirements (CONDITIONAL)
- NFR Design (CONDITIONAL)
- Infrastructure Design (CONDITIONAL)
- Code Generation (ALWAYS) — Part 1: Planning, Part 2: Generation
- Build and Test (ALWAYS)

### OPERATIONS PHASE

**Purpose**: Deployment and operational readiness  
**Focus**: How to DEPLOY and RUN it  
**Location**: `aidlc-docs/operations/`

**Stages**:
- Production Readiness Assessment (WORKSHOP)
- Deployment Planning (WORKSHOP)
- Post-Workshop Tasks (WORKSHOP)

---

## Architecture Terms

### Unit of Work

A logical grouping of related functionality for development purposes. Used during planning and decomposition.

**Usage**: "We need to decompose this feature into units of work"

### Service

An independently deployable component. In SecureEdge, services communicate via gRPC.

**Usage**: "The Device Management Service handles enrollment"

### Module

A logical grouping within a service. Modules are NOT independently deployable.

**Usage**: "The certificate module within the enrollment service"

### Component

A reusable building block — classes, functions, or packages.

**Usage**: "The DeviceValidator component validates device certificates"

---

## SecureEdge-Specific Terms

| Term | Definition |
|------|------------|
| Device | An enrolled endpoint (laptop, mobile, etc.) |
| Posture | Security state of a device (OS version, firewall, etc.) |
| Enrollment | Process of registering a device with SecureEdge |
| App Catalog | Collection of applications available to devices |
| Config Sync | Process of syncing configuration to devices |
| ZTNA | Zero Trust Network Access |
| SASE | Secure Access Service Edge |

---

## Artifact Types

### Plans

Documents with checkboxes that guide execution.

- Located in `aidlc-docs/inception/plans/` or `aidlc-docs/construction/plans/`
- Examples: `code-generation-plan.md`, `units-plan.md`

### Artifacts

Generated outputs from executing plans.

- Located in various `aidlc-docs/` subdirectories
- Examples: `requirements.md`, `architecture.md`, `functional-design.md`

### State Files

Files tracking workflow progress.

| File | Purpose |
|------|---------|
| `aidlc-state.md` | Checkbox progress for all stages |
| `audit.md` | Complete audit trail of decisions |

---

## Stage Terminology

### Planning vs Generation

- **Planning**: Creating a plan with questions and checkboxes
- **Generation**: Executing the plan to create artifacts

### Depth Levels

| Level | When Used |
|-------|-----------|
| Minimal | Bug fixes, config changes, single-file edits |
| Standard | New features, multi-file changes |
| Comprehensive | New services, architecture changes, security-sensitive |

---

## Workshop-Specific Terms

| Term | Definition |
|------|------------|
| Cross-Team Alignment | Stage where teams agree on shared interfaces |
| Interface Contract | Agreed API between two teams |
| File Ownership | Which team can edit which files |
| Integration Stage | Connecting cross-team interfaces |
| Production Readiness | Assessment of what's needed before prod |

---

## Abbreviations

| Abbrev | Meaning |
|--------|---------|
| AIDLC | AI-Driven Development Life Cycle |
| NFR | Non-Functional Requirements |
| UOW | Unit of Work |
| gRPC | Google Remote Procedure Call |
| REST | Representational State Transfer |
| K8s | Kubernetes |
| OTel | OpenTelemetry |
