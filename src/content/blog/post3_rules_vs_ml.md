---
title: "Rules Are the Bones, Models Are the Muscle"
description: "This is the third post in a short series on evaluating fraud detection. The first covered the lightweight evaluation you run during an active campaign. The second covered the heavy evaluation that gates a model release. This one steps back to a question that comes up every time someone reviews a detection system for the first time."
pubDate: 2026-05-31
tags: ["fraud", "eval"]
---

This is the third post in a short series on evaluating fraud detection. The first covered the lightweight evaluation you run during an active campaign. The second covered the heavy evaluation that gates a model release. This one steps back to a question that comes up every time someone reviews a detection system for the first time.

If you can define what good and bad look like, why do you need a model at all? Why not just write the rules?

It is a fair question, and the honest answer is more interesting than a flat defense of machine learning. The answer is that you need both, that the boundary between them is a design decision worth making deliberately, and that machine learning earns its place for a reason that is often misstated. It does not earn its place by being better than a good rule at any single decision. It earns its place by adapting faster than a rule-engineering process can.

I will use the same hypothetical surface as the rest of the series: detecting lookalike-domain impersonation on an enterprise collaboration platform, where an attacker registers a domain that closely resembles a trusted partner and sends a message designed to be mistaken for a legitimate one. The examples are composite and the patterns are drawn from the public threat landscape, not from any particular system.

## Where rules are correct

Start with the part that is uncontroversial. For the deterministic components of the problem, rules are the right tool, and reaching for a model would be over-engineering.

Checking whether a sender domain exactly matches an allowlisted domain is a rule. Checking whether it appears on a blocklist is a rule. Counting how many connection requests a sender has had accepted in a given window is a rule. Computing the raw edit distance between two strings is a rule. None of these require learning anything. They are deterministic functions, and the right place for them is a preprocessing layer that runs before any model sees the message.

In fact, the entire population definition for the detector, the filter that decides which messages are even in scope for evaluation, should be implemented as rules. Rules are fast, transparent, and trivially auditable. When the answer is deterministic, a model adds cost and opacity for no benefit.

The disagreement is never really about these parts. It is about the parts that require judgment.

## Where rules fail

Here are five concrete patterns where a rule-based system either fails outright or becomes impractical to maintain. Each one is a place where machine learning earns its keep.

### Combinatorial similarity

A rule for visual similarity might flag any domain that substitutes a character from a known confusable set: a Cyrillic letter for a Latin one, the pair r-n for an m, a zero for the letter o. That rule catches a handful of obvious cases. It does not catch the domain that appends a plausible corporate suffix, or inserts a hyphen, or adds an industry word, or pluralizes a term. Each of these is visually distinct on careful reading but close enough to deceive a hurried recipient.

To catch the full space with rules, you would need substitution rules, suffix rules, prefix rules, hyphenation rules, pluralization rules, and the cross product of all of them. Maintaining that set as attackers invent new naming patterns is a permanent engineering job, and the rule set will always trail the newest pattern by weeks. A model that learns a similarity function from examples catches patterns the rule writer never enumerated, and the team maintains a dataset rather than an ever-growing list of special cases.

### Similarity that depends on context

A given domain can be an impersonation attempt for one recipient and a legitimate partner for another. A regional variant of a trusted brand looks suspicious to an organization that only deals with the global parent, and looks perfectly legitimate to an organization that does business with the regional entity directly.

The same string, positive in one context and negative in another. A rule-based system would need per-recipient rules for every legitimate variant, maintained by every administrator at a level of detail that administrators never actually maintain. A model conditioned on the recipient context, which allowlist applies, what relationship history exists, can score the same string differently for different recipients without anyone hand-configuring it. The contextual function is learned from data rather than enumerated by hand.

### Estimating organizational legitimacy

A rule can check that a domain is more than a year old, that its registration is not hidden behind privacy protection, that it has the basic technical records a real domain has. These rules correctly flag low-effort attackers who registered something yesterday.

