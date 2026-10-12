# Test Case 07: Game with an Unavailable Authentication Service

**Status:** Exploratory fixture for breaking DAC Core  
**Normative:** No. This case does not define the final DAC schema.  
**Fixture identifier:** `dead-auth-server-game`

## Scenario

A game has been acquired and some or all client-side files are available, but the original authentication, entitlement, matchmaking, content-delivery, or gameplay service is no longer operating. The client may refuse to launch, permit only offline features, or launch but fail when it attempts to contact the service.

The service may have been required for account verification, license checks, downloading additional assets, saving progress, multiplayer sessions, anti-cheat, or core gameplay. These functions must not be assumed to be interchangeable. The scenario does not assume that the service is permanently gone, that the depositor is entitled to bypass its controls, or that any replacement implementation would be lawful or behaviorally equivalent.

The case is initially specification-level. No specific game, server protocol, account state, entitlement, or observed failure is asserted until evidence is supplied.

## What this case tests

1. **Partial preservation:** Can DAC describe client files that are intact while required remote functionality is unavailable?
2. **Service dependency:** Can DAC identify which functions depend on remote services and distinguish confirmed dependencies from inferred ones?
3. **Access versus integrity:** Can files pass integrity checks while the game remains inaccessible or only partly usable?
4. **Temporal context:** Can the archive record when a service was observed unavailable, by whom, using what evidence, without treating a dated observation as a guarantee about all future time?
5. **Account and entitlement boundaries:** Can account-bound access, license checks, and user-specific entitlements be described without storing passwords, session tokens, or other secrets?
6. **Reconstruction and replacement:** Can authorized offline modes, preserved server components, compatibility services, community servers, or reimplementations be represented as distinct artifacts and claims?
7. **Rights and safety:** Can the archive document technical barriers and lawful access limits without instructing users to bypass DRM, steal credentials, extract protected keys, or evade access controls?
8. **Experience status:** Can the record distinguish preserved bytes, launchability, offline functionality, online functionality, and fidelity of any replacement?

## Format-level expectations

A modern game may depend on more than its executable and local data. Possible external dependencies include:
- Authentication and entitlement validation.
- Account/profile services and remote configuration.
- Downloaded assets, updates, patches, or dynamically delivered content.
- Matchmaking, lobby, multiplayer, leaderboard, or social services.
- Server-authoritative simulation, game logic, inventory, progression, or save storage.
- Anti-cheat and platform-specific services.
- Third-party launchers, storefront clients, DRM components, certificates, or identity providers.
- DNS names, API endpoints, protocol versions, certificates, and service-specific configuration.

These are possible categories, not proof that a given title uses them. A service outage can be temporary, regional, account-specific, or caused by a local configuration problem. Record the observed symptom and supporting evidence; do not infer permanent shutdown from a single failed connection.

A replacement service may restore some functionality without reproducing the original protocol, security model, server-side logic, economy, multiplayer population, or behavior. A fan server or reverse-engineered implementation must not be described as the original service unless that identity is supported.

## Reference-application extraction requirements

Inventory the available local artifacts and extract supported metadata from executables, manifests, configuration files, logs supplied by the depositor, package metadata, and documentation. Do not execute unknown code merely to discover dependencies.

Where evidence exists, record:
- Client files, launchers, libraries, manifests, version/build identifiers, sizes, hashes, and file relationships.
- Explicit endpoint names, protocol/version indicators, service URLs, dependency declarations, and configuration references present in the supplied files.
- Relevant timestamps and context for supplied error messages, service notices, support pages, release notes, or shutdown announcements.
- Which claims come from local artifacts, depositor-supplied documentation, contemporaneous observations, or third-party statements.
- Whether an access attempt was made, the date/time and environment, the steps performed, the observed response, and any errors—without retaining passwords, session cookies, access tokens, private keys, or other credentials.
- Whether the test checked authentication, launch, offline play, downloaded content, single-player, multiplayer, saves, or other discrete functions.

Do not probe services beyond authorized, proportionate checks. A client error alone does not prove the service is shut down. A published shutdown notice may establish an announced end date but does not by itself establish every feature's status or the behavior of all client versions.

Keep source metadata, DAC observations, and depositor assertions distinct. Preserve unknown metadata where feasible and report unsupported, failed, or unexamined extraction states. Metadata extraction must not modify the payload.

## Three-level implementation plan

### Level 1 — Identify and inventory
- Preserve and inventory the local files, installers, patches, manifests, documentation, and logs that are actually available.
- Compute cryptographic digests from actual payload bytes and record sizes, formats, and parser errors.
- Record acquisition or provenance claims as claims unless supported by independently retained evidence.
- Do not imply that a server, account, entitlement, or downloadable asset has been preserved merely because a client is present.

