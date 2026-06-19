# GC-DPS — Grush Cottages Docking Port System
## Master Design Record

**Version:** v013
**Status:** LIVING
**Session:** MAJOR ARCHITECTURE PIVOT — door-frame retrofit, directional hub-and-spoke topology, disaster-relief deployment model
**Last updated:** June 2026
**Next action:** Continue design refinement with Claude — attorney contact and provisional filing explicitly DEFERRED until design is finalized (user instruction)

---

## RESUME PROTOCOL

Search Drive for highest GC-DPS_vNNN_LIVING.md — read — confirm state — begin at NEXT ACTION

---

## SUPERSESSION NOTICE — READ FIRST

**This version supersedes v001–v012 in full.** The prior architecture (single center alignment pin, bidirectional flow on all zones, wall-cutout/Hoffman-enclosure system, unlimited daisy-chain topology) is retired, not amended.

**Do not use v012-derived materials (Invention Disclosure Rev 2, CAD-002 spec, Cost/Time Assessment Rev 2) as current.** They describe a different physical system. v012's 5 independent patent claims need full attorney review against this architecture before any filing:
- Claim 5 (radial portal w/ integrated passageway) — layout changed, needs rewrite
- Claim 3 (separated dual-line plumbing zone) — waste removed from bulkhead scope entirely; this claim's subject matter no longer exists in the design and needs to be dropped or substantially reworked
- Claims 1, 2, 4 (unified patchbay, mechanical LOTO interlock, flex jumper tolerance) — concepts likely carry forward but need adaptation to the new architecture, not verbatim reuse

**No attorney contact has been made as of this version.** User has explicitly chosen to finish the design before initiating legal contact.

---

## VERSION LOG

| Version | Event |
|---------|-------|
| v001–v010 | Original patchbay architecture — setup through Session 4, all 4 zones + cost assessment complete |
| v011 | Walk-through passageway added — radial portal layout locked |
| v012 | All three documents updated for passageway — Invention Disclosure, CAD-002, Cost Assessment Rev 2 |
| v013 | **MAJOR PIVOT** — door-frame retrofit replaces wall-portal cut; directional hub-and-spoke replaces bidirectional patchbay; disaster-relief deployment model established; all 5 zones redesigned; passageway re-integrated into new architecture |

---

## INTENDED USE — LOCKED

**Primary application:** Rapid deployment into disaster zones
**Design priorities (in order):** infrastructure independence, fewest field failure points, fastest/lowest-friction deployment, logistics simplicity (minimize part variants needing to be stocked/shipped)
**Why this matters for downstream decisions:** every standalone-mode and redundancy choice in this document was made against this use case specifically, not as a general-purpose default

---

## SYSTEM ARCHITECTURE — FINAL (v013)

**Mounting:** Door-frame retrofit. Bulkhead attaches directly into the factory door-frame opening after removing the standard swinging doors, reusing existing hinge ears and locking cam retainers. Door opening envelope: 2,340mm W x 2,597mm H (ISO_Door_Open_W x ISO_Door_Open_H), both short ends of the 40HC, identical port each end.

**Topology:** Directional hub-and-spoke.
- **Utility-Supply Cottage (Utility HC):** male connectors across all four utility zones. Pin-Side structural frame.
- **Cottage (demand unit):** female connectors across all four utility zones, on BOTH ends. Pocket-Side structural frame.
- **Chain limit:** up to two Cottages per Utility HC. A Cottage's first end docks directly to a Utility HC (female-to-male, no adapter needed). Its second end either docks to a second Utility HC (if that single cottage's demand is high), or bridges to a second Cottage via a male-male Adapter (a small coupler component providing male halves of all four utility connectors, since two Cottage ends are both female).
- This is a deliberate departure from v001-v012's bidirectional/unlimited-daisy-chain model.

**Bulkhead frame variants:** TWO distinct structural frame designs — not universal:
1. **Pin-Side frame** — Utility HC. Carries the structural alignment pins.
2. **Pocket-Side frame** — Cottage. Carries the structural alignment pockets.

**Structural alignment/locking — zero power dependency (locked):**
- Diagonal 2-pin hermaphroditic-style mating geometry: pins at top-left/bottom-right of the Pin-Side frame, pockets at top-right/bottom-left of the Pocket-Side frame (note: directional per the frame variant decision above, not symmetric hermaphroditic across all units as in earlier drafts).
- Locking force is integrated directly into the pin/pocket assembly via a cam-actuated or turnbuckle-style draw-lock — spring-loaded auto-engage on insertion, deliberate manual action required to release.
- **No hydraulic system. No battery backup for locking.** This was an explicit simplification decision: the originally-considered hydraulic wedge-lock (30mm pins, >50kN holding force per REV2 draft) was rejected specifically because it introduces a powered subsystem (pump, cylinder, valves, dedicated battery) with field-failure characteristics that conflict with the disaster-deployment priority on zero power dependency and minimal failure points. Note: the >50kN figure was never validated against an actual structural load case for this application — it came from an independent AI-drafted design exercise (REV2), not a structural analysis.

