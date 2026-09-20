# Contributing Skills

## Directory structure

```text
skills/
└── your-skill-name/
    ├── SKILL.md
    ├── references/     # optional, detailed supporting material
    ├── templates/      # optional, reusable starting files
    ├── scripts/        # optional, deterministic helpers
    └── assets/         # optional, images or other static assets
```

## `SKILL.md` requirements

- Use a lowercase hyphenated directory and skill name.
- Start with YAML frontmatter.
- Make `description` one concise sentence beginning with “Use when …”.
- Put detailed procedures in the Markdown body.
- Describe prerequisites, safety constraints, outputs, and verification when applicable.
- Keep secrets, API tokens, private URLs, and environment-specific credentials out of the repository.

## Quality bar

A contribution should solve a repeatable task, be self-contained, and contain operational details that improve on generic instructions. Include examples where they remove ambiguity.
