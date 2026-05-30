# test-writer

> Generate comprehensive test cases from code or requirements

**Category**: Development
**Author**: UnDercontrol
**Tags**: `development`, `testing`, `quality`

## What it does

This skill generates thorough test suites covering happy paths, edge cases, error scenarios, and integration points. It follows testing best practices like one-assertion-per-concept, descriptive naming, and table-driven tests.

## Install

```bash
ud market install skill test-writer
```

Or manually:
```bash
curl -sL https://raw.githubusercontent.com/oatnil-top/ud-schemas/main/community/skills/test-writer/skill.yaml | ud apply -f -
```

## Usage

```bash
ud prompt test-writer
```
