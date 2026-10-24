# VENTLINE

**Offline mechanical ventilation decision support for paramedics and critical care transport — and a research platform for closed-loop ventilator control.**
Version 0.3.1 · Build 2026-10-24

A single-file, browser-resident decision engine for starting, adjusting, troubleshooting, and weaning mechanical ventilation. No backend, no external requests, no data transmission. Primary user: paramedics who run transport ventilators in ground, air, and prolonged field care.

---

## Engine

```
patient state  ->  rules (modules)  ->  guardrails  ->  one next step  ->  timeline
```

- **Modules** register rules. v0.2 ships three: `SAFE` (always on), `CPG92`, `ARDS`. New modules (hemodynamics, sedation) plug into the same registry without changing the core.
- **Every rule** has an ID (e.g. `CPG-ETCO2`, `ARDS-PPLAT`) and a source section shown in the UI and written to the log.
- **Guardrails** clip any proposed change to the tighter of the protocol limit and the selected ventilator's range, and say so. Example: ARDSnet pH rule asks for rate 35; a CPG 92 transport vent ranges 10–30, so the step stops at 30 and is flagged.
- **One next step** is shown with old → new values and Apply / Skip. Every check, applied change and skip is logged with the rule IDs that fired.
- **CO₂ settling hold** — after a rate or VT change, ventilation rules wait 15 minutes (LOCAL) unless the situation is critical. Found by the simulator: without it, corrections stack and PaCO₂ overshoots.
- **PaCO₂–EtCO₂ gap** — once a blood gas has both values, later EtCO₂ readings are read against that gap. Found by the simulator: in high dead space or low perfusion EtCO₂ can read normal while PaCO₂ is ~70.

## Screens

| Screen | What it does |
|---|---|
| Checklist | Annunciator lights (OXY, CO₂, PRESS, CIRC, DEVICE), vent status strip, readings in, electronic checklist out (DO IT / CHECKED / SKIP per item), all findings, manual settings, PEEP/FiO₂ tables |
| Setup | Height/sex (or swine actual weight), protocol (CPG 92 or ARDSnet), ventilator (Impact 754, Zoll EMV+ 731, Hamilton-T1, SAVe II, Ventway Sparrow, generic, ICU), conditions; initial settings |
| Non-normal | Airway compromise, DOPE, high- and low-pressure alarm lists, peak/plateau split, hypoxia steps, EtCO₂ work-up |
| ABG | Pattern, P/F, metabolic caution, ARDSnet response; send to Now |
| Wean | ARDSnet daily SBT screen and tolerance |
| Tools | Minute ventilation, Ti/Te, driving pressure, compliance, S/F, P/F, PaCO₂–EtCO₂, alarm limits, O₂ cylinder duration, VT table |
| Simulator | Single-compartment lung model, 7 scenarios, injectable events, closed-loop mode, trend charts, scenario harness with JSON/CSV export |
| Timeline | Trends and the full audit log; JSON/CSV export |

## Sources

- **[CPG92]** Joint Trauma System. *Mechanical Ventilation Basics.* CPG ID 92. 09 Apr 2025 (supersedes 27 Dec 2021). Initial settings, troubleshooting, O₂ requirement, Appendices D–H.
- **[ARDSnet]** NIH NHLBI ARDS Clinical Network. *Mechanical Ventilation Protocol Summary* (protocol card, rev. 07/2008).

Where they disagree, the app uses the selected protocol and shows both: body weight (CPG 92: 2.2 kg/in, female base 45 kg; ARDSnet: 2.3 kg/in, female 45.5 kg), rate ceiling (30 vs 35), PEEP ceiling (20 vs 24).

Tags: **LOCAL** = standard physiology or an editable default, not guideline content. **SIM** = simulator assumption, illustrative only. Nothing enters the engine before its primary source is read.

## Simulator (research use)

Equation of motion (Pplat, Ppeak, auto-PEEP from R·C and expiratory time), alveolar gas equation, CO₂ steady state from alveolar ventilation, shunt mixing with PEEP recruitment, Severinghaus saturation, Henderson–Hasselbalch, a simple PEEP–cardiac output effect. Scenarios: healthy lungs, moderate and severe ARDS, severe asthma, hemorrhagic shock, metabolic acidosis, smoke inhalation. Events: mainstem, tension pneumothorax, tube obstruction, bronchospasm, disconnect, bleeding.

All parameters are illustrative assumptions, not validated against patient or animal data. Harness numbers describe engine behavior inside the model, not clinical performance.

## Use

Open `index.html` in any modern browser. Served over https (GitHub Pages) it installs to a phone home screen and then opens with no network.

## Not in v0.2

Pediatric settings; sedation and paralytic dosing (CPG 92 refers to the JTS PFC Analgesia and Sedation CPG); prone positioning; ECMO; pressure-support step-down; hemodynamic module. Each waits for its primary source.

## Changelog

**v0.3.1** — 2026-10-24. Demo patient (simulated blast + smoke inhalation case, labeled DEMO) loads from the checklist screen. 20-patient virtual cohort (seeded, physiology varied, 30 % random emergencies) with summary and JSON export. Engine fixes found by the cohort: SAFE-REFRACT escalation when FiO₂ and PEEP are at the ceiling and the patient is still hypoxemic; no rate increase while breaths are stacking (CPG 92 air trapping) — lengthen exhalation instead.

**v0.3.0** — 2026-10-24. Full redesign as a flight-deck electronic checklist. Each check produces a checklist: warning/caution items first, the active item shows the change (old → new) with DO IT or CHECKED and SKIP, completed items turn green, items made stale by an applied change are marked RECHECK, normal parameters appear as SENSED. Annunciator lights by system, vent status strip, bottom page keys, dark-first with a day mode. DOPE and low-SpO₂ lists are tap-to-check. No rule changed.

**v0.2.0** — 2026-10-24. Research-platform rebuild. Modular rule engine with rule IDs, guardrails (protocol ∩ device), JTS CPG 92 content and five transport-ventilator profiles, patient timeline with trends, single-compartment simulator with closed-loop mode and scenario harness, JSON/CSV export, PaCO₂–EtCO₂ gap tracking, CO₂ settling hold, off-table PEEP/FiO₂ stepping one level at a time. All v0.1 functions kept (setup, PEEP/FiO₂ tables, ABG, alarm triage, SBT screen and tolerance, log). v0.1.2 preserved on branch `backup/v0.1.2-pre-v0.2`.

**v0.1.2** — 2026-10-14. Header network badge as a status dot. No rule changed.

**v0.1.1** — 2026-10-14. Fifteen scripted phone-width scenarios; table scrolling, input guards, table-edge messages, plateau > peak guard.

**v0.1.0** — 2026-10-14. First prototype.

## Publisher

HAKOYA LLC · Auravyx Systems
Selma, Texas

---

See [DISCLAIMER.md](DISCLAIMER.md) — research and decision-support aid, not a substitute for clinical judgment, not an FDA-cleared medical device.
