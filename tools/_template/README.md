# Assessment Template

> Copy this file to `tools/<tool-name>/README.md` and fill in each section.  
> Remove all `<!-- ... -->` comments before submitting a PR.

---

# Security & Usability Assessment: \<tool-name\>

> **Repository:** https://github.com/author/repo  
> **Assessed:** \<Month Year\>  
> **Method:** Static inspection — no installation  
> **Assessor:** \<your name or org\>

---

## Summary

<!-- 2-4 sentences: what the tool does, its intended audience, and your overall verdict -->

**Overall risk: \<Critical / High / Medium / Low\>**

---

## Risk Summary

| # | Finding | Severity | Category |
|---|---------|----------|----------|
| 1 | <!-- finding title --> | ⚠️ High | <!-- category --> |

<!-- Severity legend:
🔴 Critical — active exploit or credential theft possible
⚠️ High    — ToS violation, data exfiltration, or code execution
⚠️ Medium  — meaningful privacy or safety concern
ℹ️ Low     — minor concern or dual-use behaviour
✅ Pass    — good practice worth noting
-->

---

## Detailed Findings

### Finding 1 — \<Title\> \<Severity emoji\>

**What it does:**  
<!-- Describe the behaviour -->

**Risk:**  
<!-- Explain the impact -->

**Mitigation:**  
<!-- What the user can do to reduce risk -->

---

<!-- Repeat for each finding -->

---

## Safe Usage Guide

```bash
# Minimal-risk .env configuration
```

### What to verify before enterprise use

- [ ] Item 1
- [ ] Item 2

---

## References

- [Tool GitHub](https://github.com/author/repo)
- [Relevant ToS / policy links]

---

*Assessment by [omopen.ai](https://omopen.ai) · CC BY 4.0 · Not affiliated with the assessed project*
