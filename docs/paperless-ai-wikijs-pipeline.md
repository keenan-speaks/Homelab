# Paperless AI → Wiki.js Knowledge Pipeline

**Version:** 1.0  
**Status:** Production  
**Automation:** n8n  
**AI Model:** Qwen3 4B via Ollama

## Purpose

This pipeline turns selected Paperless-ngx documents into curated Wiki.js knowledge articles while keeping Paperless as the document archive and source of truth.

The system is intentionally designed so AI cannot automatically publish arbitrary Paperless documents to Wiki.js.

## Workflow

### 1. Document Ingestion

A document is uploaded or scanned into Paperless-ngx.

Paperless stores the original document and extracts its searchable text.

### 2. AI Analysis and Tagging

n8n sends the document content to Qwen through Ollama.

Qwen suggests relevant tags.

The workflow:

- Retrieves existing Paperless tags.
- Preserves tags already assigned to the document.
- Compares AI suggestions with existing tags.
- Creates missing tags when necessary.
- Combines existing and AI-generated tag IDs.
- Updates the Paperless document.

Existing user-assigned tags are never intentionally removed by the AI tagging process.

### 3. Wiki Eligibility Gate

After tagging, the workflow checks whether the document contains the special:

`wiki-source`

tag.

Without `wiki-source`:

Paperless → AI tagging → STOP

With `wiki-source`:

Paperless → AI tagging → Wiki.js knowledge pipeline

This provides an explicit safety boundary between the private Paperless document archive and the Wiki.js knowledge base.

### 4. Wiki Knowledge Generation

For eligible documents, Qwen generates a structured proposed Wiki.js article containing:

- Title
- Path
- Summary
- Content
- Tags

The Ollama request uses structured JSON output.

### 5. Namespace Validation

Generated Wiki.js paths must be inside:

`knowledge/`

Content attempting to use another namespace is rejected before reaching human review.

### 6. Human Review

A Human Review Gate pauses the n8n execution.

The reviewer can inspect:

- Title
- Path
- Summary
- Article content
- Tags

The reviewer must explicitly choose:

- Approve
- Reject

Rejected articles are not published.

### 7. Duplicate Protection

Approved articles are checked against Wiki.js before creation.

If the proposed path already exists:

`DUPLICATE → STOP`

If the path does not exist:

`CREATE WIKI.JS PAGE`

This prevents the workflow from blindly recreating an existing knowledge article.

## Safety Model

The publication path therefore requires multiple gates:

`Paperless`
→ `wiki-source`
→ `AI generation`
→ `knowledge/ namespace validation`
→ `Human approval`
→ `Duplicate check`
→ `Wiki.js`

A Paperless document cannot reach Wiki.js merely because it was uploaded or scanned.

## Human Review Notifications

Version 1.0 includes the Human Review Gate, but does not yet provide a dedicated external notification when a review is waiting.

A future enhancement may provide:

`Document → AI → Review Required → Notification → Review Link → Approve/Reject`

Possible notification targets include email or another HomeLab notification service.

## Reprocessing and Backfill

The architecture should support three processing methods:

1. Automatic processing of new documents.
2. Manual reprocessing of a single existing document.
3. Controlled backfill of existing Paperless documents.

Backfill operations must retain the same safety controls used by normal processing.

Mass backfill must never bypass:

- `wiki-source` eligibility
- namespace validation
- human review
- duplicate protection

## Verified v1.0 Behavior

- Existing Paperless tags preserved
- AI tags successfully added
- Missing Paperless tags created safely
- `wiki-source` eligibility gate working
- Qwen Wiki knowledge generation working
- Structured JSON parsing working
- `knowledge/` namespace validation working
- Human review form working
- Review content wraps correctly
- Approve path tested
- Reject path tested
- Duplicate protection tested
- Approved Wiki.js page successfully created

## Architecture Principle

**Paperless stores documents. Wiki.js stores curated knowledge.**

AI assists with organization and transformation, but publication into the knowledge base remains under human control.
