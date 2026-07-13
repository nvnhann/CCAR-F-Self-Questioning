# Domain 5: Context Management & Reliability — Self-Questioning List

*CCAR-F Prep — Domain 5 (15% of scored content).*

---

## A. Foundations of Context Management and Reliability

1. What does the context window actually hold — system prompt, conversation history, tool calls and results, and files read?
2. Why is a fuller window not as reliable as an empty one, even when both are technically "within limits"?

### What is the Context Window?
3. Why is the context window described as not "a simple list where every item gets equal attention"?
4. What is the "lost in the middle" effect, and which positions in the window tend to receive stronger attention?

### Context Accumulation across a Session
5. Why do tool results accumulate "disproportionately" compared to conversation text?
6. In the five-tool-call billing investigation example, why might the window end up holding thousands of tokens no longer needed for the current reasoning step?

### Token Budgets
7. Why should the context window be treated as "a budget you spend," not free space?
8. Why are the system prompt and tool definitions described as "fixed costs" that consume budget on every single turn?
9. What happens when the token budget runs low and no action is taken (clear, summarize, or offload)?

### Reliability and Context Growth
10. Why is "a fuller window is a less reliable window" described as the single most important idea in this section?
11. What is "context rot," and why does it cause the model to miss or confuse details as the window fills?
12. Why should errors in a long session be investigated as a context management problem *before* concluding the prompt or model is at fault?

### When Context Rot Begins
13. Why is context rot described as a gradual degradation rather than something that starts at one specific token count?
14. What behavior differences would you expect at 20-30% utilization versus 50-70% versus 80%+ versus 95%+?
15. Why should context management happen proactively, before the window is full, rather than reactively after problems appear?

### The Tools for Managing Context
16. What is the functional difference between context editing (clears stale results), compaction (summarizes and continues), and the memory tool (stores facts outside the window)?
17. Why are context editing and compaction usually combined with the memory tool rather than used alone?
18. In the "save durable facts before compacting" pattern, what specifically does the memory tool supply that the compacted summary might have rounded off or dropped?

### Multi-Turn Context Growth Rates
19. Why does file-heavy exploration consume context at a much higher rate than text-only conversation?
20. Given the growth-rate table (text-only, tool-light, file-heavy, search-heavy), when should each type of work be actively managed?

### Signs That Context Needs Management
21. What five symptoms indicate an overloaded context window (repetition, forgetting instructions, slower/vaguer answers, tool output dominating, contradicting earlier statements)?

### Prompt Caching and Stable Context
22. Does prompt caching reduce the size of the context window, or does it reduce cost and latency for reusing stable content?
23. Why should stable content (system prompt, reference documents) be placed first and changing content last?
24. Why is caching not a substitute for clearing or summarizing once the window actually fills?

### The Stateless API and What It Means for Reliability
25. Why is the Claude API described as stateless — what happens if conversation history is omitted from a request?
26. Why is "clearing" described as the developer's decision rather than something the model can choose to do?
27. Why is re-grounding after compaction described as "your responsibility" rather than something the model handles automatically?
28. If a compaction summary drops a critical detail, does the model know it was ever there?

### The Token Budget Analogy
29. In the financial-budget analogy, what does each element map to (system prompt as rent, tool definitions as utility bills, conversation history as discretionary spending, tool results as variable expense, memory tool as savings)?
30. Why can't simply getting "a bigger budget" (a larger context window) eliminate the need for context management?

---

## B. Preserving Critical Information Across Long Interactions

31. In a long interaction, what is the goal — keeping every detail, or keeping the facts that matter while letting go of the chatter that doesn't?

### Progressive Summarization and Its Risks
32. Why is summarizing the same conversation again and again described as "lossy"?
33. Why is a specific value like a date or identifier especially easy to lose in a summary that "keeps only the gist"?
34. Why does the risk of information loss compound with each summarization pass rather than staying constant?
35. If a specific value present early in a conversation is missing after several summarization passes, does the fix lie in better summarization instructions, or in a durable facts block?

### The "Lost in the Middle" Effect
36. Why do models attend most reliably to the start and end of a long context and least reliably to the middle?
37. Where should the most important facts and instructions be placed to avoid this effect?
38. Why does this effect get stronger as total context length increases — does position matter much in a short prompt?

### Durable Facts versus Passing History
39. What is the difference between durable facts (stable details needed for the whole task) and passing history (turn-by-turn chatter that can be trimmed)?
40. What is a "case facts block," and why does keeping it as one labeled block protect it from being lost in a summary?
41. Why would separate context layers be needed when one conversation covers several distinct issues?

