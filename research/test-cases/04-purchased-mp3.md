# Test Case 04: Purchased MP3 recording

**Status:** Exploratory fixture for breaking DAC Core  
**Normative:** No. This case does not define the final DAC schema.

## Scenario

A depositor purchased or otherwise lawfully obtained an MP3 recording from a store or artist. The file may contain ID3 tags, cover artwork, and technical stream information, but it may also have sparse or misleading metadata. The depositor wants to preserve the audio and the context of acquisition without conflating ownership of a copy with ownership of copyright or permission to redistribute it.

This is a specification-level case until a particular MP3 and any supporting acquisition evidence are selected. Do not assume a receipt, license, embedded copyright notice, or accurate artist field exists.

## What this case tests

1. **Audio preservation:** Can DAC preserve the original MP3 bytes and compute a digest?
2. **Metadata extraction:** Can the reference application extract supported ID3 frames and technical stream properties?
3. **Acquisition provenance:** Can the depositor's purchase claim be recorded separately from a receipt or captured store record?
4. **Rights distinctions:** Can DAC distinguish possession of a copy, copyright ownership, license scope, contractual terms, and redistribution permission?
5. **Tag reliability:** Can embedded artist, composer, copyright, or date fields be preserved without treating them as verified evidence?
6. **Experience:** Can playback and relevant decoder requirements be tested and reported?

## Format-level expectations

An MP3 audio stream can be parsed for technical properties such as MPEG audio version/layer, sample rate, channel mode, bitrate, and duration where reliably derivable. Optional ID3 tags may contain title, performer, album, composer, dates, artwork, comments, copyright notices, and other frames. Their presence and accuracy are not guaranteed.

References:
- MPEG Audio Layer III is covered by MPEG standards including ISO/IEC 11172-3 and ISO/IEC 13818-3.
- ID3v2 structure: https://id3.org/id3v2.4.0-structure
- ID3v2 frames: https://id3.org/Frames

## Evidence and rights model

Keep these separate:
- **Acquisition assertion:** the depositor's statement that they purchased or received the recording.
- **Acquisition evidence:** receipt, order history, download record, or other captured evidence, if available.
- **Embedded metadata:** ID3 values extracted from the file.
- **Rights statement or license:** a source that describes rights or permitted uses, if available.
- **Contractual terms:** terms that applied to the purchase or service, if known.
- **Technical observations:** format, stream properties, and detected tag values.
- **Rights assessment:** a separate assessment that may remain unknown.

Purchasing a copy does not automatically transfer copyright ownership or establish permission to redistribute. A copyright notice embedded in an ID3 tag is not proof of ownership. DAC should preserve bytes even if rights status is unknown and should not infer that a recording is freely reusable because it plays without technical restrictions.

## Three-level implementation plan

### Level 1 — Identify and inventory
- Identify the MP3 and report format ambiguity or parsing limitations.
- Record file size and compute a SHA-256 digest from the actual bytes.
- Perform basic frame/stream checks where practical.
- Preserve the original audio and any supplied evidence unchanged.

### Level 2 — Extract and describe
- Extract supported MPEG audio properties and all supported ID3 frames, including unknown frames where feasible.
- Record source identifiers, original values, tag version, and extraction status where practical.
- Preserve acquisition evidence as separate artifacts with their own provenance and digests.
- Keep embedded tags, depositor assertions, technical observations, and rights assessments distinct.

### Level 3 — Validate the usable experience
- Where feasible, test playback using identified software/decoder versions.
- Record playback success, warnings, seek/duration behavior when relevant, and dependencies.
- Do not infer legal rights, authenticity, or subjective audio quality from successful playback.

Level 3 is optional for byte preservation. An untested recording can still be archived with its validation status explicit.

## Suggested local container layout

```text
purchased-recording.dac/
├── dac.json
└── payload/
    ├── recording.mp3
    └── evidence/
        └── purchase-record.pdf
```

The evidence file is optional and illustrative. Do not fabricate evidence or expose personal/account information in a public fixture. Keep actual purchased recordings local unless publication is intentional, lawful, and appropriate.

## Acceptance criteria

- A recording can be preserved even if ID3 tags and purchase evidence are absent.
- Supported technical properties and ID3 frames are extracted with source and status information.
- The original MP3 is not modified by inspection.
- The digest is computed from actual bytes.
- Purchase/possession, copyright ownership, licensing, contractual terms, and redistribution permission remain distinct.
- A depositor's acquisition claim is not treated as verified solely because embedded tags name an artist or rights holder.
- Rights uncertainty does not block byte preservation.
- The archive records playback validation scope without promising future compatibility.

## Questions this case leaves open

- How should DAC represent the scope and evidence for a purchase that grants access to a download but no explicit reusable license?
- Should acquisition records be modeled as provenance events, evidence artifacts, or both?
- How should the reference application preserve large embedded artwork without duplicating it unnecessarily?
- How should DAC record contradictory artist or copyright values across ID3 frames and external evidence?
- What minimum evidence is appropriate before a rights status can be considered assessed?

Do not finalize the schema from this case alone. Compare it against the remaining hard cases first.
