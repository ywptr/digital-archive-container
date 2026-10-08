# Digital Archive Container (DAC)

## Core Specification — Draft 0.1

**Status:** Draft
**Copyright © 2026 Yogi / contributors**
**License:** Creative Commons Attribution 4.0 International

---

## 1. Purpose

The Digital Archive Container (DAC) specification defines a portable, self-describing structure for preserving digital artifacts together with the information necessary to understand, verify, and, where legally and technically possible, experience those artifacts in the future.

A DAC container may preserve:

* the digital artifact itself;
* its identity and description;
* provenance and origin;
* integrity information;
* applicable rights and restrictions;
* experience requirements;
* dependencies;
* relevant execution or viewing environment information; and
* known limitations to future preservation or reproduction.

DAC is intended to be:

* open;
* vendor-neutral;
* portable;
* independently implementable;
* usable without a network connection;
* understandable by humans;
* machine-readable; and
* resilient to the disappearance of the software, services, vendors, and platforms that originally created or distributed the artifact.

### 1.1 What DAC Does Not Do

DAC does **not**:

* create ownership or usage rights;
* override copyright or licensing restrictions;
* remove DRM;
* guarantee future compatibility;
* guarantee that an artifact can be experienced in the future;
* require blockchain, cryptocurrency, or a particular cloud service;
* require a particular storage provider;
* require a particular application or operating system.

DAC records the state of the artifact and the information available about it.

It does not manufacture permissions that do not exist.

---

## 2. Design Principle

The primary design principle of DAC is:

> **A digital archive should remain understandable even after its creators, vendors, platforms, and software have disappeared.**

A DAC implementation is therefore replaceable.

The specification is more important than any particular DAC application.

A successful DAC ecosystem would allow an independent implementation, potentially decades after the original implementation has disappeared, to inspect a DAC container and determine:

1. what the artifact is;
2. where it came from;
3. what data belongs to it;
4. whether the data has changed;
5. what rights and restrictions were known;
6. what is required to experience it;
7. what dependencies exist; and
8. what limitations prevent reproduction.

---

## 3. Conceptual Model

A DAC asset consists of five primary concerns:

```text
DAC Asset
│
├── CONTENT
│   └── What is the artifact?
│
├── PROVENANCE
│   └── Where did it come from?
│
├── RIGHTS
│   └── What may be done with it?
│
├── EXPERIENCE
│   └── What is required to experience it?
│
└── PRESERVATION
    └── What is required or possible to preserve it?
```

These concerns are related but must not be conflated.

For example:

* possessing a file does not imply permission to redistribute it;
* preserving a game executable does not imply that the game can still be played;
* knowing the original operating system does not guarantee that the original runtime can be reconstructed;
* preserving an artifact does not preserve an external service on which that artifact depends.

---

## 4. Container Requirements

A DAC container MUST be self-describing.

At minimum, a conforming DAC container MUST contain a machine-readable root metadata document identifying:

* that the container conforms to DAC;
* the DAC specification version;
* the identity of the archived asset; and
* the location or identification of the preserved payload.

The root metadata SHOULD use a format that is:

* openly specified;
* widely implemented;
* human-readable; and
* capable of representing structured data.

JSON is the initial reference format for DAC metadata.

The specification does not require the physical DAC container to be a custom filesystem or archive format.

A DAC container MAY be represented using an existing archival format such as ZIP, provided that the resulting package preserves the DAC structure and semantics.

---

## 5. Content

The **content** section identifies and preserves the artifact itself.

Content MAY consist of:

* a single file;
* multiple files;
* directories;
* executable software;
* documents;
* audio;
* video;
* images;
* games;
* datasets;
* application resources; or
* other digital artifacts.

A DAC container SHOULD preserve the original artifact without unnecessary transformation.

When transformation is necessary, the transformation SHOULD be documented.

### 5.1 Identity

The content metadata SHOULD provide, where known:

* title;
* creator;
* publisher;
* creation date;
* publication date;
* version;
* language;
* media type;
* original filename;
* original format;
* identifiers assigned by the creator or publisher.

An artifact MAY have multiple identifiers.

DAC does not require a universal identifier authority.

---

## 6. Provenance

The **provenance** section records where the archived artifact came from and how it entered the container.

Provenance SHOULD record, where known:

* source;
* acquisition date;
* acquisition method;
* original location;
* creator or publisher;
* previous transformations;
* previous custodians;
* relevant transaction or acquisition information;
* provenance confidence.

Provenance information MUST NOT be interpreted as proof of ownership unless the recorded evidence actually establishes such ownership.

For example:

```text
Source:
    Purchased from Example Store

Acquired:
    2026-04-12

Acquisition method:
    Digital purchase

Original publisher:
    Example Publisher
```

This establishes provenance information.

It does not, by itself, establish what rights the purchaser possesses.

---

## 7. Integrity

A DAC container MUST provide a means of detecting unintended modification of preserved content.

The reference mechanism is cryptographic hashing.

A conforming implementation SHOULD support SHA-256 or a stronger contemporary successor.

Integrity metadata SHOULD identify:

* the hashed object;
* the hash algorithm;
* the resulting digest.

Example:

```json
{
  "path": "payload/content/example.pdf",
  "algorithm": "sha256",
  "digest": "..."
}
```

Integrity establishes whether the archived representation has changed.

Integrity does **not** establish:

* authenticity;
* ownership;
* legality;
* authorship; or
* correctness of the original artifact.

Digital signatures MAY be used where stronger provenance or authenticity evidence is available.

---

## 8. Rights

Rights are a first-class component of DAC.

The rights section records known permissions, restrictions, licenses, and uncertainties concerning the archived artifact.

Rights information SHOULD distinguish between:

* ownership;
* possession;
* archival rights;
* backup rights;
* copying rights;
* modification rights;
* migration rights;
* emulation rights;
* execution rights;
* redistribution rights;
* public exhibition rights;
* commercial use rights; and
* other applicable permissions.

Where a machine-readable rights expression exists, DAC MAY reference or embed it.

Where rights cannot be expressed reliably in machine-readable form, human-readable legal text SHOULD be preserved.

### 8.1 Rights Must Not Be Inferred

A DAC implementation MUST NOT infer a permission merely because an action is technically possible.

For example:

```text
The user possesses a copy.
        ≠
The user may redistribute the copy.
```

Likewise:

```text
The user can technically remove DRM.
        ≠
The user has permission to remove DRM.
```

Unknown rights MUST remain unknown.

---

## 9. Experience

DAC distinguishes preservation of an artifact from preservation of the ability to experience that artifact.

The **experience** section describes what is required to use, execute, view, hear, or otherwise experience the artifact.

Experience requirements MAY include:

* operating system;
* CPU architecture;
* GPU requirements;
* runtime;
* interpreter;
* libraries;
* codecs;
* hardware;
* peripherals;
* network connectivity;
* authentication;
* activation services;
* external APIs;
* online servers;
* account requirements;
* encryption keys;
* DRM systems;
* regional restrictions; and
* other external dependencies.

### 9.1 Experience Modes

An artifact SHOULD identify its experience mode where known.

Examples:

```text
offline
online
hybrid
unknown
```

An offline artifact does not require an external service for its normal experience.

An online artifact depends on one or more external services.

A hybrid artifact contains both offline and externally dependent functionality.

### 9.2 Experience Status

DAC SHOULD distinguish between:

```text
reproducible
partially_reproducible
blocked
unknown
```

For example:

```yaml
experience_status:
  reproducibility: reproducible
  offline_capable: true
  external_services: none
```

Or:

```yaml
experience_status:
  reproducibility: blocked
  reason: external_service_required
  service: publisher.example
```

The status describes the preservation state **at the time of archival or assessment**.

It is not a guarantee of future compatibility.

---

## 10. Dependencies

Dependencies identify components outside the primary payload that are required to experience the artifact.

A dependency MAY be:

* another DAC asset;
* a software package;
* an operating system;
* a hardware component;
* a runtime;
* a service;
* a network endpoint;
* a cryptographic key;
* an account;
* a license;
* a physical device.

Where legally and technically possible, dependencies SHOULD themselves be preserved or referenced by stable identity.

A dependency that cannot be preserved SHOULD still be recorded.

Example:

```yaml
dependencies:
  - type: runtime
    name: Java
    version: "8"

  - type: service
    name: Example Authentication Service
    required: true
    preserved: false
```

The existence of a dependency does not imply that the dependency can legally be copied or preserved.

---

## 11. Preservation Context

The preservation section records information useful for future recovery.

It MAY include:

