# Test Case 06: Offline Game and Obsolete Execution Environment

**Status:** Exploratory fixture for breaking DAC Core  
**Normative:** No. This case does not define the final DAC schema.  
**Fixture identifier:** `offline-game`

## Scenario

An archived game is distributed as one or more files and was designed to run locally without requiring a currently available authentication or gameplay server. It may target an obsolete environment, such as an 8086-compatible PC running DOS, and may rely on specific graphics, sound, input, storage, BIOS, operating-system, or timing behavior.

The case is initially specification-level. No particular game binary, metadata, hardware requirement, or successful execution is asserted to exist until actual files and tests are supplied.

The original executable may be intact while its original environment is unavailable. Conversely, a modern port may preserve much of the gameplay while no longer being the original executable. Those are different preservation outcomes.

## What this case tests

1. **Payload preservation:** Can the original files, disk images, installers, manuals, patches, and available supporting material be retained without alteration?
2. **Identification and integrity:** Can each payload be identified and hashed independently, without confusing a container/archive hash with hashes of its contents?
3. **Environment description:** Can DAC record known or observed CPU architecture, operating system, memory, graphics, audio, input, timing, and other dependencies without presenting guesses as facts?
4. **Execution versus fidelity:** Can a successful launch be distinguished from tested gameplay, correct timing, audio/video behavior, and a reasonably faithful experience?
5. **Emulation and ports:** Can DAC describe an emulator, compatibility layer, virtual machine, or port as a separate means of access rather than as the original artifact?
6. **Evidence and provenance:** Can original package metadata, measured observations, and depositor claims be kept distinct?
7. **Rights uncertainty:** Can copyright, license terms, ownership of a copy, permission to make preservation copies, and permission to redistribute the game or required firmware/ROMs remain separate questions?
8. **Preservation limitations:** Can untested behavior, unavailable dependencies, known incompatibilities, and uncertain fidelity be reported explicitly?

## Format-level expectations

A game is not necessarily one self-contained executable. A release may include executables, data files, archives, disk images, configuration, save data, installers, patches, manuals, and copy-protection or hardware assumptions.

For an early PC game, a label such as “x86” is not a sufficient execution specification. Relevant details may include:
- CPU generation and instruction-set expectations, including 8086-compatible real-mode assumptions where applicable.
- Operating system and runtime expectations, such as DOS version, BIOS services, interrupt behavior, and memory layout.
- Display adapter and graphics modes, including possible dependence on CGA, EGA, VGA, or adapter-specific behavior.
- Sound hardware and driver expectations, such as PC speaker, AdLib/Sound Blaster, or other supported devices.
- Keyboard, joystick, mouse, timer, and input behavior.
- Disk layout, filesystem conventions, save-file locations, installer assumptions, and available storage.
- Timing-sensitive loops, undocumented hardware behavior, and copy-protection mechanisms.
- Dependencies on external libraries, drivers, firmware, ROM images, or physical peripherals.

These are candidate properties to investigate, not fields that every game must provide. Do not infer a game's requirements solely from its release date or marketing description.

## Reference-application extraction requirements

The reference application should inspect every supported file and archive member non-destructively, within declared support boundaries. It should not assume that every file in a game directory is part of the game or that filenames alone establish function.

Extract or record, where supported:
- File and archive inventory, paths, sizes, formats, and cryptographic digests of actual bytes.
- Executable format and structural properties, including target architecture and declared entry points when the format exposes them.
- Version strings, build information, embedded resource metadata, manifests, and other supported metadata.
- Archive and disk-image structure, filesystem metadata, boot information, and checksums where applicable.
- Readable configuration, dependency declarations, library references, scripts, and data-format identifiers.
- Parser identity/version, extraction scope, warnings, errors, and unsupported structures.
- Relationships between original files, installers, patches, extracted files, and any later derived or converted artifacts, without silently replacing originals.

Metadata extraction must not execute unknown code. If execution or dynamic analysis is offered, it must be a separate, explicit validation activity with its own environment and results. A static parser's interpretation is an observation, not proof that the game will run.

Distinguish:
- **Source metadata:** values actually read from files or embedded structures.
- **DAC observations:** format identification, measured hashes/sizes, dependency findings, parser results, and execution-test results.
- **Depositor assertions:** claims about the game's intended platform, provenance, original behavior, or history.

Preserve unknown fields where feasible, or report extraction limits. Distinguish absent, empty, unsupported, failed, and not examined states.

## Three-level implementation plan

### Level 1 — Identify and inventory
- Inventory all supplied files and nested archive members without modifying the originals.
- Identify formats and likely executable/disk-image structures where possible.
- Compute cryptographic digests from the actual bytes of each retained payload.
- Record file sizes, tool versions, ambiguous identifications, and errors.
- Keep original packages and images intact; do not make a converted or extracted copy the only retained artifact.

