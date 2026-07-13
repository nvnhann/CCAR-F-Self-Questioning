# Domain 4: Prompt Engineering & Structured Output — Practice-Based Self-Questioning

---

**1. Extract from 300+ page manuals with cross-references**
- Self-question: When a document exceeds the window, does splitting into overlapping chunks + independent extraction + a reconciliation step (resolving cross-references) beat truncating, raising max_tokens, or summarizing first?
- Key distinction: overlapping chunks + reconciliation vs. truncation vs. max_tokens (doesn't grow the window) vs. summarize-then-extract (loses detail) — which handles content larger than the window without dropping cross-referenced values?

**2. Line items don't sum to grand total (18%)**
- Self-question: Does a `calculated_total` field (model re-sums) alongside `stated_total`, flagging mismatches for review, beat proportional auto-adjustment, few-shot, or a separate reconciliation model?
- Key distinction: self-check field surfacing arithmetic mismatch vs. silently adjusting values vs. few-shot "be consistent" vs. a second validation model — which makes the inconsistency *visible* rather than hiding or guessing?

**3. OCR mangles scanned invoice layouts**
- Self-question: Does passing images directly to Claude as vision input (routing illegible ones to review) beat multi-OCR reconciliation, few-shot, or image sharpening?
- Key distinction: vision input (original layout) vs. three OCR engines vs. few-shot to compensate for noise vs. preprocessing filters — which removes the OCR-mangling step entirely?

**4. 50K docs/night, same 4K system prompt, cost rising**
- Self-question: Does Batch API (discounted async) + prompt caching (shared system prompt not reprocessed at full cost) reduce cost without hurting quality — rather than shortening the prompt, a smaller model, or concatenating docs?
- Key distinction: Batch + caching vs. removing schema/examples vs. smaller model (quality risk) vs. concatenating 20 docs (mixes extractions) — which cuts cost while preserving quality?

**5. Team eyeballs a few outputs; changes silently degrade other types**
- Self-question: Before iterating further, should you establish a golden evaluation set with verified expected outputs, scored automatically field-by-field?
- Key distinction: golden eval set + automatic scoring vs. production A/B testing vs. two-engineer review vs. longer manual reviews — which catches silent cross-type regressions systematically?

**6. Every value must be traceable to exact source text**
- Self-question: Should the model output each value alongside a verbatim source quote and location, then programmatically verify the quote appears in the document?
- Key distinction: verbatim quote + location + programmatic verification vs. logging full responses vs. double-extraction disagreement vs. confidence scores — which lets auditors verify without re-reading?

**7. payment_terms must map to 5 standard codes from varied language**
- Self-question: Does constraining the field to an enum of the five codes (with descriptions) plus a free-text field for unusual terms fix inconsistency — rather than a post-processing lookup table, a prompt instruction, or raw-then-downstream-normalize?
- Key distinction: enum + "other" free-text vs. exhaustive lookup table vs. "standardize when possible" prompt vs. raw extraction — which enforces consistency at the source while handling the unexpected?

**8. Calendar-invite JSON must strictly conform; downstream rejects malformed**
- Self-question: Does defining a tool with the target schema and having Claude call it provide the most reliable conformance — rather than prefilling a brace, prompt instructions + parse, or "output only JSON" + retry?
- Key distinction: tool + schema (structural guarantee) vs. response prefill vs. prompt instructions + parse vs. "only valid JSON" + retry — which guarantees the *shape* rather than hoping for it?

**9. Selecting which extractions to route for human review (limited capacity)**
- Self-question: Should routing be based on low model confidence or ambiguous/contradictory source documents — rather than fixed entity types, random sampling, or downstream-reported failures only?
- Key distinction: confidence/ambiguity-based routing vs. entity-type routing vs. random sampling vs. reactive downstream failures — which directs scarce review at the most error-prone cases?

**10. 12% semantic errors pass schema validation; review only 20%**
- Self-question: Does having the model output field-level confidence, then calibrating review thresholds on a labeled set, best direct limited attention — rather than flagging formatting anomalies, random sampling, or empty-field review?
- Key distinction: calibrated confidence thresholds vs. formatting-anomaly heuristics vs. random 20% vs. empty-field prioritization — which targets *semantic* errors that pass schema checks?

**11. tool_choice "auto" sometimes returns text, not a tool call**
- Self-question: When document type is unknown in advance, does `tool_choice: "any"` (with all extraction tools defined) guarantee a tool call — rather than a classification call first, a unified schema, or "auto" + prompt instructions?
- Key distinction: `"any"` (must call one of several) vs. classify-then-force-specific vs. consolidate to one tool vs. `"auto"` + instructions — which guarantees structured output when the type isn't known yet?

**12. Resume extraction must strictly conform to a predefined schema**
- Self-question: Does defining a tool whose input schema matches the required structure, extracting from the tool_use response, provide the most reliable conformance — rather than prompt instructions, two-call text-then-format, or regex parsing?
- Key distinction: tool + input schema vs. prompt + template vs. two-call formatting vs. regex extraction — which reliably enforces required fields and types?

**13. Same spec differs across sections; detailed table 90% more accurate**
- Self-question: Does an array field capturing all values with source locations (letting downstream apply precedence) fit better than a "prefer the table" instruction, rejecting conflicts, or a conflict flag?
- Key distinction: multi-value array + source location vs. "prefer detailed table" prompt vs. schema rejection of conflicts vs. conflict_detected flag — which preserves the discrepancy for downstream precedence rather than picking prematurely?

**14. Occasional invalid JSON from parse-the-text approach**
- Self-question: Does defining a tool with a JSON schema and using tool_use to constrain output beat retry loops, formatting instructions, or regex extraction?
- Key distinction: tool_use schema constraint vs. retry-on-parse-error vs. formatting instructions + examples vs. regex extraction — which prevents malformed JSON rather than recovering from it?

**15. Model fabricates plausible values for required fields when data is missing**
- Self-question: Does changing may-not-exist fields from required to optional (letting the model omit them) address fabrication better than an "only extract stated info" instruction, a confidence field, or semantic validation?
- Key distinction: required→optional/nullable (removes the forcing function) vs. prompt instruction vs. confidence field vs. semantic validation — which removes the *pressure* to invent a value?

**16. Agent follows first 3 of 10 rules, ignores rules 7-10 in long chats**
- Self-question: Does splitting rules into a short, prioritized set near the user turn (moving less-critical guidance to a reference section) beat stronger language, placing critical rules at the end, or repeating all ten twice?
- Key distinction: reduce competing instructions + prioritize placement vs. MUST/ALWAYS language vs. end-placement alone vs. duplicate all ten — which reduces the number of instructions the model must track at once?

**17. Content moderation: 5 categories + severity, hedging breaks parsing**
- Self-question: Does a tool with an enum-constrained category and integer severity (letting Claude reason in text before calling it) give machine-parsable output while preserving reasoning — rather than "don't hedge," post-processing strips, or lower temperature?
- Key distinction: tool with enum + integer fields vs. anti-hedging instruction vs. string-strip post-processing vs. temperature — which guarantees parsable output without losing reasoning quality?

**18. "Think step by step" vs. extended thinking**
- Self-question: Is the deciding distinction that extended thinking suits deep reasoning where the process needn't be shown, while step-by-step prompting suits moderate tasks where brief inline reasoning helps?
- Key distinction: task-depth + visibility distinction vs. "step-by-step is obsolete" vs. "step-by-step only for math" vs. "identical, choose by cost" — what actually differentiates the two techniques?

**19. Legal disclaimers must follow a fixed paragraph structure every time**
- Self-question: Do a small number of complete input/output examples demonstrating the exact structure (plus a description of each section) enforce structural consistency best — rather than an abstract description, self-critique, or more general guidance?
- Key distinction: few-shot complete examples vs. abstract one-time description vs. post-generation self-critique vs. more length — why do examples enforce structure better than descriptions?

**20. Avoid unverifiable superlative claims; "don't" reduces but doesn't eliminate**
- Self-question: Does pairing the negative instruction with a positive alternative and a concrete rewritten example improve compliance — rather than removing the instruction, repeating it three times, or picking the shorter of two generations?
- Key distinction: negative + positive alternative + example vs. removing it vs. triple repetition vs. shorter-of-two — why does showing what *to* do outperform only saying what not to do?

**21. skills:string[] inconsistency (compound splitting, implied skills, length)**
- Self-question: Do few-shot examples demonstrating compound handling, explicit-mention criteria, and entry granularity fix these judgment issues — rather than hard constraints, metadata enrichment, or post-normalization?
- Key distinction: few-shot for judgment/format vs. "extract 10-20 max" constraints vs. schema enrichment vs. taxonomy normalization — which teaches the nuanced judgment the failures require?

**22. Contracts with amendments; model picks one value inconsistently**
- Self-question: Does redesigning the schema so amended fields capture multiple values with source location and effective date beat "extract the newest" instructions, pattern-matching review, or preprocessing removal?
- Key distinction: multi-value schema + effective date vs. "always newest" prompt vs. post-hoc pattern matching vs. preprocessing removal — which represents the amendment relationship rather than forcing a single value?

**23. Menu extraction with inconsistent price/dietary formatting**
- Self-question: Does a strict schema + format normalization rules in the prompt fix inconsistency — rather than per-field calls, multiple attempts + vote, or extract-then-normalize in code?
- Key distinction: schema + normalization rules (fix at source) vs. per-field calls vs. multi-attempt voting vs. post-processing code — which standardizes values as they're extracted?

**24. Automating high-confidence (≥90%, 97% overall) extractions — critical validation?**
- Self-question: Before automating, is the most critical step analyzing accuracy *by document type and field* to confirm consistency across segments — not just the aggregate?
- Key distinction: stratified segment analysis vs. a 25% pilot vs. verifying 97% meets requirements vs. comparing thresholds — why does the aggregate hide segment-level failures?

**25. Retry-with-error-feedback — where would extra retries be LEAST effective?**
- Self-question: For which failure is the data simply *absent from the input* (co-authors that exist only in an external document), making it unresolvable — versus formatting errors that retries can fix?
- Key distinction: unresolvable (missing source data) vs. resolvable formatting errors (locale string, nested-vs-flat, datetime-vs-date) — which failure no retry can fix because the information was never present?

**26. Nullable fields, but model invents plausible values for unmentioned fields**
- Self-question: Does instructing the model to return null for any field not directly stated reduce false extractions best — rather than making fields required, upgrading the model, or a second-LLM verification pass?
- Key distinction: explicit "return null if not stated" vs. required + strict (forces fabrication) vs. bigger model vs. second-LLM verification — which removes the incentive to fill nullable fields with guesses?

**27. 12% of high-confidence (≥85%) extractions still wrong; need sustainable measurement**
- Self-question: Does stratified random sampling (fixed % of high-confidence extractions weekly) enable both error-rate measurement over time and novel-pattern detection — rather than heuristic table/appendix flagging, a re-extraction verification pass, or lowering the threshold?
- Key distinction: stratified sampling (measurable, sustainable) vs. heuristic flagging vs. double-extraction verification vs. threshold lowering — which lets you *measure whether improvements work* over time?

**28. Recurring correction pattern: informal measurements ("a handful")**
- Self-question: Do few-shot examples showing correct handling of informal measurements (extract verbatim, not invented or omitted) address the 23% pattern — rather than pattern-matching post-processing, a measurement_type enum, or fine-tuning?
- Key distinction: targeted few-shot from the correction data vs. post-processing detection vs. enum field vs. fine-tuning on 847 corrections — which uses the feedback to teach the specific behavior most directly?

**29. Pydantic errors like "expected float, got '2 to 3'"; identical on retry**
- Self-question: Does a follow-up request including the specific validation error (asking the model to correct) recover these best — rather than preprocessing formats, a larger-model pipeline, or temperature 0?
- Key distinction: retry-with-specific-error-feedback vs. source preprocessing vs. larger-model reprocessing vs. temperature 0 — which targets the resolvable formatting error with the precise information needed to fix it?
