# YaraWeave

> Threat Intel → YARA Rule Generator

A single-file browser tool that queries open-source threat intelligence feeds and uses an LLM to generate production-quality YARA detection rules — with a condition-by-condition explanation for SOC analysts.

No server. No install. Open the HTML file and go.

---

## What it does

1. **Input** — Enter a SHA256 hash or a malware family name (e.g. `Emotet`, `QakBot`)
2. **Query** — Hits up to 5 threat intel sources in parallel
3. **Generate** — LLM synthesises a YARA rule from the aggregated intel
4. **Explain** — A second LLM pass produces deployment-ready analysis: threat context, rule logic rationale, string annotations, where to deploy, and confidence rating

---

## Threat Intel Sources

| Source | Type | API Key |
|---|---|---|
| MalwareBazaar (abuse.ch) | Malware samples DB | None |
| URLhaus (abuse.ch) | Malware distribution URLs | None |
| ThreatFox (abuse.ch) | IOC database | None |
| VirusTotal | Multi-engine AV + metadata | Free (optional) |
| OTX AlienVault | Open Threat Exchange pulses | Free (optional) |

MalwareBazaar, URLhaus, and ThreatFox are completely free with no key required. VirusTotal and OTX both offer free API keys that unlock richer metadata.

---

## Getting Started

### 1. Get an API key

**Groq (recommended — free, fast, reliable)**
Go to [console.groq.com](https://console.groq.com), sign in, and create a free API key. No credit card required.

**Gemini (alternative)**
Go to [aistudio.google.com](https://aistudio.google.com), sign in, and create a free API key.

### 2. Open the tool

```bash
# Just open in any browser — no server needed
open yaraweave.html
# or
xdg-open yaraweave.html
```

### 3. Configure

Click **⚙ Configure** in the top-right, select your provider, and paste your API key. Keys are stored in `localStorage` — never hardcoded, never sent anywhere except directly to the respective APIs.

Optionally add VirusTotal and OTX keys for richer intel context.

### 4. Run your first query

Try **malware family** mode with `Emotet` — works with just the free sources, no keys needed. Click **Weave YARA Rule**.

---

## Output

Each analysis produces three panels:

**Threat Intelligence** — source hit/miss status, family name, file type, detection rate, first-seen date, OTX pulse count, tags

**Generated YARA Rule** — syntax-highlighted, copyable rule with:
- Descriptive `detect_` prefixed name
- Rich metadata block (description, date, hash, severity, TLP)
- Meaningful string patterns derived from known malware behaviors
- Weighted condition (not just `all of them`)

**Rule Explanation** — structured breakdown across five sections:
- `THREAT_CONTEXT` — what this malware does operationally
- `RULE_LOGIC` — how the condition reduces false positives
- `STRING_RATIONALE` — per-pattern annotation
- `DEPLOYMENT_NOTES` — where to deploy (EDR, network, sandbox) and evasion risks
- `CONFIDENCE` — LOW / MEDIUM / HIGH with justification

---

## API Keys

| Key | Where to get | Cost |
|---|---|---|
| Groq | [console.groq.com](https://console.groq.com) | Free, no credit card |
| Gemini | [aistudio.google.com](https://aistudio.google.com) | Free |
| VirusTotal | [virustotal.com](https://www.virustotal.com) | Free (4 req/min, 500/day) |
| OTX AlienVault | [otx.alienvault.com](https://otx.alienvault.com) | Free, no limits |

All keys are stored in your browser's `localStorage` only. They are passed directly from your browser to each API — this tool has no backend.

---

## LLM Providers

Select in **⚙ Configure → Provider**:

**Groq** (default)
- Model: `llama-3.3-70b-versatile` (default)
- Fast inference, free tier, no credit card required

**Gemini**
- `gemini-1.5-flash` (fast, free)
- `gemini-1.5-pro` (more capable, free tier available)
- `gemini-2.0-flash-exp` (latest, free tier)

---

## Limitations

- YARA rules are AI-generated and should be reviewed before production deployment
- Free VirusTotal API is rate-limited (4 lookups/minute)
- CORS: MalwareBazaar, URLhaus, and ThreatFox support browser-direct requests. VirusTotal and OTX may require a CORS proxy in some browser environments
- SHA256 queries work best when the hash is known to at least one of the selected sources

---

## Part of the Vibecoding Series

This is Project #17 in a series of tools built to translate security practitioner experience into demonstrable artifacts. The philosophy: **do, create, ship** — rather than pontificate about AI in security.

Other projects in the series: [mrdee.in](https://mrdee.in)
