# Security & Usability Assessment: linkedin-skills

> **Repository:** https://github.com/sergebulaev/linkedin-skills  
> **Assessed:** September 2026  
> **Method:** Static inspection — no installation  
> **Assessor:** omopen.ai

---

## Summary

`linkedin-skills` is a MIT-licensed collection of 12 AI-powered Claude Code / Codex skills for LinkedIn content creation and scheduling. It is well-structured with a draft-first approval workflow, environment-variable-based credential handling, and a responsible security disclosure policy.

The primary concerns are: reliance on a third-party scraping service (Apify) that likely violates LinkedIn's Terms of Service; content routing through multiple third-party cloud APIs; and a custom poster variable that can execute arbitrary Python modules.

**Overall risk: ⚠️ Medium** (safe for personal use with understood trade-offs; elevated caution for enterprise / regulated environments).

---

## Risk Summary

| # | Finding | Severity | Category |
|---|---------|----------|----------|
| 1 | Apify scraping violates LinkedIn ToS | ⚠️ High | Platform compliance |
| 2 | `LINKEDIN_SKILLS_CUSTOM_POSTER` allows arbitrary module execution | ⚠️ Medium | Code execution |
| 3 | Content routed through three third-party cloud services | ⚠️ Medium | Data privacy |
| 4 | No official LinkedIn API used — account ban risk | ⚠️ Medium | Platform compliance |
| 5 | Humanizer skill designed to evade AI detection | ℹ️ Low | Dual-use concern |
| 6 | No hardcoded credentials; env-file pattern used correctly | ✅ Pass | Credential handling |
| 7 | Draft-first workflow; nothing posts without approval | ✅ Pass | User safety |
| 8 | Minimal dependencies (requests, python-dotenv) | ✅ Pass | Supply chain |
| 9 | SECURITY.md present with clear responsible disclosure | ✅ Pass | Security posture |

---

## Detailed Findings

### Finding 1 — Apify scraping likely violates LinkedIn ToS ⚠️ High

