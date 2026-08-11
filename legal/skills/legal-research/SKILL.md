---
name: legal-research
description: >-
  Research what the law actually says, in primary sources. Use for any question that
  turns on a statute, regulation, provision, procedure, or court decision: what a rule
  requires, whether something is lawful, which deadline applies, how a court has ruled,
  what a contract clause is worth, or how the same question is answered in two
  jurisdictions. Covers Austrian, German, EU, Schengen cross-border, US federal and US
  state law, including regional codes. Identify the jurisdiction, then use the `legal`
  MCP server to ground every statement in the provision text it returns. For a patient
  fighting a healthcare decision, use the patient-rights-advocate skill instead.
---

# Legal research

Answer legal questions from primary law, not from recollection. The value here is that
every sentence you produce can be traced to a provision or a decision that the tools
returned, quoted accurately, and cited precisely enough for someone to check.

## Connect the `legal` MCP server, then sign in

This skill is backed by the **`legal`** MCP server (legal.maxhealth.tech). Connect it and
sign in on first use. On connection it advertises which jurisdictions it currently covers
and how to research them, and that list adapts to the user's plan. Follow what it says
rather than assuming coverage.

## The one rule that matters most

**Never state a legal rule, threshold, deadline, or holding from memory.** Statutes are
amended, numbering shifts, and a plausible-sounding section number is worse than no answer
because it looks checkable. Every assertion must come from text the tools returned, and
you cite the exact provision (`Art 15 GDPR`, `§ 630g BGB`, `§ 1295 ABGB`, `42 U.S.C.
§ 1395`). If the tools do not support a claim, say the sources do not show it. If two
sources conflict, show both and say which governs and why.

## Establish jurisdiction before researching

The same question has different answers in different legal systems, and often at
different levels within one. Pin down which law governs, then route to the **most specific
jurisdiction the server advertises**: a Bavarian building question belongs in Bavarian
state law with German federal law behind it, a Tyrolean one in Tyrolean law with Austrian
federal law behind it. Where the governing law is genuinely unsettled, say so and research
the candidates rather than picking one silently.

## Use the tool that matches the question

- A question in words, or you do not yet know the provision: **search**.
- You know the citation and need the operative text: **provision**.
- Whether courts have decided it, and how: **case_law**, then **get_case** for the full
  decision.
- How something is actually done, step by step: **procedure**.
- The same question across two jurisdictions: **compare**, rather than two searches you
  reconcile by hand.

Chain them. A search that names a provision is a lead to fetch the provision, and a
provision worth relying on is worth checking for case law that narrows it.

## Answer in a form the reader can act on and verify

Lead with the answer, then the provision text that produces it, then the citation. Quote
the operative words rather than paraphrasing them away. Where a rule has conditions,
enumerate them, because a condition the reader fails is the part that decides their case.
Where a deadline exists, state it and where it comes from.

Say plainly when something turns on facts you do not have, when it needs a licensed
professional in that jurisdiction, and when the sources simply do not answer it. Hand over
everything you found either way. Research is not advice, and being clear about the line
is part of doing this well.
