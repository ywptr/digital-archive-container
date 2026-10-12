# Test Case 03: DRM-free purchased PDF

**Status:** Exploratory fixture for breaking DAC Core  
**Normative:** No. This case does not define the final DAC schema.

## Scenario

A depositor purchased or otherwise lawfully obtained a PDF e-book, manual, report, or other publication that is not protected by technical DRM. The depositor wants to preserve a personal copy and enough context to understand its origin, access conditions, and permitted uses later.

The file may have sparse or contradictory metadata. The purchase record, product page, license, terms of sale, and the bytes themselves are separate evidence sources. “DRM-free” describes the absence of a detected technical access-control mechanism within the inspection scope; it does not mean public domain, unrestricted redistribution, or permission to make every kind of derivative.

This case is specification-level until a particular PDF and its supporting evidence are selected. Do not assume any purchase receipt, license text, metadata field, or rights statement is present.

## What this case tests

1. **Byte preservation:** Can DAC preserve the PDF unchanged and compute its digest?
2. **Metadata extraction:** Can the reference application extract supported PDF metadata and structure?
3. **Provenance:** Can the depositor's account of where the file came from be separated from independently captured evidence?
4. **Rights and restrictions:** Can DAC distinguish copyright status, license terms, purchase terms, and permission to preserve or redistribute?
5. **Access versus permission:** Can DAC record that no technical DRM was detected without inferring unrestricted legal rights?
6. **Evidence preservation:** Can receipts, license text, product descriptions, or terms snapshots be recorded as separate artifacts with their own provenance and hashes?
7. **Usability:** Can DAC describe whether the PDF can be opened and rendered with available software without making unsupported future-compatibility promises?

## Format-level expectations

PDF metadata and structural properties are covered by the PDF format and its specifications, including ISO 32000. The reference application should extract supported document information, XMP metadata, page structure, and relevant features within its declared capabilities. Metadata fields such as title, author, creator, producer, and timestamps may be missing or unreliable.

A PDF being readable by ordinary software is not a rights determination. Likewise, the lack of detected encryption or access restrictions does not establish that a file is in the public domain or can be redistributed.

Reference:
- ISO 32000, PDF: https://www.iso.org/search.html?q=ISO%2032000

## Evidence and rights model

Keep at least these concepts separate:

- **Acquisition assertion:** e.g. “I purchased this publication from vendor X on date Y.”
- **Acquisition evidence:** receipt, order confirmation, account export, or other retained record, if available.
- **Rights statement:** copyright notice, license, or publisher statement captured from a source.
- **Contractual terms:** terms applicable to the transaction or service, where available and relevant.
- **Technical access observation:** whether the inspected PDF appears encrypted or restricted by mechanisms the tool detects.
- **Depositor's intended use:** e.g. personal preservation. Intent does not itself establish legal permission.

Evidence should have its own artifact identity, provenance, and digest when archived. A screenshot or saved product page is evidence of what that capture contains, not necessarily proof that the terms were legally applicable to the depositor's transaction. Record source and capture context rather than silently upgrading evidence into a legal conclusion.

DAC should not force the depositor to supply a license or receipt before preserving bytes. It should allow rights assessment to remain unknown.

## Three-level implementation plan

### Level 1 — Identify and inventory
- Identify the PDF and report ambiguous or unsupported format detection.
- Record file size and compute a SHA-256 digest from the actual bytes.
- Run basic PDF structural checks where practical.
- Preserve the original PDF and any supplied evidence files unchanged.

### Level 2 — Extract and describe
- Extract supported PDF document information, XMP metadata, page properties, and structural features.
- Record encryption/restriction observations within the tool's capabilities; do not equate them with legal rights.
- Record provenance for any receipt, license, terms capture, or other supporting evidence.
- Keep file metadata, depositor assertions, technical observations, and rights assessments distinct.

### Level 3 — Validate the usable experience
- Where feasible, open and render the PDF using identified software and versions.
- Record rendering errors, missing resources, password requirements, and relevant dependencies.
- Test accessibility or text extraction only when in scope, and report exactly what was tested.
- Do not claim that rendering success proves legal access or that a non-encrypted PDF is legally unrestricted.

Level 3 is optional for byte preservation. An untested PDF can still be archived with its status recorded.

## Suggested local container layout

```text
purchased-drm-free-publication.dac/
├── dac.json
└── payload/
    ├── publication.pdf
    └── evidence/
        ├── purchase-receipt.pdf
        └── license-or-terms-capture.pdf
```

The evidence files shown are optional examples, not assumed available. Do not fabricate them or include personal/account information in a public fixture. Keep actual purchased content and receipts local unless publication is intentional, lawful, and appropriate.

## Four preservation targets

These targets describe preservation outcomes, not implementation stages. Evaluate them independently; do not collapse them into a single `preserved` Boolean.

| Target | Question | Evidence to record |
|---|---|---|
| **1. Original artifact** | Are the original bytes retained and independently verifiable? | Actual payload inventory, byte-level digests, source/derivative relationships, and any known gaps. |
| **2. Execution or use environment** | Is the environment needed to access the artifact available, documented, or reconstructable? | Relevant applications, decoders, operating systems, hardware, services, dependencies, versions, configuration, licensing/access constraints, and unavailable components. For non-executable artifacts, interpret this as the relevant viewing, reading, or playback environment. |
| **3. Behavior** | Have the artifact's intended functions been tested, and what happened? | Specific tests, environment, tool/version, results, warnings, dependencies, failures, and untested functions. A launch or open operation alone does not prove all intended behavior. |
| **4. Historical experience and fidelity** | How closely does the accessible experience match the historically intended or observed experience? | Relevant comparisons and evidence for rendering, timing, sound, interaction, editability, structure, or other case-specific properties; known differences; uncertainty; and explicit limits on the assessment. Do not claim fidelity without evidence. |

For this case, distinguish retained PDF bytes from available rendering software, successful rendering/text extraction, and the original reading experience. Technical readability or absence of detected DRM does not establish permission to redistribute.

A target may be **unknown**, **not tested**, **partially achieved**, **blocked**, or **supported by evidence**; choose terminology that accurately reflects the evidence. These are candidate status distinctions for evaluation, not a finalized DAC schema. A useful archive may preserve the original artifact even when the environment is unavailable, behavior is untested, or historical fidelity cannot be assessed.

## Acceptance criteria

- A PDF can be preserved even when descriptive metadata or purchase evidence is missing.
- The application extracts supported metadata and reports its source and extraction status.
- A checksum is computed from actual bytes, never guessed.
- Technical DRM/access-control observations are distinct from copyright and permission assessments.
- Purchase claims, source documents, licenses, and terms are represented as different kinds of information.
- Supporting evidence, when supplied, can be preserved as separate artifacts with provenance and integrity records.
- Unknown rights status does not block byte preservation.
- DAC does not infer redistribution permission from “DRM-free” or “purchased.”
- Inspection does not modify the original PDF.

## Questions this case leaves open

- How should DAC represent rights statements whose text is captured but whose applicability is uncertain?
- Should acquisition records and license/terms captures use the same generic evidence model?
- How should the manifest distinguish a depositor's description of a license from a captured copy of the license itself?
- How much acquisition detail is useful without collecting unnecessary personal information?
- What does DAC mean by “DRM-free” when different tools detect different restrictions?

Do not finalize the schema from this case alone. Compare it against the remaining hard cases first.
