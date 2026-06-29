# 🗄️ DBMS — Complete Placement & Interview Notes
> **Covers:** Introduction → Architecture → ER Model → Normalization → Transactions → SQL → Indexing → Sharding  
> **Audience:** CS/IT students preparing for campus placements, FAANG interviews, viva  
> **Style:** Concept + Diagram + Java + C++ code side-by-side

---

## 📋 TABLE OF CONTENTS

| # | Topic | Key Interview Weight |
|---|-------|---------------------|
| 1 | [DBMS Introduction](#1-dbms-introduction) | ⭐⭐ |
| 2 | [DBMS Architecture & CAP Theorem](#2-dbms-architecture--cap-theorem) | ⭐⭐⭐ |
| 3 | [Data Abstraction & Independence](#3-data-abstraction--independence) | ⭐⭐ |
| 4 | [Types of Data Models](#4-types-of-data-models) | ⭐⭐ |
| 5 | [ER Model](#5-er-model) | ⭐⭐⭐ |
| 6 | [Relational Model](#6-relational-model) | ⭐⭐⭐ |
| 7 | [Types of Keys](#7-types-of-keys) | ⭐⭐⭐⭐ |
| 8 | [Normalization (1NF → BCNF)](#8-normalization-1nf--bcnf) | ⭐⭐⭐⭐⭐ |
| 9 | [Denormalization](#9-denormalization) | ⭐⭐⭐ |
| 10 | [Transactions & ACID](#10-transactions--acid) | ⭐⭐⭐⭐⭐ |
| 11 | [Concurrency Control](#11-concurrency-control) | ⭐⭐⭐⭐ |
| 12 | [SQL Commands & Queries](#12-sql-commands--queries) | ⭐⭐⭐⭐⭐ |
| 13 | [Indexing & B+ Trees](#13-indexing--b-trees) | ⭐⭐⭐⭐ |
| 14 | [Sharding & Scaling](#14-sharding--scaling) | ⭐⭐⭐ |
| 15 | [Security: RBAC & Encryption](#15-security-rbac--encryption) | ⭐⭐ |
| 16 | [MCQ Answer Key](#16-mcq-answer-key) | — |

---

## 1. DBMS Introduction

### 🧠 What is a DBMS?

> A **Database Management System (DBMS)** is software that lets you **create, store, retrieve, modify, and administer** data in an organized, secure, and efficient way.

**Real-life analogy:**  
A college library — books arranged by subject → you go straight to "C-Programming" rack instead of digging through piles. The catalog = DBMS index.

---

### ✅ Why DBMS over plain files?

```
Without DBMS (Excel files per dept)          With DBMS
─────────────────────────────────────        ─────────────────────────────────
Admissions → student address (v1)            Single STUDENT table
Fees        → student address (v2)     ───▶  All depts READ same copy
Exams       → student address (v3)           One update = everywhere updated
                 ↑ data clash!
```

---

### 📌 Main Advantages

| Advantage | One-liner | Example |
|-----------|-----------|---------|
| **Security** | Control who sees/edits what | Fee clerk sees fees only; Principal sees marks too |
| **No Redundancy** | One copy of truth | Student address stored once, used everywhere |
| **ACID Accuracy** | All-or-nothing updates | Library book issue: both "Issued=YES" and "Copies−1" written together |
| **Speed (Indexing)** | B-tree O(log n) / Hash O(1) | Student ID lookup in milliseconds |
| **Scalability** | Add RAM/servers without code changes | 5× more students → attach more storage |

---

### 🗂️ Popular Database Types

| Type | Core Structure | Best For | Real Example |
|------|---------------|----------|--------------|
| **Relational (SQL)** | Tables + keys, ACID | Structured, transactional | UPI payments, college ERP |
| **Document** | JSON/BSON documents | Flexible schemas | Swiggy menus |
| **Key-Value** | Hash map | Ultra-fast lookups | Flipkart session storage |
| **Graph** | Nodes + edges | Network/relationships | Fraud detection, LinkedIn |
| **Time-Series** | Time-stamped records | Sensors, stock ticks | IMD weather data |
| **Object-Oriented** | Persistent objects | Complex, behavior-rich | Medical device data |

> 💡 **Interview tip:** "Choose your DB by the problem, not by popularity. SQL is a language — many NoSQL DBs now support SQL-like queries too."

---

## 2. DBMS Architecture & CAP Theorem

### 🏗️ Three Architecture Tiers

```
ONE-TIER                TWO-TIER               THREE-TIER
┌─────────────┐         ┌──────────┐            ┌──────────┐
│  UI + DB    │         │  Client  │            │  Client  │
│  (same box) │         │  (Java   │            │ (Mobile) │
└─────────────┘         │   GUI)   │            └────┬─────┘
                        └────┬─────┘                 │ HTTP
 SQLite on laptop            │ JDBC            ┌─────▼──────┐
 (single user)          ┌────▼─────┐           │ App Server │
                        │  MySQL   │           │ (Node.js)  │
                        │  Server  │           └─────┬──────┘
                        └──────────┘                 │ SQL
                                               ┌─────▼──────┐
                                               │ PostgreSQL  │
                                               └────────────┘
```

| Tier | Multi-user? | Business Logic | Example |
|------|------------|----------------|---------|
| One-tier | ❌ Single | Embedded | Python + SQLite script |
| Two-tier | ✅ Limited | On client | Lab PCs → MySQL server |
| Three-tier | ✅ Best | Middle server | Any modern web app |

---

### 🌐 Distributed Databases

Data split across geographically separated servers, presented as one logical DB.

```
Delhi Campus         Mumbai Campus        Bengaluru Campus
┌─────────┐          ┌─────────┐          ┌─────────┐
│ Server A│◄────────▶│ Server B│◄────────▶│ Server C│
│ (Delhi  │  SYNC    │ (Mumbai │  SYNC    │(Bengaluru│
│  data)  │          │  data)  │          │  data)  │
└─────────┘          └─────────┘          └─────────┘
       ▲                  ▲                    ▲
       └──────────────────┴────────────────────┘
              Single logical database view
```

---

### ⚡ CAP Theorem (Must-Know for Interviews!)

> In a distributed system experiencing network partition, you can guarantee **at most 2 of 3**:

```
                    Consistency (C)
                         ●
                        /|\
                       / | \
                      /  |  \
            CP       /   |   \    CA
         MongoDB    /    |    \  Oracle
         HBase     /  Pick 2   \ MySQL
                  /      |      \
                 ●────────────────●
      Partition     AP Category    Availability
      Tolerance   Cassandra, RIAK      (A)
         (P)        CouchDB
```

| System | Chooses | Reason |
|--------|---------|--------|
| UPI / PayTM | **C + P** | Money must never diverge; brief downtime OK |
| College notice board | **A + P** | Old notice > no notice during Wi-Fi outage |
| LAN scoreboard | **C + A** | Partitions rare, scores must be live |

> 💡 **Interview answer:** "Financial apps like Paytm choose **Consistency + Partition Tolerance (CP)** because monetary transfers must never produce conflicting balances, even if the system becomes temporarily unavailable."

---

## 3. Data Abstraction & Independence

### 🔍 Three Levels of Abstraction

```
 ┌─────────────────────────────────────────────────────┐
 │              VIEW LEVEL  (highest)                  │
 │  Warden sees: RollNo, Name, FeeStatus               │
 │  Exam branch sees: RollNo, Name, Marks              │
 ├─────────────────────────────────────────────────────┤
 │              LOGICAL LEVEL                          │
 │  STUDENT(RollNo INT PK, Name VARCHAR, Dept CHAR)    │
 │  MARKS(RollNo FK, CourseCode FK, Grade FLOAT)       │
 ├─────────────────────────────────────────────────────┤
 │              PHYSICAL LEVEL  (lowest)               │
 │  8KB blocks on SSD, RAID-1, B-tree index on RollNo  │
 └─────────────────────────────────────────────────────┘
         Changes flow DOWN; knowledge flows UP
```

---

### 🔄 Data Independence

| Type | Definition | Example |
|------|-----------|---------|
| **Logical Independence** | Change logical schema → apps unaffected | Split Address into Street, City, Pin → hostel app keeps working |
| **Physical Independence** | Change storage → SQL unchanged | HDD → SSD migration, add index → no query rewrite needed |

> 💡 **Key point:** Physical independence is *easier* to achieve than logical independence.

---

## 4. Types of Data Models

### 📊 Quick Comparison

| Model | Structure | Best For | Limitation |
|-------|-----------|----------|------------|
| **Hierarchical** | Parent → child tree | One-to-many chains | Each child has ONE parent only |
| **Network** | Multi-parent sets | Complex M:N | Complex pointer traversal |
| **Relational** | Tables + keys | General purpose | Joins can be expensive |
| **Object-Oriented** | Persistent objects | Behavior-rich | Niche, not widely used |
| **ER** | Blueprint diagram | Conceptual design | Design phase only |

### 🌳 Hierarchical vs Network (Visual)

```
HIERARCHICAL                          NETWORK
    College                        Student1  Student2  Student3
   /       \                           \    /   |   \  /
Dept1     Dept2                         Club1  Club2  Club3
 |   \       |                              \    /
CS101  CS102  EC101                         Event1
```

---

## 5. ER Model

### 📐 ER Diagram Notation

```
 ┌───────────┐            SHAPE → MEANING
 │  ENTITY   │            ────────────────────────────────────
 └───────────┘            Rectangle        → Strong entity
 ╔═══════════╗            Double rectangle → Weak entity
 ║  WEAK ENT ║            Ellipse          → Attribute
 ╚═══════════╝            Double ellipse   → Multivalued attribute
                          Dashed ellipse   → Derived attribute
   (Attr)                 Underlined       → Primary key attribute
  ╱     ╲                 Diamond          → Relationship
Attr   Attr               Double diamond   → Identifying relationship
                          Line             → Connection
  ◇  Relationship         |—|  1:1    |—<  1:N    >—<  M:N
```

---

### 🔑 ER Terminology

| Term | Definition | Example |
|------|-----------|---------|
| **Entity** | Real-world object with a unique key | Individual Student, specific Course |
| **Weak Entity** | No own primary key; depends on owner | Occupancy (depends on Hostel Room) |
| **Attribute** | Property of an entity | RollNo (key), Name, Age (derived) |
| **Relationship** | Association between entities | Student ENROLLED_IN Course |
| **Cardinality 1:1** | One A ↔ exactly one B | Student ↔ Library Card |
| **Cardinality 1:N** | One A ↔ many B | Instructor teaches many Courses |
| **Cardinality M:N** | Many A ↔ many B | Students ↔ Courses |

---

### 🏗️ Full ER Diagram — Netflix (Simplified)

```
(Firstname) (Lastname)
     \         /
   ┌──────────────┐       1:5       ┌──────────────┐
   │ User_Account │───────────────▶ │   Profile    │
   │  USER_ID(PK) │                 │PROFILE_ID(PK)│
   └──────────────┘                 └──────┬───────┘
                                           │ M:N
                           ┌───────────────┼───────────────┐
                           ▼               ▼               ▼
                       ◇ QUEUE        ◇ RATING      ◇ USER_PREF
                           │               │               │
                           ▼               ▼               ▼
                      ┌─────────┐   (Average_Rating)  ┌─────────┐
                      │  Movie  │                      │  Genre  │
                      │MOVIE_ID │◄────────────────────▶│GENRE_ID │
                      └────┬────┘                      └─────────┘
                           │ M:N
                      ◇ STARRED_BY
                           │
                      ┌────▼────┐
                      │  Actor  │
                      │ACTOR_ID │
                      └─────────┘
```

---

### 🧬 Generalization vs Specialization

```
GENERALIZATION (bottom-up)       SPECIALIZATION (top-down)
  STUDENT + INSTRUCTOR           PERSON splits into:
  share common attrs      ───▶      STUDENT (rollNo, programme)
  → merge into PERSON              INSTRUCTOR (dept, salary)

        [PERSON]                         [PERSON]
        /      \                         /      \
  [STUDENT] [INSTRUCTOR]          [STUDENT] [INSTRUCTOR]
```

**Aggregation** — treating a relationship itself as an entity:
```
 ┌─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─┐
 │  Project Submission         │
 │  ┌─────────┐ WORKS_ON       │──── ◇ SUBMITTED_TO ──── [Submission Status]
 │  │ Student │◄──────────────▶│
 │  └─────────┘  ┌──────────┐  │
 │               │ Faculty  │  │
 └ ─ ─ ─ ─ ─ ─ ─└──────────┘─ ┘
```

---

## 6. Relational Model

### 🧱 Building Blocks (Remember This Ladder!)

```
  Domain ──▶ Attribute ──▶ Tuple ──▶ Relation
  (valid      (column)     (row)     (table)
   values)
```

| Block | Definition | Example |
|-------|-----------|---------|
| **Domain** | Set of valid values + constraints | RollNo: INT CHECK (21000000–21999999) |
| **Attribute** | One column; values from one domain | RollNo, Name, Programme |
| **Tuple** | One complete row (atomic values only) | (22104567, Ananya, BTech CSE) |
| **Relation** | The full table | STUDENT table with all students |

---

### 📋 Sample Relation: STUDENT

| Roll_No | Name | Programme | Mobile_No |
|---------|------|-----------|-----------|
| 22104567 | Parikshit Sharma | BTech CSE | 9876543210 |
| 22104612 | Rajat Mehta | BTech ECE | 9123456780 |
| 22104755 | Sakshi Bansal | MBA | 9001122334 |

---

### 🔒 Integrity Constraints

| Constraint | Rule | Example |
|-----------|------|---------|
| **Key Constraint** | Each table has ≥1 candidate key; values unique | Two students can't share RollNo |
| **Entity Integrity** | Primary key ≠ NULL, ever | Cannot insert student without RollNo |
| **Referential Integrity** | FK must match existing PK in referenced table | MARKS.RollNo must exist in STUDENT.RollNo |

---

## 7. Types of Keys

### 🗝️ Key Hierarchy

```
                  ┌─────────────────────────────────┐
                  │          SUPER KEY               │
                  │  {rollNo, mobileNo}, {rollNo},   │
                  │  {email}, {rollNo, name, email}  │
                  │                                  │
                  │    ┌──────────────────────┐      │
                  │    │   CANDIDATE KEY      │      │
                  │    │  {rollNo}, {email}   │      │
                  │    │  (minimal super keys)│      │
                  │    │                      │      │
                  │    │  ┌────────────────┐  │      │
                  │    │  │  PRIMARY KEY   │  │      │
                  │    │  │  {rollNo}      │  │      │
                  │    │  │ (one chosen CK)│  │      │
                  │    │  └────────────────┘  │      │
                  │    │  ALTERNATE KEY:{email}      │
                  │    └──────────────────────┘      │
                  └─────────────────────────────────┘
```

---

### 📌 Quick Reference — All Key Types

| Key Type | Definition | Example |
|----------|-----------|---------|
| **Super Key** | Any column-set that uniquely identifies rows (may have extras) | {rollNo, mobile}, {rollNo}, {email} |
| **Candidate Key** | Minimal super key — remove any column and uniqueness breaks | {rollNo} or {email} |
| **Primary Key** | The ONE candidate key officially chosen by DBA | College picks rollNo |
| **Alternate Key** | Remaining candidate keys not chosen as PK | email (often indexed for fast lookups) |
| **Composite Key** | Key built from 2+ columns | MARKS: {rollNo + courseCode} |
| **Foreign Key** | Column set pointing to PK/AK of another table | MARKS.rollNo → STUDENT.rollNo |

---

### 🔍 Detailed Key Examples

```
STUDENT Table                    MARKS Table
┌──────────┬──────────┬───────┐  ┌──────────┬──────────┬───────┐
│ RollNo   │ Name     │ Email │  │ RollNo   │CourseCode│ Grade │
│  (PK)    │          │ (AK)  │  │  (FK)    │  (FK)    │       │
├──────────┼──────────┼───────┤  ├──────────┼──────────┼───────┤
│ 22104567 │ Ananya   │ a@c.e │  │ 22104567 │  CS101   │  8.5  │
│ 22104612 │ Rajat    │ r@c.e │  │ 22104567 │  MA102   │  7.0  │
└──────────┴──────────┴───────┘  └──────────┴──────────┴───────┘
  RollNo alone → unique            {RollNo + CourseCode} → composite PK
  Email alone  → unique            Neither alone is unique
  Both are candidate keys
```

> 💡 **Exam trick — Minimality test:** Drop one column from the key. If rows are still uniquely identified → it was NOT minimal (it's a super key, not a candidate key).

> ⚡ **Null rule:** Primary key columns CANNOT be NULL. Foreign keys CAN be NULL if the relationship is optional.

---

## 8. Normalization (1NF → BCNF)

### 🎯 What & Why

> **Normalization** decomposes large, messy tables into smaller, logically clean ones — removing redundancy, preventing update anomalies, and ensuring data integrity.

---

### 🔗 Functional Dependency (FD) Types

| Type | Rule | Example |
|------|------|---------|
| **Full FD** | Y depends on ALL of X | Grade depends on {RollNo AND CourseCode} |
| **Partial FD** | Y depends on PART of composite X | StudentName depends only on RollNo (not CourseCode) — violates 2NF |
| **Trivial FD** | Y ⊆ X | {RollNo, Name} → Name (obvious) |
| **Non-trivial FD** | Y ⊄ X | RollNo → DeptName |
| **Transitive FD** | A→B→C chain | RollNo → DeptID → Building |
| **Multivalued FD** | A →→ B, A →→ C independently | RollNo →→ Hobbies, RollNo →→ Sports |

---

### 📐 Normal Forms — At a Glance

```
UNF ──▶ 1NF ──▶ 2NF ──▶ 3NF ──▶ BCNF
   Remove   Remove   Remove   Every FD's
   multi-   partial  transitive LHS must be
   valued   FDs      FDs        a superkey
   cells
```

| Normal Form | Rule | Violation Removed |
|-------------|------|------------------|
| **1NF** | Every cell = one atomic value; no repeating groups | Multi-valued cells |
| **2NF** | 1NF + no partial FDs on composite PK | Partial dependencies |
| **3NF** | 2NF + no transitive FDs | Transitive dependencies |
| **BCNF** | For every FD X→Y, X must be a superkey | Anomalies missed by 3NF |

---

### 🚶 Walk-through: UNF → BCNF (College Example)

#### ❌ Un-Normalized (UNF)

| RollNo | StudentName | Programme | CourseList | GradeList |
|--------|-------------|-----------|------------|-----------|
| 22104567 | Ananya Sharma | BTech CSE | CS101, MA102 | 8.5, 7.0 |
| 22104755 | Farah Khan | MBA | MG201, MG202 | 7.5, 8.2 |

**Problems:** Multi-valued cells. Can't insert a new course easily.

---

#### ✅ After 1NF — Make every cell atomic

| RollNo | StudentName | Programme | CourseCode | Grade |
|--------|-------------|-----------|------------|-------|
| 22104567 | Ananya Sharma | BTech CSE | CS101 | 8.5 |
| 22104567 | Ananya Sharma | BTech CSE | MA102 | 7.0 |
| 22104755 | Farah Khan | MBA | MG201 | 7.5 |

**Problem still:** StudentName & Programme repeat on every row → partial FD.

---

#### ✅ After 2NF — Remove partial FDs

Composite PK = {RollNo, CourseCode}. StudentName depends only on RollNo → move out.

**STUDENT table:**

| RollNo (PK) | StudentName | Programme |
|-------------|-------------|-----------|
| 22104567 | Ananya Sharma | BTech CSE |
| 22104755 | Farah Khan | MBA |

**COURSE table:**

| CourseCode (PK) | CourseName | Credits | InstructorID |
|-----------------|------------|---------|--------------|
| CS101 | Intro to CS | 4 | I01 |
| MA102 | Calculus I | 3 | I02 |

**RESULT table:**

| RollNo (FK) | CourseCode (FK) | Grade |
|-------------|-----------------|-------|
| 22104567 | CS101 | 8.5 |
| 22104567 | MA102 | 7.0 |

---

#### ✅ After 3NF — Remove transitive FDs

In COURSE: `CourseCode → InstructorID → InstructorName` (transitive chain!)  
→ Create separate INSTRUCTOR table.

**INSTRUCTOR table:**

| InstructorID (PK) | InstructorName |
|-------------------|----------------|
| I01 | Dr Rao |
| I02 | Prof Gupta |

COURSE now keeps InstructorID (FK) but drops InstructorName.

---

#### ✅ BCNF Final Schema

```
STUDENT     (rollNo PK, studentName, programme)
              │
              │ FK
              ▼
RESULT      (rollNo FK, courseCode FK, grade)    PK = {rollNo + courseCode}
              │
              │ FK
              ▼
COURSE      (courseCode PK, courseName, credits, instructorID FK)
              │
              │ FK
              ▼
INSTRUCTOR  (instructorID PK, instructorName)
```

> 💡 **BCNF Rule:** In every FD X→Y, X MUST be a candidate key. Stronger than 3NF.

---

### ⚖️ Lossless Join vs Dependency Preservation

| Property | Meaning | Goal |
|----------|---------|------|
| **Lossless Join** | Joining decomposed tables recreates original exactly (no extra/missing rows) | Always achieve this |
| **Dependency Preservation** | All original FDs can be enforced without joining tables | Aim for this; BCNF may force you to sacrifice it |

---

## 9. Denormalization

### 🔄 What & When

> **Denormalization** deliberately introduces controlled redundancy — pre-computing joins or storing aggregates — to speed up read-heavy workloads.

```
NORMALIZED (clean, slower reads)         DENORMALIZED (redundant, faster reads)
────────────────────────────────         ──────────────────────────────────────
STUDENT × RESULT × COURSE               RESULT_FLAT
× INSTRUCTOR → 4-table join             (single wide table, refreshed nightly)
≈ 100ms per query × 5000 rows           ≈ 10ms full scan
= 100 seconds for result report         = seconds for same report
```

---

### 📊 Trade-off Table

| Metric | Normalized | Denormalized | Impact |
|--------|-----------|--------------|--------|
| Read Performance | 100ms/join | 10ms flat | 10× faster |
| Write Performance | 5ms | 20ms + sync | 4× slower |
| Storage | 100 GB | 150–300 GB | +50–200% |
| Consistency | Immediate | Eventual (batch) | Staleness risk |

### ✅ When to Denormalize

- Heavy read, light write (placement dashboard shows CGPA every hour, updates once a semester)
- Aggregates needed repeatedly (monthly mess bill totals)
- Star schema for analytics/reports (NAAC, BI dashboards)

### ⚠️ Rule of Thumb

> Denormalize **ONLY** after measuring that joins are your real bottleneck. Measure first with `EXPLAIN`. Target hot paths only — keep the rest normalized.

---

## 10. Transactions & ACID

### 📦 What is a Transaction?

> A **transaction** is a group of database operations treated as one indivisible unit — either ALL succeed or NONE do.

**Example — "Publish Result":**
```
BEGIN TRANSACTION
  1. INSERT grades for every subject
  2. RECALCULATE each student's CGPA
  3. SET ResultPublished = TRUE
COMMIT  ←── all 3 written permanently
  (if power fails between 1 and 2 → ROLLBACK → back to start)
```

---

### ⚡ ACID Properties (Critical for Every Interview)

```
 ┌─────────────────────────────────────────────────────────────┐
 │                        ACID                                 │
 │                                                             │
 │  A ─ Atomicity      All-or-nothing. No half-done updates.   │
 │  C ─ Consistency    DB rules (PK, FK, CHECK) valid before   │
 │                     AND after every transaction.            │
 │  I ─ Isolation      Concurrent txns don't see each other's  │
 │                     intermediate state.                     │
 │  D ─ Durability     Once committed, survives crashes.       │
 │                     Achieved via Write-Ahead Logging (WAL). │
 └─────────────────────────────────────────────────────────────┘
```

| Property | Example in College DB |
|---------|----------------------|
| **Atomicity** | Fee payment: both debit + credit written, or neither |
| **Consistency** | Student transfer: dept quotas preserved, no student left dept-less |
| **Isolation** | Two teachers grading same paper: Serializable makes one wait |
| **Durability** | Server crash after grade submit: grades recoverable from WAL |

---

### 🔄 Transaction Life-cycle

```
                     R/W ops
  ┌──────────┐  ─────────────▶  ┌───────────────────┐  Permanent ┌─────────────┐
  │  ACTIVE  │                  │ PARTIALLY COMMITTED│ ─────────▶ │  COMMITTED  │
  └──────────┘                  └───────────────────┘   store     └─────────────┘
       │
       │ Failure
       ▼
  ┌──────────┐   Roll Back    ┌─────────────┐
  │  FAILED  │ ─────────────▶ │   ABORTED   │
  └──────────┘                └─────────────┘
```

---

### 🔒 Isolation Levels (Weakest → Strongest)

| Level | Dirty Read | Non-Repeatable Read | Phantom Read | Speed |
|-------|-----------|--------------------|--------------|----|
| **Read Uncommitted** | ✅ Possible | ✅ Possible | ✅ Possible | Fastest |
| **Read Committed** | ❌ Blocked | ✅ Possible | ✅ Possible | Fast |
| **Repeatable Read** | ❌ Blocked | ❌ Blocked | ✅ Possible | Medium |
| **Serializable** | ❌ Blocked | ❌ Blocked | ❌ Blocked | Slowest |

---

### 🐛 Isolation Problems Explained Simply

```
DIRTY READ              NON-REPEATABLE READ       PHANTOM READ
──────────────          ───────────────────       ────────────
T1: UPDATE fees=0       T1: READ room=101          T1: SELECT WHERE
T2: READ fees=0   ◄──   T2: UPDATE room=202        attendance>85%
T1: ROLLBACK            T1: READ room=202    ◄──   T2: INSERT new
T2 has WRONG data!      Two different values!      qualifying student
                        for same read.             T1 re-reads: EXTRA row!
```

> 💡 **Memory trick:** "Dirty = uncommitted, Non-repeatable = updated between reads, Phantom = inserted between reads."

---

## 11. Concurrency Control

### 🔐 Locking

```
SHARED (S) LOCK                EXCLUSIVE (X) LOCK
───────────────────            ──────────────────
Multiple users can READ        Only ONE txn can read + write
No one can WRITE               All others BLOCKED
"Read lock"                    "Write lock"

Lock compatibility:
      S    X
S  [ OK  WAIT ]
X  [WAIT WAIT ]
```

---

### 🔄 Two-Phase Locking (2PL)

```
Phase 1: GROWING          Phase 2: SHRINKING
──────────────────        ──────────────────
Acquire locks ✅          Release locks ✅
Cannot release ❌         Cannot acquire ❌

       Lock count
          ▲
          │    ╱╲
          │   ╱  ╲
          │  ╱    ╲
          │ ╱      ╲
          │╱        ╲
          ├──────────────▶ time
         Lock        Lock
         point       point
       (growing)  (shrinking)
```

> **Strict 2PL (S2PL):** Hold ALL locks until commit/abort. Guarantees serializability + no dirty reads.

---

### 🕒 Serializability

| Type | Test | Safe If |
|------|------|---------|
| **Conflict-serializable** | Build precedence graph: T_i → T_j when T_i writes and T_j later reads/writes same data | Graph has **no cycle** |
| **View-serializable** | Same final values + same first-read data as some serial schedule | Superset of conflict-serializable |

---

### 🔧 Concurrency Control Techniques

| Technique | Idea | Best When |
|-----------|------|-----------|
| **Locking (2PL)** | Grab locks before access | General purpose |
| **Timestamp Ordering** | Older timestamp wins conflict | Many reads, few writes |
| **Optimistic** | No locks; check conflicts at commit | Read-heavy, rare writes |

---

### 🛠️ Recovery Techniques

| Technique | How | Example |
|-----------|-----|---------|
| **Backup & Restore** | Regular full/incremental copies | Daily attendance backup |
| **Log-Based (WAL)** | Every op logged before applied | Fee portal: log before DB write |
| **Shadow Paging** | Old page kept until new one fully saved | College portal profile update |
| **Checkpointing** | Snapshot every N minutes | Course registration saves state every 15 min |
| **Rollback/Rollforward** | Undo uncommitted / Redo committed from log | Scholarship app failure recovery |

---

## 12. SQL Commands & Queries

### 🗂️ SQL Command Families

```
┌─────┬──────────────────────────────────────────────────────────────────┐
│ DDL │ CREATE  ALTER  DROP  TRUNCATE  RENAME  COMMENT                  │
│     │ (Data Definition — shapes tables/schema)                        │
├─────┼──────────────────────────────────────────────────────────────────┤
│ DML │ SELECT  INSERT  UPDATE  DELETE  MERGE                           │
│     │ (Data Manipulation — add/change/remove rows)                    │
├─────┼──────────────────────────────────────────────────────────────────┤
│ DCL │ GRANT  REVOKE  DENY                                             │
│     │ (Data Control — access/permissions)                             │
├─────┼──────────────────────────────────────────────────────────────────┤
│ TCL │ COMMIT  ROLLBACK  SAVEPOINT  SET TRANSACTION                    │
│     │ (Transaction Control — save/undo work)                          │
└─────┴──────────────────────────────────────────────────────────────────┘
```

---

### 📝 Core SQL with Java JDBC & C++ Connector (Beginner-Friendly)

#### 🔵 SELECT — Fetch data

```sql
-- SQL
SELECT rollNo, name FROM STUDENT WHERE programme = 'MBA';
```

```java
// Java (JDBC) — beginners: JDBC is the bridge between Java and your DB
import java.sql.*;

public class SelectDemo {
    public static void main(String[] args) throws Exception {
        // Step 1: Load driver and connect
        Connection conn = DriverManager.getConnection(
            "jdbc:mysql://localhost:3306/college", "root", "password");

        // Step 2: Write your SQL
        String sql = "SELECT rollNo, name FROM STUDENT WHERE programme = ?";

        // Step 3: PreparedStatement prevents SQL injection attacks
        PreparedStatement ps = conn.prepareStatement(sql);
        ps.setString(1, "MBA");          // ? gets replaced with "MBA"

        // Step 4: Execute and loop through results
        ResultSet rs = ps.executeQuery();
        while (rs.next()) {
            System.out.println(rs.getInt("rollNo") + " | " + rs.getString("name"));
        }

        // Step 5: Always close resources
        rs.close(); ps.close(); conn.close();
    }
}
```

```cpp
// C++ (MySQL Connector/C++) — beginners: install mysql-connector-c++
#include <mysql_driver.h>
#include <mysql_connection.h>
#include <cppconn/prepared_statement.h>
#include <cppconn/resultset.h>
#include <iostream>
using namespace std;

int main() {
    // Step 1: Get a driver instance
    sql::mysql::MySQL_Driver* driver = sql::mysql::get_mysql_driver_instance();

    // Step 2: Connect to database
    sql::Connection* conn = driver->connect(
        "tcp://127.0.0.1:3306", "root", "password");
    conn->setSchema("college");          // SELECT which database to use

    // Step 3: Prepare your query
    sql::PreparedStatement* ps = conn->prepareStatement(
        "SELECT rollNo, name FROM STUDENT WHERE programme = ?");
    ps->setString(1, "MBA");

    // Step 4: Run and read results
    sql::ResultSet* rs = ps->executeQuery();
    while (rs->next()) {
        cout << rs->getInt("rollNo") << " | " 
             << rs->getString("name") << endl;
    }

    // Step 5: Clean up memory
    delete rs; delete ps; delete conn;
    return 0;
}
```

---

#### 🟢 INSERT — Add a row

```sql
-- SQL
INSERT INTO STUDENT (rollNo, name, programme)
VALUES (22105123, 'Nisha Verma', 'BTech ECE');
```

```java
// Java JDBC INSERT
Connection conn = DriverManager.getConnection(
    "jdbc:mysql://localhost:3306/college", "root", "password");

String sql = "INSERT INTO STUDENT (rollNo, name, programme) VALUES (?, ?, ?)";
PreparedStatement ps = conn.prepareStatement(sql);
ps.setInt(1, 22105123);
ps.setString(2, "Nisha Verma");
ps.setString(3, "BTech ECE");

int rowsInserted = ps.executeUpdate();   // returns number of rows affected
System.out.println("Rows inserted: " + rowsInserted);

ps.close(); conn.close();
```

```cpp
// C++ INSERT
sql::PreparedStatement* ps = conn->prepareStatement(
    "INSERT INTO STUDENT (rollNo, name, programme) VALUES (?, ?, ?)");
ps->setInt(1, 22105123);
ps->setString(2, "Nisha Verma");
ps->setString(3, "BTech ECE");
int rows = ps->executeUpdate();
cout << "Rows inserted: " << rows << endl;
delete ps;
```

---

#### 🟡 UPDATE — Modify existing row

```sql
-- SQL
UPDATE MARKS SET grade = 9.1
WHERE rollNo = 22104567 AND courseCode = 'CS101';
```

```java
// Java JDBC UPDATE
String sql = "UPDATE MARKS SET grade = ? WHERE rollNo = ? AND courseCode = ?";
PreparedStatement ps = conn.prepareStatement(sql);
ps.setDouble(1, 9.1);
ps.setInt(2, 22104567);
ps.setString(3, "CS101");
int updated = ps.executeUpdate();
System.out.println("Rows updated: " + updated);
```

```cpp
// C++ UPDATE
sql::PreparedStatement* ps = conn->prepareStatement(
    "UPDATE MARKS SET grade = ? WHERE rollNo = ? AND courseCode = ?");
ps->setDouble(1, 9.1);
ps->setInt(2, 22104567);
ps->setString(3, "CS101");
cout << "Rows updated: " << ps->executeUpdate() << endl;
```

---

#### 🔴 DELETE — Remove a row

```sql
-- SQL
DELETE FROM STUDENT WHERE rollNo = 22109999;
```

```java
// Java JDBC DELETE
PreparedStatement ps = conn.prepareStatement(
    "DELETE FROM STUDENT WHERE rollNo = ?");
ps.setInt(1, 22109999);
System.out.println("Deleted: " + ps.executeUpdate() + " row(s)");
```

```cpp
// C++ DELETE
sql::PreparedStatement* ps = conn->prepareStatement(
    "DELETE FROM STUDENT WHERE rollNo = ?");
ps->setInt(1, 22109999);
cout << "Deleted: " << ps->executeUpdate() << " row(s)" << endl;
```

---

#### 🔵 Transaction — ACID in Code

```sql
-- SQL Transaction
BEGIN;
  UPDATE accounts SET balance = balance - 500 WHERE id = 1;  -- debit
  UPDATE accounts SET balance = balance + 500 WHERE id = 2;  -- credit
COMMIT;
-- If anything fails before COMMIT → ROLLBACK automatically
```

```java
// Java JDBC Transaction
Connection conn = DriverManager.getConnection(...);
conn.setAutoCommit(false);   // disable auto-commit — CRITICAL step!

try {
    PreparedStatement debit = conn.prepareStatement(
        "UPDATE accounts SET balance = balance - ? WHERE id = ?");
    debit.setDouble(1, 500.0);
    debit.setInt(2, 1);
    debit.executeUpdate();

    PreparedStatement credit = conn.prepareStatement(
        "UPDATE accounts SET balance = balance + ? WHERE id = ?");
    credit.setDouble(1, 500.0);
    credit.setInt(2, 2);
    credit.executeUpdate();

    conn.commit();   // both succeed → save permanently
    System.out.println("Transfer successful!");

} catch (SQLException e) {
    conn.rollback(); // one fails → undo BOTH changes (Atomicity!)
    System.out.println("Transfer failed, rolled back: " + e.getMessage());
} finally {
    conn.setAutoCommit(true);
    conn.close();
}
```

```cpp
// C++ Transaction
conn->setAutoCommit(false);   // disable auto-commit

try {
    sql::PreparedStatement* debit = conn->prepareStatement(
        "UPDATE accounts SET balance = balance - ? WHERE id = ?");
    debit->setDouble(1, 500.0);
    debit->setInt(2, 1);
    debit->executeUpdate();
    delete debit;

    sql::PreparedStatement* credit = conn->prepareStatement(
        "UPDATE accounts SET balance = balance + ? WHERE id = ?");
    credit->setDouble(1, 500.0);
    credit->setInt(2, 2);
    credit->executeUpdate();
    delete credit;

    conn->commit();              // Atomicity: both or neither
    cout << "Transfer done!" << endl;

} catch (sql::SQLException& e) {
    conn->rollback();            // Undo everything on failure
    cout << "Rollback! Error: " << e.what() << endl;
}
conn->setAutoCommit(true);
```

---

### 🔗 Joins

| Join | Returns | Use Case |
|------|---------|----------|
| **INNER JOIN** | Only matching rows from both tables | Students with marks |
| **LEFT JOIN** | All left rows + matching right (NULL if no match) | All students, whether or not they have marks |
| **RIGHT JOIN** | All right rows + matching left | All courses, whether or not enrolled |
| **FULL OUTER JOIN** | All rows from both tables | Complete audit |

```sql
-- INNER JOIN: students with their grades
SELECT s.rollNo, s.name, m.grade
FROM STUDENT AS s
INNER JOIN MARKS AS m ON s.rollNo = m.rollNo;

-- LEFT JOIN: all students, grades if exist
SELECT s.rollNo, s.name, m.grade
FROM STUDENT s
LEFT JOIN MARKS m ON s.rollNo = m.rollNo;

-- UNION: female students OR MBA students (removes duplicates)
SELECT rollNo FROM STUDENT WHERE gender = 'F'
UNION
SELECT rollNo FROM STUDENT WHERE programme = 'MBA';
```

---

### 📊 Aggregate Functions

| Function | Returns | Example |
|----------|---------|---------|
| `COUNT(*)` | Number of rows | `SELECT COUNT(*) FROM STUDENT` |
| `SUM(col)` | Total | `SELECT SUM(amount) FROM FEES` |
| `AVG(col)` | Average | `SELECT AVG(CGPA) FROM STUDENT` |
| `MAX(col)` | Highest | `SELECT MAX(CGPA) FROM STUDENT` |
| `MIN(col)` | Lowest | `SELECT MIN(attendance) FROM RECORDS` |

---

### 🗂️ SQL Clause Execution Order

```
 Written order:          Execution order (how DB actually runs it):
 ─────────────           ──────────────────────────────────────────
 SELECT       ◄───  6.   SELECT (project columns)
 FROM         ◄───  1.   FROM   (load table)
 WHERE        ◄───  2.   WHERE  (filter rows)
 GROUP BY     ◄───  3.   GROUP BY (group)
 HAVING       ◄───  4.   HAVING (filter groups)
 ORDER BY     ◄───  5.   ORDER BY (sort)
 LIMIT        ◄───  7.   LIMIT  (cut results)
```

---

### 🎯 Top 15 Interview SQL Queries

```sql
-- 1. Second highest salary (classic!)
SELECT MAX(pay) FROM staff
WHERE pay NOT IN (SELECT MAX(pay) FROM staff);

-- 2. Nth highest salary (generic)
SELECT DISTINCT pay FROM staff s1
WHERE :n >= (SELECT COUNT(DISTINCT pay) FROM staff s2 WHERE s1.pay <= s2.pay)
ORDER BY s1.pay DESC;

-- 3. Employees with duplicate salaries
SELECT s1.* FROM staff s1
JOIN staff s2 ON s1.pay = s2.pay AND s1.staff_id <> s2.staff_id;

-- 4. Department with most employees
SELECT division, COUNT(staff_id) AS total
FROM staff
GROUP BY division
ORDER BY total DESC
LIMIT 1;

-- 5. Employees NOT receiving bonus
SELECT staff_id FROM staff
WHERE staff_id NOT IN (SELECT staff_ref_id FROM incentive);

-- 6. Top earner per department
SELECT s.division, s.given_name, s.pay
FROM staff s
JOIN (SELECT division, MAX(pay) AS top_pay FROM staff GROUP BY division) t
  ON t.division = s.division AND t.top_pay = s.pay;

-- 7. Find odd rows
SELECT * FROM staff WHERE MOD(staff_id, 2) != 0;

-- 8. Students above course average (correlated subquery)
SELECT rollNo, grade FROM MARKS m
WHERE grade > (SELECT AVG(grade) FROM MARKS WHERE courseCode = m.courseCode);

-- 9. Students with perfect attendance (EXISTS)
SELECT rollNo, name FROM STUDENT s
WHERE NOT EXISTS (
  SELECT 1 FROM ATTENDANCE WHERE rollNo = s.rollNo AND status = 'ABSENT'
);

-- 10. Average grade per course (≥8 only, top 5 hardest)
SELECT courseCode, AVG(grade) AS avgGrade
FROM MARKS
WHERE semester = 6
GROUP BY courseCode
HAVING AVG(grade) >= 8
ORDER BY avgGrade ASC
LIMIT 5;

-- 11. Create a view (virtual table)
CREATE VIEW V_TOPPERS AS
SELECT rollNo, name, CGPA FROM STUDENT WHERE CGPA >= 9.0;

-- 12. Last 5 rows by ID
(SELECT * FROM staff ORDER BY staff_id DESC LIMIT 5) ORDER BY staff_id;

-- 13. Divisions with fewer than 4 employees
SELECT division, COUNT(*) AS headcount FROM staff
GROUP BY division HAVING headcount < 4;

-- 14. Clone a table
CREATE TABLE staff_copy LIKE staff;
INSERT INTO staff_copy SELECT * FROM staff;

-- 15. Records in table A but not table B
SELECT s.* FROM staff s
LEFT JOIN staff_copy USING (staff_id)
WHERE staff_copy.staff_id IS NULL;
```

---

### 🖥️ Aggregate + Join in Java and C++

```java
// Java: Find average CGPA per department using GROUP BY
String sql = "SELECT programme, AVG(CGPA) AS avgCGPA FROM STUDENT GROUP BY programme";
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery(sql);

System.out.println("Programme       | Avg CGPA");
System.out.println("----------------|----------");
while (rs.next()) {
    System.out.printf("%-15s | %.2f%n",
        rs.getString("programme"), rs.getDouble("avgCGPA"));
}
rs.close(); stmt.close();
```

```cpp
// C++: Find average CGPA per department using GROUP BY
sql::Statement* stmt = conn->createStatement();
sql::ResultSet* rs = stmt->executeQuery(
    "SELECT programme, AVG(CGPA) AS avgCGPA FROM STUDENT GROUP BY programme");

cout << "Programme       | Avg CGPA" << endl;
cout << "----------------|----------" << endl;
while (rs->next()) {
    cout << rs->getString("programme") << "\t| "
         << rs->getDouble("avgCGPA") << endl;
}
delete rs; delete stmt;
```

---

## 13. Indexing & B+ Trees

### 📚 What is an Index?

> An **index** is a tiny, sorted copy of one or more columns that lets the DB jump straight to a row instead of scanning the whole table — like a textbook index pointing to page numbers.

```
WITHOUT INDEX                   WITH INDEX (B+ Tree)
─────────────────               ──────────────────────────
Scan row 1... not it            Root: [50]
Scan row 2... not it             /        \
Scan row 3... not it          [25]        [75]
...                           /   \       /   \
Scan row 10000... FOUND!    [10][25]   [50][75]  (leaf nodes, linked)
                            ← key → row pointer
O(n)                        O(log n)
```

---

### 🌳 B-tree vs B+-tree

```
B-TREE                              B+-TREE (used by most DBMSs)
──────────────────────────          ──────────────────────────────────
Keys stored in internal             Keys stored ONLY in leaf nodes
AND leaf nodes                      Internal nodes = guides only
                                    Leaf nodes linked left→right
[50]                                [50]
/    \                              /    \
[25] [75]                        [25]   [75]
 ↓    ↓                           ↓      ↓
data  data                    [10]→[25]→[50]→[75] (linked list!)

Range scan: jump around tree        Range scan: find start, walk linked leaves
 → SLOW for ranges                  → VERY FAST for ranges
                                    e.g., CGPA BETWEEN 8 AND 9
```

---

### 🗂️ Index Types

| Type | Idea | Example |
|------|------|---------|
| **Primary (Clustered)** | Table physically sorted by this key | STUDENT sorted by rollNo |
| **Secondary (Non-clustered)** | Separate index file; table order unchanged | Index on mobileNo |
| **Composite Index** | Index on (col1, col2) | Index on (lastName, firstName) |

---

### ⚡ SQL Optimization Tips

| Tip | Why | Example |
|-----|-----|---------|
| Avoid `SELECT *` | Fetches unneeded columns | Use `SELECT rollNo, name` |
| Filter early | Reduces rows before expensive joins | `WHERE` before `JOIN` |
| Use indexed columns in `WHERE` | DB can use index | `WHERE rollNo = 22104567` ✅ |
| Avoid functions on indexed cols | Hides index from optimizer | `WHERE UPPER(name)='ANANYA'` ❌ |
| Use `LIMIT` | Fetch only what UI needs | `ORDER BY cgpa DESC LIMIT 10` |
| Check `EXPLAIN` | See if index is being used | `EXPLAIN SELECT ...` |

---

### 🔍 EXPLAIN in Java & C++

```java
// Java: use EXPLAIN to check query plan
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery("EXPLAIN SELECT * FROM STUDENT WHERE rollNo = 22104567");
while (rs.next()) {
    // If 'type' = 'ALL' → full table scan → add an index!
    // If 'type' = 'ref' or 'eq_ref' → index is being used ✅
    System.out.println("Type: " + rs.getString("type") +
                       " | Key used: " + rs.getString("key") +
                       " | Rows examined: " + rs.getInt("rows"));
}
```

```cpp
// C++: EXPLAIN to analyze query performance
sql::Statement* stmt = conn->createStatement();
sql::ResultSet* rs = stmt->executeQuery(
    "EXPLAIN SELECT * FROM STUDENT WHERE rollNo = 22104567");
while (rs->next()) {
    cout << "Type: " << rs->getString("type")
         << " | Key: " << rs->getString("key")
         << " | Rows: " << rs->getInt("rows") << endl;
}
delete rs; delete stmt;
```

---

## 14. Sharding & Scaling

### 📈 Vertical vs Horizontal Scaling

```
VERTICAL SCALING (Scale-UP)         HORIZONTAL SCALING (Scale-OUT)
────────────────────────────        ───────────────────────────────
One machine, add more power         Add more machines, split data

  ┌───────────┐                      ┌────────┐ ┌────────┐ ┌────────┐
  │ 128GB RAM │  ◄── upgrade         │Shard 1 │ │Shard 2 │ │Shard 3 │
  │  64 cores │                      │RollNo  │ │RollNo  │ │RollNo  │
  │   10TB    │                      │1–10000 │ │10001–  │ │20001–  │
  └───────────┘                      └────────┘ │20000   │ └────────┘
                                                └────────┘
  Hit a ceiling. Expensive.           Infinite horizontal growth.
```

---

### 🔪 Sharding Methods

| Method | How | Pro | Con |
|--------|-----|-----|-----|
| **Range-Based** | RollNo 1–1000 → Shard 1 | Simple | Uneven data ("hotspots") |
| **Hashed** | hash(AdmissionID) → shard | Even distribution | Rehashing needed on resize |
| **Directory** | Lookup table: CourseID → shard | Flexible | Lookup table = single point of failure |
| **Geo** | Delhi students → Delhi server | Low latency | Uneven if user density varies |

---

### ⚙️ Sharding: Key Components

```
Shard Key: student's Branch (determines which shard)
           │
           ▼
┌──────────────────────────────────────────────────┐
│              Shard Router / Config Server         │
│  Branch=CSE ──▶ Server A                         │
│  Branch=ECE ──▶ Server B                         │
│  Branch=MBA ──▶ Server C                         │
└──────────────────────────────────────────────────┘
         │              │              │
    ┌────▼────┐    ┌────▼────┐    ┌────▼────┐
    │ Shard A │    │ Shard B │    │ Shard C │
    │  CSE    │    │  ECE    │    │  MBA    │
    │students │    │students │    │students │
    └─────────┘    └─────────┘    └─────────┘
    (shared-nothing: each shard independent)
```

> ⚠️ **Avoid:** Monotonic keys (e.g., auto-increment timestamps) as shard keys → all new writes pile onto one shard (write hotspot).

---

## 15. Security: RBAC & Encryption

### 👥 Role-Based Access Control (RBAC)

```
DEFINE ROLES → ATTACH PERMISSIONS → ASSIGN USERS TO ROLES

Roles:      Student        Faculty           Admin
            │              │                 │
Perms:   SELECT marks   INSERT/UPDATE     Full rights
         Pay fees       marks (own         (all tables)
                        courses)

User 22104567 ──▶ inherits "Student" role on first login
```

```sql
-- SQL: GRANT and REVOKE
GRANT SELECT ON MARKS TO student_role;
GRANT INSERT, UPDATE ON MARKS TO faculty_role;
GRANT ALL PRIVILEGES ON *.* TO admin_role;
REVOKE INSERT ON MARKS FROM student_role;
```

---

### 🔐 Encryption Layers

| Layer | What Is Protected | Example |
|-------|-----------------|---------|
| **At rest** | Files on disk | Stolen hard disk → can't read Aadhaar numbers |
| **In transit** | Data over network | HTTPS on student portal → Wi-Fi snooper blocked |
| **Column-level** | Sensitive columns only | BankAccountNo stored with AES encryption |

---

### 🎭 Data Masking Types

| Technique | Effect | Example |
|-----------|--------|---------|
| **Static (SDM)** | Permanently masks data in a copy | Test DB: real names → fake names |
| **Dynamic (DDM)** | Masks on-the-fly, original unchanged | Helpdesk sees only last 4 digits of account |
| **Deterministic** | Same input → same masked output always | email@college.edu → masked1@test.com (always) |
| **Non-deterministic** | Same input → different masked output | Parent's number masked differently per report |
| **Format-Preserving** | Keeps original format/length | AB1234567 → XY9876543 (same format) |
| **Shuffling** | Rearranges values within column | Exam scores shuffled across students |
| **Redaction** | Completely blacks out / removes | Medical history → [REDACTED] |
| **Nulling Out** | Replaces with SQL NULL | Student addresses → NULL for external analysts |

---

## 16. MCQ Answer Key

```
 1.B   2.B   3.C   4.B   5.C   6.B   7.C   8.B   9.B  10.B
11.C  12.A  13.D  14.B  15.C  16.B  17.A  18.C  19.B  20.C
21.B  22.B  23.C  24.B  25.B  26.C  27.A  28.A  29.B  30.B
31.C  32.B  33.B  34.B  35.B  36.C
```

---

## 🎯 Quick Interview Revision Cheatsheet

```
ACID        ─── Atomicity, Consistency, Isolation, Durability
CAP         ─── Consistency, Availability, Partition Tolerance (pick 2)
Keys        ─── Super ⊃ Candidate ⊃ Primary (one of them) + Alternate (rest)
NF order    ─── 1NF→2NF→3NF→BCNF (each fixes a specific FD problem)
2NF fix     ─── Remove partial FDs (split composite-PK table)
3NF fix     ─── Remove transitive FDs (A→B→C, move B→C out)
BCNF fix    ─── Every FD's LHS must be a candidate key
Isolation   ─── Read Uncommitted < Read Committed < Repeatable Read < Serializable
2PL         ─── Growing phase (acquire) → Lock point → Shrinking phase (release)
Index       ─── B+-tree: all keys in leaves, linked → fast range scans
Shard key   ─── High cardinality + avoid monotonic + avoid hotspot columns
RBAC        ─── Roles carry permissions, users carry roles (not permissions directly)
```

---

## 💼 Top 20 Placement Interview Questions

| # | Question | Key Answer Points |
|---|---------|-------------------|
| 1 | What is ACID? | Atomicity/Consistency/Isolation/Durability; give bank transfer example |
| 2 | Difference: DELETE vs TRUNCATE vs DROP | DELETE=DML/logged/WHERE possible; TRUNCATE=DDL/faster/no WHERE; DROP=removes table |
| 3 | What is normalization? Why? | Decompose tables; remove redundancy + anomalies; 1NF→BCNF |
| 4 | Primary vs Foreign Key | PK=unique identifier, never NULL; FK=references another table's PK, can be NULL |
| 5 | What is a transaction? | Unit of work; ACID properties; COMMIT or ROLLBACK |
| 6 | Dirty read vs Phantom read | Dirty=uncommitted data read; Phantom=new rows appear between two reads |
| 7 | What is indexing? When to avoid? | Speeds up SELECT; avoid on small tables, columns with low cardinality, heavy-write tables |
| 8 | INNER JOIN vs LEFT JOIN | INNER=matching rows only; LEFT=all left + matching right (NULL if no match) |
| 9 | What is CAP theorem? | C+A+P, pick 2; financial apps pick CP |
| 10 | GROUP BY vs HAVING vs WHERE | WHERE filters rows before group; HAVING filters groups after |
| 11 | What is a view? | Virtual table from a query; no stored data; simplifies complex queries |
| 12 | What is sharding? | Horizontal partitioning; choose shard key carefully; avoid monotonic keys |
| 13 | 2NF vs 3NF vs BCNF | 2NF removes partial FDs; 3NF removes transitive FDs; BCNF every FD's LHS = candidate key |
| 14 | What is serializability? | Concurrent schedule equivalent to some serial schedule; tested via precedence graph |
| 15 | What is deadlock? How to prevent? | T1 waits for T2, T2 waits for T1 → cycle; prevent via timeout, wait-die, wound-wait |
| 16 | B-tree vs B+-tree | B+ stores data only in leaves, leaves linked → faster range scans; used in most DBMSs |
| 17 | What is denormalization? When? | Add controlled redundancy; use when joins are bottleneck on read-heavy workloads |
| 18 | Difference: UNION vs UNION ALL | UNION removes duplicates; UNION ALL keeps all rows (faster) |
| 19 | What is a candidate key? | Minimal super key; remove any attribute → uniqueness breaks |
| 20 | What is RBAC? | Roles defined, permissions attached to roles, users assigned to roles |

---

*📌 These notes are compiled from standard DBMS curriculum, optimized for placement interviews and technical vivas.*  
*💡 Practice all 50 SQL queries from the SQL Queries Practice section — they appear frequently in online assessments.*