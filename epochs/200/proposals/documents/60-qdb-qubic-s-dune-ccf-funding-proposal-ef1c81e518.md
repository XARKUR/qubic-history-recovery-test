# Qdb: Qubic's Dune - CCF Funding Proposal

## Executive Summary

**Project Name:** Qdb  
**Organization:** Kairos Tek  
**Proposal Date:** 05/02/2026  
**Total Requested:** 40,000 USD equivalent to 80 billion QUBIC (at 0.0000005 USD per QUBIC)  
**Wallet Address:** FQJYDAAGMPTOSGYNHYOVVAANQMAAPIGZDCWKEUHBFCOTVANGLMBWMDRAHZMB  


**Voting Options:**

❌ 0: No, I don't want  
✅ 1: Yes, approve the budget of 80 billion QUBIC.

---

### Overview

Qdb  is a lean, focused Qubic data analytics backend and query interface 
designed to solve the most critical problem facing the Qubic ecosystem: the lack 
of visibility into smart contract-generated transfers that do not appear in the 
Qubic explorer.

This  will be **integrated into Easy Connect**, Kairos Tek's existing platform, 
providing:
- **SQL-based query interface** for Qubic data analysis  
- **REST API access** for seamless integrations
- **Query management** (save, history, export)

This is an agile  approach prioritizing:
- **Core functionality** over advanced features
- **Fast delivery** over extensive scope
- **User value** over technical complexity
- **Sustainable base** for future enhancement

Through Qdb, Kairos Tek will launch a fully functional SQL-based query platform with dashboards, exports, and API access—enabling the entire Qubic community to see smart contract transfers for the first time.

---

## 1. Problem Statement

### Current Situation

Qubic has achieved remarkable technical capabilities: 15.52 million transactions per second (verified by CertiK on mainnet), deterministic smart contract execution, and feeless transactions.


However, an important limitation undermines user confidence and ecosystem adoption.

#### The Explorer Limitation

The current Qubic explorer displays user-initiated transactions but does NOT display:
- **Smart contract-generated transfers** using QPI functions like `qpi.transfer()`
- **Token transfers** executed by smart contracts
- **Payment distributions** from contracts (mining rewards, dividends...)

#### Affected Stakeholders & Pain Points

This limitation generally affects the entire Qubic ecosystem. Some examples:

1. **QX Exchange Users** - Cannot verify their buys/sells or track transaction history
2. **Qubic Miners** - Receive rewards but have no visibility into payment timing or amounts
3. **Qubic Capital Investors** - Cannot see dividend payments or returns
4. **QMine Investors** - Cannot track when returns are distributed
5. **Wallet Providers** - Cannot integrate transfer data into their platforms
6. **Developers** - Lack of complete data infrastructure for analysis and debugging

### Business Impact

This transparency gap creates:
- Reduced user confidence in the network
- Adoption barriers for new projects
- Integration challenges for exchanges and wallets
- Data analysis limitations for researchers and traders
- Reputation risk for a network built on transparency

---

## 2. Proposed Solution: Qdb  Architecture

Qdb represents the most efficient path to solving Qubic's smart contract transfer visibility limitation.


### 2.1 Core Components

#### A. Query Module (Integrated into Easy Connect)
A lightweight query interface integrated into Easy Connect's existing platform:
- **SQL Query Interface** - Text editor with basic syntax highlighting
- **Query History** - Save and manage past queries  
- **Query Results Display** - Paginated table view with sorting
- **Data Export** - CSV and JSON formats
- **User Authentication** - Leverages Easy Connect's existing auth system

**Note:** This is a module within Easy Connect, not a standalone application.

#### B. Lite Core Node
A single Qubic Lite Core Node (64 GB RAM, 8 cores per Network Guardians spec[4]) configured to:
- Index all transactions and transfers from Qubic network
- Extract QPI transfer data invisible to the explorer
- Maintain indexed data in PostgreSQL for fast queries
- Provide stable data source for platform

#### C. REST API (For Easy Connect Premium Users)
A developer-focused API enabling Easy Connect premium subscribers to:
- Execute SQL queries programmatically via HTTP POST
- JSON response format with pagination
- Rate limiting based on subscription tier
- API key authentication (managed through Easy Connect dashboard)
- API documentation for integration

#### D. Data Pipeline
- Continuous sync from Lite Core Node to indexed database
- Smart contract transfer extraction from logs
- Data validation and normalization
- PostgreSQL for indexed queries

### 2.2 Architecture Diagram

Three-tier  architecture:
- **Presentation Layer:** Angular-based web portal + REST API endpoints
- **Processing Layer:** .NET API, transfer extraction logic
- **Data Layer:** Lite Core Node, PostgreSQL for blockchain data, PostgreSQL for application metadata in separated contexts

---

## 3.  Features (In Scope)

### 3.1 Essential Features

