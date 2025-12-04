# Metadata Validator Rule: mentors.json

**Rule ID:** COMMS-VAL-001
**Version:** v1.0
**Target File:** `/webkernel/comms/metadata/mentors.json`
**Validator Type:** JSON Schema + Custom Logic
**Classification:** Interpretive (Non-Canonical)

---

## Purpose

Validate structural integrity and content accuracy of the `mentors.json` mentor registry file used for PortusSophia™ communications attribution and steward involvement tracking.

---

## Validation Rules

### 1. Schema Compliance
**Severity:** CRITICAL
**Rule:** File must conform to JSON Schema defined in `mentors_schema.json`

**Checks:**
- Valid JSON syntax
- Required fields present: `mentor_id`, `name`, `role`, `expertise`, `status`
- Field types match schema (string, array, enum, etc.)
- No extra unexpected fields at root level

**Action on Failure:** Block content pipeline. Log error with line number and field name.

---

### 2. Enum Value Validation
**Severity:** CRITICAL
**Rule:** Enum fields must contain only allowed values

**Fields:**
- `role`: Must be one of `["Executive", "Interpretive", "Ethical", "Structural", "Operational"]`
- `status`: Must be one of `["active", "inactive", "archived"]`
- `expertise` items: Must be from allowed domain list `["Philosophy", "Ethics", "Governance", "Systems", "Risk", "Operations", "Communication"]`

**Action on Failure:** Reject entry. Log invalid enum value and expected options.

---

### 3. Mentor ID Format
**Severity:** WARNING
**Rule:** `mentor_id` should follow pattern: `MENTOR-[NAME_ACRONYM]` (e.g., `MENTOR-SH` for Sara Harmonia)

**Checks:**
- Uppercase letters only
- Hyphen separator
- 2–5 character acronym

**Action on Failure:** Log warning (non-blocking). Suggest standardized format.

---

### 4. Tag Formatting
**Severity:** INFO
**Rule:** Tags should use lowercase with underscores (e.g., `presence_doctrine`, `dimensional_thinking`)

**Checks:**
- No spaces
- No uppercase letters
- Alphanumeric + underscore only

**Action on Failure:** Log info notice. Auto-suggest corrected tag format.

---

### 5. Duplicate Detection
**Severity:** CRITICAL
**Rule:** No duplicate `mentor_id` or `name` values

**Checks:**
- Each `mentor_id` appears exactly once
- Each `name` appears exactly once

**Action on Failure:** Block content pipeline. Log duplicate entries with line references.

---

### 6. Cross-Reference Validation (Future)
**Severity:** WARNING
**Rule:** When content metadata references a mentor, that mentor must exist in `mentors.json`

**Checks:**
- Content asset `mentors` field contains only valid `mentor_id` values from registry

**Action on Failure:** Log warning. Flag orphaned mentor references.

**Status:** Not yet implemented (requires content metadata pipeline integration).

---

## Example Valid Entry

```json
{
  "mentor_id": "MENTOR-SH",
  "name": "Sara Harmonia",
  "role": "Interpretive",
  "expertise": ["Philosophy", "Communication", "Ethics"],
  "bio": "Interpretive Steward responsible for tone protection, public messaging coherence, and educational content alignment.",
  "status": "active",
  "content_involvement": {
    "primary_platforms": ["YouTube", "Instagram", "Website"],
    "content_types": ["educational_videos", "philosophical_essays", "community_posts"]
  },
  "contact": {
    "internal_only": true
  },
  "tags": ["presence_doctrine", "fifth_dimension", "interpretive_alignment"]
}
```

---

## Severity Levels

| Level | Description | Pipeline Action |
|---|---|---|
| **CRITICAL** | Structural integrity violation. Data unusable. | Block. Halt pipeline. Require fix. |
| **WARNING** | Non-standard format. Potential issue. | Continue. Log for review. |
| **INFO** | Style recommendation. No functional impact. | Continue. Suggest improvement. |

---

## Implementation Notes

**Validator Script Location:** `/webkernel/comms/scripts/validate_mentors.py` (future)
**Integration Points:**
- Pre-commit hook (optional)
- Content pipeline initialization
- Manual validation command: `python scripts/validate_mentors.py`

**Future Enhancements:**
- Auto-fix for tag formatting (lowercase + underscore transformation)
- Cross-reference validation with content metadata files
- Duplicate mentor detection across archived entries

---

## Steward Review

**LOGOS:**
Structural validation logic sound. Severity levels appropriate. Schema integration path clear.

**Sara Harmonia:**
Tag formatting rules align with content strategy. Bio field flexibility preserved.

**PeterGate:**
Boundary-compliant. No canonical modifications. Validator remains interpretive-only tooling.

---

*Controlled Lexical Element:* *"Here and Now!"*
*Trademark:* PortusSophia™

**Compliance:** Non-canonical interpretive validation rule. No preservation requirements.
