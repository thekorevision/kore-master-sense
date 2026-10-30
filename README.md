# kore-master-sense

The master repository for the Kore sensing platform: environmental sensor hardware, firmware, calibration, and the data pipeline that turns raw readings into a trustworthy record of the air and the spaces people live in.

**Owner:** Kristian Brown, founder of Kore ("Keeping Our Roots Enriched")
**Status:** Concept stage, moving into first bench prototype. Kore is unincorporated and pre-revenue. This repository is the clean master design, built from logic first. It does not replace or modify any earlier repository; it stands beside them as the reference architecture.

## The end-state vision

Designed as if resources were unlimited, the end state is a sensing platform that can cover a single room, a whole building, a fleet of vehicles, or a neighborhood, with the same parts and the same data language:

- A **Root** hub line that anchors each site, aggregates local sensors, and keeps recording through internet outages.
- A **Seed** sensor line that senses air, water, motion, sound, presence, contact, and links between nodes: Seed Sense, Seed Air, Seed Water, Seed Motion, Seed Sound, Seed Presence, Seed Contact, Seed Link.
- **Filter sensors**: custom air filter sensors built from reclaimed electronics, made to fit the environment they protect. They sense, filter, log, and report the air, and they are offered free to community members who need one during the research and development stage.
- **Presence detection** through WiFi CSI (channel state information), so a space can know it is occupied without a camera.
- A calibration and provenance discipline where every reading carries its confidence, its device history, and its calibration state. A reading without provenance is treated as a rumor, not a fact.
- A fleet pipeline that ingests, validates, stores, and serves readings to the Kore Vision hub, the Command app, and community air logs, with consent boundaries enforced in the data model itself.

## Why this repo exists

Earlier Kore work grew across several repositories under real-world constraints: limited parts, limited time, one phone for a computer. This master repo asks a different question: if the design were derived purely from logic, with unlimited resources assumed, what should the sensing platform be? The answer lives here, and the constrained builds can be measured against it.

## Architecture at a glance

See [ARCHITECTURE.md](ARCHITECTURE.md) for the full design. In short:

```
Seeds (sense) -> Root (aggregate, buffer, attest) -> Pipeline (ingest, validate, store) -> Consumers (hub, app, community logs)
```

Every layer is independently testable, every interface is a written contract, and every device can explain what it measured, when, and how sure it is.

## Build order

See [ROADMAP.md](ROADMAP.md). The short version: bench node first, then a calibrated Root plus Seed pair, then the pipeline, then fleet tooling, then community deployment hardening.

## Design principles

1. **Measurement before claims.** Record what the sensor actually measured, with confidence. Never upgrade a proxy reading into a health claim.
2. **Documentation, not diagnosis.** The platform documents environments. It does not diagnose people or places.
3. **Provenance is part of the reading.** Device id, firmware version, calibration state, and confidence travel with every data point.
4. **Offline is a normal state.** Devices buffer and attest locally; the network is a convenience, not a dependency.
5. **Consent is structural.** Personal and household data is private per person and per household, shared only by explicit opt-in, enforced in schema and API, not by policy text alone.
6. **Reclaimed before new.** Where a reclaimed component meets the spec, it is preferred. Materials language: reclaimed electronics.

## Repository layout

- `firmware/` Seed and Root firmware (ESP32 family), shared libraries, OTA and provisioning.
- `hardware/` Board designs, mechanical drawings, bills of materials, assembly instructions.
- `calibration/` Calibration procedures, reference profiles, drift tracking.
- `pipeline/` Ingestion, validation, storage, and API services.
- `products/` Per-product design files and notes for the Root and Seed lines.
- `docs/` Cross-cutting documentation: data contracts, naming, test strategy.

## License

MIT. See [LICENSE](LICENSE).