### Level 2 — Extract and describe
- Use mature format-specific parsers for executable formats, archives, disk images, manifests, and supported metadata.
- Extract all relevant supported fields rather than relying on a short fixed list.
- Record dependencies and platform indicators with their evidence source and confidence/limitations.
- Preserve original paths and archive-member relationships where possible.
- Represent emulators, compatibility layers, virtual machines, patches, and ports as separate tools or derived artifacts with their own versions, provenance, and hashes.
- Record unknown, unsupported, and unexamined information honestly.

### Level 3 — Validate the usable experience
- Attempt execution only in an explicitly described, appropriate environment.
- Record host hardware/software, emulator or VM name/version/configuration, guest OS, required firmware/ROMs, settings, and any patches or compatibility changes.
- Test beyond launch where feasible: menus, gameplay, controls, sound, display modes, save/load, timing-sensitive behavior, and exit/restart.
- Capture failures and warnings as well as successes. Identify which checks were not performed.
- Distinguish “original executable launched,” “selected behaviors tested,” and “experience assessed as reasonably faithful.” Do not collapse these into one pass/fail field.
- If a port or reimplementation is tested, record it as a distinct artifact and describe what is known—and unknown—about behavioral equivalence.

An emulator's existence does not prove the game works in it. A successful launch does not establish complete functionality or fidelity. Level 1 and Level 2 remain useful even when no runnable environment can be reconstructed.

## Suggested local container layout

```text
offline-game.dac/
├── dac.json
├── payload/
│   ├── original-package.zip
│   ├── game.exe
│   └── disk-image.img
└── access/
    ├── environment-notes.md
    └── test-results.json
```

The names above are illustrative only. Do not duplicate payload files unnecessarily; a manifest may instead describe their actual locations. Environment notes and test results are descriptive records, not replacements for original bytes.

## Four preservation targets

These targets describe preservation outcomes, not implementation stages. Evaluate them independently; do not collapse them into a single `preserved` Boolean.

| Target | Question | Evidence to record |
|---|---|---|
| **1. Original artifact** | Are the original bytes retained and independently verifiable? | Actual payload inventory, byte-level digests, source/derivative relationships, and any known gaps. |
| **2. Execution or use environment** | Is the environment needed to access the artifact available, documented, or reconstructable? | Relevant applications, decoders, operating systems, hardware, services, dependencies, versions, configuration, licensing/access constraints, and unavailable components. For non-executable artifacts, interpret this as the relevant viewing, reading, or playback environment. |
| **3. Behavior** | Have the artifact's intended functions been tested, and what happened? | Specific tests, environment, tool/version, results, warnings, dependencies, failures, and untested functions. A launch or open operation alone does not prove all intended behavior. |
| **4. Historical experience and fidelity** | How closely does the accessible experience match the historically intended or observed experience? | Relevant comparisons and evidence for rendering, timing, sound, interaction, editability, structure, or other case-specific properties; known differences; uncertainty; and explicit limits on the assessment. Do not claim fidelity without evidence. |

For this case, distinguish original game files from an available execution environment, successful testing of specific gameplay functions, and assessment of timing/presentation/input fidelity. An emulator, compatibility layer, or port is a separate means of access and must be identified as such.

A target may be **unknown**, **not tested**, **partially achieved**, **blocked**, or **supported by evidence**; choose terminology that accurately reflects the evidence. These are candidate status distinctions for evaluation, not a finalized DAC schema. A useful archive may preserve the original artifact even when the environment is unavailable, behavior is untested, or historical fidelity cannot be assessed.

## Acceptance criteria

- Every retained original file is inventoried and hashed from its actual bytes.
- Inspection does not modify the original payload.
- File-format observations and platform requirements identify their evidence and uncertainty.
- The archive can distinguish original software from emulators, patches, compatibility configurations, ports, and recordings.
- Execution attempts record the tested environment and outcomes, including failures and untested areas.
- Rights and redistribution status are not inferred from possession or technical executability.
- Unknown requirements and unavailable components remain explicit.
- Optional descriptions and execution tests are not prerequisites for preserving the payload.

## Questions this case leaves open

- How should DAC represent a game as a multi-file artifact without imposing one universal directory structure?
- Should an execution environment be embedded, referenced, or separately archived? How should licensing and missing firmware dependencies be recorded?
- How should confidence and evidence for inferred hardware/OS requirements be represented?
- How can test coverage and fidelity be described without implying that a few successful checks prove equivalence?
- How should save files, user-generated content, patches, and multiple regional/release variants relate to the original package?
- How should DAC describe a port that preserves gameplay concepts but not the original executable or exact behavior?

Do not finalize the schema from one case alone. Compare it against the remaining hard cases first.