### Tool Output Accumulation
42. Why are tool results described as "the fastest way to fill a window"?
43. When a tool returns a large JSON response with dozens of fields, what should happen before that result enters conversation history?
44. If only one function from a 500-line file is needed, why extract just that function rather than loading the entire file?

### Position-Aware Input Ordering
45. Why should key findings and instructions go at the very beginning or the very end of an input, with long reference material in the middle?
46. Why does the case facts block belong at the top of each new turn specifically?

### Structured Data versus Verbose Content between Agents
47. Why should an upstream agent return structured data with key facts and citations rather than long prose to a downstream agent?
48. In the research-agent example, why does a 200-token structured summary preserve more useful signal per token than a 2,000-word prose analysis?

### Retention versus Retrieval
49. What is the difference between retention (keeping information in the window) and retrieval (storing externally and fetching on demand)?
50. Why does retention risk triggering context rot, while retrieval risks depending on good recall to know when to fetch?

### Re-grounding after Compaction
51. Why should a summary be treated as "a starting point, not the full record"?
52. What specifically should be re-injected after compaction to keep durable values exact?

---

## C. Escalation and Ambiguity Resolution Patterns

53. Why is escalation described as "a reliability skill," not just a fallback — what happens if you escalate too late, or on the wrong signal?

### What Triggers an Escalation?
54. What are the three concrete triggers for escalation (explicit human request, policy gaps/stalled progress, authority limits)?
55. Why should a direct request for a human be honored immediately rather than the agent attempting to resolve the issue first?

### Immediate Escalation versus Offered Resolution
56. What is the difference in appropriate response between "a direct request for a human" and "a frustrated user who has not asked for a human"?
57. Why is delaying an immediate escalation by trying to resolve first listed as a common exam trap?
58. Why is overriding an explicit request for a human with an "offered resolution" also listed as a trap?

### The Unreliability of Sentiment and Self-Rated Confidence
59. Why is sentiment (tone of the message) an unreliable escalation trigger — can a calm message hide a hard problem, and can a frustrated one have an easy fix?
60. Why is the model's own self-rated confidence also unreliable — what does "confidently wrong" mean here?
61. Does a low confidence score necessarily mean the answer is wrong, or could it just indicate source ambiguity?
62. If a question offers sentiment analysis or self-rated confidence as the escalation trigger, why are both considered distractors?

### Ambiguity and Clarification
63. When a request has more than one reasonable reading, should the agent guess, or ask a clarifying question?
64. Why is a specific clarifying question ("I found two accounts... which one?") better than a generic one ("Could you clarify?")?
65. Why is guessing on ambiguity described as producing a "confident but possibly wrong action," worse than a brief question?

### Multi-Step Escalation Paths
66. What distinguishes Level 0 (agent handles autonomously) from Level 1, 2, and 3 escalation destinations?
67. Why should an agent route directly to the correct escalation level when possible, rather than always starting at Level 1 and letting humans re-route?

### Escalation with Context Handoff
68. What five components make up a well-structured escalation handoff (reason, case facts, steps already taken, current state, recommended next action)?
69. Why does handing a human "a warm start, not a blank slate" matter for the quality of the resolution that follows?

### Repeated Failure and Loop Detection
70. Why is an agent that retries the same failing step forever described as "a reliability failure of its own"?
71. Why does tracking attempts per step and escalating after a set number of failures avoid burning tokens and time?

---

## D. Error Propagation Across Multi-Agent Systems

72. In a multi-agent pipeline, why do errors in one agent affect every agent downstream?
73. If a data-gathering agent fails silently, what does the synthesis agent produce — and why does "nobody know" there's a hole in it?

### What a Useful Error Carries
74. What four fields does a useful error carry (category, description, retryable, source), and what does each let the coordinator do?
75. Why does a generic error like "something went wrong" tell the coordinator nothing actionable?

