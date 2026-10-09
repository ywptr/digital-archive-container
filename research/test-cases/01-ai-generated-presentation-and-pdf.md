# Test Case 01: AI-generated presentation and PDF derivative

**Status:** Exploratory fixture for breaking DAC Core  
**Normative:** No. This case does not define the final DAC schema.

## Scenario

A depositor has both:
- a PPTX presentation reportedly generated with ChatGPT; and
- a PDF reportedly exported from that presentation.

The two files are related but are not interchangeable. The PPTX may retain editability and presentation-specific features. The PDF may be easier to view consistently, but an export can omit or alter features such as animations, transitions, speaker notes, embedded media, or editability. Which features were actually lost must be assessed from the files, not assumed.

This test case is designed first from the format specifications and expected format behavior. It does not depend on having the depositor's actual files in the repository. No claim is made that any particular optional metadata field or presentation feature is present in the hypothetical files.

## What this case tests

1. **Multiple related artifacts:** Can one archive preserve the source presentation and its PDF derivative?
2. **Lineage:** Can metadata represent that the PDF was exported from the PPTX?
3. **Provenance assertions:** Can DAC record the depositor's account of how the files were created without presenting it as independently verified evidence?
4. **Independent integrity:** Can each payload receive its own checksum, verified against the bytes actually archived?
5. **Metadata extraction:** Can the reference application extract relevant metadata that is actually available in each supported format?
6. **Metadata provenance:** Can each extracted value be associated with its source and distinguished from a depositor claim or a DAC-generated observation?
7. **Rights uncertainty:** Can rights status remain unknown without inventing ownership, a license, or permission?
8. **Experience requirements:** Can the archive distinguish viewing the PDF from opening and editing the presentation?
9. **Preservation limitations:** Can known, observed, and merely possible losses be distinguished?

## Format-level expectations

These expectations are based on the formats' specifications and common package/document structures. They are not assertions about the contents of any particular file.

### PPTX

PPTX is an Office Open XML package. A reference implementation should, where supported:
- inspect package structure and relationships;
- extract available core properties and other supported package metadata;
- identify relevant presentation components and relationships, such as slides, layouts, masters, notes, and media;
- report package validation and extraction results within the implementation's capabilities.

Core properties such as title, creator, subject, keywords, and timestamps may be absent, blank, stale, or inaccurate. Their presence does not prove who created the content or which application generated it.

### PDF

PDF is a document format standardized by ISO 32000. A reference implementation should, where supported:
- extract available document information dictionary values and XMP metadata;
- report structural observations such as page count and page dimensions;
- identify supported features such as bookmarks, annotations, attachments, forms, tagged structure, encryption, and digital signatures;
- report extraction and inspection limits.

PDF metadata does not, by itself, establish which source document produced the PDF or whether the PDF faithfully preserves all source features.

### Specification references

- ECMA-376, Office Open XML: https://ecma-international.org/publications-and-standards/standards/ecma-376/
- ISO 32000, PDF: https://www.iso.org/search.html?q=ISO%2032000

## Reference-application metadata extraction requirements

The reference application should extract all relevant metadata it can reliably read from supported formats, rather than limiting extraction to a small fixed set such as title and author. Extraction should be comprehensive within declared support boundaries, reproducible where practical, and non-destructive.

Keep three categories distinct:

- **Source metadata:** values read from the file, including the source location or metadata mechanism when available (for example, a PPTX core-properties part, a PDF document information dictionary, or PDF XMP metadata).
- **DAC observations:** facts computed or observed by the reference application, such as a SHA-256 digest, detected media type, file size, parser result, and extractor version.
- **Depositor assertions:** statements supplied by the depositor, such as “ChatGPT generated this PPTX” or “this PDF was exported from that PPTX.”

Do not silently convert source metadata or depositor assertions into verified facts about authorship, origin, rights, or derivation.

Extraction results should distinguish at least these states where applicable:
- value present and extracted;
- field absent or empty;
- feature or field unsupported by the implementation;
- extraction failed;
- not examined.

Do not treat these states as interchangeable. Preserve original extracted values and representations where practical. If values are normalized for search or interoperability, retain enough information to distinguish the normalized value from the original. The archived payload must not be modified by extraction.

The core specification should define the common way to identify an observation, its source, and its status. It should not require every PPTX- or PDF-specific field to become part of DAC Core. Format-specific metadata should remain extensible.

**Boundary:** metadata extraction is not the same as full content reconstruction, rendering, semantic comparison, or verification that a PDF faithfully represents a PPTX. Those can be separate capabilities and should report their own scope and results.

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
- A relationship explicitly states that the PDF is a derivative of the PPTX, with the assertion's status recorded.
- Each artifact can have its own format, role, and integrity record. Hashes are computed from actual bytes; they must not be guessed or derived from the specification.
- Missing hashes, title, dates, evidence, or rights conclusions are represented as missing or unknown—not fabricated.
- The reference application extracts supported metadata from both formats, including available fields beyond a fixed minimal list.
- Each extracted value can be traced to its source; extracted values, DAC observations, and depositor assertions remain distinguishable.
- Missing, unsupported, failed, and unexamined extraction states are not conflated.
- Extraction does not alter the archived payload.
- The provenance record distinguishes reported claims from verified observations.
- A consumer can identify likely viewing and editing paths without assuming every application or feature will remain available.
- A limitation can be recorded without claiming that a possible loss actually occurred.
- A minimally useful archive does not require optional descriptive metadata such as title, creator, or timestamps.

## Questions this case leaves open

- Should lineage relationships live in the core asset model or in a separate provenance event model?
- How should DAC represent a transformation event, its agent, and the evidence for that event?
- What is the shared data model for extracted metadata, and how should format-specific values be preserved?
- How should the application record parser identity, version, configuration, and extraction errors?
- Which metadata is required for a minimally useful archive, and which should remain optional?
- Should hashes be stored directly in the manifest, in a checksum file, or both?
- How should DAC distinguish a rights holder, a rights claim, a license, contractual terms, and an unresolved assessment?

Do not finalize the schema from this case alone. Compare it against the remaining hard cases first.
