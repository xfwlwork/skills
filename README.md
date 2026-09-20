# skills

A curated collection of reusable agent skills.

Skills are portable instruction packages. Each skill lives in its own directory and has a `SKILL.md` file with YAML frontmatter followed by the workflow, constraints, and verification guidance.

## Install

Use the skill directory with an agent that supports the Agent Skills convention, or copy the folder into your local skills directory.

```text
skills/
└── black-white-minimal-html/
    └── SKILL.md
```

## Included skills

| Skill | Description |
| --- | --- |
| [`black-white-minimal-html`](skills/black-white-minimal-html/SKILL.md) | A black-and-white, line-based minimal HTML visual system for technical reports, product documentation, diagrams, and developer-facing pages. |
| [`html-report-to-png`](skills/html-report-to-png/SKILL.md) | Build a polished single-file HTML report and export it to a tightly cropped PNG with Playwright and Chrome verification. |

## Skill format

Each `SKILL.md` includes:

- a short trigger-oriented `description`
- workflow and design guidance
- reusable CSS and layout conventions
- responsiveness and rendering verification requirements

## Contributing

1. Create a directory under `skills/<skill-name>/`.
2. Add a `SKILL.md` with YAML frontmatter.
3. Keep the description concise and action-oriented.
4. Include validation steps when the skill creates artifacts or changes external state.
5. Open a pull request with a clear example or use case.

## License

MIT. See [LICENSE](LICENSE).