### Access Failure versus Valid Empty Result
76. What is the difference between an access failure (couldn't reach the source) and a valid empty result (successfully queried, found nothing)?
77. Why is "the absence of data is not meaningful" true for an access failure but not for a valid empty result?
78. If the system treats both the same way, what might a downstream agent incorrectly report ("no data exists" instead of "we could not check")?
79. For each of the four situations (API 500, API 200 with zero records, API timeout, API 200 with data), what is the correct response versus the wrong response?

### Error Categorization for Recovery Decisions
80. For each error category (`timeout`, `rate_limited`, `permission_denied`, `not_found`, `validation_error`, `service_unavailable`), what is the correct recovery strategy?
81. Why should the coordinator's recovery logic branch on the category field rather than parsing the description text?

### Anti-Patterns in Error Handling
82. What is "silent suppression," and why is it described as "the most dangerous anti-pattern"?
83. What is "over-reaction," and why does terminating an entire workflow over one recoverable error waste the progress of agents that succeeded?
84. Why does a timeout on one API call not justify destroying a report that is 90% complete?

### Local Recovery before Escalation
85. What is the correct sequence when a tool call fails — retry locally first, or escalate immediately?
86. If a source is unavailable, what should be checked before giving up (an alternative source)?
87. If local recovery fails, what should happen instead of hiding the failure or killing the workflow (annotate the specific gap)?

### Coverage Gap Annotation
88. In the billing/shipping analysis worked example, why does marking `shipping_analysis` as `"status": "incomplete"` with a reason and recommendation make the report more trustworthy than silently omitting it?

### Propagation Chains and Cascading Failures
89. In the Agent A → Agent B → Agent C cascade example, why does the system never flag an issue even though Agent A failed?
90. How does the structured error pattern "break the chain" at each handoff point?

### Partial Success and Coverage Honesty
91. Why is a report covering 4 out of 5 data sources, with honest annotation of the missing one, more useful than either a complete-looking report with hidden gaps or a total failure?
92. What is the "coverage honesty principle"?

---

## E. Context Management in Large Codebase Exploration

93. Why is exploring a large codebase described as "one of the most context-intensive tasks in production use"?
94. In the 15-file, 500-tokens-per-file example, how much raw context does the exploration consume before any reasoning is added?

### The Problem: File Reads Fill the Window
95. Why does the model start losing track of earlier findings once the window fills with old file contents?
96. Why does adding the model's reasoning about each file roughly double the token cost of a file read?

### Subagent Delegation for Verbose Reads
97. What is the difference between what the main session holds (strategy, scratchpad, high-level understanding) and what a subagent holds (full file contents)?
98. Why does only a structured summary — not raw file contents — enter the main session's context under this pattern?
99. What is the difference in main-session context cost between "direct read" and "delegated read," per the reference table?

### When to Read Directly vs. Delegate
100. For a small file early in a session, why is reading directly the right call?
101. For a large file (500+ lines), why is delegating to a subagent preferred?
102. For a targeted lookup of one function or class, why does Grep-then-read-the-section beat loading the entire file?
103. Late in a long session (window >60% full), why should delegation always be the default regardless of file size?

### The Scratchpad Pattern
104. Why does a scratchpad file need to persist to disk rather than just live in conversation history?
105. What survives a scratchpad file that conversation history alone does not (compaction, clearing, session boundaries)?
106. When starting a new phase of exploration, why re-read the scratchpad first?

### The Exploration Journal Pattern
107. What is the difference between the four sections of an exploration journal — findings, questions, hypotheses, and decisions?
108. Why does marking something as a "hypothesis" specifically prevent it from being treated as a confirmed finding?
109. In the 15-file context budget worked example, what specifically produces the ~60% context savings when comparing direct reads to subagent delegation?

### Progressive Discovery vs. Exhaustive Reading
110. Why does progressive discovery let each stage's findings guide the next stage's focus, rather than loading everything up front?
111. Why does exhaustive reading fail — what happens to the model's room to reason once 15 files are loaded before any analysis begins?
112. Why do files loaded early in an exhaustive read "fade from attention" by the time later files are read?

---

## F. Human Review Workflows and Confidence Calibration

113. What is a confidence threshold, and what does it separate (automatic acceptance vs. human review)?

### When to Route to Human Review
114. What four conditions call for human review (low confidence, novel input, high stakes, conflicting sources)?
115. Why does a "novel input" (a document format never seen before) give the system "no basis for calibration"?
116. Why should high-stakes domains (financial, medical, legal) route to human review "regardless of confidence"?

### Setting Confidence Thresholds
117. Why should the threshold be based on the cost of errors in a specific domain rather than a fixed universal number?
118. Why is the threshold described as "not permanent" — what should happen after measuring the accuracy of what passes automatically?
119. In the invoice-extraction calibration worked example, what does the 92%-above-threshold vs. 64%-below-threshold accuracy split show about whether the threshold is working?
120. If the review queue becomes overwhelmed, does that necessarily mean the threshold is wrong, or could the model genuinely be struggling with that input type?

### The Hidden Weakness Problem
121. Why can a single aggregate accuracy number (like 96%) hide a badly failing segment?
122. In the standard-invoices (99%) vs. scanned-invoices (79%) vs. handwritten-receipts (68%) example, why does the high-volume segment dominate and mask the others?

### Stratified Sampling
123. What is the difference between random sampling (measures overall accuracy) and stratified sampling (measures accuracy within each segment)?
124. Why does random sampling under-represent rare document types, hiding their true failure rate?
125. If overall accuracy looks acceptable but a specific document type is failing, is the fix a bigger sample size, or segment-level measurement?

### Review Queue Management
126. What does a review queue exceeding 25% of output volume suggest (threshold too strict, or model genuinely struggling)?
127. Why does tracking reviewer agreement (inter-rater agreement) reveal whether a task is inherently ambiguous?
128. How can a feedback loop from review outcomes reveal that a specific low-scoring pattern is actually safe to auto-accept?

---

## G. Information Provenance in Multi-Source Synthesis

129. When a system synthesizes information from multiple sources, why does "where did each fact come from" become the central question?
130. Without provenance, why is a synthesis "unverifiable," and why do conflicts become "invisible"?

### Why Provenance Matters
131. Why is "revenue grew 12% last quarter" less useful than the same claim with a specific source and page citation?
132. If a source later turns out to be wrong, what does provenance let you do (identify which claims in the synthesis are affected)?

### Claim-Source Mappings
133. What should every factual claim in a synthesis link back to?
134. If a claim is supported by multiple sources, what should happen (list all of them)?
135. If a claim isn't supported by any source, what are the two acceptable outcomes (excluded, or flagged as the model's own inference)?

