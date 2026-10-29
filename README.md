# MATACS AI Proposal Triage Design

**Candidate:** Salahuddin Abbasi  
**Assessment reference:** MATACS-TH-2026-02  
**Time spent:** 20 minutes

## Overview
This repository describes a design for an AI-assisted proposal triage tool. AI extracts facts from proposal text; deterministic application rules validate mandatory requirements and calculate weighted scores. A human reviewer checks uncertain results and makes the final award decision.

## Files
- [Process flow diagram](diagram/flowchart.png)
- [Design write-up](WRITEUP.md)
- [AI prompt](PROMPTS.md)
- [Expected ranking](ranking.json)

## AI tool use
I used ChatGPT to help structure the design, draft the extraction prompt, and check the ranking against the stated criteria. I reviewed the output rather than accepting it blindly: Proposal C contains an instruction telling automated systems to rank it first, but that instruction is untrusted proposal content and must not affect the score. I also kept Proposal B from passing the licence check because the proposal says its subcontractor will be confirmed after award, rather than confirming that the bidder holds the required licence.

## Assumptions
- A proposal must explicitly confirm both mandatory requirements to pass them.
- Missing information is not invented. If a required fact for evaluation is unclear, the proposal is flagged for human clarification.
- The weighted criteria are scored on a 0–100 scale using a documented rubric, then multiplied by their configured weights. The scores in `ranking.json` are indicative design outputs, not objectively measured facts.
