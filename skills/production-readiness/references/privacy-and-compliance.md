# Privacy and Compliance

Apply only where relevant given users, region, data, and exposure. Never claim legal sign-off.

## Consider when relevant
- Privacy policy, terms of service, imprint / provider information.
- Cookie / tracking consent where applicable.
- Handling of personal data: minimization, purpose, storage location.
- Data export and account/data deletion.
- Retention periods and deletion of derived data.
- Third-party subprocessors and cross-border data transfer.
- Email marketing: unsubscribe path, sender identification.
- Payment data: raw card numbers, CVCs, and equivalent sensitive authentication data must not be stored by the application unless it operates within an appropriately validated PCI environment. Provider tokens, transaction identifiers, subscription IDs, payment status, and permitted display metadata (e.g. brand, last four) may be stored when necessary and protected appropriately.
- AI processing of sensitive user input: provider data-use terms, opt-out, redaction.

## Framing
Use language such as:
- "Manual legal review required."
- "Compliance evidence not available."
- "Policy presence verified; legal adequacy not assessed."

Missing required legal disclosures, consent mechanisms, deletion rights, or other clearly applicable controls may block launch. Absence of documented external legal review is not by itself a blocker unless the project operates in a high-risk regulated domain (health, finance, children's data, etc.), processes specially protected data, or the user has identified legal approval as a release requirement.


## Do not
- Do not draft final legal texts.
- Do not certify GDPR/CCPA/PCI/HIPAA compliance.
- Do not assume defaults (e.g. "cookies are fine") without checking what the app actually sets.
