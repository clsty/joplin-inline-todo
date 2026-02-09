# Query Summary Examples

This file contains practical examples of using the Query Summary Note feature.

## Example 1: Simple Category Filter

**Use Case**: Show only work-related tasks

```json
{
  "query": {
    "CATEGORY": "work"
  }
}
```

**Input TODOs**:
```markdown
- [ ] @work Complete project report
- [ ] @personal Buy groceries
- [ ] @work Review code changes
```

**Output**:
```markdown
- [ ] @work Complete project report
- [ ] @work Review code changes
```

---

## Example 2: Multiple Conditions with AND

**Use Case**: Show urgent work tasks that are still open

```json
{
  "query": {
    "AND": [
      {"CATEGORY": "work"},
      {"TAG": "urgent"},
      {"STATUS": "open"}
    ]
  }
}
```

**Input TODOs**:
```markdown
- [ ] @work +urgent Fix production bug
- [ ] @work +later Update documentation
- [x] @work +urgent Deploy hotfix
- [ ] @personal +urgent Call dentist
```

**Output**:
```markdown
- [ ] @work +urgent Fix production bug
```

---

## Example 3: Custom Sorting and Grouping

**Use Case**: Organize work tasks by priority tags

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
      "sortBy": "tag",
      "sortOrder": "custom",
      "sortOrderCustom": "critical,high,medium,low"
    }
  ],
  "groupLevel": 1
}
```

**Input TODOs**:
```markdown
- [ ] @work +medium Update API documentation
- [ ] @work +critical Fix security vulnerability
- [ ] @work +low Refactor old code
- [ ] @work +high Complete feature X
```

**Output**:
```markdown
# critical
- [ ] @work +critical Fix security vulnerability

# high
- [ ] @work +high Complete feature X

# medium
- [ ] @work +medium Update API documentation

# low
- [ ] @work +low Refactor old code
```

---

## Example 4: Multi-Level Grouping

**Use Case**: Organize tasks by category and then by priority

```json
{
  "query": {
    "STATUS": "open"
  },
  "sortOptions": [
    {
      "sortLevel": "1",
      "sortBy": "category",
      "sortOrder": "custom",
      "sortOrderCustom": "work,study,personal"
    },
    {
      "sortLevel": "2",
      "sortBy": "tag",
      "sortOrder": "custom",
      "sortOrderCustom": "urgent,normal,later"
    }
  ],
  "groupLevel": 2
}
```

**Input TODOs**:
```markdown
- [ ] @work +urgent Fix bug
- [ ] @work +later Code review
- [ ] @study +urgent Prepare exam
- [ ] @study +normal Read chapter 5
- [ ] @personal +urgent Pay bills
```

**Output**:
```markdown
# work
## urgent
- [ ] @work +urgent Fix bug

## later
- [ ] @work +later Code review

# study
## urgent
- [ ] @study +urgent Prepare exam

## normal
- [ ] @study +normal Read chapter 5

