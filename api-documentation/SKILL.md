---
name: api-documentation
description: Create or update API_DOCUMENTATION.md using the project's established format and style
---

Create or update the `API_DOCUMENTATION.md` file in the current project root, following the style and structure below exactly.

## Step 1 — Assess the current state

Run these in parallel:
- Check if `API_DOCUMENTATION.md` already exists (use Glob or Read)
- Read the router files in `routers/` to understand all registered routes
- Read the relevant controller files to understand request/response shapes, validation, and error cases
- Check `app.js` or the entry point to understand the base URL, middleware, and rate limit tiers

If `API_DOCUMENTATION.md` already exists, read it fully before making any changes. Identify what needs to be added, modified, or removed — do not rewrite sections that are still accurate.

## Step 2 — Gather endpoint details

For each endpoint (new or changed), collect:
- HTTP method and full route (e.g., `POST /api/v1/users/signup`)
- Authentication requirement: `None (Public)`, `JWT required`, or role restriction (e.g., `Admin only`)
- Rate limit tier (check middleware — typically `100 requests per hour per IP` for general, `5 requests per hour per IP` for auth endpoints)
- Use case: one or two sentences describing when and why a client calls this endpoint
- Request body: all accepted fields with types and descriptions
- Which fields are required vs. optional
- All success responses with status codes and full JSON shape
- All error responses with status codes and messages
- Any special notes (edge cases, side effects, env variable dependencies, migration behavior, etc.)

## Step 3 — Write or update the document

Follow this structure and formatting precisely:

---

### Document header

```markdown
# API Documentation

<Project name and one-line description>

Base URL: `/api/v1`

---
```

---

### Table of contents

Group entries by section. Each line: numbered entry linked to its heading, with inline method + route.

```markdown
## Contents

### <Section Name>
- [1. Endpoint Name](#1-endpoint-name) — `METHOD /api/v1/route`
- [2. Another Endpoint](#2-another-endpoint) — `METHOD /api/v1/route` *(Role)*
```

Append `*(Admin)*`, `*(Treasurer)*`, etc. for role-restricted endpoints.

Use `---` after the contents block before the first section.

---

### Section headers

```markdown
## <Section Name> Endpoints
```

Or for top-level standalone sections (Health Check, Calendar Subscription, Logs):

```markdown
## <Section Name>
```

---

### Endpoint entry format

Every endpoint must follow this template exactly, in this field order:

````markdown
### N. Endpoint Name (Role if restricted)

**Method:** `METHOD`
**Route:** `/api/v1/route`
**Authentication:** <None (Public) | JWT required | Admin only | Treasurer only>
**Rate Limit:** <X requests per hour per IP>

**Use Case:**
<One to two sentences describing when and why a client calls this endpoint.>

**Request Body:**
```json
{
  "field": "exampleValue"
}
```

**Required Fields:**
- `field` — description

**Optional Fields:**
- `field` — description

**Success Response (STATUS Description):**
```json
{
  "status": "success",
  "data": { ... }
}
```

**Error Responses:**
| Status | Message | Cause |
|--------|---------|-------|
| 400 | "..." | ... |
| 401 | "..." | ... |

**Special Notes:**
- Note one
- Note two

---
````

**Field-by-field rules:**
- `**Method:**` — backtick-wrapped uppercase verb (`GET`, `POST`, `PATCH`, `DELETE`)
- `**Route:**` — backtick-wrapped full path including base (e.g., `` `/api/v1/users/:id` ``)
- `**Authentication:**` — plain text, no backticks
- `**Rate Limit:**` — plain text
- `**Use Case:**` — paragraph on the next line, no bullet
- `**Request Body:**` — omit entirely for endpoints with no body (GET, DELETE with no body)
- `**Required Fields:**` / `**Optional Fields:**` — omit if no body; use backtick field names and an em dash separator
- `**Success Response:**` — include status code and HTTP reason phrase in parentheses
- `**Error Responses:**` — Markdown table; include every distinct error the endpoint can return
- `**Special Notes:**` — omit if there are no special cases worth documenting
- End every entry with `---`

---

### Numbering and ordering

- Number entries sequentially across the entire document (not per section)
- Order within a section: read/list first, then write/create, then update, then delete
- Public/unauthenticated endpoints come before protected ones within the same section

---

## Step 4 — Verify completeness

Before finishing, confirm:
- Every route registered in the router files appears in the document
- The table of contents matches all section headings and entry anchors
- Numbering is sequential with no gaps or duplicates
- No stale entries remain for routes that have been removed
- JSON examples are valid (no trailing commas, matching braces)
- All role restrictions match what the middleware actually enforces

## Step 5 — Report

After writing or updating the file, output a brief summary:
- How many endpoints were added, updated, or removed
- Any sections that were restructured
- Any gaps or ambiguities found in the source code that could not be resolved from reading the code alone
