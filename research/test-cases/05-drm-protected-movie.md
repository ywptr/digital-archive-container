# Test Case 05: DRM-protected movie

**Status:** Exploratory fixture for breaking DAC Core  
**Normative:** No. This case does not define the final DAC schema.

## Scenario

A depositor purchased or licensed a movie delivered in a format or application that uses technical access controls. The file may be encrypted, tied to an account, restricted to approved devices, dependent on a playback application, or subject to online authorization. The depositor wants to preserve what they lawfully possess and record what is required to access it in the future.

This case does not assume that the depositor has a standalone movie file. The accessible asset might instead be an application-managed download, a set of encrypted media segments, or content available only through an authorized service. Do not bypass DRM, extract keys, or defeat access controls for this test. Record the actual preservation boundary and the lawful access path.

## What this case tests

1. **Preservation boundary:** Can DAC describe exactly which bytes and related files are actually available to the depositor?
2. **Technical restrictions:** Can the archive record observed encryption, authorization, device binding, or other access dependencies without attempting to defeat them?
3. **Rights and access:** Can DAC distinguish a purchase/license claim from possession of decryptable media or continuing authorization?
4. **Dependency capture:** Can the application, codec, device, account, network, or service dependencies be recorded as requirements without storing credentials or secrets?
5. **Service dependency risk:** Can the archive record what happens if a vendor shuts down, an account is closed, or authorization becomes unavailable?
6. **Evidence and provenance:** Can receipts, license terms, product pages, and access observations be recorded separately with their source and capture status?
7. **Honest preservation status:** Can DAC say that bytes are preserved while the viewing experience remains inaccessible or unverified?

## Format-level expectations

There is no single universal “DRM movie” format. A protected movie may use a standard media container and codec alongside a separate digital rights management system, encrypted segments, a license server, device-level protection, or proprietary application state.

The reference application should identify only what it can reliably detect. It may be possible to identify a media container or codec while the payload remains encrypted or inaccessible to a general-purpose parser. Failure to inspect the audiovisual stream must not be misreported as proof that the stream is corrupt.

Technical access restrictions and legal permissions are separate dimensions. The absence of detected DRM does not imply permission to redistribute; the presence of DRM does not by itself describe the full legal scope of a purchase or license.

## Evidence and access model

Keep at least these concepts separate:

- **Acquisition assertion:** what the depositor says they purchased or licensed.
- **Acquisition evidence:** receipt, order record, or other retained evidence, if available.
- **Rights/contract evidence:** captured license terms or service terms, with source and date/context where known.
- **Preserved payload:** the actual files or bytes available to the depositor, including whether they are encrypted or incomplete.
- **Access requirements:** approved application, account, device, network, license server, subscription, or other dependencies.
- **Access test:** what was tried, when, using which authorized environment, and what happened.
- **Preservation assessment:** whether bytes, metadata, dependencies, and the user experience are preserved, incomplete, or unknown.

Never put passwords, access tokens, decryption keys, or other secrets into the public DAC manifest. If an account or credential is needed, record the dependency and a safe reference to the user's separately managed credential process—not the credential itself.

Do not infer that a movie can be archived in a fully usable state merely because a device currently plays it. Do not infer that preserving an encrypted payload preserves the ability to decrypt it in the future.

## Three-level implementation plan

### Level 1 — Identify and inventory
- Inventory only the files and metadata the depositor can lawfully access and preserve.
- Record sizes and compute digests from actual bytes.
- Identify known containers or formats where possible; report ambiguity or inaccessible content explicitly.
- Preserve the available bytes unchanged and do not bypass access controls.

### Level 2 — Extract and describe
- Extract available container metadata and technical properties where the content is accessible to supported parsers.
- Record encryption and authorization observations only within the implementation's capabilities.
- Describe access dependencies and evidence sources without including secrets.
- Separate file metadata, depositor claims, contractual evidence, and access observations.

### Level 3 — Validate the usable experience
- Test playback only through an authorized path using the required application/device/account.
- Record test date, application/device context, network or authorization dependency, outcome, and limitations.
- If playback cannot be tested, say so; do not imply that the movie is corrupt or usable.
- Record whether access depends on a live service and what has not been tested, without attempting circumvention.

Level 3 is optional for byte preservation. A DAC archive may preserve a partial or encrypted payload while accurately reporting that future playback is not assured.

## Suggested local container layout

```text
protected-movie.dac/
├── dac.json
└── payload/
    ├── available-content-files/
    └── evidence/
        ├── purchase-record.pdf
        └── terms-capture.pdf
```

This is illustrative only. Some platforms do not expose a stable, standalone content file; the archive must not invent one. Evidence files are optional and should not expose personal/account data in a public fixture.

## Acceptance criteria

- DAC records only bytes and metadata actually available; it does not imply a standalone movie file exists when it does not.
- Available files receive independent integrity records based on actual bytes.
- Encryption, access authorization, rights, and current playback success are represented as separate facts.
- The application does not attempt to bypass DRM, extract protected keys, or defeat access controls.
- Access dependencies can be documented without embedding credentials or secrets.
- Purchase/license claims and captured evidence are distinguished.
- A preserved-but-unplayable or not-yet-tested state is expressible without labeling the payload corrupt.
- The archive makes no promise that vendor services, accounts, licenses, or devices will remain available.
- Unknown rights or inaccessible playback do not prevent honest documentation of the bytes that can be preserved.

## Questions this case leaves open

- How should DAC represent a protected asset when the vendor application exposes no stable file path or export?
- What is the right level of detail for documenting dependencies without retaining sensitive account information?
- How should the manifest represent separate status values for byte preservation, metadata extraction, and experience availability?
- How can DAC record an access test that was successful once but depends on a service that may later disappear?
- How should a depositor distinguish legally retained evidence from content they are not permitted to copy into the archive?

Do not finalize the schema from this case alone. Compare it against the remaining hard cases first.
