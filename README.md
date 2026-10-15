# VENTLINE

**Offline mechanical ventilation decision support.**
Version 0.1.2 · Prototype build 2026-10-14

A single-file, browser-resident decision support tool for initiating, adjusting, troubleshooting, and weaning mechanical ventilation. It runs entirely in the browser with no backend, no external requests, and no data transmission — built for the ICU and emergency department, transport and prolonged field care, preclinical research ventilation, and bedside teaching.

---

## What it does

Five linked steps, each producing the next action with its source section:

1. **Setup** — predicted body weight, initial tidal volume (8 → 6 mL/kg PBW schedule with the 4 mL/kg floor), rate ceiling, minimum PEEP, oxygenation target. Environment selector: ICU/ED, transport/PFC, preclinical swine (actual weight), education.
2. **Adjust** — enter current settings plus plateau pressure, pH, PaCO₂, PaO₂/SpO₂ and get the next single step: PEEP/FiO₂ table position (both the lower-PEEP and higher-PEEP tables), plateau pressure rules, pH rules, rate ceiling. Out-of-goal values are flagged in a banner.
3. **ABG** — primary acid–base pattern, P/F ratio against the ARDS inclusion threshold, and the ventilator response the pH goal calls for.
4. **Alarm** — DOPES checklist for acute deterioration; peak-versus-plateau triage separating resistance from compliance problems.
5. **Wean** — daily spontaneous breathing trial screen (all five criteria), trial setup, tolerance assessment with the five distress signs, and the 30-minute / 120-minute decision points.

A device-local session log with Copy and Print carries inputs, outputs, citations, and the disclaimer.

## Source

Every engine rule cites a section of one document:

> NIH NHLBI ARDS Clinical Network. *Mechanical Ventilation Protocol Summary* (protocol card, revised 07/2008).

Content not traceable to the card is tagged **LOCAL** in the interface and in the code: swine weight-based defaults, acid–base classification, the DOPES mnemonic, and peak/plateau triage. These are editable defaults, not guideline content. Nothing is added to the engine without its primary source being read first.

## Design constraints

1. **Offline first.** One HTML file. Save it, open it, use it with the radio off.
2. **No transmission.** No analytics, no fonts from a CDN, no fetch calls. The header shows network state so the operator can verify.
3. **Deterministic.** A rule base, not a language model. Same inputs, same output, every output traceable.
4. **One step at a time.** The engine returns the next protocol step, not a plan — the clinician decides and re-measures.
5. **Legible under load.** 15 px base type, 44 px touch targets, night theme for dark environments.

## Use

Open `index.html` in any modern browser. No install, no build step, no dependencies.

Served over https (including GitHub Pages) the app registers a service worker and can be added to a phone home screen; it then opens with no network at all. The service worker caches only this app's own files.

## Repository layout

```
index.html             complete application (UI + rule engine)
manifest.webmanifest   home-screen install metadata
sw.js                  service worker — caches this app's own files only
icon-192.png           app icons
icon-512.png
README.md              this file
DISCLAIMER.md          intended use and limitations — read before clinical use
LICENSE                copyright and terms
```

## Not in v0.1

Pediatric settings, pressure-support step-down schedule, prone positioning and other rescue measures, neuromuscular blockade dosing, transport-ventilator model limits, and PFC-specific ventilation guidance. Each will be added after its primary source is read.

## Changelog

**v0.1.2** — 2026-10-14. Header network badge no longer looks like a button: a plain dot + "no network use", dot turns green when offline. No rule changed.

**v0.1.1** — 2026-10-14. Fifteen scripted user scenarios on a phone-width viewport. PEEP/FiO₂ tables scroll inside their card instead of widening the page; empty-input guards on Adjust and SBT tolerance; table-edge messages (top: no further card step; bottom: go to the weaning screen); plateau-greater-than-peak guard; log entries record inputs and step titles. No rule changed.

**v0.1.0** — 2026-10-14. First prototype: Setup, Adjust, ABG, Alarm, Wean, Log.

## Publisher

HAKOYA LLC
Selma, Texas

---

See [DISCLAIMER.md](DISCLAIMER.md) — this software is a reference aid, not a substitute for clinical judgment, and is not an FDA-cleared medical device.
