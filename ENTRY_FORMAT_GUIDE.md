# Query Summary Entry Format Guide

## Overview

The `entryFormat` field in the query summary JSON configuration allows you to customize how each TODO entry is displayed. This provides complete control over the output format while maintaining all query, sorting, and grouping functionality.

## Configuration Location

The `entryFormat` is specified inside the `json:query-summary` code block:

```json
{
  "query": {
    "STATUS": "open"
  },
  "entryFormat": "- {{{STATUS}}} {{{CATEGORY}}} {{{TAGS}}} {{{DATE}}} {{{CONTENT}}} [origin](:/{{{NOTE_ID}}})"
}
```

## Available Placeholders

The following placeholders can be used in your format template:

| Placeholder | Description | Example Output |
|------------|-------------|----------------|
| `{{{STATUS}}}` | Checkbox status | `[ ]` or `[x]` |
| `{{{CATEGORY}}}` | TODO category with @ prefix | `@work` |
| `{{{TAGS}}}` | Space-separated tags with + prefix | `+health +expensive` |
| `{{{DATE}}}` | Due date with // prefix | `//2025-09-23` |
| `{{{CONTENT}}}` | The TODO message text | `买抗癌药。` |
| `{{{NOTE_ID}}}` | The note's unique ID | `08faeafdea3247598fd9f18e6c4784e5` |
| `{{{NOTE_TITLE}}}` | The note's title | `Shopping List` |
| `{{{NOTEBOOK}}}` | Parent notebook name | `Personal` |

## Default Format

If `entryFormat` is not specified, the default format is:

```
- {{{STATUS}}} {{{CATEGORY}}} {{{TAGS}}} {{{DATE}}} {{{CONTENT}}} [origin](:/{{{NOTE_ID}}})
```

**Example output:**
```markdown
- [ ] @work +health +expensive //2025-09-23 买抗癌药。 [origin](:/08faeafdea3247598fd9f18e6c4784e5)
```

## Custom Format Examples

### Example 1: Simple List Format

**Configuration:**
```json
{
  "query": {"STATUS": "open"},
  "entryFormat": "- {{{STATUS}}} {{{CONTENT}}}"
}
```

**Output:**
```markdown
- [ ] 买抗癌药。
- [ ] Complete project report
```

### Example 2: Link-First Format with Newlines

**Configuration:**
```json
{
  "query": {"STATUS": "open"},
  "entryFormat": "[{{{CONTENT}}}](:/{{{NOTE_ID}}}) {{{DATE}}} {{{TAGS}}}\\n"
}
```

**Output:**
```markdown
[买抗癌药。](:/08faeafdea3247598fd9f18e6c4784e5) //2025-09-23 +health +expensive

[Complete project report](:/a1b2c3d4e5f6) //2025-10-01 +work +important

```

### Example 3: Table Row Format

**Configuration:**
```json
{
  "query": {"STATUS": "open"},
  "entryFormat": "| {{{STATUS}}} | {{{CONTENT}}} | {{{CATEGORY}}} | {{{DATE}}} | [{{{NOTE_TITLE}}}](:/{{{NOTE_ID}}}) |"
}
```

**Output:**
```markdown
| [ ] | 买抗癌药。 | @work | //2025-09-23 | [Shopping List](:/08faeafdea3247598fd9f18e6c4784e5) |
| [ ] | Complete report | @work | //2025-10-01 | [Project Notes](:/a1b2c3d4e5f6) |
```

### Example 4: Compact Format

**Configuration:**
```json
{
  "query": {"STATUS": "open"},
  "entryFormat": "{{{STATUS}}} {{{CONTENT}}} ({{{NOTEBOOK}}})"
}
```

**Output:**
```markdown
[ ] 买抗癌药。 (Personal)
[ ] Complete report (Work)
```

### Example 5: Detailed Format with All Fields

**Configuration:**
```json
{
  "query": {"STATUS": "open"},
  "entryFormat": "### {{{CONTENT}}}\\n- Status: {{{STATUS}}}\\n- Category: {{{CATEGORY}}}\\n- Tags: {{{TAGS}}}\\n- Due: {{{DATE}}}\\n- From: [{{{NOTE_TITLE}}}](:/{{{NOTE_ID}}}) in {{{NOTEBOOK}}}\\n"
}
```

**Output:**
```markdown
### 买抗癌药。
- Status: [ ]
- Category: @work
- Tags: +health +expensive
- Due: //2025-09-23
- From: [Shopping List](:/08faeafdea3247598fd9f18e6c4784e5) in Personal

```

## Special Notes

### Empty Placeholders

If a placeholder has no value (e.g., no tags, no category), it will be replaced with an empty string. The template handles this automatically.

### Escaped Newlines

Use `\\n` in your template to insert actual newlines in the output:

```json
"entryFormat": "- {{{CONTENT}}}\\n  Note: [{{{NOTE_TITLE}}}](:/{{{NOTE_ID}}})\\n"
```

### Whitespace Handling

The template processor:
- Preserves intentional spaces in your template
- Removes extra consecutive spaces
- Trims leading/trailing whitespace from each entry

### Compatibility with Sorting and Grouping

The `entryFormat` only affects how individual TODO entries are displayed. It does **not** affect:
- Query filtering
- Sorting behavior
- Grouping logic
- Section headers

All these features continue to work normally with custom formats.

## Complete Example

Here's a complete query summary configuration using a custom format:

```json
{
  "query": {
    "AND": [
      {"CATEGORY": "work"},
      {"STATUS": "open"}
    ]
  },
  "sortOptions": [
    {
      "sortLevel": "1",
      "sortBy": "category",
      "sortOrder": "ascend"
    }
  ],
  "groupLevel": 1,
  "entryFormat": "- {{{STATUS}}} {{{CONTENT}}} `{{{CATEGORY}}}` {{{TAGS}}} [{{{NOTE_TITLE}}}](:/{{{NOTE_ID}}})"
}
```

This configuration:
1. Filters for open work items
2. Groups by category
3. Uses a custom format that shows content, category in code format, tags, and a link to the source note

## Creating a Query Summary Note

Two ways to create a query summary note:

1. **Via Menu**: Tools → Create Query summary note
2. **Manually**: Create a note and add a `json:query-summary` code block

The menu option creates a note with a basic template to get you started.

## Toolbar Refresh Button

When viewing a query summary note, a refresh button (🔄) appears in the toolbar. Click it to manually refresh the summary without waiting for auto-refresh.

The button only appears when you're viewing a note with a `json:query-summary` block.
