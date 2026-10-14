# Web Preflight

A lightweight, non-destructive website release-check skill: a pre-launch check for a site you own, run before you deploy. It helps identify accidental exposure of secrets or sensitive files (`.env`, `.git`, config), unsafe deployment surfaces, weak security controls, misleading SPA catch-all responses, and common scanner noise.

## When to use it

Use Web Preflight when shipping, deploying, publishing, or reviewing a website or API—for example, when someone asks for a preflight, a production URL check, or a security check before launch. It can also help interpret access-log or analytics evidence.

It is not a penetration-testing or application-code-review workflow. It does not exploit vulnerabilities, brute-force paths, submit forms, authenticate, or perform broad port scans.

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

Place `SKILL.md` in your agent's skills directory, for example:

```bash
mkdir -p ~/.qoder/skills/web-preflight
cp SKILL.md ~/.qoder/skills/web-preflight/SKILL.md
```

## License

MIT. See [`LICENSE`](LICENSE).
