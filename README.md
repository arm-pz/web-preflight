# Web Preflight

A lightweight, non-destructive website release-check skill: a pre-launch check for a site you own, run before you deploy. It helps identify accidental exposure of secrets or sensitive files (`.env`, `.git`, config), unsafe deployment surfaces, weak security controls, misleading SPA catch-all responses, and common scanner noise.

## When to use it

Use Web Preflight when shipping, deploying, publishing, or reviewing a website or API—for example, when someone asks for a preflight, a production URL check, or a security check before launch. It can also help interpret access-log or analytics evidence.

It is not a penetration-testing or application-code-review workflow. It does not exploit vulnerabilities, brute-force paths, submit forms, authenticate, or perform broad port scans. For executed authorization and abuse testing, use its sibling [pre-ship-security-red-team](https://github.com/arm-pz/pre-ship-security-red-team): a Web Preflight `PASS` never means IDOR, quotas or input handling were tested.

## Safety and scope

Run checks only against a site the user owns or is authorized to test. The audit uses a fixed, compact path set, bounded GET/HEAD requests, and explicit per-host and per-audit request limits. It does not save raw sensitive response bodies or unsanitized headers.

The skill reads `robots.txt` and, by default, does not request paths it disallows. Testing those paths requires explicit authorization. A robots.txt rule is crawler guidance, not a security control.

## The problem it solves

Most "check my site before launch" prompts produce one of two wrong answers. A `200` on `/.env` is reported as a critical leak when the single-page app was simply serving its usual HTML shell. Or a firewall that answers every probe with the same `403` is reported as "secure", when nothing about the origin was ever observed. The skill's core rule is that a status code is a signal, not a finding, and that an unrun check is never a pass.

## Layout

- [`SKILL.md`](SKILL.md) — the complete procedure, verdicts, response classifications, and reporting requirements
- [`tests.md`](tests.md) — behavioral tests that check the agent's decisions and reporting, not file existence
- [`examples/sample-report-spa.md`](examples/sample-report-spa.md) — a fictional report showing the required output format

## Install

```
npx skills add arm-pz/web-preflight --skill web-preflight
```

Or place `SKILL.md` in your agent's skills directory, for example:

```bash
mkdir -p ~/.qoder/skills/web-preflight
cp SKILL.md ~/.qoder/skills/web-preflight/SKILL.md
```

## Related skills

This family covers pre-launch checks; each stays in its lane, and a pass in one is not a pass in another:

- [pre-ship-security-red-team](https://github.com/arm-pz/pre-ship-security-red-team) — active, authenticated testing of application logic: 14 authorization and abuse checks with captured evidence, then fixes and reruns.
- [pre-ship-performance-audit](https://github.com/arm-pz/pre-ship-performance-audit) — four read-only subagent speed audits with measured before/after Lighthouse medians.

Findings cross-reference by ID (`PREFLIGHT-01`, `REDTEAM-S02`, `PERF-03`) rather than merging verdicts; each skill defines the shared five-field record in its own `SKILL.md`.

## License

MIT. See [`LICENSE`](LICENSE).
