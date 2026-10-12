# Test Case 09: Artifact Requires an Obsolete Application

**Status:** Exploratory fixture for breaking DAC Core  
**Normative:** No. This case does not define the final DAC schema.  
**Fixture identifier:** `obsolete-application`

## Scenario

A document or project file survives, but its original application is obsolete, unavailable, or no longer supported. The artifact may be readable by a modern tool, but that tool may not reproduce the original layout, behavior, calculations, editability, embedded objects, or interactive features.

The original application might be preserved separately, available only on old media, dependent on an obsolete operating system, or legally unavailable for redistribution. The case does not assume that the original application is lost forever or that a modern substitute is equivalent.

The initial case is specification-level. No particular application, file, dependency, or compatibility result is asserted until actual artifacts and tests are supplied.

## What this case tests

1. **Artifact versus application:** Can DAC identify the document/project separately from the application required to interpret it?
2. **Environment reconstruction:** Can DAC record the operating system, runtime, fonts, plugins, libraries, or hardware that may be required?
3. **Original versus substitute:** Can it distinguish the original application from a replacement viewer, converter, compatibility layer, or reimplementation?
4. **Behavior and fidelity:** Can it describe which features were tested and which differences remain unknown?
5. **Migration:** Can a migrated or converted file be retained as a derivative without replacing the original?
6. **Application rights:** Can DAC record that preserving or redistributing the application may have different rights from preserving the document?
7. **Partial access:** Can an artifact be preserved even when it cannot currently be opened or faithfully rendered?

## Format-level expectations

“Opens successfully” is not a sufficient fidelity test. Depending on the artifact, important behavior may include:
- Page layout, fonts, line breaks, and embedded media.
- Formulas, macros, scripting, calculations, and external data links.
- Layers, animation, transitions, timelines, or interactive controls.
- Color management, embedded profiles, and output-device assumptions.
- Plugins, codecs, proprietary libraries, templates, and application-specific extensions.
- Editability and structural semantics, not just a flattened visual rendering.
- Licensing, activation, serial numbers, or other restrictions affecting the original application.

These are candidate concerns, not universal requirements. Determine the features that matter for the actual artifact and identify the evidence for each dependency.

A modern converter may preserve some visible output while discarding hidden structure or interactive behavior. A screenshot or PDF may document appearance but is not a substitute for the editable original.

## Reference-application extraction requirements

The reference application should:
- Inventory and hash the original artifact and any available original application installers, packages, plugins, templates, and related dependencies.
- Extract supported format identifiers, application/version identifiers, embedded metadata, document structure, external references, and dependency declarations.
- Record which facts are embedded in the artifact and which are inferred from file signatures, documentation, or depositor statements.
- Identify external resources referenced by the document where possible, without claiming they are present merely because a reference exists.
- Preserve original files and metadata non-destructively.
- Represent a converted file as a derivative with its own identity, hash, creation tool/version, and known conversion limitations.
- Record parser/tool identity, version, extraction scope, errors, unsupported fields, and unexamined areas.
- Avoid executing macros, scripts, or unknown content during static inspection. Any dynamic testing should be separate, explicit, and appropriately isolated.

Distinguish source metadata, DAC observations, and depositor assertions. Preserve unknown fields where feasible; otherwise report extraction limits. Distinguish absent, empty, unsupported, failed, and not examined states.

## Three-level implementation plan

### Level 1 — Identify and inventory
- Inventory and hash the original artifact and all supplied environment components.
- Identify file formats and known package structures where possible.
- Preserve original bytes without conversion or modification.
- Record missing application installers, plugins, fonts, or other known components as missing rather than silently omitting them.

### Level 2 — Extract and describe
- Use mature format-specific parsers to extract all relevant supported metadata and structure.
- Record declared application/version requirements and external references with evidence sources.
- Record the original application, substitute tools, and converted derivatives as separate entities.
- Preserve conversion provenance, including tool/version, settings, warnings, and known loss of features.
- Describe unavailable dependencies, licensing/access barriers, and unknown requirements without guessing.

### Level 3 — Validate the usable experience
- Test with the original application if an authorized, appropriate environment is available.
- Test substitute tools separately and record the exact version and operations performed.
- Define artifact-specific checks such as rendering, editability, formulas, embedded media, or interactive behavior.
- Compare results against available reference output or behavior; record differences and uncertainty.
- Do not claim full fidelity because one page renders or one file opens.
- If the original application cannot be tested, state that clearly. A substitute may still provide useful access without proving equivalence.

## Four preservation targets

These targets describe preservation outcomes, not implementation stages. Evaluate them independently; do not collapse them into a single `preserved` Boolean.

| Target | Question | Evidence to record |
|---|---|---|
| **1. Original artifact** | Are the original bytes retained and independently verifiable? | Payload inventory, byte-level digests, and known gaps. |
| **2. Execution or use environment** | Is the environment needed to access the artifact available or documented? | Original application, OS/runtime, plugins, fonts, libraries, hardware, versions, access restrictions, and missing components. |
| **3. Behavior** | Have intended functions been tested, and what happened? | Rendering, editing, calculation, interaction, or other case-specific tests and results. |
| **4. Historical experience and fidelity** | How closely does the current experience match the historical one? | Reference outputs, visual/structural/functional comparisons, known differences, and uncertainty. |

A target may be unknown, not tested, partially achieved, blocked, or supported by evidence. These are candidate status distinctions, not a finalized DAC schema.

## Suggested local container layout

```text
obsolete-application.dac/
├── dac.json
├── payload/
│   ├── original-project-file
│   └── original-application-installer
└── access/
    ├── environment-notes.md
    ├── conversion-history.md
    └── test-results.json
```

Illustrative only. Include the application installer or other components only when available and lawful to retain. An application may be represented as a separate DAC asset and linked as a dependency rather than embedded in the same package.

## Acceptance criteria

- The original artifact is preserved and hashed independently of any derivative.
- Original application requirements and missing dependencies are identified when evidence permits.
- Original application, replacement tools, and converted derivatives are not conflated.
- Tests specify which features were exercised and which were not.
- Conversion losses and fidelity uncertainty are recorded rather than hidden.
- Legal ability to retain or redistribute the application is not inferred from technical availability.
- The archive remains useful when no current tool can open the original artifact.

## Questions this case leaves open

- How should DAC model a required application as a separate asset and dependency?
- What evidence is needed to compare original and replacement behavior?
- How should application installers, patches, license files, and hardware requirements be related?
- Should a migration profile capture feature-by-feature loss, or is a human-readable report sufficient for core?
- How should the archive represent an application that is preserved but cannot legally be redistributed with the document?

Do not finalize the schema from one case alone. Compare it against the remaining hard cases first.
