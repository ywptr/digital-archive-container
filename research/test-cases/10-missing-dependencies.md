# Test Case 10: Missing External Dependencies

**Status:** Exploratory fixture for breaking DAC Core  
**Normative:** No. This case does not define the final DAC schema.  
**Fixture identifier:** `missing-dependencies`

## Scenario

A project or artifact is preserved, but it refers to external resources or components that were not included in the archive. Examples may include fonts, images, audio, linked spreadsheets, libraries, plugins, datasets, templates, configuration files, or remote APIs.

The primary file may open successfully while the result is incomplete or different from the original. A dependency may be known by name or identifier but unavailable; it may be present but at an unknown version; or it may have been embedded into the primary file and no longer require an external copy.

The initial case is specification-level. No specific missing dependency or resulting damage is asserted until actual artifacts and tests are supplied.

## What this case tests

1. **Declared versus available:** Can DAC distinguish a dependency reference from the dependency bytes actually being present?
2. **Completeness:** Can it describe which parts of the artifact are available, missing, unresolved, or unexamined?
3. **Dependency relationships:** Can dependencies be linked to the artifact or to specific subcomponents rather than represented as an unqualified list?
4. **Impact:** Can DAC distinguish a missing dependency that blocks opening from one that only changes appearance or removes optional functionality?
5. **Versions and identity:** Can the archive identify dependency versions, hashes, and sources where known without inventing them?
6. **Reconstruction:** Can a future user determine what is needed to restore functionality, even when the missing component cannot be recovered?
7. **Rights and security:** Can DAC record license/access constraints and avoid unsafe automatic retrieval or execution?

## Format-level expectations

Dependencies may be:
- Embedded in the primary file.
- Stored as separate local files referenced by path or identifier.
- Resolved through a package manager or runtime environment.
- Retrieved from a remote URL, API, database, or service.
- Bound to a particular version, architecture, or platform.
- Optional, required for opening, required for some functions, or required for a particular historical rendering.
- Unavailable because of deletion, withdrawal, expired access, account restrictions, changed URLs, or an unknown original configuration.

A reference to a URL does not establish that the resource is still available or that it can be lawfully downloaded. A filename does not establish that a supplied file is the intended dependency. A currently available dependency with the same name may not be equivalent to the historical version.

## Reference-application extraction requirements

The reference application should:
- Inventory all supplied payload files and extract supported references to external files, packages, libraries, fonts, plugins, data, and services.
- Preserve the original reference string and its location/context in the source where feasible.
- Distinguish embedded resources, locally resolved dependencies, declared external dependencies, observed runtime dependencies, inferred dependencies, and unresolved references.
- Record dependency identity/version/hash/source only when supported by evidence.
- Avoid automatically fetching arbitrary URLs, installing packages, or executing scripts as part of ordinary metadata extraction.
- If optional dependency resolution is performed, make it an explicit operation with recorded tool/version, source, time, result, and security constraints.
- Preserve the original artifact and its dependency references unchanged.
- Record whether an external resource was present during extraction, absent, unsupported by the parser, failed to resolve, or not examined.
- Report dependency graph limitations and parser errors.

Keep source metadata, DAC observations, and depositor assertions separate. A dependency declaration is evidence that a reference exists; it is not proof that the referenced bytes were part of the original release or that a currently obtained replacement is identical.

## Three-level implementation plan

### Level 1 — Identify and inventory
- Inventory and hash the primary artifact and all supplied supporting files.
- Record archive paths and relationships where known.
- Identify apparent external references without attempting to resolve them automatically.
- Record obvious missing files only when the reference or expected structure gives a defensible basis for doing so.

### Level 2 — Extract and describe
- Parse supported dependency declarations and external references.
- Build an evidence-backed relationship between the artifact, its components, and dependencies.
- Record known version, source, identifier, hash, requirement scope, and availability state where evidence exists.
- Preserve unresolved references and missing-dependency information as first-class descriptive data.
- Distinguish declared, observed, inferred, and depositor-reported dependencies.
- Do not require every dependency to be bundled into one DAC container; references to separately archived DAC assets may be appropriate.

### Level 3 — Validate the usable experience
- Test opening and relevant functions in a recorded environment.
- Where feasible, test with the dependency missing and with an identified version supplied, so that the impact is not merely guessed.
- Record warnings, substitutions, missing-resource behavior, changed layout, failed operations, and functions not tested.
- Treat substitute fonts, libraries, or assets as substitutions, not as the original dependencies.
- Do not claim completeness from a successful open operation.
- Do not automatically execute or install untrusted dependencies. Any dynamic validation should be explicit and appropriately isolated.

## Four preservation targets

These targets describe preservation outcomes, not implementation stages. Evaluate them independently; do not collapse them into a single `preserved` Boolean.

| Target | Question | Evidence to record |
|---|---|---|
| **1. Original artifact** | Are the original bytes retained and independently verifiable? | Primary artifact and available dependency inventory, hashes, and known gaps. |
| **2. Execution or use environment** | Are the required tools and dependencies available or reconstructable? | Declared/observed dependencies, versions, locations, availability, and missing components. |
| **3. Behavior** | Have intended functions been tested with the available dependencies? | Tests, environment, missing-resource symptoms, substitutions, results, and untested functions. |
| **4. Historical experience and fidelity** | How closely does the result match the intended or historical output? | Reference outputs, known substitutions/losses, functional differences, and uncertainty. |

A target may be unknown, not tested, partially achieved, blocked, or supported by evidence. These are candidate status distinctions, not a finalized DAC schema.

## Suggested local container layout

```text
missing-dependencies.dac/
├── dac.json
├── payload/
│   ├── project-file
│   └── available-assets/
└── preservation/
    ├── dependency-inventory.json
    └── test-results.json
```

Illustrative only. Do not create placeholder files that could be mistaken for recovered originals. Missing components should be described as missing records, not represented by fabricated payloads.

## Acceptance criteria

- Primary artifact and available dependencies are independently inventoried and hashed.
- A dependency reference is not confused with the dependency itself.
- Missing, unresolved, unsupported, failed-to-resolve, and unexamined states are distinguishable.
- Dependencies are linked to evidence and, where possible, to affected functions or components.
- The impact of missing dependencies is tested where feasible or explicitly left unknown.
- Substitutes are identified separately from original dependencies.
- Automatic retrieval and execution of untrusted dependencies are not required.
- The archive can accurately describe a useful but incomplete artifact without claiming full reproducibility.

## Questions this case leaves open

- What is the minimum dependency relationship model DAC Core needs?
- Should dependency availability be time-stamped observations, a current status, or both?
- How should transitive dependencies and environment lockfiles be represented?
- How should one dependency's effect on only part of an artifact be captured?
- When is a dependency a separate DAC asset versus a descriptive external reference?
- How should DAC distinguish an incomplete original package from an incomplete archive assembled later?

Do not finalize the schema from one case alone. Compare it against the remaining hard cases first.
