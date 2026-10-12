# Test Case 02: MP3 audio with minimal or absent metadata

**Status:** Exploratory fixture for breaking DAC Core  
**Normative:** No. This case does not define the final DAC schema.

## Scenario

A depositor has an MP3 audio file containing an original musical composition and recording reportedly generated with an AI music service. The file may have no descriptive tags, or only a small number of optional metadata fields.

This case begins with the MP3 and tagging specifications, not an inspection of a particular file. It does not assume that the file contains a title, artist, composer, lyrics, artwork, encoder name, timestamp, copyright notice, license, or provenance record.

## What this case tests

1. **Audio payload preservation:** Can DAC preserve the original MP3 bytes without modifying them?
2. **Format identification:** Can the reference application identify and report the actual audio format and stream properties it can reliably parse?
3. **Metadata extraction:** Can it extract supported metadata from optional ID3 tags and other recognized structures when present?
4. **Metadata absence:** Can the application represent a valid audio file with no descriptive tags?
5. **Technical versus descriptive metadata:** Can encoding properties be distinguished from claims about the work, its creator, or its legal status?
6. **Provenance assertions:** Can the depositor's account of AI generation be recorded without treating it as proven by the MP3 file?
7. **Independent integrity:** Can DAC compute a checksum over the actual file bytes?
8. **Rights uncertainty:** Can ownership, copyright status, service terms, and permissions remain unresolved unless supported by separate evidence?

## Format-level expectations

These expectations describe what a capable implementation may inspect. They are not assertions about any particular file.

### MPEG Audio Layer III bitstream

An MP3 file contains MPEG Audio Layer III audio data, often with additional data structures around or between the audio frames. A parser may be able to identify technical properties from the stream, including:
- MPEG audio version and layer;
- sample rate;
- channel mode;
- bitrate and whether it varies across frames;
- frame count and duration, where the stream can be parsed reliably;
- encoder or seek-related information from recognized optional structures, where present.

The exact observations depend on the file and parser capabilities. Duration and bitrate reporting should explain relevant limitations, especially for variable-bitrate files or malformed/truncated streams. Do not infer audio quality, authorship, creation date, or rights from technical properties alone.

### Optional ID3 tags

ID3 is a tagging convention used to store metadata alongside audio. ID3 tags are optional; an MP3 can contain playable audio without title, artist, album, date, genre, lyrics, artwork, or other descriptive tags.

Where supported, the reference application should extract all recognized ID3 versions and frames it can reliably parse, not just a fixed list of familiar fields. Examples include:
- title, performer/artist, album, composer, lyricist, genre, and recording/release date fields;
- comments, copyright notices, license-related fields, and URLs;
- lyrics or synchronized lyrics;
- attached pictures such as cover artwork;
- encoder-related text and user-defined frames;
- unique identifiers and unknown frames that the implementation can preserve safely.

ID3 frame names and meanings vary by version. The application should retain the tag version, frame identifier, original value/representation where practical, and extraction status. Unknown frames should not be silently discarded during a preservation-oriented read. If an implementation cannot safely interpret a frame, it should report that limitation rather than invent a meaning.

Tags can be missing, inaccurate, stale, or edited after the audio was generated. A field such as artist, composer, copyright, encoder, or date is metadata content—not independent proof of the corresponding claim.

### Specification references

- MPEG Audio Layer III is part of the MPEG audio standards, including ISO/IEC 11172-3 and ISO/IEC 13818-3.
- ID3v2 structure: https://id3.org/id3v2.4.0-structure
- ID3v2 frames: https://id3.org/Frames
- ID3v2.4 frame definitions: https://id3.org/id3v2.4.0-frames

## Reference-application metadata extraction requirements

The reference application should:
- identify the audio stream and extract technical properties it can reliably derive;
- inspect recognized metadata/tag structures and extract supported fields and frames;
- distinguish tag absent, field absent, field empty, unsupported frame, parse failure, and not examined;
- preserve original extracted values and their source locations/identifiers where practical;
- retain unknown tag/frame data where feasible, or explicitly report what could not be preserved;
- calculate a cryptographic digest from the actual complete file bytes;
- record the extractor and version, and report parsing limitations;
- never modify the source MP3 as a side effect of inspection.

As with other formats, keep these categories separate:

- **Source metadata:** values read from ID3 or other recognized structures.
- **DAC observations:** values measured or derived by the application, such as file size, digest, detected stream parameters, and parser outcome.
- **Depositor assertions:** statements such as “this composition and recording were generated using [service].”

The reference application should not treat embedded tags as verified identity, provenance, licensing, or rights evidence. DAC should not require optional descriptive fields merely to archive the audio.

## AI-generation and rights boundary

