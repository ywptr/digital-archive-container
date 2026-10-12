# Test Case 11: Withdrawn or Delisted Digital Product

**Status:** Exploratory fixture for breaking DAC Core  
**Normative:** No. This case does not define the final DAC schema.  
**Fixture identifier:** `withdrawn-product`

## Scenario

A publisher, distributor, or platform removes a digital product from sale or its catalogue. Existing copies may still exist on a user's device or in a backup, but the product's future availability and support may be uncertain.

The product could be an e-book, purchased audio/video, application, game, plugin, or other digital item. Delisting may affect only new purchases, or it may coincide with the loss of downloads, updates, activation, account access, or online functionality. Those outcomes must be investigated separately.

This is initially a specification-level case. Unless actual evidence and files are supplied, it does not assert that a particular product was withdrawn, that an existing copy remains usable, or that any legal rights changed.

**Central distinction:** removal from a storefront is an event about catalogue or sales availability. By itself, it does not establish that an existing copy was deleted, a license was terminated or preserved, an authentication service shut down, or redistribution became permitted.

## What this case tests

1. **Event precision:** Can DAC describe what was withdrawn, by whom, from which service, and when, without generalizing beyond the evidence?
2. **Availability dimensions:** Can it distinguish availability for new purchase from existing downloads, updates, account access, activation, support, and online services?
3. **Existing copies:** Can DAC record the actual bytes a depositor possesses, their provenance, and integrity independently of the product's current sales status?
4. **Rights uncertainty:** Can DAC avoid inferring ownership, license termination, continued access rights, or redistribution permission from delisting alone?
5. **Evidence over time:** Can DAC preserve dated evidence and distinguish observed status at a particular time from current or permanent status?
6. **Service and dependency separation:** Can it distinguish product withdrawal from a separately unavailable authentication, delivery, update, or gameplay service?
7. **Preservation outcomes:** Can it describe an intact but inaccessible or untested copy without calling the product fully preserved or declaring it lost?

## Format-level and event-level expectations

“Withdrawn,” “delisted,” “discontinued,” “unsupported,” “unavailable,” and “revoked” are not interchangeable facts.

A product may be:
- Removed from sale while previously acquired copies remain downloadable.
- Removed from sale and also removed from a user's download library.
- No longer receiving updates while the current version remains available.
- Dependent on a separate service that remains operational, has shut down, or cannot currently be reached.
- Available as local files but blocked by account authentication, license checks, missing dependencies, or an unavailable application.
- Replaced by a new edition that is not byte-identical or behaviorally equivalent.
- Withdrawn for different reasons, including commercial decisions, licensing disputes, security concerns, legal orders, or an unknown reason.

These are possible scenarios, not facts to infer. Evidence should identify the scope of each claim. A publisher announcement may establish an announced withdrawal date; it does not necessarily establish that every distribution channel stopped working on that date. A failed download attempt establishes that the attempt failed under recorded conditions, not that all downloads are permanently impossible.

Record temporal and source context for claims where available: source URL or identifier, capture or observation time, publisher/distributor identity as presented, relevant product/version/region/platform, and the exact scope of the statement. Preserve a local copy or capture of public evidence only when lawful and technically appropriate. A screenshot or announcement is evidence of what it states, not automatic proof of every underlying claim.

Delisting alone does not establish the legal status of an existing copy. Copyright ownership, license terms, contract terms, access permissions, backup permissions, and redistribution permissions remain distinct questions. Record the applicable evidence and unresolved uncertainty without offering a legal conclusion unsupported by it.

## Reference-application extraction requirements

The reference application should:
- Inventory and hash only payload bytes actually supplied or otherwise explicitly made available to it.
- Keep product identity, edition, version, platform, region, account association, and acquisition details optional when unknown.
- Distinguish depositor-reported acquisition and access history from extracted metadata and independently observed results.
- Support evidence records for a withdrawal/delisting claim, including source, observation or publication time, subject, scope, and confidence or verification method where supported.
- Preserve the original wording or relevant excerpt of an announcement when practical, while avoiding unsupported paraphrases such as “all copies are revoked.”
- Record each availability dimension separately where examined: new purchase, download of an existing entitlement, updates, activation, account access, support, and online services.
- Record the date, time, region, platform, account/entitlement conditions where relevant and safe to disclose, tool/version, attempted operation, and result of an availability test.
- Distinguish a failed attempt, confirmed withdrawal, inaccessible source, unsupported check, and not-examined state.
- Avoid repeated or automated account actions, unauthorized access, or attempts to bypass licensing or access controls.
- Preserve source metadata, DAC observations, and depositor assertions as distinct classes of information.
- Avoid converting a dated observation into a timeless claim. Historical evidence and the latest known observation should remain distinguishable.
- Never modify the original payload during identification or metadata extraction.

## Three-level implementation plan

