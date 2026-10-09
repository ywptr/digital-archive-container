# Test Case 01: AI-generated presentation and PDF derivative

**Status:** Exploratory fixture for breaking DAC Core  
**Normative:** No. This case does not define the final DAC schema.

## Scenario

A depositor has both:
- the original PPTX presentation generated with ChatGPT; and
- a PDF exported from that presentation.

The two files are related but are not interchangeable. The PPTX may retain editability and presentation-specific features. The PDF may be easier to view consistently, but an export can omit or alter features such as animations, transitions, speaker notes, embedded media, or editability. Which features were actually lost must be assessed from the files, not assumed.

## What this case tests

1. **Multiple related artifacts:** Can one archive preserve the source presentation and its PDF derivative?
2. **Lineage:** Can metadata represent that the PDF was exported from the PPTX?
3. **Provenance assertions:** Can DAC record the depositor's account of how the files were created without presenting it as independently verified evidence?
4. **Independent integrity:** Can each payload receive its own checksum, verified against the bytes actually archived?
5. **Rights uncertainty:** Can rights status remain unknown without inventing ownership, a license, or permission?
6. **Experience requirements:** Can the archive distinguish viewing the PDF from opening and editing the presentation?
7. **Preservation limitations:** Can known, observed, and merely possible losses be distinguished?

## Reported history

The depositor reports that ChatGPT generated the presentation and that the PDF was exported from the PPTX. Unless supporting evidence is separately captured, these are depositor assertions—not cryptographic proof of origin or derivation.

The use of an AI system does not by itself settle copyright status, ownership, or contractual permissions. Those questions should be recorded separately and assessed in the relevant jurisdiction and under any applicable service terms.

## Suggested local container layout

```text
ai-generated-presentation.dac/
├── dac.json
└── payload/
    ├── source/
    │   └── presentation.pptx
    └── derivatives/
        └── presentation.pdf
```

These names are illustrative. The actual PPTX and PDF are not included in this repository fixture. Use local copies for testing unless publication of the actual files is intentional and appropriate.

## Acceptance criteria

- Both artifacts can be present in the same archive and identified independently.
- A relationship explicitly states that the PDF is a derivative of the PPTX.
- Each artifact can have its own format, role, and integrity record.
- Missing hashes, title, dates, evidence, or rights conclusions are represented as missing or unknown—not fabricated.
- The provenance record distinguishes reported claims from verified observations.
- A consumer can identify the likely viewing and editing paths without assuming that every application or feature will remain available.
- The metadata can describe a limitation without claiming that a possible loss occurred.

## Questions this case leaves open

- Should lineage relationships live in the core asset model or in a separate provenance event model?
- How should DAC represent a transformation event, its agent, and the evidence for that event?
- Which metadata is required for a minimally useful archive, and which should remain optional?
- Should hashes be stored directly in the manifest, in a checksum file, or both?
- How should DAC distinguish a rights holder, a rights claim, a license, contractual terms, and an unresolved assessment?

Do not finalize the schema from this case alone. Compare it against the remaining hard cases first.
