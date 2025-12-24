# Database

> Schema, relationships, and query conventions.

## Overview

**Database:** [Type]
**ORM/Query:** [Library or raw SQL]
**Location:** [Connection info or file path]

## Schema

### [table_name]

[Purpose of this table]

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | [type] | PRIMARY KEY | [Description] |
| [column] | [type] | [constraints] | [Description] |
| created_at | [type] | NOT NULL | Timestamp |
| updated_at | [type] | | Timestamp |

**Indexes:**
- [index_name] on [column] - [why indexed]

**Relationships:**
- Has many: [related_table]
- Belongs to: [parent_table]

### [another_table]

...

## Entity Relationship Diagram

```
[Your ERD here]
```

## Query conventions

### Repository pattern

```
[Your repository pattern here]
```

### Parameterized queries

Always use parameterized queries. See docs/pitfalls.md#sql-injection.

### Date handling

[Your date/time conventions here]

### Transactions

```
[Your transaction pattern here]
```

## Migrations

**Location:** [Path]
**Format:** [Naming convention]

```
[Example migration structure]
```

## Common queries

### [Query pattern name]

```
[Query with description of when to use]
```