**Passageway — direction-agnostic, locked:**
- 28in clear-width door, identical on both Pin-Side and Pocket-Side frames (no male/female split needed — walking through a doorway and exchanging air are inherently two-way functions, unlike the utility zones).
- Mechanically tied to the same pin-engagement linkage as the rest of the locking system: opens automatically when the structural lock engages, seals shut when undocked.
- Dual function when docked: foot traffic between structures, AND supplemental bulk HVAC airflow path (in addition to the dedicated Zone 3 ducts) — the open ~28in x ~96in doorway provides far more cross-sectional area for air exchange than the 12in ducts alone.
- Flag for later: opening a direct walkway between two separate habitable structures may have fire/smoke-separation code implications depending on how the AHJ classifies docked modular units — not yet researched.

---

## ZONE LAYOUT — FINAL (v013)

Zone numbering (locked, user-specified): **Zone 1 Electrical, Zone 2 Data, Zone 3 Ventilation, Zone 4 Plumbing.**

### Zone 1 — Electrical

- **Connector:** Amphenol RADSOK series contacts, 480V AC three-phase, 100A. RADSOK is a contact technology (hyperbolic socket grid) used across many Amphenol product lines, not one fixed product — exact part number/housing still needs to be sourced and verified for the rating and any hot-mate/load-break listing before fabrication.
- **Safety — mechanical interlock retained as hardware backstop, independent of connector choice:** disconnect switch + gate-plate interlock (gate physically blocks access to the power contacts unless disconnect is OFF, linkage rod ties the two together, gravity fail-safe). This was kept specifically because RADSOK's hot-break rating is an optional, housing-specific feature, not a property of all RADSOK products, and no specific part number had been confirmed.
- **Docked-mode power path (per cottage):** dock connector (480V 3ph) → dedicated pass-through bus (when chained to a 2nd cottage, this bus bypasses Cottage A's transformer and panel ENTIRELY — not just a separate bus within the same panel, a fully separate high-voltage circuit) → dedicated step-down transformer per cottage (480V 3ph → 120/240V single-phase) → automatic transfer switch (ATS) → 200A standard panel (120/240V) → branch circuits.
- **Standalone-mode power path:** Generator → ATS → same 200A panel. Generator chosen over solar+battery as the default — lower permitting/code friction (CEC Article 702, UL 1008 transfer switches, well-documented AHJ review path) versus battery storage's added fire-department review track, smoke/heat detection, and unit-separation requirements under CFC/Title 24. Also independently the right fit for disaster deployment specifically — fast to deploy, refuelable via tanker, not weather-dependent.
- **ATS function:** senses which source is active, switches automatically, never connects both sources to the panel simultaneously (prevents paralleling/backfeed hazard).
- **Capacity note:** REV2's 100A/480V-3ph dock connector was evaluated against a worst-case "Shop Cottage" load (table saw, air compressor, welder, dust collector, lighting, general receptacles, supplied as 200A-panel headroom) and found to have substantial margin — 100A at 480V three-phase delivers roughly 83kVA, vs. ~24kVA for a 100A/240V single-phase circuit researched in typical workshop sizing guides. Connector kept at 100A; no upsizing needed.
- **Chained-cottage capacity:** when two cottages chain off one Utility HC, the system assumes a diversity/de-rate factor — NOT sized for both cottages' full rated panel capacity simultaneously.

### Zone 2 — Data

- **Connector:** All-fiber MPO/MTP cassette — multi-fiber, blind-mate rated (with guide pins), dual redundant optical paths, single-mode backbone. NO copper conductors cross the dock interface (adopted from RBUIS's DIPA philosophy: eliminates ground-loop and lightning-surge risk between separately-grounded structures). This was chosen over bare LC fiber connectors specifically because LC is a manual push-clip design not rated for repeated blind-mate docking cycles, while MPO/MTP has industrial blind-mate variants built for exactly this application.
- **Per-cottage hardware:** local network switch with a fiber uplink (SFP into the MPO/MTP trunk), providing copper RJ45 ports locally for in-room devices.
- **Standalone-mode uplink:** satellite internet, feeding the same local switch. Chosen over cellular specifically for the disaster-relief use case — terrestrial cell/fiber infrastructure is exactly what disasters take out first; FEMA's own mobile response communications rely on satellite for this reason.

### Zone 3 — Ventilation

- **Connector:** two 12in (305mm) round duct ports — supply and return — 16-gauge 304 stainless steel. Kept at original full-capacity diameter (REV2 had proposed downsizing to 200mm/~7.9in; rejected — roughly 57% less total cross-sectional area for no clearly established benefit).
- **Auto-seal:** pure mechanical butterfly damper per port, driven by a direct cam linkage to the structural pin engagement travel — NO motor, no sensor, no power draw. (REV2's original "automated motorized... open mechanically" phrasing was self-contradictory; resolved in favor of the zero-power mechanical option, consistent with the locking-mechanism decision.)
- **Central plant model:** actual heating/cooling (AC and heat) originates at the Utility HC, delivered to cottages as already-conditioned supply air through the ducts. The return ducth path completes the loop back to the Utility HC's air handler.
- **Pass-through runs:** when conditioned air travels through Cottage A to reach a chained Cottage B, that run needs insulated duct liner/wrap — bare 16-ga stainless has no meaningful R-value on its own, and uninsulated pass-through would lose significant conditioning effect before reaching Cottage B, especially in Riverside-area summer heat.
- **Internal distribution:** ONE universal, field-adjustable manifold per cottage (not a separate fixed manifold drawing per cottage type). Manifold provides simple directable airflow distribution to internal zones — NOT independent per-zone temperature control (no motorized zone dampers, no multi-zone thermostat system). "Advanced," cottage-type-specific HVAC zoning is an explicitly separate design exercise the user will handle downstream, per cottage type (Shop Cottage, MYCOttage, etc.) — internal zone layouts for those types not yet defined.
- **Docked/standalone switchable input:** the manifold accepts either the docked duct supply OR a local fan + heat-pump/AC unit when deployed without a Utility HC — same shared manifold, switchable source, not two parallel systems.
- **Fan function:** the local fan is NOT standalone-only — it also operates as a booster in docked/pass-through mode, assisting airflow over the longer run to a chained second cottage.

### Zone 4 — Plumbing (potable water only)

- **Scope change:** waste handling REMOVED from the bulkhead entirely. Each cottage has an independent, side-mounted discharge valve to an external receptacle (holding tank, septic, etc.) — this is outside the GC-DPS CAD package scope, a separate adjacent design task.
- **Connector:** dry-break quick-disconnect fittings (CEJN/Walther/Stäubli-style; no-drip on disconnect) — adopted over the original plain Dixon cam-groove couplers.
- **Backflow protection:** Watts 009 RPZ backflow preventer RETAINED. California's cross-connection framework (enforced locally via Riverside Public Utilities Water Rule 13) treats movable/changeover connections between water systems as cross-connections requiring protection; exact required tier is unconfirmed for this novel use case. RPZ is the most protective common tier and should satisfy whatever hazard classification RPU eventually assigns. **Action flagged: consult RPU's Cross-Connection Control office (951-351-6386) once a deployment site is locked in.**
- **Pass-through topology:** when Cottage A relays potable supply to a chained Cottage B, that supply splits off UPSTREAM of Cottage A's own RPZ and shutoff — same protective logic as the electrical pass-through bus. Servicing Cottage A's plumbing never cuts off Cottage B.
- **Standalone-mode source:** water storage tank/cistern, refillable via tanker truck. Chosen over a well (requires pre-existing drilled infrastructure unavailable at an arbitrary disaster site) or municipal hookup (municipal systems are frequently among the first things compromised in a disaster) — tank/cistern resupply via tanker is the standard disaster-relief water logistics model.

---

## DOCKED / STANDALONE DUALITY — LOCKED (applies to all 4 zones)

| Zone | Docked source | Standalone source | Switching mechanism |
|------|---------------|--------------------|--------------------|
| Electrical | Utility HC via transformer | Generator | Automatic transfer switch |
| Data | Utility HC via fiber dock | Satellite | Local switch, dual uplink |
| Ventilation | Utility HC central plant via duct | Local fan + heat pump/AC | Shared manifold, switchable input |
| Plumbing | Utility HC via dock connector | Water tank/cistern | (mechanism TBD — likely valve-based source selection, not yet detailed) |

Principle established across all four: never connect both sources to the same load/panel/manifold simultaneously without an explicit switching mechanism.

---

## SUPPLY CHAIN / COMPONENT NOTES

- Several part numbers carried over from earlier drafts (REV2 and the original CAD-001 spec) have not been independently verified as real, current catalog items — flagged during this session: "Panduit CMDS6-AW3" could not be confirmed as an existing harsh-environment Panduit part. General caution: verify all part numbers before they go into a real sourcing BOM.
- v008-era "Global Standards" (Hoffman A-Series, Unistrut P1000, Roxtec RM frames, 16x20in 2x2 grid cutouts, single key pin, etc.) are SUPERSEDED by this version's architecture and should not be treated as current.

---

## OPEN ITEMS

- Plumbing standalone switching mechanism (tank vs. dock source) — concept locked, mechanical detail not yet designed.
- Cottage-type-specific internal HVAC zone layouts (Shop Cottage, MYCOttage, others) — needed before the "advanced" per-cottage ventilation design can proceed; not yet provided.
- Fire/smoke code implications of the open passageway connecting two habitable structures — not yet researched.
- RPU Cross-Connection Control consult for the RPZ/backflow tier — pending site selection.
- GC-DPS-CAD-002 drafter brief (referenced in v012) is stale and needs a full rewrite against this architecture before any drafter is engaged.

---

## IMMEDIATE NEXT ACTIONS

1. Continue design refinement (user's explicit sequencing — design finalization before attorney contact).
2. Define cottage-type-specific internal zone layouts for ventilation.
3. Design the plumbing standalone source-switching mechanism.
4. RPU consult once a deployment site is identified.
5. Full CAD package rewrite once design is finalized.
6. Attorney contact and provisional filing — DEFERRED, do not initiate until design work is complete.

---

*End of GC-DPS_v013_LIVING.md*