### Level 1 — Identify and inventory
- Inventory each supplied product file, installer, package, manual, receipt, license document, or other evidence item.
- Compute digests from actual bytes and record file sizes, format identification, and tool/version.
- Record the depositor's description of the product and withdrawal event as an assertion unless independently supported.
- Preserve supplied payload bytes unchanged.
- Do not infer withdrawal, availability, provenance, or rights status from filenames alone.

### Level 2 — Extract and describe
- Extract all relevant supported metadata from supplied files and documents.
- Record product and version identifiers, embedded license/manifest information, URLs, timestamps, and distribution or service references where present.
- Capture evidence for a withdrawal event separately from evidence about existing entitlements, downloads, activation, or service operation.
- Record the source and temporal scope of each observation, including conflicting evidence.
- Keep acquisition evidence, product metadata, rights statements, publisher announcements, and DAC observations distinct.
- Represent unknown or unavailable evidence explicitly; do not require a receipt, title, author, date, or license statement to preserve the bytes.
- Do not automatically access accounts or remote services as part of ordinary metadata extraction.

### Level 3 — Validate the usable experience
- Where authorized and feasible, test relevant operations separately: opening or launching an existing local copy, retrieving an already entitled download, checking for updates, activation, and online features.
- Record the exact operation, date/time, environment, relevant account/entitlement assumptions, tool/version, result, and warnings.
- Distinguish local-file usability from continued publisher or platform support.
- If a test fails, record the observed failure and avoid claiming permanent unavailability unless stronger evidence supports that conclusion.
- Compare a replacement edition or substitute service with the archived version only where evidence and access allow; do not treat it as the original.
- Do not bypass DRM, authentication, or other access controls.
- Explicitly mark untested paths and any conclusions that depend on depositor statements or third-party evidence.

## Four preservation targets

Evaluate each target independently. Delisting is context about availability; it is not itself a preservation outcome.

| Target | Question | Evidence to record |
|---|---|---|
| **1. Original artifact** | Are the existing product bytes retained and independently verifiable? | Actual supplied files, hashes, provenance evidence, version/edition where known, and missing components. A storefront listing is not a substitute for the bytes. |
| **2. Execution or use environment** | Can the artifact's required environment and access path still be used or reconstructed? | Applications, platforms, dependencies, entitlement/download paths, activation and service requirements, their observed status, and evidence dates. |
| **3. Behavior** | Have relevant functions been tested, and what happened? | Separate results for local use, download, updates, activation, account access, and online features; include environment, date, failures, and untested paths. |
| **4. Historical experience and fidelity** | Does the accessible copy provide the experience associated with the withdrawn edition? | Edition/version evidence, comparison with historical descriptions or captures, differences from replacements, missing online features, and limits of the assessment. |

A target may be unknown, not tested, partially achieved, blocked, or supported by evidence. These are candidate status distinctions, not a finalized DAC schema. Do not represent all four targets with one `preserved` Boolean.

## Suggested local container layout

```text
withdrawn-product.dac/
├── dac.json
├── payload/
│   └── [actual files available to the depositor]
└── preservation/
    ├── availability-observations.json
    └── evidence/
        └── [lawfully retained announcements, receipts, captures, or documents]
```

Illustrative only. Include only files that actually exist. Do not create placeholder payloads or evidence and do not treat a URL as proof that its target was captured. Consider privacy and access restrictions before retaining account records or receipts.

## Acceptance criteria

- A withdrawal or delisting claim has a source, time context, and defined scope, or is clearly marked as an assertion or unknown.
- New sales, existing downloads, updates, activation, support, account access, and online services are not conflated.
- Existing artifact bytes are inventoried and hashed independently of the product's catalogue status.
- A failed access attempt is not silently promoted to proof of permanent unavailability.
- Product withdrawal does not automatically change the recorded legal status of existing copies or grant redistribution permission.
- Rights evidence, depositor assertions, extracted source metadata, and DAC observations remain distinguishable.
- Tests identify their date, environment, operation, and result; untested behavior remains explicit.
- The original payload is not modified, and no DRM or access-control circumvention is required.
- An archive can accurately represent a withdrawn product whose files survive, whose environment is blocked, or whose availability status remains uncertain.

## Questions this case leaves open

- Should DAC model product lifecycle events (published, delisted, discontinued, service shut down) as a general event history, or leave them in extensible evidence records?
- How should multiple storefronts, regions, editions, and platform-specific entitlements be associated with one product without conflating them?
- How should observations with different dates or conflicting sources be represented without a misleading single current status?
- What minimum provenance is needed to connect a locally held file to a particular published edition?
- How should DAC link a withdrawn product to separately archived installers, updates, manuals, or service dependencies?
- How can the archive express that bytes survive while lawful access, practical usability, or historical experience remains uncertain?

Do not finalize the schema from one case alone. Compare it against the remaining hard cases first.