**What it does:**  
The `linkedin-engager-analytics`, `linkedin-thread-monitor`, and `linkedin-hook-extractor` skills optionally use [Apify](https://apify.com) actors to retrieve LinkedIn post content, comment threads, and user profiles. Apify accomplishes this via automated browser sessions and LinkedIn's private (unofficial) API.

**Risk:**  
LinkedIn's [User Agreement §8.2](https://www.linkedin.com/legal/user-agreement) explicitly prohibits scraping, crawling, or automated data extraction without LinkedIn's written consent. Using Apify to pull LinkedIn data is therefore a ToS violation regardless of whether Apify itself complies with data export regulations.

**Practical consequences for users:**
- Account warning, temporary restriction, or permanent ban
- Elevated risk when operating at scale (agency use, multiple clients)
- GDPR concern: profiles of other users are extracted and processed without their knowledge

**Severity note:** This affects only the optional Apify integration. Skills that operate in draft-only mode (no Apify token configured) are unaffected.

**Safe alternative:**  
Use only manual copy-paste as input. Do not configure `APIFY_TOKEN`. All 12 skills work without Apify — data must be supplied by the user directly.

---

### Finding 2 — `LINKEDIN_SKILLS_CUSTOM_POSTER` executes arbitrary Python code ⚠️ Medium

**What it does:**  
The `.env.example` documents an advanced variable:
```
LINKEDIN_SKILLS_CUSTOM_POSTER=mymodule.poster:post_function
```
When set, the publishing pipeline imports and calls the specified Python module path at runtime.

**Risk:**  
If an attacker can write to your `.env` file (e.g., via a compromised package or shared environment), they can redirect this variable to execute arbitrary code with your user permissions. In multi-user or CI environments where `.env` is shared, this is an elevated risk.

**Mitigation:**
- Do not set `LINKEDIN_SKILLS_CUSTOM_POSTER` unless you authored the module yourself
- Treat it as equivalent to granting shell access — verify the module path before setting it
- For team/CI use: restrict `.env` file write access to privileged principals only

---

### Finding 3 — Content routed through up to three third-party cloud services ⚠️ Medium

**What it does:**  
When all optional integrations are configured, your LinkedIn content passes through:

| Service | What it receives | Data residency |
|---------|-----------------|----------------|
| **Apify** | LinkedIn post content, user comments, profile data | EU/US cloud |
| **Publora** | Finalized post text, publishing schedule, LinkedIn credentials (session) | Unknown |
| **Pixfaro** | Post topic/text for image generation | Unknown |

**Risk:**  
- Posts may contain proprietary, confidential, or personally identifiable information
- Publora receives your finalized post content and issues publishing requests to LinkedIn on your behalf — this is a significant trust boundary
- Privacy policies and data retention for Publora and Pixfaro were not independently verified at time of assessment

**Mitigation:**
- Configure only the integrations you need
- Review Publora's privacy policy before connecting your LinkedIn account
- Do not run skills over content containing client-confidential or regulated information (GDPR, HIPAA, CCPA)

---

### Finding 4 — No official LinkedIn API; account ban risk ⚠️ Medium

**What it does:**  
Publishing via Publora uses LinkedIn's private internal API (the same one the LinkedIn mobile app uses), not the [LinkedIn Marketing API](https://learn.microsoft.com/en-us/linkedin/marketing/) or [LinkedIn REST API](https://learn.microsoft.com/en-us/linkedin/shared/api-guide/overview). There is no OAuth2 authorization code flow — Publora holds a session token.

**Risk:**  
LinkedIn periodically detects and blocks access from unofficial API clients. Your account could be flagged for unusual activity. For high-volume use (>15 posts/month), the risk increases.

**Official API alternative:**  
LinkedIn's Community Management API (available to content creators and approved partners) provides legitimate programmatic posting. It requires application approval but carries no ban risk.

---

### Finding 5 — Humanizer skill evades AI detection tools ℹ️ Low

**What it does:**  
The `linkedin-humanizer` skill rewrites AI-generated drafts to pass common AI detection services (GPTZero, Originality.ai, etc.), removing characteristic patterns.

**Concern:**  
The tool functions as designed. However, if used to publish AI-generated content represented as original human writing in a professional context, it may conflict with:
- Platform community guidelines around AI disclosure
- Emerging regulatory expectations (EU AI Act Article 52 transparency obligations)
- Your own organization's AI disclosure policy

**This is not a vulnerability** — it is a deliberate feature. Users should apply their own ethical and legal judgment about disclosure.

---

### Finding 6 — Credential handling ✅ Pass

All API tokens (`PUBLORA_API_KEY`, `APIFY_TOKEN`, `PIXFARO_TOKEN`) are read from environment variables or gitignored `.env` files. The repository does not hardcode credentials anywhere in inspected source. The `SECURITY.md` explicitly names this as a design constraint.

---

### Finding 7 — Draft-first workflow ✅ Pass

All 12 skills follow a draft-then-approve pattern. The README explicitly states: *"All drafts require user approval before publishing — nothing posts automatically."* The publishing step is a separate, deliberate action.

---

### Finding 8 — Minimal dependency surface ✅ Pass

`requirements.txt` contains only:
```
requests>=2.31.0
python-dotenv>=1.2.3
```

Both packages are widely used, actively maintained, and have no known critical CVEs at time of assessment. The small dependency surface significantly reduces supply-chain risk compared to tools with dozens of transitive dependencies.

---

### Finding 9 — Responsible disclosure policy ✅ Pass

`SECURITY.md` documents:
- Report via GitHub Security Advisories or email to `s@bulaev.org`
- 72-hour acknowledgement SLA
- 14-day fix/disclosure decision SLA
- Explicit scope notes including the `LINKEDIN_SKILLS_CUSTOM_POSTER` risk

This is well above average for an open-source utility project.

---

## Safe Usage Guide

### Minimal-risk configuration (recommended)

```bash
# .env — no third-party integrations; draft-only mode
# PUBLORA_API_KEY=        # leave unset — no auto-publishing
# APIFY_TOKEN=            # leave unset — no LinkedIn scraping
# PIXFARO_TOKEN=          # leave unset — no image generation
# LINKEDIN_SKILLS_CUSTOM_POSTER=  # leave unset — no custom execution
```

With this configuration:
- All content is drafted locally and reviewed manually
- Nothing is sent to external services except your configured LLM (Claude, Codex)
- No LinkedIn scraping occurs; you supply post text manually
- No account ban risk from Apify or Publora

### What to verify before enterprise use

- [ ] Review Publora's privacy policy and data processing agreement
- [ ] Confirm your organization's AI disclosure policy before using the Humanizer
- [ ] Check if your LinkedIn account falls under any contractual restrictions on automation
- [ ] Do not use Apify integration in GDPR-regulated contexts without a DPIA

---

## Skill Inventory

| Skill | Purpose | External call? |
|-------|---------|---------------|
| `linkedin-post-writer` | Draft posts using 20 hook formulas | LLM only |
| `linkedin-comment-drafter` | Draft comments on posts | LLM only |
| `linkedin-reply-handler` | Manage threaded replies | LLM only |
| `linkedin-post-audit` | Check draft for AI patterns | LLM only |
| `linkedin-humanizer` | Remove AI writing tells | LLM only |
| `linkedin-hook-extractor` | Extract formulas from viral posts | Apify (optional) |
| `linkedin-content-planner` | Generate 7-day content calendar | LLM only |
| `linkedin-engager-analytics` | Track post engagement | Apify (optional) |
| `linkedin-profile-optimizer` | Rewrite profile sections | LLM only |
| `linkedin-employee-advocacy` | Plan team LinkedIn programs | LLM only |
| `linkedin-repurposer` | Convert content from other platforms | LLM only |
| `linkedin-interviewer` | Build a story bank | LLM only |

8 of 12 skills make no external calls beyond your LLM provider.

---

## Comparison with omopen.ai Free Tools

If you are looking for AI-assisted content and SEO/AEO tools that require no third-party account setup and carry no platform ToS risk, these omopen.ai tools may address some of the same needs:

| Need | linkedin-skills approach | omopen.ai alternative |
|------|-------------------------|----------------------|
| Structured data for posts | Manual schema writing | [Schema Generator](https://omopen.ai/tools) — free, no account |
| FAQ content for LinkedIn articles | Manual | [FAQ Extractor](https://omopen.ai/tools) — free, no account |
| Check if brand appears in AI answers | Not covered | [AEO Tester](https://omopen.ai/tools) — free, no account |
| Multi-model LLM routing | Via Claude/Codex | [AI API Proxy](https://omopen.ai/signup) — free tier |
| Voice content creation | Not covered | [Voice Sandbox](https://omopen.ai/dashboard/voice-agent) |
| Workflow automation | Skill chaining | [Workflow Playground](https://omopen.ai/dashboard/scenarios) |

---

## References

- [linkedin-skills GitHub](https://github.com/sergebulaev/linkedin-skills)
- [LinkedIn User Agreement §8.2 — Automated tools prohibition](https://www.linkedin.com/legal/user-agreement)
- [LinkedIn Marketing API — Official programmatic posting](https://learn.microsoft.com/en-us/linkedin/marketing/)
- [Apify Terms of Service](https://apify.com/terms-of-service)
- [Publora](https://app.publora.com)
- [EU AI Act Article 52 — Transparency obligations](https://artificialintelligenceact.eu/article/52/)

---

*Assessment by [omopen.ai](https://omopen.ai) · CC BY 4.0 · Not affiliated with LinkedIn, Microsoft, or sergebulaev*