* original platform;
* original operating system;
* architecture;
* required software versions;
* known compatible versions;
* hardware characteristics;
* virtualization requirements;
* emulation requirements;
* migration history;
* format identification;
* known obsolete components;
* known preservation attempts.

Preservation context SHOULD favor durable descriptions over references to a particular modern product.

For example:

```text
CPU architecture: x86-64
Operating system: Windows
Filesystem requirement: case-insensitive
Network requirement: none
```

is generally more durable than:

```text
Works on Product X version 4.2.
```

Product-specific information MAY still be recorded when relevant.

---

## 12. Preservation Limitations

DAC MUST allow an archive to explicitly record things that could not be preserved.

Examples include:

* missing source files;
* unavailable encryption keys;
* proprietary DRM;
* inaccessible authentication servers;
* unavailable hardware;
* unknown dependencies;
* expired licenses;
* legal restrictions;
* missing external media;
* corrupted original data;
* incomplete provenance.

Failure is itself useful archival information.

An archive SHOULD NOT conceal a known preservation failure merely because the artifact is otherwise present.

Example:

```yaml
preservation:
  status: incomplete

  limitations:
    - external_authentication_server_unavailable
    - original_license_terms_unknown
```

---

## 13. Optional Components

DAC MAY contain additional metadata or payload components.

Implementations MUST ignore unknown optional components rather than treating their presence as a reason to reject the entire container.

This provides forward compatibility.

A future DAC implementation may therefore understand:

```text
DAC 1.0
```

without understanding every extension introduced by:

```text
DAC 2.x
DAC 3.x
```

provided that the core remains compatible.

---

## 14. Human and Machine Readability

DAC metadata SHOULD be understandable by both humans and machines.

Human readability is important because future software may not exist.

A future archivist should be able to open the metadata with a text editor and understand the basic nature of the archive.

Machine readability is important because large collections cannot reasonably be managed manually.

Neither requirement replaces the other.

---

## 15. Trust and Authenticity

DAC MAY contain digital signatures.

A signature MAY establish that a particular party signed a particular representation at a particular time.

A signature MUST NOT be interpreted as permanent proof of legal ownership.

Long-term validation of signatures is outside the DAC core specification and may depend on external trust infrastructure.

Unsigned DAC containers remain valid DAC containers.

---

## 16. External Services

An artifact MAY depend upon an external service.

DAC MUST NOT pretend that preservation of the artifact constitutes preservation of that service.

For example:

```text
game files preserved
+
authentication server required
+
authentication server no longer exists
=
artifact preserved
experience unavailable
```

The archive SHOULD preserve the identity and known characteristics of the dependency even when the dependency itself cannot be preserved.

Where legally permitted, a preservation project MAY preserve additional service infrastructure separately.

DAC does not require that this be possible.

---

## 17. Rights and Future Recovery

DAC recognizes two independent questions:

### Question A — Can the artifact be recovered?

This concerns:

* data;
* formats;
* integrity;
* dependencies;
* environment;
* preservation context.

### Question B — May the recovered artifact legally be experienced?

This concerns:

* copyright;
* license;
* DRM;
* contractual restrictions;
* authentication;
* territorial restrictions;
* other applicable rights.

A successful DAC archive should preserve evidence relevant to **both** questions.

However:

> DAC cannot transform an unlawful use into a lawful use.

If future software chooses to ignore a restriction, that is a property of that software and its operator, not a property of DAC.

---

## 18. Conformance

A DAC implementation conforms to the core specification if it:

1. recognizes the DAC container structure;
2. can identify the DAC specification version;
3. can identify the archived asset;
4. can locate or identify its payload;
5. can evaluate the declared integrity information;
6. can expose recorded rights information;
7. can expose experience requirements;
8. can expose known dependencies; and
9. does not misrepresent unknown or unavailable preservation information as known facts.

An implementation MAY provide capabilities beyond the core specification.

---

# 19. Initial Validation Cases

The following cases are normative design tests for the DAC model.

A future DAC schema and implementation SHOULD be tested against each case.

## Case 1 — DRM-Free PDF

A user purchases a PDF that contains no DRM and keeps a local copy.

Expected result:

```text
content:
    PDF preserved

rights:
    purchase/license information preserved

experience:
    PDF reader required

dependencies:
    none beyond reader

preservation:
    straightforward
```

DAC should represent this without unnecessary complexity.

