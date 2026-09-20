# om-linkedin-tools

> Independent assessments of LinkedIn automation tools — plus free AI tools from [omopen.ai](https://omopen.ai)

**Maintained by [omopen.ai](https://omopen.ai) · Licensed CC BY 4.0**

---

## What This Is

This repository provides independent, no-install security and usability assessments of open-source LinkedIn automation tools. Each assessment covers credential handling, third-party data routing, platform Terms of Service compliance, and safe usage guidance.

This is not affiliated with LinkedIn, Microsoft, or any assessed project. Assessments reflect the state of each project at the time of review.

---

## Assessments

| Tool | Version Assessed | Overall Risk | LinkedIn ToS | Assessment |
|------|-----------------|-------------|-------------|------------|
| [linkedin-skills](https://github.com/sergebulaev/linkedin-skills) | `main` (Sep 2026) | ⚠️ Medium | ⚠️ Partial concern | [Full report →](tools/linkedin-skills/README.md) |

> **Add a tool:** Open an issue using the [tool request template](.github/ISSUE_TEMPLATE/tool-request.md) or submit a PR using the [assessment template](tools/_template/README.md).

---

## Free AI Tools from omopen.ai

Use these free tools directly — no sign-up required.

| Tool | What it does | Link |
|------|-------------|------|
| 🏗️ **Schema Generator** | Generate JSON-LD structured data (Article, FAQPage, HowTo, Organization) from any URL or text | [Try free →](https://omopen.ai/tools) |
| ❓ **FAQ Extractor** | Extract Q&A pairs from any content and produce ready-to-paste FAQPage schema | [Try free →](https://omopen.ai/tools) |
| 🎯 **AEO Tester** | Test whether your brand appears in AI engine responses (ChatGPT, Perplexity, Claude) | [Try free →](https://omopen.ai/tools) |
| 🤖 **AI API Proxy** | Route prompts across OpenAI, Anthropic, and Gemini via a single unified endpoint | [Sign up →](https://omopen.ai/signup) |
| 🎙️ **Voice Sandbox** | Build and test voice agent scripts with real-time speech synthesis | [Dashboard →](https://omopen.ai/dashboard/voice-agent) |
| ⚙️ **Workflow Playground** | Visual scenario builder — connect AI actions, conditions, and triggers without code | [Dashboard →](https://omopen.ai/dashboard/scenarios) |

The first three tools (Schema Generator, FAQ Extractor, AEO Tester) require no account. The rest are available on the free tier after sign-up.

---

## Who This Is For

- **Developers** evaluating LinkedIn automation tools before adding them to a workflow
- **Founders and marketers** who want to understand what data leaves their machine
- **Security teams** reviewing third-party tool risk for enterprise use

---

## Methodology

Each assessment follows the same framework:

1. **Static inspection** — README, source files, `.env.example`, `requirements.txt`, `SECURITY.md`, CI config
2. **Dependency audit** — All packages listed; known CVEs noted
3. **Data flow mapping** — Which third-party services receive data and under what conditions
4. **Credential handling** — How API keys are stored, read, and transmitted
5. **Platform compliance** — LinkedIn/third-party ToS implications
6. **Risk rating** — Critical / High / Medium / Low / Info per finding

We do **not** install or execute the tools being assessed.

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to add an assessment.

---

## License

Content: [CC BY 4.0](LICENSE) — free to share and adapt with attribution.  
Code snippets: MIT.