# personal
## urgent
- [ ] @personal +urgent Pay bills
```

---

## Example 5: Date-Based Sorting

**Use Case**: Show all open tasks sorted by due date

```json
{
  "query": {
    "STATUS": "open"
  },
  "sortOptions": [
    {
      "sortLevel": "1",
      "sortBy": "date",
      "sortOrder": "ascend"
    }
  ],
  "groupLevel": 0
}
```

**Input TODOs**:
```markdown
- [ ] @work //2026-02-15 Submit report
- [ ] @work //2026-02-10 Team meeting
- [ ] @personal Review insurance
- [ ] @study //2026-02-12 Complete assignment
```

**Output**:
```markdown
- [ ] @work //2026-02-10 Team meeting
- [ ] @study //2026-02-12 Complete assignment
- [ ] @work //2026-02-15 Submit report
- [ ] @personal Review insurance
```
(Tasks without dates appear at the end)

---

## Example 6: OR Logic for Multiple Categories

**Use Case**: Show tasks from either work or study categories

```json
{
  "query": {
    "OR": [
      {"CATEGORY": "work"},
      {"CATEGORY": "study"}
    ]
  }
}
```

**Input TODOs**:
```markdown
- [ ] @work Finish presentation
- [ ] @study Read textbook
- [ ] @personal Exercise
- [ ] @work Code review
```

**Output**:
```markdown
- [ ] @work Finish presentation
- [ ] @study Read textbook
- [ ] @work Code review
```

---

## Example 7: Negation - Exclude Completed Tasks

**Use Case**: Show all tasks except completed ones

```json
{
  "query": {
    "STATUS": "done",
    "negated": true
  }
}
```

**Input TODOs**:
```markdown
- [ ] @work Ongoing task
- [x] @work Completed task
- [ ] @personal Open task
- [x] @study Finished homework
```

**Output**:
```markdown
- [ ] @work Ongoing task
- [ ] @personal Open task
```

---

## Example 8: Complex Nested Query

**Use Case**: Show high-priority tasks from work or personal categories that are still open

```json
{
  "query": {
    "AND": [
      {
        "OR": [
          {"CATEGORY": "work"},
          {"CATEGORY": "personal"}
        ]
      },
      {
        "OR": [
          {"TAG": "urgent"},
          {"TAG": "high"}
        ]
      },
      {"STATUS": "open"}
    ]
  },
  "sortOptions": [
    {
      "sortLevel": "1",
      "sortBy": "category",
      "sortOrder": "ascend"
    },
    {
      "sortLevel": "2",
      "sortBy": "tag",
      "sortOrder": "custom",
      "sortOrderCustom": "urgent,high"
    }
  ],
  "groupLevel": 2
}
```

**Input TODOs**:
```markdown
- [ ] @work +urgent Security patch
- [ ] @work +high Feature release
- [ ] @work +low Documentation
- [ ] @personal +urgent Doctor appointment
- [ ] @personal +low Clean garage
- [ ] @study +high Exam preparation
- [x] @work +urgent Previous sprint
```

**Output**:
```markdown
# personal
## urgent
- [ ] @personal +urgent Doctor appointment

# work
## urgent
- [ ] @work +urgent Security patch

## high
- [ ] @work +high Feature release
```

---

## Example 9: Completed Tasks Report

**Use Case**: View recently completed tasks grouped by category

```json
{
  "query": {
    "STATUS": "done"
  },
  "sortOptions": [
    {
      "sortLevel": "1",
      "sortBy": "category",
      "sortOrder": "ascend"
    }
  ],
  "groupLevel": 1
}
```

**Input TODOs**:
```markdown
- [x] @work Deploy feature
- [x] @work Update docs
- [x] @personal Buy gift
- [x] @study Finish homework
```

**Output**:
```markdown
# personal
- [x] @personal Buy gift

# study
- [x] @study Finish homework

# work
- [x] @work Deploy feature
- [x] @work Update docs
```

---

## Tips for Creating Effective Queries

1. **Start Simple**: Begin with a basic query and add complexity incrementally
2. **Test Your Query**: After creating a query, check if the results match your expectations
3. **Use Descriptive Tags**: Consistent tag naming makes queries more powerful
4. **Combine Sorting**: Use multi-level sorting for better organization
5. **Document Your Queries**: Add comments in your note explaining what the query does

## Common Patterns

### Weekly Review
```json
{
  "query": {
    "AND": [
      {"STATUS": "open"},
      {
        "OR": [
          {"TAG": "this-week"},
          {"TAG": "urgent"}
        ]
      }
    ]
  },
  "sortOptions": [
    {"sortLevel": "1", "sortBy": "category", "sortOrder": "ascend"},
    {"sortLevel": "2", "sortBy": "date", "sortOrder": "ascend"}
  ],
  "groupLevel": 1
}
```

### Project Dashboard
```json
{
  "query": {
    "AND": [
      {"NOTEBOOK": "project-id-here"},
      {"STATUS": "open"}
    ]
  },
  "sortOptions": [
    {"sortLevel": "1", "sortBy": "tag", "sortOrder": "custom", 
     "sortOrderCustom": "blocked,in-progress,ready,backlog"}
  ],
  "groupLevel": 1
}
```

### Priority Inbox
```json
{
  "query": {
    "AND": [
      {"STATUS": "open"},
      {
        "OR": [
          {"TAG": "urgent"},
          {"TAG": "important"}
        ]
      }
    ]
  },
  "sortOptions": [
    {"sortLevel": "1", "sortBy": "date", "sortOrder": "ascend"}
  ],
  "groupLevel": 0
}
```