---

## Case 2 — Purchased MP3

A user purchases an MP3 from a legitimate distributor.

Expected result:

* audio preserved;
* acquisition provenance preserved;
* license information preserved;
* redistribution rights recorded separately from possession;
* playback requirements recorded.

The archive must not assume that purchasing the file grants unrestricted redistribution rights.

---

## Case 3 — DRM-Protected Movie

A purchased movie uses proprietary DRM.

The user possesses the encrypted media but does not possess the decryption keys independently.

Expected result:

```text
content:
    encrypted media preserved

rights:
    license preserved if available

experience:
    DRM system required

dependencies:
    DRM infrastructure

preservation:
    limited

experience_status:
    potentially blocked
```

DAC must be capable of representing the artifact without claiming that it has preserved a usable copy.

---

## Case 4 — Offline Windows Game

A game runs entirely offline and requires Windows x86-64.

Expected result:

```text
content:
    game files

experience:
    Windows x86-64

external_services:
    none

offline_capable:
    true
```

If the license permits backup and migration, those rights should be recorded.

The archive should contain enough environmental information to support future reconstruction.

---

## Case 5 — Game Requiring a Dead Authentication Server

A game executable and all local resources are preserved.

The original authentication server no longer exists.

Expected result:

```text
content:
    preserved

integrity:
    verifiable

experience:
    authentication server required

dependency:
    unavailable

experience_status:
    blocked
```

The DAC remains valid.

The fact that the game cannot currently be played does not make the archive defective.

---

## Case 6 — Backup Permitted, Redistribution Prohibited

A software license explicitly permits personal backup but prohibits redistribution.

Expected result:

```text
rights:
    archival_copy: permitted
    backup: permitted
    redistribution: prohibited
```

The existence of the archive does not imply that the archive may legally be shared.

---

## Case 7 — Original Application Disappeared

A document can only be properly rendered by an obsolete application.

The application is no longer commercially available.

Expected result:

```text
content:
    document preserved

experience:
    obsolete application required

preservation:
    original application unavailable

experience_status:
    blocked or partially_reproducible
```

If the application itself can legally be preserved, it may be included as a dependency or separate DAC asset.

---

## Case 8 — Missing External Assets

A project file references external fonts, images, or libraries that were not archived.

Expected result:

```text
content:
    primary project preserved

dependencies:
    missing external assets

experience_status:
    partially_reproducible
```

DAC must make the incompleteness visible.

---

## Case 9 — Publisher Withdrawal

A publisher removes an otherwise downloadable product from its store.

A user already possesses a lawful local copy.

The publisher's withdrawal does not alter the historical provenance of the archived copy.

DAC should preserve:

* acquisition information;
* original publisher;
* original version;
* known license;
* withdrawal information where relevant.

DAC must not infer that withdrawal automatically grants or removes any legal right.

---

## Case 10 — Artifact Preserved, Experience No Longer Legally or Technically Possible

A digital artifact is completely preserved.

However:

* its license has expired;
* required service infrastructure no longer exists; or
* another documented restriction prevents lawful or technical use.

Expected result:

```text
content:
    preserved

integrity:
    verified

experience_status:
    blocked

reason:
    explicitly recorded
```

This is a successful archival record even though the original experience has not been preserved.

---

# 20. The Core Distinction

DAC therefore makes a deliberate distinction:

> **Preserving the thing is not necessarily preserving the ability to use the thing.**

A serious digital archive must preserve both where possible, and must clearly record where it cannot.

---

# 21. Future Work

The following are intentionally outside DAC Core 0.1:

* physical container format;
* detailed JSON Schema;
* content-addressed storage;
* distributed replication;
* peer-to-peer distribution;
* DRM circumvention;
* legal interpretation;
* digital-rights enforcement;
* archival institutions;
* trust authorities;
* executable sandboxing;
* virtual machines;
* emulation;
* automated dependency capture;
* automatic format migration;
* AI-assisted archival analysis.

These may become DAC profiles, extensions, implementations, or companion specifications.

They must not become requirements of the core unless there is a demonstrated need.

---

## 22. Guiding Rule

When deciding whether something belongs in DAC Core, ask:

> **Would an independent person, decades from now, need this information to understand what was preserved, where it came from, what they are permitted to do with it, or what is required to experience it?**

If the answer is no, it probably does not belong in the core.
