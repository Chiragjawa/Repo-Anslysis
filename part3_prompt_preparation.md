# Prompt Preparation

## Selected PR
PR #4199 – Duplicate Keys & Query Enhancements

---

## Repository Context

Beets is a command-line music library management system written in Python. It actively manages metadata by fixing tags, renaming files, and maintaining a local SQLite database. The system targets power users comfortable with CLI tools and YAML-based configuration.

---

## Pull Request Description

### Previous Behavior
Duplicate detection during import was based on hardcoded fields such as artist and title, leading to false positives.

### New Behavior
Users can now configure which fields define uniqueness using `duplicate_keys`. The query system is enhanced to support exact (`=`) and regex (`~`) matches.

---

## Acceptance Criteria

| ID | Criteria | Expected Behavior |
|----|---------|------------------|
| AC1 | Config Loading | Uses `duplicate_keys` or defaults |
| AC2 | Exact Match | `field=value` is strict |
| AC3 | Regex Match | `field~pattern` uses regex |
| AC4 | Custom Duplicate Logic | Uses configured keys |
| AC5 | Backward Compatibility | `:` queries still fuzzy |

---

## Edge Cases

| Case | Scenario | Expected Handling |
|-----|----------|------------------|
| EC1 | Missing Keys | Ignored safely |
| EC2 | Case Sensitivity | Matches DB behavior |
| EC3 | Empty Keys | No duplicate detection |

---

## Initial Prompt

You are an expert Python developer contributing to the Beets open-source music library.

**Objective:** Implement configurable duplicate detection and enhance query parsing to support exact and regex matching.

**Tasks:**
- Update query parser to support `=` and `~`
- Add `duplicate_keys` to default config
- Refactor importer duplicate detection logic

**Constraints:**
- Maintain backward compatibility
- Add tests validating exact match behavior