#### 1. SQL Query Editor
- Text editor with syntax highlighting
- Query history (last 20 queries)
- Execute, clear, copy buttons
- Error messaging

#### 2. Query Results
- Paginated table view (50 rows/page)
- Column sorting and basic filtering
- Row count display

#### 3. Query History & Management
- Save queries with custom titles
- View last 20 executed queries
- Quick re-execute from history
- Delete saved queries

#### 4. Data Export
- Export query results as CSV
- Export query results as JSON
- Direct browser download

#### 5. REST API 
- Execute queries directly via API

#### 6. Authentication & Access Control
- **Integration with Easy Connect auth system**
- Users access queries through Easy Connect dashboard
- API keys managed through Easy Connect account settings
- No separate registration/login required

---

## 4. Technical Implementation Details

### 4.1 Technology Stack

| Component | Technology | Rationale |
|-----------|-----------|-----------|
| **Frontend** | Angular 18 + TypeScript | Enterprise-grade, TypeScript-native, robust component architecture |
| **Backend API** | .NET 8 Web API | High performance, type-safe, enterprise-ready API framework |
| **Query Engine** | PostgreSQL (blockchain data) + PostgreSQL (metadata) | Optimized for analytics and time-series data |
| **Lite Core Node** | Latest version from public Qubic repository | Best version available |
| **Data Pipeline** | Data ingestion service + PostgreSQL | Efficient data pipeline |
| **Authentication** | JWT | Standard, lightweight |

### 4.2 Data Schema (Preliminar version)

The  uses PostgreSQL with two separated schemas for clear domain boundaries:

#### **Blockchain Schema** (High-Volume Data)

Core tables for Qubic network data indexed from Lite Core Node:

```sql
blockchain.transfers
├─ id (BIGSERIAL PRIMARY KEY)
├─ tick_number (BIGINT, indexed)
├─ source_id (VARCHAR(60), indexed)
├─ dest_id (VARCHAR(60), indexed)  
├─ amount (BIGINT)
├─ transfer_type (VARCHAR(20)) -- 'QU_TRANSFER', 'ASSET_TRANSFER'
├─ asset_name (VARCHAR(8), nullable) -- For asset transfers
├─ timestamp (TIMESTAMP, indexed)
└─ epoch (INT)

blockchain.ticks
├─ tick_number (BIGINT PRIMARY KEY)
├─ epoch (INT, indexed)
├─ timestamp (TIMESTAMP)
├─ transaction_count (INT)
└─ total_volume (BIGINT)

```

**Indexes for Performance:**
- `tick_number` (range queries)
- `source_id`, `dest_id` (entity lookups)
- `asset_name` (asset lookups)
- `timestamp` (time-based queries)

---

#### **Application Schema** (Metadata)

Tables for user accounts, saved queries, and dashboards:

```sql
application.users
├─ id (SERIAL PRIMARY KEY)
├─ email (VARCHAR(255) UNIQUE)
├─ password_hash (VARCHAR(255))
├─ display_name (VARCHAR(100))
├─ created_at (TIMESTAMP)
└─ last_login (TIMESTAMP)

application.queries
├─ id (SERIAL PRIMARY KEY)
├─ user_id (INT, FK → users.id)
├─ title (VARCHAR(255))
├─ query_text (TEXT)
├─ is_public (BOOLEAN, default false)
├─ execution_count (INT, default 0)
├─ created_at (TIMESTAMP)
└─ updated_at (TIMESTAMP)


```

**Schema Separation Benefits:**
- **Isolation**: Blockchain data operations don't impact user metadata queries
- **Scaling**: Can optimize indexes differently for each domain
- **Backups**: Separate backup strategies (frequent for blockchain, less for metadata)
- **Permissions**: Fine-grained access control per schema

**Note:** Table structures may evolve over time, but the core schema and concepts are expected to remain stable.

### 4.3 Query Module UI (Integrated into Easy Connect)

The Qdb query module consists of **2 main screens** integrated into Easy Connect's existing platform, replacing the previously planned 7 standalone screens.

**Integration Approach:** Qdb is not a separate web application. It is a feature module within Easy Connect that users access through their existing Easy Connect dashboard.

---

#### **Screen 1: SQL Query Editor** (Main Interface)

**Purpose:** Execute SQL queries against indexed Qubic blockchain data

![SQL Query Editor - sample](qdb-new-query.png)

