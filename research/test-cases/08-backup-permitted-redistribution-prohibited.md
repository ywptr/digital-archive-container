# Test Case 08: Backup Permitted, Redistribution Prohibited

**Status:** Exploratory fixture for breaking DAC Core  
**Normative:** No. This case does not define the final DAC schema.  
**Fixture identifier:** `backup-not-redistribution`

## Scenario

A person lawfully possesses a software copy and the applicable license or other terms permit making a backup copy, but prohibit redistribution. The person wants to preserve the software in a DAC container.

The scenario assumes the stated terms are supported by the available license text or other evidence. It does not assume that every jurisdiction treats backup, format migration, emulation, cloud storage, sharing with family, or archival deposit the same way. If the applicable legal position is uncertain, record that uncertainty rather than offering a definitive legal conclusion.

The archive may be technically complete and usable while still not being lawful to publish or share. A private backup and a publicly downloadable DAC package are materially different actions.

## What this case tests

1. **Action-specific rights:** Can DAC record permissions and restrictions separately for backup, preservation, migration, execution, modification, and redistribution?
2. **Evidence and interpretation:** Can the archive preserve the relevant license text and distinguish its literal terms from an archivist's interpretation?
3. **Container versus payload:** Can DAC distinguish permission to make a backup from permission to redistribute either the original files or the resulting DAC package?
4. **Rights uncertainty:** Can the archive represent ambiguous, conflicting, expired, or jurisdiction-dependent terms without guessing?
5. **Technical versus legal status:** Can the artifact remain intact and usable while sharing remains restricted?
6. **Secondary material:** Can the archive account for bundled manuals, fonts, libraries, artwork, or other components that may have different rights?
7. **Access controls:** Can the case be documented without treating technical ability to copy or share as legal permission?

## Format-level expectations

The payload may be an installer, application directory, disk image, or other software package. The technical format is secondary to the rights problem.

Potentially relevant evidence includes:
- License agreement or terms version and the point in the acquisition process at which they were presented.
- Purchase receipt, entitlement record, or other acquisition evidence, if available and appropriate to retain.
- Publisher, product version, acquisition date, and jurisdictional context, where known.
- Exact wording concerning backups, archival copies, modification, migration, execution, transfer, and redistribution.
- Any separate terms governing bundled components, account-bound access, cloud storage, or third-party services.

A receipt may support a claim of acquisition; it does not independently establish every legal permission. A product page or a general statement that a product is “owned” is not a substitute for the applicable terms.

## Reference-application extraction requirements

The reference application should:
- Inventory and hash the actual software files without modifying them.
- Extract supported product identifiers, version information, embedded license files, package metadata, and other relevant machine-readable information.
- Preserve relevant license text or reference the exact source and version when retention is not possible or appropriate.
- Keep the original license wording distinct from human-entered summaries and automated interpretations.
- Record the source, date, scope, and author of any rights assertion.
- Represent separate actions independently where evidence permits, instead of using one global field such as `allowed: true`.
- Avoid storing credentials, payment details, or unnecessary personal information in ordinary metadata.
- Make uncertainty and missing terms visible; do not infer rights from possession, successful execution, or a hash match.

## Three-level implementation plan

### Level 1 — Identify and inventory
- Inventory and hash the retained software and available rights evidence.
- Record product/version identifiers and original filenames where known.
- Keep the original payload unchanged.
- Distinguish evidence actually present in the archive from claims supplied by the depositor.

### Level 2 — Extract and describe
- Extract supported software metadata and embedded license text.
- Record license/terms identity, version, source, and relevant dates where available.
- Preserve exact source wording separately from a summary or interpretation.
- Describe permissions and restrictions as action-specific claims with evidence and uncertainty.
- Keep the license applicable to the software distinct from terms that may apply to the DAC package, hosting service, or included third-party components.

### Level 3 — Validate the usable experience
- Test opening, installation, or execution only where appropriate and authorized.
- Record the environment and specific behaviors tested.
- Do not treat successful execution as proof of backup or redistribution permission.
- Do not publish or transmit a restricted payload merely to test whether the container can be shared.
- If evaluating a sharing workflow, use non-restricted fixture data or a suitably authorized copy.

## Four preservation targets

These targets describe preservation outcomes, not implementation stages. Evaluate them independently; do not collapse them into a single `preserved` Boolean.

| Target | Question | Evidence to record |
|---|---|---|
| **1. Original artifact** | Are the original bytes retained and independently verifiable? | Actual payload inventory, byte-level digests, and any known gaps. |
| **2. Execution or use environment** | Is the environment needed to access the artifact available or documented? | Applications, operating systems, dependencies, versions, access constraints, and unavailable components. |
| **3. Behavior** | Have intended functions been tested, and what happened? | Specific authorized tests, environment, results, failures, and untested functions. |
| **4. Historical experience and fidelity** | How closely does the accessible experience match the historically intended or observed experience? | Relevant comparisons, known differences, uncertainty, and explicit assessment limits. |

**Case-specific interpretation:** The rights assessment is an additional axis, not a fifth preservation target. A technically complete archive may still have restricted redistribution. Record the permissions and restrictions relevant to the intended action, including the evidence and uncertainty; do not infer permission from technical success.

A target may be unknown, not tested, partially achieved, blocked, or supported by evidence. These are candidate status distinctions, not a finalized DAC schema.

## Suggested local container layout

```text
backup-not-redistribution.dac/
├── dac.json
├── payload/
│   └── original-software-package/
└── rights/
    ├── license-text.txt
    └── rights-assessment.md
```

Illustrative only. Preserve and distribute license text and payload only as permitted. If the rights evidence itself cannot be redistributed, record a suitable description and source reference without implying that the evidence has been archived.

## Acceptance criteria

- The software and retained evidence are independently inventoried and hashed.
- Backup permission is not interpreted as redistribution permission.
- Rights are described by action and scope where the evidence supports that distinction.
- Original terms are distinguishable from summaries, interpretations, and depositor assertions.
- Unknown, conflicting, or jurisdiction-dependent legal questions remain unresolved rather than guessed.
- The technical integrity and usability of the artifact can be reported independently of the permission to share it.
- The case does not require DAC to provide legal advice or enforce a license.

## Questions this case leaves open

- How should DAC represent action-specific permissions without making the core a general-purpose legal rules engine?
- What minimal evidence should accompany a rights claim: source, exact text, version, date, jurisdiction, interpreter, or all of these when available?
- How should a container distinguish permission to create a private archive from permission to transfer it to an archive institution?
- How should different rights across components of one software package be represented?
- Should DAC offer machine-readable rights expressions as optional extensions while preserving human-readable terms?

Do not finalize the schema from one case alone. Compare it against the remaining hard cases first.
