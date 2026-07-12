# Security Policy

## Reporting a vulnerability

Do **not** report exploitable vulnerabilities, exposed secrets, or private data in a public issue, discussion, pull request, or commit.

Use GitHub's private vulnerability reporting flow when the repository shows a **Report a vulnerability** button under the **Security** tab. This is the preferred channel because it keeps the report private and preserves a clear audit trail.

When private vulnerability reporting is unavailable, contact the repository owner privately through the contact options on the GitHub profile. Do not include sensitive exploit details in a public message asking for contact.

## What to include

A useful report contains:

- the affected file, workflow, example, or repository feature;
- the vulnerability class or security concern;
- realistic impact and affected users;
- clear reproduction steps or a minimal proof of concept;
- required preconditions;
- whether exploitation is currently public or active;
- a proposed mitigation, when known;
- any secrets or personal data that may already have been exposed.

Use sanitized examples wherever possible. Never include unrelated customer data, credentials, or private project material.

## Scope

Security reports may cover:

- malicious or unsafe instructions in a published skill or recipe;
- examples that expose credentials, tokens, private URLs, or personal data;
- repository configuration that allows unauthorized changes;
- dependency or workflow issues that affect this repository;
- links or automation that could lead users to execute untrusted code;
- advice that creates a realistic authentication, authorization, privacy, or data-loss risk;
- accidental publication of proprietary or confidential material.

The following are generally not vulnerabilities in this repository:

- generic claims that AI-generated code may contain bugs;
- vulnerabilities in Lovable or third-party services that are unrelated to this repository;
- missing production hardening in an intentionally simplified educational example when the limitation is clearly documented;
- purely theoretical concerns without a plausible impact path;
- social engineering, spam, or denial-of-service testing against maintainers or contributors.

Report vulnerabilities in Lovable itself directly to Lovable through its official security channel. Do not use this repository as an intermediary.

## Handling process

Maintainers will make a reasonable effort to:

1. acknowledge a valid private report;
2. assess severity, scope, and reproducibility;
3. remove exposed secrets or dangerous content quickly when necessary;
4. prepare a correction or mitigation;
5. coordinate disclosure when public notice is useful;
6. credit the reporter when requested and appropriate.

No fixed response or remediation deadline is guaranteed. This is a community-maintained repository, not a staffed security program.

## Safe research expectations

Do not:

- access data that is not yours;
- persist, download, alter, or delete third-party data;
- degrade GitHub, Lovable, or contributor systems;
- use automated scanning that creates excessive traffic;
- publicly disclose a vulnerability before maintainers have had a reasonable opportunity to assess it;
- retain exposed secrets after confirming the issue.

Stop testing once you have enough evidence to demonstrate the problem safely.

## Secrets and sensitive data

Never add the following to examples, issues, pull requests, screenshots, or commit history:

- API keys, access tokens, session cookies, or passwords;
- Supabase service-role keys or database credentials;
- private repository, preview, or project links;
- customer records or personally identifiable information;
- payment details;
- internal prompts, documents, or proprietary source material without permission.

If a secret is committed, deleting it in a later commit is not sufficient. Revoke or rotate it immediately and then remove it from repository history where practical.

## Security disclaimer

Lovable Cookbook contains educational material and reusable workflows. It does not guarantee that generated applications are secure, compliant, or production-ready, and it does not replace threat modeling, code review, dependency review, penetration testing, or professional security assessment.