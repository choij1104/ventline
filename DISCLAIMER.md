# Intended Use and Limitations

**VENTLINE is a reference and decision-support aid. It is not a medical device.**

- This software has **not** been reviewed, cleared, or approved by the U.S. Food and Drug Administration or any other regulatory authority, and it is not intended to diagnose, treat, cure, or prevent any disease.
- Output is generated from a static rule base transcribed from a published protocol summary. It does not account for the full clinical picture, the patient's underlying disease, hemodynamics, ventilator model limits, or local protocol and medical direction.
- **Clinical judgment governs.** Any step produced by this tool must be verified against the current published protocol, local practice, and the treating provider's own assessment before it is acted on. The tool returns one next step; it does not manage the patient.
- Thresholds and tables are drawn from the protocol current as of the stated build date. Protocols change. Verify against the current published version.
- Items marked **LOCAL** in the interface are editable defaults that are not traceable to the cited protocol. They are not guideline content.
- The preclinical (swine) mode provides weight-based research defaults only. It is not a validated animal ventilation protocol and must be set per the approved study protocol.
- Items marked **SIM** come from the built-in simulator. The simulator uses standard physiology equations with illustrative parameters that have not been validated against patient or animal data. Simulator output, closed-loop runs, and harness scores describe the engine's behavior inside the model only and say nothing about clinical performance. The closed-loop mode exists only inside the simulator; the app never changes a real ventilator.
- Guardrails clip proposed changes to protocol and listed ventilator ranges as stated in the cited sources. They do not encode every limit of every device. Check the device's own manual.
- The application stores a session log in browser local storage on the device for personal convenience only. **It is not a medical record** and must not be used as one. It is not designed for, and must not be used to store, protected health information.

Use of this software is at the user's own risk. No warranty, express or implied, is made as to accuracy, completeness, or fitness for any particular purpose.
