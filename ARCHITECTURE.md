# Architecture: kore-master-sense

The full end-state design, derived from logic. Where a choice would change under real constraints, the constrained fallback is noted, but the design target is the unlimited end state.

## 1. System shape

Four tiers:

1. **Seed tier.** Low-cost sensor nodes. Each Seed has one primary sense, a microcontroller (ESP32 family as the reference platform), local buffering, and a signed telemetry envelope.
2. **Root tier.** Site hub. Aggregates Seeds over local radio and wired links, maintains site time, buffers through outages, runs edge rules (threshold alerts, filter runtime control), and attests batches before upload.
3. **Pipeline tier.** Cloud or self-hosted services: ingest gateway, validation, time-series storage, device registry, calibration registry, and read APIs.
4. **Consumer tier.** Kore Vision hub (kore-master-vision), the Command app, community air logs, and third-party integrations through a public API.

## 2. Device model

### Root line
- **Root**: the site hub. Local aggregation, storage, edge rules, uplink.
- **Root Mini**: single-room hub for small spaces.
- **Root Home**: household hub with the Personal Intelligence boundary built in (see kore-master-vision).
- **Root Pro**: multi-zone commercial hub, wired backhaul, rack or wall mount.

### Seed line
- **Seed Sense**: core environment (temperature, humidity, pressure, VOC index).
- **Seed Air**: particulate matter and CO2; the air quality workhorse. The community filter sensor is a Seed Air variant paired with a filtration stage sized to its room.
- **Seed Water**: leak, moisture, and water quality proxy sensing.
- **Seed Motion**: motion and vibration (machinery health proxy, occupancy support).
- **Seed Sound**: sound level envelope only. No audio recording leaves the device; levels and events, never speech.
- **Seed Presence**: WiFi CSI based presence and movement inference. Camera-free by design.
- **Seed Contact**: open and close state for doors, panels, enclosures.
- **Seed Link**: bridge node extending range and translating between local links.

### Filter sensors
A filter sensor is a Seed Air class node fused with a filtration unit built substantially from reclaimed electronics and reclaimed fan and housing stock. It senses incoming air, filters it, logs what it caught in aggregate terms (runtime, loading, air deltas), and reports events to its Root. Filter sizing is per environment: room volume, source profile, and occupancy pattern are recorded at install.

## 3. Firmware architecture

- One shared core library: telemetry envelope, buffering, time sync, provisioning, OTA, diagnostics.
- Per-product firmware is a thin composition of the core plus that product's sense drivers.
- Drivers are isolated behind a sense interface: `sample() -> Reading { value, unit, confidence, timestamp, calibration_id }`.
- Every device holds a **device passport**: identity, hardware revision, sensor set, calibration history pointer. The passport is signed at provisioning and presented on every session.
- OTA updates are signed, staged, and reversible. A failed update returns to the last known good image automatically.
- Edge rules on Root are declarative and versioned, so behavior can be audited after the fact.

## 4. Data contracts

### Reading
```
reading_id, device_id, site_id, sense, value, unit, confidence (0..1),
observed_at, received_at, calibration_id, firmware_version, flags[]
```
Flags include `proxy` (the reading is an indirect estimate), `uncalibrated`, `drift_suspect`, and `offline_buffered`. Consumers must surface flags, not hide them.

### Event
Threshold crossings, filter loading alerts, device health events. Events reference the readings that caused them.

### Batch attestation
Roots sign batches of buffered readings with device keys, so offline data can be verified as untampered when it finally uploads.

## 5. Pipeline

- **Ingest gateway**: authenticates device passports, accepts readings and batches, rejects malformed or unsigned data.
- **Validation**: range checks, unit checks, confidence sanity, duplicate suppression by reading_id.
- **Storage**: time-series store for readings; relational store for devices, sites, calibrations, and consent records.
- **Calibration registry**: every calibration event, reference instrument, and drift estimate, queryable per device and per sensor lot.
- **Read APIs**: site-scoped and consent-scoped. A query can never return data outside the consent scope of the credential that asked.

## 6. Presence via WiFi CSI

Seed Presence and Root cooperate to read channel state information from ordinary WiFi traffic. Presence, movement, and rough occupancy counts are inferred on-device. Raw CSI does not leave the site by default; only derived presence states do. This is the platform's answer for schools, churches, and senior living spaces that want awareness without cameras: a camera-free option, offered as a choice, never imposed.

## 7. Reliability and honesty rules

- Missing data is represented as missing. The pipeline never interpolates silently; any gap filling is a derived series, labeled derived.
- Confidence is computed from calibration age, sensor self-checks, and environmental plausibility, and it decays honestly over time.
- Every public number the platform produces can be traced back to readings, devices, and calibrations. If it cannot be traced, it is not published.

## 8. Security

- Per-device keys, provisioned at build or install; no shared fleet secrets.
- Signed firmware, signed batches, encrypted transport.
- Consent and household boundaries enforced at the API layer with per-person, per-point sharing grants (see kore-master-vision for the consent model this platform feeds).
