# Salesforce Code Review Checklist

> **Source:** [Apex Hours — Salesforce Code Review Checklist](https://www.apexhours.com/code-review-checklist)  
> **Author:** Amit Chaudhary (Salesforce MVP, 23x Certified)  
> **Last Updated:** March 2025

---

## Table of Contents

1. [What is Code Review?](#1-what-is-code-review)
2. [Why Do We Need Code Review?](#2-why-do-we-need-code-review)
3. [Basic vs Expert Code Review](#3-basic-vs-expert-code-review)
4. [Code Review Checklist](#4-code-review-checklist)
   - 4.1 [Standard Guidelines](#41-standard-guidelines)
   - 4.2 [Defined Architecture](#42-defined-architecture)
   - 4.3 [Non-Functional Requirements](#43-non-functional-requirements)
   - 4.4 [Apex Best Practices](#44-apex-best-practices)
   - 4.5 [Object-Oriented Analysis & Design (OOAD)](#45-object-oriented-analysis--design-ooad)
   - 4.6 [Static Code Analysis](#46-static-code-analysis)
   - 4.7 [Integration Patterns](#47-integration-patterns)
5. [Clean Code Principles](#5-clean-code-principles)
6. [Code Review Best Practices (Process)](#6-code-review-best-practices-process)
7. [Further Learning](#7-further-learning)

---

## 1. What is Code Review?

Code review (also called **peer review**) is a software quality assurance activity where one or more people inspect a program by reading parts of its source code. At least one reviewer must **not** be the code's original author.

---

## 2. Why Do We Need Code Review?

| Reason | Detail |
|---|---|
| **Project Cost** | 40–80% of a software's lifetime cost goes to maintenance |
| **Maintainability** | Code is rarely maintained forever by its original author |
| **Readability** | Conventions let engineers understand new code quickly |
| **Consistency** | Aligns coding practices across all implementations |
| **Code Reusability** | Promotes reuse of repeatable processes and patterns |
| **Productivity** | Reduces development time and effort significantly |
| **Risk Reduction** | Minimises Points of Failure (POFs) during implementations |
| **[Tech Debt](https://www.apexhours.com/what-is-technical-debt/)** | Prevents accumulation of technical debt over time |

---

## 3. Basic vs Expert Code Review

### Basic Code Review

A baseline pass that any reviewer should perform:

- [ ] Can I understand what this code does?
- [ ] Is the class or method too large?
- [ ] Are there DML or SOQL statements inside `for` loops?
- [ ] Is code coverage at 75% or above?
- [ ] Does the code follow formatting and naming conventions?
- [ ] Is the code following established standards/guidelines?

### Expert Code Review

An advanced pass that senior developers and architects should perform:

- [ ] Separation of concerns enforced
- [ ] Design patterns applied correctly
- [ ] Governor limits handled proactively
- [ ] Security model respected (FLS, CRUD, Sharing)
- [ ] Architecture layers maintained (trigger → handler → service → selector)
- [ ] Asynchronous processing used where appropriate
- [ ] Testability considered from the start
- [ ] Integration patterns followed

---

## 4. Code Review Checklist

### 4.1 Standard Guidelines

> Reference: [Naming Conventions](https://www.apexhours.com/salesforce-naming-conventions-best-practices/) | [Code Layouts & Formatting](https://www.apexhours.com/code-layouts-and-formatting-salesforce/)

- [ ] Follow [Salesforce naming conventions](https://www.apexhours.com/salesforce-naming-conventions-best-practices/) for classes, methods, variables, and custom objects
- [ ] Apply consistent [code layout and formatting](https://www.apexhours.com/code-layouts-and-formatting-salesforce/) (indentation, whitespace, line length)
- [ ] Keep methods short and focused
- [ ] Apply the **DRY principle** — Don't Repeat Yourself
- [ ] Each method should do **one thing only**

---

### 4.2 Defined Architecture

> Reference: [Order of Execution](https://www.apexhours.com/order-of-execution-in-salesforce/) | [Exception Handling](https://www.apexhours.com/exception-handling-using-platform-events/) | [Trigger Framework](https://www.apexhours.com/trigger-framework-in-salesforce/)

- [ ] **[Order of Execution](https://www.apexhours.com/order-of-execution-in-salesforce/)** — Be aware of Salesforce's record-save lifecycle (validation rules, workflow, triggers, etc.)
- [ ] **Exception Handling & Logging** — A [logging framework using Platform Events](https://www.apexhours.com/exception-handling-using-platform-events/) or equivalent is in place; exceptions are caught and surfaced appropriately
- [ ] **[Trigger Framework](https://www.apexhours.com/trigger-framework-in-salesforce/)** — A consistent trigger framework is implemented (handler pattern, virtual class, or interface-based), with logic delegated out of the trigger body

#### Trigger Framework Quick Reference

| Pattern | Description |
|---|---|
| Trigger Handler Pattern | Thin trigger delegates to a handler class |
| Virtual Class Framework | Base class with overridable event methods |
| Interface-based Framework | Interface contract for each trigger event |
| Apex Enterprise Patterns | Full fflib-style architecture |

---

### 4.3 Non-Functional Requirements

> Reference: [Scalable Solutions](https://www.apexhours.com/building-scalable-solutions-on-salesforce/) | [Security in Salesforce](https://www.apexhours.com/security-in-salesforce/)

- [ ] **[Scalability](https://www.apexhours.com/building-scalable-solutions-on-salesforce/)** — Code is designed to support a large number of users and record volumes (bulk-safe, no per-record design assumptions)
- [ ] **Performance** — Acceptable performance confirmed with large datasets; SOQL queries are selective and indexed
- [ ] **[Security](https://www.apexhours.com/security-in-salesforce/)** — Object-level security (CRUD), field-level security (FLS), and sharing rules are enforced appropriately
- [ ] **Maintainability** — Code can be modified without cascading side effects; clear comments and self-documenting naming used

---

### 4.4 Apex Best Practices

> Reference: [20 Apex Best Practices](https://www.apexhours.com/apex-code-best-practices/) | [Test Class Best Practices](https://www.apexhours.com/apex-test-class-best-practices/) | [SOQL Best Practices](https://www.apexhours.com/soql-best-practices/)

- [ ] **Prefer clicks over code** — Use declarative tools (Flow, Validation Rules, Formula Fields) wherever possible
- [ ] **Avoid hardcoding IDs** — Use Custom Metadata, Custom Labels, or Custom Settings instead
- [ ] **Bulkify code** — All Apex logic must handle collections of 200 records at a time
- [ ] **No DML or [SOQL](https://www.apexhours.com/soql-best-practices/) inside `for` loops** — Move all database operations outside of iteration blocks
- [ ] **One Trigger per Object** — A single trigger per sObject per event context; delegate to handler classes
- [ ] **Use Maps for queries** — Map-based lookups for efficient cross-referencing of queried data (e.g., `Map<Id, SObject>`)
- [ ] **Use `Limits` methods** — Check governor limits proactively using `Limits.getQueries()`, `Limits.getDmlStatements()`, etc.

#### Apex Test Class Checklist

> Reference: [Apex Test Class Best Practices](https://www.apexhours.com/apex-test-class-best-practices/)

- [ ] Write meaningful assertions — do not rely on coverage alone
- [ ] One logical assertion per test method
- [ ] Follow **Test Driven Development (TDD)** where feasible
- [ ] Target **100% code coverage**, with 75% as the absolute minimum
- [ ] Write test methods that verify **large dataset scenarios** (`@isTest static void testBulkProcessing()`)
- [ ] Use a `TestDataFactory` class for all test data creation; avoid inline `insert` calls
- [ ] Use `System.runAs()` to test sharing and permission scenarios

---

### 4.5 Object-Oriented Analysis & Design (OOAD)

> Reference: [Apex Enterprise Patterns](https://www.apexhours.com/apex-enterprise-patterns/)

- [ ] **Separation of Concerns** — Business logic, data access, and presentation layers are kept separate
- [ ] **DRY Principle** — No duplicated logic; shared utilities are extracted into helper/service classes
- [ ] **SOLID Principles:**

| Principle | Description |
|---|---|
| **S** — Single Responsibility | Each class or method has one and only one reason to change |
| **O** — Open/Closed | Classes are open for extension but closed for modification |
| **L** — Liskov Substitution | Subclasses can replace their base class without breaking behaviour |
| **I** — Interface Segregation | Clients should not depend on interfaces they don't use |
| **D** — Dependency Inversion | Depend on abstractions, not concretions |

- [ ] **Dependency Injection** — Dependencies are injected rather than instantiated inside classes, enabling testability and flexibility

---

### 4.6 Static Code Analysis

> Reference: [Apex PMD Static Code Analyzer](https://www.apexhours.com/apex-pmd-static-code-analysis/)

- [ ] Run **[PMD for Apex](https://www.apexhours.com/apex-pmd-static-code-analysis/)** (or equivalent static analysis tool) before submitting for review
- [ ] Resolve all **Critical** and **High** severity violations
- [ ] Address **Medium** severity violations where feasible
- [ ] Automate PMD checks in the CI/CD pipeline (e.g., Salesforce CLI pre-deploy hook or GitHub Actions)

#### Common PMD Rule Categories for Apex

| Category | Examples |
|---|---|
| Best Practices | Avoid globals, DebugsShouldUseLoggingLevel |
| Code Style | FieldNamingConventions, VariableNamingConventions |
| Design | ExcessiveClassLength, ExcessiveMethodLength, TooManyFields |
| Error Prone | EmptyCatchBlock, EmptyIfStmt |
| Performance | AvoidDmlStatementsInLoops, AvoidSoqlInLoops |
| Security | ApexCRUDViolation, ApexFlsViolation, ApexSharingViolations |

---

### 4.7 Integration Patterns

> Reference: [Integration Pattern Best Practices](https://www.apexhours.com/salesforce-integration-pattern-best-practices/)

- [ ] Follow [Salesforce Integration Patterns](https://www.apexhours.com/salesforce-integration-pattern-best-practices/) appropriate to the use case
- [ ] Use proper error handling and retry logic for callouts
- [ ] Avoid callouts inside triggers (use `@future` or `Queueable`)
- [ ] Implement idempotency where applicable (safe to retry without side effects)
- [ ] Secure all external endpoints (Named Credentials, no hardcoded auth)

#### Integration Pattern Quick Reference

| Pattern | Use Case |
|---|---|
| Request & Reply | Real-time sync; caller waits for response |
| Fire & Forget | Async outbound; no response needed |
| Batch Data Sync | Large dataset sync on a schedule |
| Remote Call-In | External system calls into Salesforce |
| UI Update | Browser-based live update via streaming |

---

## 5. Clean Code Principles

Clean code should satisfy all of the following:

- **Readable by others** — Any developer (not just the author) can understand intent without needing to ask questions
- **Easy to maintain** — Low risk of accidentally introducing bugs when making changes
- **Safe to modify** — No hidden coupling; changing one method doesn't unexpectedly break another
- **Not overly compact** — Avoid cramming multiple operations onto one line; clarity beats cleverness

---

## 6. Code Review Best Practices (Process)

> Reference: [How to Improve Salesforce Code Quality](https://www.apexhours.com/how-to-improve-salesforce-code-quality/)

1. **Automate first** — Use PMD, Salesforce Scanner, or Gearset Code Review to catch mechanical issues automatically before human review
2. **Define review goals** — Agree upfront on what the review is checking (correctness, security, performance, style, etc.)
3. **Follow a standard submission checklist** — Authors self-review against this checklist before requesting review
4. **During review:**
   - Respond in a timely fashion (agree on an SLA, e.g. 24 hours)
   - Set clear expectations on scope and depth
   - Aim to resolve reviews quickly to avoid blocking deployment
5. **Compile feedback constructively** — Frame comments as requests or questions, not commands or criticisms
6. **Be open to follow-up** — Authors should clarify intent; reviewers should acknowledge context

---

## 7. Further Learning

| Topic | Link |
|---|---|
| Apex Code Best Practices (20 rules) | [apexhours.com/apex-code-best-practices](https://www.apexhours.com/apex-code-best-practices/) |
| Apex Naming Conventions | [apexhours.com/salesforce-naming-conventions-best-practices](https://www.apexhours.com/salesforce-naming-conventions-best-practices/) |
| Code Layouts & Formatting | [apexhours.com/code-layouts-and-formatting-salesforce](https://www.apexhours.com/code-layouts-and-formatting-salesforce/) |
| Order of Execution | [apexhours.com/order-of-execution-in-salesforce](https://www.apexhours.com/order-of-execution-in-salesforce/) |
| Exception Handling with Platform Events | [apexhours.com/exception-handling-using-platform-events](https://www.apexhours.com/exception-handling-using-platform-events/) |
| Trigger Framework in Salesforce | [apexhours.com/trigger-framework-in-salesforce](https://www.apexhours.com/trigger-framework-in-salesforce/) |
| Building Scalable Solutions | [apexhours.com/building-scalable-solutions-on-salesforce](https://www.apexhours.com/building-scalable-solutions-on-salesforce/) |
| Security in Salesforce | [apexhours.com/security-in-salesforce](https://www.apexhours.com/security-in-salesforce/) |
| SOQL Best Practices | [apexhours.com/soql-best-practices](https://www.apexhours.com/soql-best-practices/) |
| Apex Test Class Best Practices | [apexhours.com/apex-test-class-best-practices](https://www.apexhours.com/apex-test-class-best-practices/) |
| Apex Enterprise Patterns (fflib) | [apexhours.com/apex-enterprise-patterns](https://www.apexhours.com/apex-enterprise-patterns/) |
| Apex PMD Static Code Analysis | [apexhours.com/apex-pmd-static-code-analysis](https://www.apexhours.com/apex-pmd-static-code-analysis/) |
| Integration Pattern Best Practices | [apexhours.com/salesforce-integration-pattern-best-practices](https://www.apexhours.com/salesforce-integration-pattern-best-practices/) |
| Technical Debt | [apexhours.com/what-is-technical-debt](https://www.apexhours.com/what-is-technical-debt/) |
| Clean Code in Salesforce | [apexhours.com/clean-code-in-salesforce](https://www.apexhours.com/clean-code-in-salesforce/) |
| Improve Salesforce Code Quality | [apexhours.com/how-to-improve-salesforce-code-quality](https://www.apexhours.com/how-to-improve-salesforce-code-quality/) |

---

*Generated from [apexhours.com/code-review-checklist](https://www.apexhours.com/code-review-checklist) and its deep-linked references. All credit to Amit Chaudhary and the Apex Hours team.*
