# Test Case NN: [Artifact or scenario]

**Status:** Exploratory fixture for breaking DAC Core  
**Normative:** No. This case does not define the final DAC schema.

## Scenario

Describe the artifact and preservation problem. State whether this is specification-level analysis or analysis of actual files. Do not imply any metadata, feature, provenance, or rights status has been observed unless it has.

## What this case tests

1. **Payload preservation:** Can DAC preserve the original bytes unchanged?
2. **Identification and integrity:** Can the format be identified and an independent digest computed from the actual bytes?
3. **Metadata extraction:** Can the reference application extract supported metadata comprehensively within declared support boundaries?
4. **Evidence and provenance:** Can observations, source metadata, and depositor assertions be distinguished?
5. **Rights uncertainty:** Can unknown rights and permissions remain unresolved rather than fabricated?
6. **Experience requirements:** Can relevant ways of viewing, editing, playing, or executing the artifact be described?
7. **Preservation limitations:** Can tested, observed, possible, and unknown limitations be distinguished?

## Format-level expectations

Describe the relevant format and metadata specifications. Separate specification-level capabilities from observations about a particular file. Include authoritative references where available.

## Reference-application extraction requirements

Describe metadata and structural information the reference application should extract. Explain how it should preserve source locations/identifiers, unknown fields, and extraction status. Distinguish:
- **Source metadata:** values read from the artifact.
- **DAC observations:** facts measured or derived by the application, including hashes and parser outcomes.
- **Depositor assertions:** claims supplied by the depositor.

At minimum, distinguish applicable states such as present and extracted, absent/empty, unsupported, failed, and not examined. Do not silently treat metadata as verified truth. Extraction must not modify the original payload.

## Three-level implementation plan

Use the same staged implementation model across DAC test cases. These levels are an implementation roadmap, not a promise that every format has the same complexity.

### Level 1 — Identify and inventory
- Detect the format and report ambiguity or confidence where applicable.
- Record file size and compute a cryptographic digest from the actual bytes.
- Perform basic structural or stream checks where practical.
- Record tool/version and errors.
- Preserve the original payload unchanged.

### Level 2 — Extract and describe
- Use mature format-specific libraries to extract supported metadata and structure.
- Extract all relevant supported fields rather than a small hard-coded subset.
- Preserve source locations/identifiers and original values where practical.
- Distinguish source metadata, DAC observations, and depositor assertions.
- Distinguish absent, empty, unsupported, failed, and unexamined states.
- Preserve unknown metadata where feasible or report what cannot be preserved.

### Level 3 — Validate the usable experience
- Test relevant viewing, editing, playback, or execution paths where feasible.
- Record the application/tool and version, test performed, result, dependencies, and warnings.
- Inspect relevant format-specific features and possible losses.
- Clearly report untested behavior.
- Do not claim faithful rendering, playback, execution, feature preservation, or future compatibility without evidence.

Level 1 and Level 2 should provide useful preservation value without requiring Level 3. Define format-specific Level 3 checks according to the artifact's intended experience. An untested artifact can still be archived if its validation status is explicit.

## Suggested local container layout

```text
example.dac/
├── dac.json
└── payload/
    └── [original-file]
```

## Acceptance criteria

- [Define format-specific, testable criteria.]
- Missing metadata and unresolved claims are represented as missing or unknown, not fabricated.
- Hashes are computed from actual bytes, never guessed.
- Original payload bytes are not modified by inspection.
- Extraction and validation scope, tool versions, errors, and limitations are reported.
- The archive does not require optional descriptive metadata merely to preserve the payload.

## Questions this case leaves open

- [Record schema questions raised by this case.]

Do not finalize the schema from one case alone. Compare it against the remaining hard cases first.