### Handling Source Conflicts
136. Why should publication dates be checked first before treating a disagreement as a genuine conflict — what's the difference between an update and a conflict?
137. When two contemporaneous sources genuinely disagree, what is the correct response (present both values with sources) versus the incorrect one (silently pick one)?
138. Why is averaging two conflicting figures described as producing "a number that no source reported" — looking precise while being "entirely fabricated"?
139. Given the conflict pattern table (newer updates older, contemporaneous disagreement, authority difference, different definitions), what is the correct resolution for each, and what's the matching anti-pattern?

### The Source Authority Hierarchy
140. What is the difference between primary, secondary, and tertiary sources in terms of authority and use case?
141. Why do secondary sources risk "introducing errors through interpretation" even though they're useful?
142. Why are tertiary sources described as "furthest from the original data" despite being convenient?

### Multi-Agent Provenance Preservation
143. What is the "source laundering problem" — how does attribution get dropped as a claim passes from Agent A through the coordinator to Agent B?
144. What is the fix for source laundering — what must every inter-agent handoff preserve?
145. Why should the coordinator merge subagent outputs while preserving attributions, rather than summarizing them into prose?
146. Why should the final synthesis trace each claim back to the original source, not just to "the search agent"?

### Anti-Patterns in Provenance
147. What is "invented consensus," and why does it make a synthesis "read as if all sources agree, when they do not"?
148. What is "source laundering" specifically, as distinct from simply dropping attribution?
149. If a report shows two sources with different figures for the same metric but displays only one number with no attribution, what three things should the fix include (preserve claim-source mappings, annotate the conflict, use dates to interpret it)?

---

## Cross-Cutting Self-Questioning Prompts for Domain 5

150. For any scenario describing reliability dropping over the course of a long session — is context growth (not the prompt or model) the first thing to investigate?
151. For any scenario describing a specific value (date, ID, constraint) that was present early but missing after several summarization passes — does this call for a durable facts / case facts block rather than better summarization instructions?
152. For any scenario offering sentiment or self-rated confidence as an escalation trigger — is this a distractor, with the real trigger being a concrete condition (explicit request, policy gap, authority limit, repeated failure)?
153. For any scenario describing a downstream agent reporting "no data" when the real problem was an unreachable source — is this an access-failure-vs-valid-empty-result confusion?
154. For any scenario describing an agent silently continuing after a tool failure, producing a confident-looking but incomplete report — is this the silent suppression anti-pattern?
155. For any scenario describing an agent that loses track of earlier findings during a long codebase exploration — does the fix combine a scratchpad/journal with subagent delegation, rather than a bigger model or periodic clearing?
156. For any scenario showing a healthy aggregate accuracy number alongside a specific failing document type or category — is stratified sampling by segment the fix, rather than a larger sample size?
157. For any scenario describing two sources with different figures for the same fact — does the fix involve checking publication dates, annotating the conflict, and never averaging or silently picking one?
158. For any scenario describing a claim that loses its source as it passes between agents in a pipeline — is this the source laundering problem, and does the fix require preserving claim-source mappings at every handoff?
