# Design Write-up

## 1. AI vs. rules
Use AI to extract facts from each proposal into a fixed JSON schema: licence, WSIB, insurance, experience, price, timeline, warranties, team, safety and monitoring. AI is useful because proposal wording and layout vary. Require evidence snippets and an `unknown` value when a fact is absent. Use ordinary code for schema validation, mandatory pass/fail checks, weight validation, score calculation, sorting and audit logging. These steps must be deterministic and testable; the model should not make the final ranking decision by free-form judgement.

## 2. Consistency and explainability
Store a versioned rubric for each category, including weights, scoring bands and rules for missing evidence. Validate that weighted criteria total 100%. Calculate the final score in code: each criterion score (0–100) multiplied by its weight, summed. Save the source text/evidence, extracted JSON, model and prompt version, rubric version, criterion-level scores and reason codes. With the same extracted facts and rubric version, code produces the same result. AI extraction can vary, so use a fixed prompt/settings where available, validate output, and require review of low-confidence or conflicting fields. Show reviewers each criterion's evidence, score, weight and contribution.

## 3. Human in the loop
A reviewer confirms extracted facts and resolves uncertain or missing mandatory information before scoring is finalized. The system shows flagged proposals and evidence, not just a rank. Reviewers can correct extracted fields or override a result with a required reason. A human makes the final award decision. Log uploads, user actions, extracted values, evidence, rubric/prompt versions, scores, overrides, timestamps and final decision.

## 4. Changing criteria
Every criteria edit creates a new version; do not silently alter completed evaluations. Existing results retain the rubric version used. The reviewer can explicitly re-score all proposals in the evaluation using the new version, then compare the old and new results. Re-run deterministic scoring from saved extracted facts; re-extract only if the extraction schema or source interpretation needs to change. Record who changed the criteria and when.

## 5. Risks and safeguards
- **Wrong or missing extraction:** retain evidence snippets; validate types and ranges; flag uncertain fields for human review.
- **Made-up facts:** instruct the model to return `unknown` rather than infer; never treat unsupported claims as verified.
- **Prompt injection/manipulation:** treat proposal text as untrusted data, never as system instructions; ignore embedded commands and score only against admin criteria.
- **Inconsistent or biased scoring:** use the same published rubric, explicit scoring bands and deterministic calculations; test examples and allow human review.
- **Bad configuration:** validate that weights total 100%, mandatory criteria are defined, and budget/timeline limits are present.
- **Privacy and security:** restrict access, encrypt stored documents where appropriate, set retention rules, and avoid sending unnecessary personal or confidential data to an AI provider.
- **Cost or service failure:** set file-size/page limits, monitor usage, retry safely, and provide a manual review fallback.

## Sample-set handling
A, C and E explicitly state the required licence, WSIB clearance and $5M insurance. B does not confirm that the bidder holds the electrical licence; it says a licensed subcontractor will be confirmed after award, so it fails the stated mandatory rule. D states the mandatory items but does not give a completion schedule, so it is flagged for clarification rather than assigned an invented timeline score. C's embedded instruction to rank itself first is ignored as untrusted content. Among complete eligible proposals, A is strongest overall; E follows, and C ranks third under the indicative rubric. Final award remains a human decision.