### Level 2 — Extract and describe
- Parse supported manifests, configuration, dependency declarations, endpoint references, package metadata, and version information.
- Record service dependencies as evidence-linked statements, with distinctions such as declared, observed during testing, inferred, disputed, or unknown where appropriate.
- Record the source and date of service-status claims, along with scope and uncertainty.
- Represent server software, protocol specifications, client patches, offline launchers, community services, and reimplementations as separate artifacts with independent provenance and hashes.
- Do not store credentials or sensitive account material as ordinary archive metadata. If user-specific data is necessary for a lawful preservation purpose, define a separate secure handling policy rather than embedding secrets in the manifest.

### Level 3 — Validate the usable experience
- Test only authorized access paths and document the exact client version, environment, account/entitlement assumptions, date, and discrete function tested.
- Distinguish failures of local launch, authentication, entitlement, content delivery, matchmaking, server gameplay, and saving where evidence permits.
- Test offline features separately from online features; do not assume that one result predicts the other.
- If a preserved or replacement service is available, record its operator/identity claim, version, configuration, protocol compatibility, limitations, and test results. Do not equate a working substitute with the original service.
- Clearly label features that could not be tested, including those requiring unavailable accounts, hardware, remote services, or permissions.
- Do not bypass DRM, defeat authentication, extract protected keys, or evade access controls as part of the reference application's normal preservation workflow.

The archive should remain useful when the game cannot launch at all. Lack of a working authentication service is a preservation limitation to record, not a reason to discard intact files or to claim that the game has been fully preserved.

## Suggested local container layout

```text
dead-auth-server-game.dac/
├── dac.json
├── payload/
│   ├── game-client/
│   ├── installer/
│   └── available-documentation/
└── access/
    ├── service-status-evidence.md
    └── test-results.json
```

This is illustrative, not a required structure. Preserve only files that are actually available and may be retained lawfully. Keep logs and screenshots separate from original payloads, and review them for credentials and personal information before archiving or sharing.

## Four preservation targets

These targets describe preservation outcomes, not implementation stages. Evaluate them independently; do not collapse them into a single `preserved` Boolean.

| Target | Question | Evidence to record |
|---|---|---|
| **1. Original artifact** | Are the original bytes retained and independently verifiable? | Actual payload inventory, byte-level digests, source/derivative relationships, and any known gaps. |
| **2. Execution or use environment** | Is the environment needed to access the artifact available, documented, or reconstructable? | Relevant applications, decoders, operating systems, hardware, services, dependencies, versions, configuration, licensing/access constraints, and unavailable components. For non-executable artifacts, interpret this as the relevant viewing, reading, or playback environment. |
| **3. Behavior** | Have the artifact's intended functions been tested, and what happened? | Specific tests, environment, tool/version, results, warnings, dependencies, failures, and untested functions. A launch or open operation alone does not prove all intended behavior. |
| **4. Historical experience and fidelity** | How closely does the accessible experience match the historically intended or observed experience? | Relevant comparisons and evidence for rendering, timing, sound, interaction, editability, structure, or other case-specific properties; known differences; uncertainty; and explicit limits on the assessment. Do not claim fidelity without evidence. |

For this case, distinguish retained client files from available dependencies, local/offline behavior from online behavior, and any substitute service from the original service. A failed connection is a dated observation, not by itself proof of permanent shutdown.

A target may be **unknown**, **not tested**, **partially achieved**, **blocked**, or **supported by evidence**; choose terminology that accurately reflects the evidence. These are candidate status distinctions for evaluation, not a finalized DAC schema. A useful archive may preserve the original artifact even when the environment is unavailable, behavior is untested, or historical fidelity cannot be assessed.

## Acceptance criteria

- Available local files are inventoried and hashed from actual bytes.
- Service dependencies are grounded in explicit evidence or labelled as inference.
- Service-status observations include a date, context, and evidence source where available.
- A failed authentication attempt is not automatically treated as proof of permanent service shutdown.
- Access, launchability, offline functions, online functions, and fidelity are represented as distinct results.
- Original artifacts are not silently replaced by patched clients, substitute servers, ports, or recordings.
- Replacement implementations are labelled as replacements, with their own provenance and limitations.
- Credentials and secrets are not placed in ordinary metadata.
- Rights and technical restrictions remain distinct; the case does not require or encourage access-control circumvention.
- The archive can describe useful preservation of bytes even when the game is not currently playable.

## Questions this case leaves open

- How should DAC model external services as dependencies without requiring every dependency to be preserved inside the same container?
- How should time-bounded or region-specific service-status observations be represented?
- How should account-specific entitlement be described without storing secrets or implying that an archived copy includes transferable rights?
- What evidence is sufficient to label a service discontinued rather than temporarily unreachable?
- How should a substitute service or reimplementation state protocol compatibility and behavioral differences?
- Should service dependencies and test results be represented as linked records rather than a single status field?

Do not finalize the schema from one case alone. Compare it against the remaining hard cases first.