What a rule cannot do is judge whether the website on that domain looks like a real business, whether the professional presence behind it is genuine or synthetic, whether the registered entity has ever actually done anything an entity does, whether the people named on it have histories that predate the domain. These are judgment signals, and the most a rule can do is check that a signal exists, not evaluate its quality. A rule can confirm that a professional profile is present. It cannot tell that the profile was generated last week. A model can ingest these signals as features and learn that legitimate organizations have distributional properties that manufactured identities do not.

### Patterns nobody anticipated

This is the example that makes the adaptation argument concrete. A class of attack appears that uses an encoding trick to render a domain that visually approximates a trusted one while being technically unrelated underneath. The technique was not a meaningful vector in prior years, so no rule writer had written a rule for it.

A rule-based system that did not specifically anticipate the technique fails silently. The rule has to be discovered, written, deployed, and tuned while the attack runs. A model trained on how domains render rather than how they are encoded handles the new pattern on day one, with no retraining, because the rendered string is similar to the impersonated one regardless of the encoding underneath. The model had learned a more general notion of similarity than any specific rule encoded. That generalization to unanticipated patterns is the core of the adaptation argument.

### Many weak signals, no single strong one

A domain is fourteen months old. That could be legitimate, or it could be an aged domain bought from a marketplace. The registration is privacy-protected. That is true of most legitimate domains now, and also of attackers hiding. The website has a dozen pages. That could be a small business, or generated content.

No single one of these is diagnostic. A rule that fires only when every signal looks suspicious misses attackers who get most signals right. A rule that fires when any signal looks suspicious drowns in false positives. The actual signal is in the joint distribution across many weak features, and the right combination varies by attack pattern, by context, and over time. Rules do not learn joint distributions across weak features; the rule writer has to guess the combinations. Models learn those distributions directly from labeled data, doing the cross-feature integration that no human can practically maintain by hand.

## The argument, stated plainly

Across all five examples the pattern is the same. Machine learning earns its place where the decision requires judgment under uncertainty, where the relevant signals are many and individually weak, where context changes the answer, and where attack patterns evolve faster than a rule-engineering process can follow. Lookalike-domain impersonation has all four properties, which is why a serious system uses a model for the hard part.

But notice what the argument is not. It is not that the model is more accurate than a well-designed rule on any single decision. On a narrow, stable, well-understood decision, a good rule is often as accurate and far more transparent. The model wins on a different axis entirely: it adapts.

A rule-based system is a static target. The attacker observes which rules fire, learns the boundary, and designs the next attack to fall just outside it. Rules cannot move without manual engineering, so every rule you add is a fixed obstacle the attacker maps and routes around. A model trained on a moving dataset is a moving target. As it retrains on new attacks, its decision boundary shifts, and the attacker has to keep paying the cost of adaptation. The defender's advantage, where it exists, comes from this asymmetry, and it holds only as long as the model retrains faster than the attacker adapts. That cadence, how often the model refreshes against how fast the threat moves, is itself one of the more important decisions in the system.

## The hybrid is the answer

The framing that resolves the original question is that rules and machine learning are not competing approaches. They are complementary layers.

Rules handle the deterministic parts: the scope filter, the blocklists, the hard policy enforcement, the auditable checks that must behave predictably. The model handles the judgment parts: the similarity scoring, the legitimacy estimation, the aggregation of many weak signals into one risk estimate. A decision layer then sits on top, applying policy, thresholds, and overrides to turn the model output into an action.

A useful way to hold it: rules are the bones, the model is the muscle, and the decision layer is the brain. A team that tries to do everything with rules ends up with a rule set nobody can maintain, trailing every new pattern by weeks. A team that tries to do everything with a model ends up with a system that anyone who reads it can evade, and that cannot enforce a hard policy reliably. Neither extreme survives contact with a real adversary.

The interesting work is not choosing a side. It is deciding where the boundary between the layers sits, which decisions are deterministic enough to be rules and which require a model, and revisiting that boundary as the threat changes. That boundary is a design decision. Make it on purpose.

---

*This is the third post in a series on evaluating fraud detection. The first two cover the lightweight evaluation for active campaigns and the heavy evaluation that gates a release. Archana Kumari writes about communications fraud, applied AI, and the systems that sit between them. More at archanakumari.fyi*
