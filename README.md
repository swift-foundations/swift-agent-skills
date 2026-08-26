# swift-agent-skills

![Development Status](https://img.shields.io/badge/status-active--development-blue.svg)

Parsing and validating canonical agent-skill documents: closed YAML frontmatter carrying only `name` and `description`, a non-empty body, and the canonical limits.

## Overview

This package owns the agent-skill document contract for the Institute. `Skill.Document` parses a skill source and enforces the deliberately small interface:

- frontmatter is a closed `---` block containing exactly `name` and `description` (scalar or block-literal), each at most once;
- the name is 1–64 bytes of lowercase ASCII letters, digits, and interior hyphens, and must match the directory name when one is expected;
- the description is non-empty after trimming, at most 1024 UTF-8 bytes, and free of angle-bracket markup;
- the body is non-empty and the whole document stays within 500 lines.

Every violation is a typed `Skill.Error` case with a human-readable description. Routing, dependencies, and review history belong in the skill index, prose, or version control — never in boot metadata — so no other fields exist.

## Installation

```swift
dependencies: [
    .package(url: "https://github.com/swift-compositions/swift-agent-skills.git", branch: "main")
]
```

## Usage

```swift
import Skill_Validation

let document = try Skill.Document(source: source, expectedName: "example-skill")
print(document.name, document.description)
```
