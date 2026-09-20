# Contributing to om-linkedin-tools

Thank you for helping grow this resource. Contributions that add new tool assessments, correct errors, or improve the safe-usage guidance are all welcome.

## How to add a tool assessment

1. **Open an issue first** using the [tool request template](.github/ISSUE_TEMPLATE/tool-request.md) to confirm the tool is in scope and not already assigned.
2. **Fork the repo** and create a branch named `assess/<tool-name>`.
3. **Copy** `tools/_template/README.md` to `tools/<tool-name>/README.md`.
4. **Inspect the tool** using static analysis only (no installation). See the methodology in the main [README](README.md).
5. **Fill in all sections** of the template. Every finding must have a mitigation.
6. **Add a row** to the assessments table in `README.md`.
7. **Open a pull request.** It will be reviewed within 5 business days.

## Scope

In scope:
- Open-source LinkedIn automation, analytics, or content tools
- Claude Code / Codex skills that interact with LinkedIn or social platforms
- Tools that request LinkedIn credentials or API tokens

Out of scope:
- Closed-source or commercial tools (we cannot inspect source)
- Tools unrelated to professional networking or content

## Standards

- All findings must be reproducible from the public source code — no speculation
- Severity ratings follow the scale defined in the template
- Do not publish exploit code or working attack PoCs
- Assessment language must be factual and neutral — no marketing language

## License

By contributing you agree your work is licensed CC BY 4.0 with attribution to omopen.ai.