**Key Features:**
- SQL editor with syntax highlighting for PostgreSQL
- Line numbers for easy reference
- Execute button 
- Paginated results table
- Column sorting 
- Export dropdown (CSV/JSON)
- Save query button (stores to user's history)
- Clear/Copy buttons for editor

**Components:**
- Query Editor (Monaco Editor or similar)
- Results Table 
- Export Menu
- Action Buttons

---

#### **Screen 2: Query History**

**Purpose:** Manage and re-execute saved queries

![Query History - sample](qdb-query-history.png)

**Key Features:**
- List of saved queries
- Query title (editable) and SQL preview
- Quick actions:
  - **Run Again** - Execute query immediately
  - **Edit** - Load into editor for modification
  - **Delete** - Remove from history
- Search queries

**Components:**
- Query List (cards or table)
- Search Bar
- Sort Dropdown
- Action Buttons per query
- Search query input

---

**Subscription Tiers (Easy Connect):**
- **Free:** Web UI, 10 queries/day via API 
- **Premium:** Web UI, 1000 queries/day via API
- **Enterprise:** Web UI, unlimited queries via API, technical support, custom SLA

---

## 5. Lean Team Structure (4 People)

### 5.1 Team Roles & Responsibilities

| Role | FTE | Primary Responsibilities | Key Skills |
|------|-----|-------------------------|-----------|
| **Product Manager / Project Manager** | 1 | Vision, roadmap, requirements, stakeholder communication, sprint planning, risk management | Product strategy, Qubic domain knowledge, team coordination |
| **UX/UI Designer + QA Engineer** | 1 | Interface design, user flows, wireframes, visual design, quality assurance, test planning, bug tracking | Figma, design systems, user testing, manual/automated testing |
| **Backend Developer** | 1 | Lite Core Node integration, data pipeline, API development, database schema, query optimization | C++, PostgreSQL, API design, blockchain data structures, C# .Net API |
| **Full Stack Developer** | 1 | Frontend development, API integration, authentication, deployment, DevOps, monitoring | Angular/TypeScript, Docker, CI/CD, frontend frameworks |

**Team Availability:**
- Core team: 4 months part-time commitment  
- Overlap: Backend and Full Stack can cross-support on API and data pipeline
- PM/Designer: Ongoing support post-launch for user feedback and iteration

---

## 6. Project Timeline & Milestones

### 6.1 12-Week Development Roadmap

#### Phase 1: Foundation (Weeks 1-4)
**Goal:** Establish technical infrastructure and project foundations

**Week 1:**
- Project kickoff and team alignment
- Dev environment setup (Docker, Lite Core Node, PostgreSQL)
- Database schema design (transfers, queries, users, dashboards)
- API specification (OpenAPI/Swagger)
- UX wireframes for core screens

**Week 3:**
- Lite Core Node deployment and configuration
- Data pipeline skeleton (log parsing, basic ingestion)
- Backend API scaffolding (authentication, basic endpoints)
- Frontend project setup (React + Vite, routing, state management)
- CI/CD pipeline setup (GitHub Actions, automated tests)

**Deliverables:**
- Working dev environment
- Database schema documented
- API spec approved
- UX wireframes complete
- Lite Core Node operational

---

#### Phase 2: Development (Weeks 5-12)
**Goal:** Build core features and integrate components

**Weeks 3-4: Data Layer**
- Complete data pipeline (log parsing → PostgreSQL)
- Implement transfer indexing logic
- Create database for common queries
- Performance optimization for large datasets  

**Weeks 5-6: Backend API**
- Query execution endpoint (SQL validation, rate limiting)
- Dashboard CRUD endpoints
- Query history and saved queries
- API documentation and examples 

**Weeks 7-8: Frontend**
- SQL query editor (syntax highlighting, autocomplete)
- Query results table (pagination, sorting, filtering)
- Data export (CSV, JSON)

**Deliverables:**
- Working API with all endpoints
- Functional frontend with core features
- Data pipeline ingesting live data

---

#### Phase 3: Testing & Launch (Weeks 13-16)
**Goal:** Finalize, test, and deploy to production

**Week 13: Integration Testing**
- End-to-end testing (user flows)
- Performance testing (load testing with realistic data volume)
- Security testing (OWASP top 10, SQL injection prevention)
- Cross-browser testing

**Week 14: Beta Testing**
- Deploy to staging environment
- Invite 20-30 community beta testers
- Collect feedback on UX and bugs
- Fix critical issues
- Prepare documentation (user guides, API docs)

**Week 15: Production Preparation**
- Production server setup and hardening
- Database migration and indexing
- Monitoring and alerting (Datadog/CloudWatch)
- Backup and disaster recovery procedures

**Week 16: Launch**
- Deploy to production
- Announce to Qubic community (Discord, Twitter, blog post)
- Monitor for issues and respond to user feedback
- Begin collecting analytics (usage metrics, query patterns)
- Post-launch retrospective

**Deliverables:**
- Live production platform
- Documentation (user guides, API reference)
- Community announcement materials
- Post-launch monitoring dashboard

---

### 6.2 Project Timeline Summary Table

| Phase | Duration | Key Deliverables | Effort (Hours) |
|-------|----------|-------------------|----------------|
| **Phase 1: Foundation** | Weeks 1-4 | Infrastructure, schema, API spec, dev environment | 220 |
| **Phase 2: Development** | Weeks 5-12 | Working API, frontend, data pipeline, Easy Connect integration | 380 |
| **Phase 3: Testing & Launch** | Weeks 13-16 | QA testing, production deploy, documentation, live product | 200 |
| **TOTAL** | 16 weeks (4 months) | Full  launch | **800** |

---

## 7. Budget Breakdown

### 7.1 Personnel Costs

**Team Composition:**
- 1 Product Manager / Project Manager (part-time)
- 1 UX/UI Designer + QA Engineer (part-time)
- 1 Backend Developer (part-time)
- 1 Full Stack Developer (part-time)

**Hourly Rate:** $50/hour

**Total Project Duration:** 16 weeks (4 months)

| Role | Total Hours | Avg Hours/Week | 4-Month Cost |
|------|-------------|----------------|--------------|
| PM/Project Manager | 160 | 10 hrs/week | $8,000 |
| Backend Developer | 280 | 17.5 hrs/week | $14,000 |
| Full Stack Developer | 280 | 17.5 hrs/week | $14,000 |
| UX/UI + QA | 80 | 5 hrs/week | $4,000 |
| **Total Personnel** | **800** | **50 hrs/week** | **$40,000** |

**Breakdown by Month:**

| Role | Hrs/Month | Monthly Cost |
|------|-----------|--------------|
| PM/Project Manager | 40 | $2,000 |
| Backend Developer | 70 | $3,500 |
| Full Stack Developer | 70 | $3,500 |
| UX/UI + QA | 20 | $1,000 |
| **Monthly Total** | **200** | **$10,000** |

**Note:** Team members work part-time on this project, allowing for cost-effective execution while maintaining quality.

### 7.2 Infrastructure Costs (3 Months)

| Component | Specification | Monthly Cost | 3-Month Total |
|-----------|---------------|--------------|---------------|
| **Web Server** | 2 vCPU, 4GB RAM, CDN | $100 | $300 |  
| **Database Server** | 4 vCPU, 16GB RAM | $300 | $900 | 
| **Lite Core Node** | 64GB RAM | $400 | $1200 |
| **Domain & SSL** | Annual, includes renewal | - | $20 |
| **Monitoring** | CloudWatch, Datadog basic | $50 | $150 |
| **Infrastructure Total** | | | **$2,570** |

---

## 7.3 TOTAL BUDGET SUMMARY

| Category | Cost |
|----------|------|
| **Personnel (800 hours @ $50/hr)** | $40,000 |
| **Infrastructure (3 months)** | $2,570 |
| **GROSS TOTAL** | **$42,570** |
| **CCF Request** | **$40,000** (personnel only) |

**Note:** Recurring infrastructure costs are planned to be covered through license payments in the Premium and Enterprise modalities, to ensure a sustainable environment.

---

## 8. Success Metrics & KPIs


| Metric | Target | Rationale |
|--------|--------|-----------|
| **Product Availability** | 99% uptime | Reliability baseline |
| **API Reliability** | 99% | Service dependability |
| **Data Freshness** | <2 minutes lag | Near real-time analytics |
| **Documentation** | 100% of features | User enablement |

---

## 9. Governance & Reporting

### 9.1 Reporting Schedule

1. **Bi-Weekly Community Updates** - Feature previews, community feedback incorporation
2. **Milestone Completion Reports** - Deliverables verification, next phase readiness

### 9.2 Milestone Verification

- **Phase 1 Complete:** Schema designed, API scaffolded, dev environment ready
- **Phase 2 Complete:** API operational, data pipeline working, frontend integrated
- **Phase 3 Complete:** Product live, documentation available, launch successful

Each milestone requires PM review and community acknowledgment before proceeding.

---

## 10. Kairos Tek: Track Record & Credibility


**Kairos Tek** is a specialized Web3 development team focused on Qubic ecosystem innovation:

- **Proven execution** - Delivered QBuild and EasyConnect to production quality
- **Deep Qubic expertise:** Extensive knowledge of Qubic architecture, smart contracts, QPI and frontend integration
- **Experience** - More than a decade building products
- **Ability to perform under pressure** - Winners of the MAD Hack and Raise Your Hack hackathons
- **Community trust** - Established relationships with Qubic ecosystem and community leaders

---

## 11. Conclusion

Qdb represents the most efficient path to solving Qubic's smart contract transfer visibility limitation. By focusing on core functionality with a lean 4-person team, this $40,000 investment will deliver:

✅ **Immediate Impact** - First platform to show smart contract transfers  
✅ **User Value** - Solves real problems for users, projects, miners, investors  
✅ **Fast Delivery** - 12-week launch timeline to market impact  
✅ **Team Confidence** - Kairos Tek's track record ensures execution