The MP3 format cannot, by itself, establish that the composition or recording was generated by a particular AI service, which service plan was used, which terms applied at generation time, whether human creative contributions qualify for protection, or who owns any relevant rights.

If such information matters to the depositor, record it as a separate assertion with its source and confidence/evidence status. Relevant service terms, dates, account records, project history, and legal analysis belong to separate evidence and rights-assessment workflows. Do not encode a legal conclusion into a tag-extraction result.

## Suggested local container layout

```text
ai-generated-music.dac/
├── dac.json
└── payload/
    └── composition.mp3
```

The actual MP3 is not included in this repository fixture. Use a local copy for implementation testing unless publication of the file is intentional and appropriate.

## Three-level implementation plan

Apply the shared DAC implementation levels to this case:

### Level 1 — Identify and inventory
- Identify the audio format and report ambiguity, file size, and a SHA-256 digest computed from the actual bytes.
- Perform basic frame/stream checks where practical.
- Preserve the original MP3 unchanged.

### Level 2 — Extract and describe
- Parse supported MPEG audio properties and recognized ID3 versions/frames.
- Preserve or account for unknown frames where feasible, with source identifiers and extraction status.
- Separate embedded tags from values derived by the parser and from depositor assertions.

### Level 3 — Validate the usable experience
- Where feasible, test playback using identified software/decoder versions.
- Record whether playback succeeded, any parsing or decoding warnings, and relevant playback dependencies.
- Check duration or seek behavior when relevant, particularly for variable-bitrate or malformed/truncated files.
- Do not infer subjective audio quality or legal rights from successful playback.

Level 3 is optional for byte preservation; an untested file can still be archived with its validation status clearly recorded.

## Four preservation targets

These targets describe preservation outcomes, not implementation stages. Evaluate them independently; do not collapse them into a single `preserved` Boolean.

| Target | Question | Evidence to record |
|---|---|---|
| **1. Original artifact** | Are the original bytes retained and independently verifiable? | Actual payload inventory, byte-level digests, source/derivative relationships, and any known gaps. |
| **2. Execution or use environment** | Is the environment needed to access the artifact available, documented, or reconstructable? | Relevant applications, decoders, operating systems, hardware, services, dependencies, versions, configuration, licensing/access constraints, and unavailable components. For non-executable artifacts, interpret this as the relevant viewing, reading, or playback environment. |
| **3. Behavior** | Have the artifact's intended functions been tested, and what happened? | Specific tests, environment, tool/version, results, warnings, dependencies, failures, and untested functions. A launch or open operation alone does not prove all intended behavior. |
| **4. Historical experience and fidelity** | How closely does the accessible experience match the historically intended or observed experience? | Relevant comparisons and evidence for rendering, timing, sound, interaction, editability, structure, or other case-specific properties; known differences; uncertainty; and explicit limits on the assessment. Do not claim fidelity without evidence. |

For this case, distinguish retaining the MP3 bytes from having a usable decoder, successful playback, and assessment of duration/seek behavior. Playback success does not verify AI origin, subjective quality, or rights.

A target may be **unknown**, **not tested**, **partially achieved**, **blocked**, or **supported by evidence**; choose terminology that accurately reflects the evidence. These are candidate status distinctions for evaluation, not a finalized DAC schema. A useful archive may preserve the original artifact even when the environment is unavailable, behavior is untested, or historical fidelity cannot be assessed.

## Acceptance criteria

- A valid MP3 with no descriptive tags can be archived.
- The application extracts all supported metadata it can reliably read, including supported ID3 frames beyond a hard-coded minimum.
- Audio stream properties are recorded as technical observations, not as claims about authorship or rights.
- Optional or absent tags do not cause archive creation to fail.
- Source metadata, DAC observations, and depositor assertions remain distinguishable.
- Unknown or unsupported frames are preserved where feasible or reported explicitly.
- Missing, unsupported, failed, and unexamined extraction states are not conflated.
- The original payload is not modified by inspection.
- A checksum is computed from the actual file bytes, not inferred from metadata.
- No title, artist, composer, date, license, ownership, or AI-origin claim is invented when absent.
- The archive can preserve an unresolved rights assessment without blocking byte preservation.

## Questions this case leaves open

- How should DAC represent large binary metadata values such as embedded artwork without duplicating them unnecessarily?
- Should unknown ID3 frames be copied verbatim into an extraction record, retained only in the source payload, or both?
- How should extraction results distinguish metadata that is embedded in the audio file from values inferred by a media parser?
- Should technical audio properties be stored in the common artifact model or in format-specific extension records?
- What evidence model should link an AI-generated audio file to a service, project, generation date, prompt, and applicable terms?
- How should DAC record that a rights assessment is incomplete without implying that the work is either protected or unprotected?

Do not finalize the schema from this case alone. Compare it against additional hard cases first.
