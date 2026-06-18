# Principles Working Together

*Vol 2 · Precision Agents*

---

The individual principle chapters cover each architectural pattern in isolation. This chapter covers the rules that apply across principles — decisions and failure modes that arise specifically from how code-first design, modular architecture, progressive loading, validation, context hygiene, and the flywheel interact as a system.

---

## Do use structured outputs throughout the pipeline

Ask the LLM to return JSON or a schema-defined response at every step, not just the final one. Structured outputs at intermediate steps — entity extraction, intent classification, confidence scoring — enable code-side validation at each stage, catching failures early when they're cheapest to handle. This is the mechanism that connects progressive loading to the flywheel: without structured outputs, you can't measure pass rates or label escalation root causes.

---

## Do version your skills alongside your code

Skills are behavioral code. A change to a skill can break a workflow just as surely as a change to a function. Use version control for skill files, deploy skill changes through the same review process as code changes, and test skill activation accuracy before and after every change. The flywheel cannot measure what isn't version-controlled.

---

## Don't write skills that cover multiple independent domains

A skill covering both "billing inquiries" and "technical troubleshooting" is two skills jammed into one file. When the billing sub-domain activates this skill for a billing query, the troubleshooting instructions are irrelevant context — tokens paid with no benefit, plus potential for confusion. Split skills aggressively. The cost of too many narrow skills is slightly higher routing complexity. The cost of too-broad skills is lower accuracy on every query in every affected domain, invisible in aggregate metrics until you measure per-skill pass rates.

---

## Don't conflate escalation with retries

A retry sends the same prompt to the same model. An escalation sends a more specific prompt with additional context. Retries fix transient failures (network timeouts, model glitches). Escalations fix structural failures (insufficient context). Using retries to fix structural failures produces inconsistent results and obscures the root cause. Build them as separate mechanisms with separate triggers — they are not interchangeable, and conflating them breaks the flywheel's ability to measure what's actually failing.

---

## Don't make escalation invisible to the caller

If a query escalates to Pass 3 and takes 10 seconds and 50,000 tokens, the caller should know. Log it, surface it in your monitoring, and optionally expose it in the response metadata. Invisible escalations are silent cost and latency spikes that are impossible to optimize without visibility. The flywheel only turns on labeled data; unlabeled escalations are wasted signal.

---

## Don't skip measurement because the system seems to be working

A system that "seems to be working" has an unknown single-pass success rate. Without measurement, you don't know whether you're handling 80% of queries on the first pass or 40%. You can't tell whether a skill change improved or degraded accuracy. The flywheel doesn't turn without measurement. Instrument from the start — not as a future improvement, but as the prerequisite for improvement.

---

*→ Next: [Common Misconceptions](09-misconceptions.md)*
*← Previous: [Writing Precision Skills & Reference Architecture](07-precision-skills-and-architecture.md)*
