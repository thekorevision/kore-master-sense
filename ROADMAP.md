# Roadmap: kore-master-sense

Phases run from the first bench build to the unlimited end state. Each phase ends with something verifiable working, not a slide.

## Phase 0: Contracts before hardware
- Freeze the Reading, Event, and device passport contracts.
- Publish the naming and materials language (filter sensors; reclaimed electronics).
- Exit test: contracts reviewed against the honesty rules in ARCHITECTURE.md; no contract allows an unlabeled proxy or an untraceable number.

## Phase 1: Bench node
- One Seed Sense and one Seed Air on the bench, core firmware library running, readings landing in a local store with full provenance.
- Exit test: 72 hours of continuous bench logging with zero silent gaps; every gap explained by a device health event.

## Phase 2: Root plus Seed pair
- Root hub aggregating both Seeds, offline buffering with signed batch attestation, edge threshold rules.
- Exit test: network pulled for 24 hours; all data arrives on reconnect, verified against attestations, flagged offline_buffered.

## Phase 3: Calibration discipline
- Calibration bench procedure, reference profiles, drift tracking in the registry.
- Exit test: two identical Seeds, one calibrated and one deliberately drifted, are distinguishable in the data by confidence and flags alone.

## Phase 4: Pipeline
- Ingest gateway, validation, time-series storage, device and calibration registries, consent-scoped read APIs.
- Exit test: a consumer query outside its consent scope returns nothing, and the attempt is logged.

## Phase 5: Filter sensor
- First community filter sensor: Seed Air class sensing fused with a reclaimed-electronics filtration build, sized to a real room on Salt Springs Place, Austell, Georgia.
- Exit test: a full season of logged operation in the community air log, runtime and loading reported honestly, unit offered free as an R and D deployment.

## Phase 6: Presence
- Seed Presence CSI pipeline: on-device inference, derived presence states only.
- Exit test: a camera-free room reports occupancy patterns for a month with no raw CSI leaving the site.

## Phase 7: Fleet and community scale
- Provisioning at volume, OTA fleet management, multi-site dashboards, public community air log publishing with traceable numbers.
- Exit test: 100 nodes across multiple sites reporting through one pipeline with per-site consent boundaries intact.

## Phase 8: End state
- The full Root and Seed lines in production, machinery, vehicle, and refrigeration variants, an open hardware program where builders can reproduce Seeds from published designs, and a calibration exchange where community sites share reference checks.
- The platform is the reference implementation other Kore repos build against.
