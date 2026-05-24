---
title: "Friction Debt"
description: "Friction debt is the cost a platform accumulates every time it makes it easier for users to do actions at scale"
pubDate: 2026-05-17
tags: ["fraud", "fintech", "telecom"]
---

Friction debt is the cost a platform accumulates every time it makes it easier for users to do actions at scale such as sign ups, send messages, add payees, invite strangers, move money without pricing the fraud that follows. The damage control arrives years later, in headcount, in fraud loss, in court filings, regulatory scrutiny. It always lands on a team that didn't create it. And it always looks like a sudden crisis when it was actually compounding in the dark the whole time.

In 2025, a state attorney general named a product team's specific design decisions — "immediate funds availability," "limited recipient information" — in a court filing. Those decisions were conversion optimizations when they shipped in 2017. They became the mechanism that made fraud unrecoverable by 2025.

This essay answers one question: what can a product manager, a team lead or a decision maker actually do differently about that on Monday?

---

## Opening — The seam

In 2024, [Arup wired $25 million](https://www.weforum.org/stories/2025/02/deepfake-ai-cybercrime-arup/) to fraudsters after a finance employee joined a video call where every face—including the CFO's—was a deepfake

In 2025, [Estonian residents lost €29 million to scam calls](https://www.ria.ee/en/surge-scams-costs-estonian-people-29-million-euros) in fluent native Estonian, with a 71-year-old who sold two apartments and handed the cash over losing €138,000. The fraud crossed phone networks.

**The seam:** Comms platforms deliver the message; fintech moves the money. Fintech cannot see where the fraud originated. Both invest heavily in prevention, but effectiveness breaks at the channel boundary.

Regulators now lead rather than lag: UK strict-liability fraud prevention active, EU PSR politically agreed, US state AGs naming design decisions in court filings, Singapore already enforcing dual liability between banks and telecoms.

Generative AI accelerates fraud—not through cost collapse but through **signal collapse**. The [FBI reported](https://www.fbi.gov/news/press-releases/cryptocurrency-and-ai-scams-bilk-americans-of-billions) 22,364 AI-related complaints costing $893 million in 2025. A synthetic-voice call in native Estonian to a Smart-ID user defeats every behavioral signal: call duration normal, caller behavior normal, language native, volume one. Velocity, language mismatch, geographic anomaly—none fire. Only friction at high-risk actions remains: PIN entry, transfer authorization, new beneficiary confirmation. This is trust granularity most platforms lack.

This essay examines what happens at the seam: why shipping features today books fraud debt, and why platforms least like banks discover the cost first.

---

## Part 1: A story you have already lived through

Imagine a product whose first selling point is that anyone can join in seconds and immediately reach anyone else. It begins clean. People sign up, find friends, send messages, make calls. The product team optimizes for activation. Every removed click lifts conversion numbers. Reaching strangers requires one search box and one button. This is the pitch.

Then spam arrives. Then scams. Then fraud.

The consumer version lets anyone create groups and invite outsiders with open-ended custom invitation content. Thousands advertise crypto groups with flashy messages. Because the consumer product integrates with the enterprise version, spam crosses the boundary. Enterprise users receive messages from consumer accounts impersonating senior executives. The fraud is wearing institutional clothes before detection tools exist.

The fraud team adds limits: search rate caps, message volume caps, group creation caps. Legitimate users notice. Support tickets rise. Growth pushes back.

The fraud team adds models: behavioural patterns, sequence detectors. They work briefly. Then fraud goes low-volume and high-fanout. A million accounts sending three messages per day looks indistinguishable from a million real users having three conversations.

The fraud team adds user signals: reports, blocks. The signals are real but slow. By the time enough signal arrives, the harm has shipped.

The identity team is asked to slow signup. They add device fingerprinting, behavioural biometrics, CAPTCHA. Fraud rings buy CAPTCHA-solving services for a tenth of a cent per solve. Account creation continues at scale.

Six months pass before fraud and identity teams share signals. Three more months pass before integration ships. The model improves for four weeks and plateaus. Fraud adjusts.

Customer support becomes the largest cost centre. Real users get blocked. Real users leave. PR risk teams pay attention. Growth flattens.

Someone proposes self-serve SMS account recovery. Within months, the SMS flow is abused for international revenue share fraud and SMS pumping. A new fraud type, born from a feature designed to fix the previous problem.

Eventually the product is replaced by a younger successor, not because it is better but because it has not accumulated the debt. Every feature the successor adds is a friction insertion the original lacked. Each was designed in response to a fraud pattern the original's growth-optimised design made inevitable.

This is not one company's story. Yahoo Messenger ran the same arc through the 2000s, with chat rooms reportedly overrun by bots and the consumer service finally shut down in 2018 [after a failed replacement attempt](https://www.google.com/search?q=https://techcrunch.com/2018/06/08/yahoo-messenger-is-finally-shutting-down-on-july-17/). Google Hangouts ran it through the 2010s, openly described as a haven for scammers and catfishers before being phased out in favour of Google Chat, with [basic user-reporting features added only in 2019](https://9to5google.com/2019/08/02/google-hangouts-user-reporting/), by which point the product was already on its way out. Microsoft Teams ships today with [sender provenance indicators](https://learn.microsoft.com/en-us/microsoftteams/trusted-organizations-external-meetings-chat), accept-or-block flows for unknown external senders, and templated invitations precisely because Skype, the consumer messenger it absorbed, did not. Different companies, different decades, same arc. The industry has lived this pattern repeatedly without ever giving it a name.

---

## Part 2 — Name the pattern: friction debt

Software engineers call invisible compounding costs "technical debt." Every shortcut taken to ship faster is a loan paid back in maintenance. The principal arrives when the system needs to change in a way the shortcut made impossible. Product managers lack an equivalent term for removing friction from high-velocity actions without pricing subsequent fraud. Call it friction debt.

Every removed friction is a loan. Interest is paid in fraud volume. Principal arrives in customer support cost, false positive churn, regulatory liability, and product reputation. The PM who took the loan is gone when the bill arrives. Signup is most visible because it is where most platforms first cut friction, but the dynamic applies to any high-velocity action: new payees, external invitations, account recovery, API keys, payout instructions.

Friction debt has four dangerous properties distinguishing it from technical debt.

It compounds silently, looking like nothing for eighteen months then accelerating. By the time the curve is visible, architecture is locked. Worse, fraud may be present day one but the product ships without measurement surfaces (report buttons, block flows, signal pipelines) to reveal it. What looks sudden is inherited.

It is paid by a different team than the one who took the loan. Growth removes friction on signup, payments on new beneficiaries, platform on API issuance, support on account recovery. Trust and Safety, Customer Support, and Compliance pay the interest. The cost is real but off the decision-maker's P&L.

It is not eliminable, only allocable. Adding friction later does not pay principal; it shifts who pays. When a fintech implements aggressive fraud controls, legitimate customers get blocked. Block less, fraud rises. Same debt, different ledger.  [J.P. Morgan research](https://www.jpmorgan.com/insights/payments/data-intelligence/cnp-fraud-prevention-combat-chargebacks) indicating false positives represent 19 percent of total fraud costs, versus 7 percent for actual fraud losses. 

Then It transmutes. This is the part almost nobody sees coming. After transmutation comes the regulatory scrutiny.

<img src="/lifecycle.png" alt="lifecycle" />

---

## Part 3 — The Transmutation

**The standard response to friction debt is to add friction back at signup. KYC. Document verification. Liveness checks. Sanctions screening. Behavioral biometrics. The signup becomes a citadel. Fraud does not stop. It changes form.**

Opportunistic fraud required low entry cost. A teenager, a small ring, a script kiddie. Enterprise SaaS raised that cost. KYB, verified domain, identity proofing. The citadel works against opportunism. It does not work against fraud that adapts to price of entry. A reseller sells seats. The shelf company impersonates a legitimate firm. Legitimate contacts receive a verified message from a verified domain. The fraud wears institutional clothes.

In ride-hailing, at least 80 Facebook groups host verified Uber, Lyft, and DoorDash driver accounts for rent and resale, [with groups reaching 22,000 members](https://www.techtransparencyproject.org/articles/for-sale-on-facebook-fraudulent-uber-driver-accounts). The verified accounts on offer were verified. The people driving were not. In Nigeria, Rest of World documented Bolt drivers selling verified accounts to unregistered third parties, partly because Bolt's [onboarding grew more rigorous](https://restofworld.org/2022/bolt-drivers-in-nigeria-are-illicitly-selling-their-accounts-putting-passengers-at-risk/). Verification did its job. The fraud transmuted around it. 

Fraud volume falls. Fraud sophistication rises. Loss-per-incident rises faster. Detection signals that worked against opportunism (velocity, volume, geography) do not fire on single, well-funded, organisationally-fronted attacks. Verified status becomes part of the attack surface, not a defence against it. Friction debt does not disappear when you add friction. It selects for sophistication.

---

## Part 4 — The seam, in detail

Telecom and fintech are the same problem at different layers. Telecom moves identity and information across a network. Fintech moves money across an application. Both operate under public-trust conditions where failure is reputational and licence-threatening.

Almost every consumer fraud is a relay across the seam. Communication arrives first. Transaction follows. Telecom sees the call but not the payment. Banks see the payment but not the call. Neither sees fraud whole.

**What each side learned the hard way**

Telecom learned fraud detection cannot live in silos. Operators participate in GSMA Device Registry to share blocked devices across networks globally. STIR/SHAKEN is a caller ID authentication framework using digital certificates to verify that a call is from the [number it claims to be](https://www.fusionconnect.com/resources/glossary/stir-shaken). The reason is economic. AIT and IRSF cost the originating operator directly, in the same billing cycle. Pain forced collaboration. [GSMA](https://www.gsma.com/solutions-and-impact/connectivity-for-good/public-policy/mobile-policy-handbook/consumer-protection/mobile-device-theft/)

Banks learned friction allocation has to be precise. PSD2 enforces Strong Customer Authentication regulations mandating rigorous authentication for [electronic transactions in the EEA](https://docs.adyen.com/online-payments/psd2-sca-compliance-and-implementation-guide/). Thirty years of regulatory pressure built risk engines that decide which transactions need step-up, which do not. The operating skill is real.

**Each industry has the lesson the other needs**

Banks did not collaborate because loss landed on the next bank, not the originating one. APP fraud reimbursement rules came into force in October 2024, entitling victims to reimbursement of up to £85,000 with a 50:50 liability split between [sending and receiving PSPs](https://www.threatmark.com/six-months-of-psr-scam-reimbursement/). Banks now eat the loss. The economics that forced telecoms to collaborate are now landing on banks. Confirmation of Payee has completed over 2.5 billion checks since 2020 and is now mandated across UK PSPs. EU Verification of Payee requirements became mandatory in October 2025, requiring PSPs to verify payee identity before executing [SEPA credit transfers](https://legal.pwc.de/en/news/articles/verification-of-payee-requirements-vop-under-the-eus-instant-payments-regulation-ipr). Information sharing is finally happening. The muscle is decades behind telecom. 

Telecoms never allocated friction precisely because a blocked call is not, by itself, a financial loss with clear attribution. Now agentic AI features are about to invert that. The economics are about to flip.

### The signup citadel is a distraction

A regulated fintech in 2026 has device fingerprinting, behavioral biometrics, document verification, sanctions screening, PEP checks, and continuous KYC refresh. The citadel is built. It works. The argument is not that it failed. The argument is that it is in the wrong place.

Friction debt does not accumulate at signup. It accumulates in the gap between onboarding and the ten thousand actions after: the new payee, the larger transfer, the unusual hour, the first international beneficiary, the device change, the dormancy break. Each is a trust decision. In most product organisations, none is an explicit design choice. They inherit the signup decision and are adjudicated post-hoc by a risk engine that arrived too late.

The signup citadel is the line of defence everyone agrees on. The line of defence at the point of each high-risk action, the one that actually matters, is the one nobody is debating.

### The platforms that haven't learned either lesson

A third class of platform now exists. Not bank. Not telecom. Moves money. Moves identity. Runs at scale.

Ride-hailing with in-app wallets. Food delivery with credit. BNPL providers without banking licenses. Marketplaces as escrow. Content platforms with payouts. Crypto on-ramps. Super-apps with sub-merchant economies.

None have bank-like fraud infrastructure. None have telecom-like cross-operator collaboration. Most grew on the consumer messaging playbook: fast signup, network effects, growth at any friction cost. The friction debt is being booked at 2005 bank rates. Repayment is being set by 2026 regulation.

In 2017, Zelle launched with design decisions prioritizing frictionless transfers: quick registration, minimal verification, immediate funds availability, limited recipient information. Growth metrics were strong. By 2025, the New York Attorney General filed a complaint against Early Warning Services, revealing EWS designed Zelle without critical safety features, allowing scammers to steal over $1 billion between 2017 and 2023, and that EWS knew from the beginning that key features made Zelle uniquely susceptible to fraud yet [failed to adopt basic safeguard](https://ag.ny.gov/press-release/2025/attorney-general-james-sues-company-behind-zelle-enabling-widespread-fraud) The complaint named specific design decisions by name: quick registration, lack of verification, limited information displayed, and emphasis on immediate funds availability. The PM who designed immediate funds availability in 2016 optimized for conversion. The fraud team, if consulted, would have known the risk. The bill arrived as a state attorney general lawsuit naming the features in discovery.

The clock has changed. Telecom had decades to learn its lesson. Banking had decades longer. The new wallet platforms have between three and seven years.

**Two things make 2026 the year this matters more than it did in 2022. Regulators have stopped lagging the fraud and started leading it — though not uniformly.**

The UK has APP fraud reimbursement in force since October 2024, with sending and receiving banks splitting the cost. On 1 September 2025, the Failure to Prevent Fraud offence came into force — a strict-liability offence for large organisations whose associated persons commit fraud for the organisation's benefit. The Serious Fraud Office has [signalled it wants to be first to prosecute.](https://www.wilmerhale.com/en/insights/client-alerts/20250825-uk-failure-to-prevent-fraud-offence-to-come-into-force-is-your-organisation-prepared)

The EU went further on architecture. PSD3 and the Payment Services Regulation reached political agreement on 27 November 2025, with final compromise texts published in April 2026 and entry into force anticipated in 2027. The PSR makes payment service providers liable for customer losses where fraud prevention mechanisms fail, mandates payee-name and IBAN matching, and — the part the comms industry should read twice — extends scope to technical service providers and, in defined constellations, electronic communications providers and online platforms. Singapore's dual-liability regime between banks and telecoms is already operational and is the architecture the [EU is now building toward](https://www.europarl.europa.eu/legislative-train/theme-an-economy-that-works-for-people/file-revision-of-eu-rules-on-payment-services)

Whoever runs payments through a platform without precise friction allocation in 2027 will be the named defendant in 2029.

---

## Part 5 — What you can do about this on Monday

### The vocabulary

Four concepts make the invisible visible.

**Friction debt.** Every friction removed from a high-velocity action is a loan. The bill arrives later, on someone else's P&L. The team who shipped the loan rarely pays the principal.

**Trust granularity.** The resolution at which a system makes trust decisions is a design choice. "This user is verified, therefore everything they do is trusted" is a coarse choice. "This user is verified for receiving payments, but each new outbound payment over X to a new beneficiary requires re-verification" is a fine choice. The right granularity matches the risk of the action, not the friction tolerance of the user or the conversion target of the growth team.

**What trust granularity looks like?**

1. **Payment systems**
<img src="/payment.png" alt="payment" />

The core argument is visible at a glance: on the left, one gate and then a wide open channel regardless of what the user does next. On the right, the entry gate is identical but each action downstream carries its own friction sized to its risk. Routine payment passes with no gate. New beneficiary, large transfer, account recovery all require re-verification. International payment gets a lighter confirm step.

The gate size variation on the right is intentional. It is the thing most diagrams miss when they try to show fine-grained trust — they draw friction as uniform rather than proportional.

2. **Similar granularity should exists for CPaaS systems but with some caveats** 

The gate hierarchy on the right reads as intended: domestic transactional passes with no friction; high-volume campaigns and volume spikes get a lighter approval/review gate; international destinations, financial/auth categories, and new country activations get the full re-register or verify-use gate. The left side shows the same six action types with no differentiation — registered means trusted for all of them.

<img src="/fraudreverify.png" alt="fraudreverify" />

**Two things this illustration does that the payment version does not.** 
First, it shows that fine trust is not just about the user — it is about the _action type and destination combination_. A sender trusted for domestic transactional is not automatically trusted for financial category messages to a new country. 

Second, the "volume spike" row makes the AIT/SMS pumping argument visual without naming it. This row shows why volume anomaly detection belongs inside the trust model, not only in a separate fraud layer downstream.

 **Friction allocation.** Friction is finite. Someone always pays. The choice is whether the cost lands on the fraudster, the legitimate user, the support team, the risk team, or the regulator. Most product specs do not name this choice. They should.

**Trust transmutation.** Adding friction does not eliminate fraud. It selects for sophistication. The fraud you are left with is more organised, more expensive per incident, and harder to detect than the fraud you removed. Plan for the fraud you create, not just the fraud you stop.

Vocabulary is useful. It is not sufficient. Vocabulary does not change behaviour. What follows is why.

---

#### The risk evaluation trap

Most large platforms already have something that looks like fraud review before launch. A risk assessment. A security review. A compliance check. The process exists. The checkbox gets checked. The fraud still arrives.

Here is what actually happens.
<img src="/fraudcycle.png" alt="fraudcycle" />

A new consumer product is about to launch with group messaging and enterprise integration. A risk assessment is requested and assigned to a risk consultant either in the same company or a vendor with advisory authority, no decision-making power, and no accountability after the document is filed. The  risk consultant knows the legacy product. They do not know the new architecture, because the architecture does not exist yet. The PRD is still in draft.

The risk consultant or owner produces a document. Thorough in places, shallow in others. It flags patterns from the old product but cannot translate them to the new surface because the integration points have not been built. The PM reviews it once, agrees a follow-up is needed, and brings in a manager.

The follow-up never happens. The  risk consultant has no ownership stake in the outcome. The PM has no incentive to chase it. Nobody kills the thread. It is deprioritised into silence.

Meanwhile the fraud team needs product data: user signals, group membership, message metadata. That data flows through two other teams, identity and permissions, whose priority is product liveness, not fraud signals. A one-time data pull requires privacy review. Privacy is booked a month out. Follow-ups add another month. Time zones cause missed meetings. The thread dies again.

The product launches. Fraud is present from day one. But the product shipped without report buttons, block flows, or signal pipelines. The fraud is not arriving later. It is invisible, compounding in the dark, until someone builds the measurement surface. When reporting features eventually ship, the fraud does not arrive. It becomes visible. What looks like a sudden crisis is an inherited condition.

This is not a failure of people. It is a failure of structure. The  risk consultant was asked to assess a product with no architecture, on a surface they did not own, and produce a document nobody had authority or incentive to act on. The dependency chain, fraud team needs data, data team needs integration, integration needs privacy review, privacy needs scheduling, is not dysfunction. It is the normal operating rhythm of a large platform company. Fraud assessment is nobody's first priority. It is everyone's third or fourth. A third-priority task with a four-team dependency chain does not get deprioritised. It gets deprioritised into silence.

#### What looked different when it worked

In the contrasting case, a programmable communications platform designed trust tiers for API access. The original assumption was intuitive: a fully registered, verified sender at the highest tier had earned frictionless throughput. Pass the gate once, pass everything.

The fraud team was consulted before the architecture was locked. Their argument was simple. Account compromise does not respect tier boundaries. A compromised verified account sends messages with full institutional authority — registered brand, verified sender ID, established delivery history. Auto-trusting the top tier does not protect against the attack. It makes verified status part of the attack surface.

The design changed. Anomalous volume, unusual destination patterns, and behavioural deviations on any tier, including the highest, routed to review rather than automatic pass. Registration became the entry condition, not the ongoing authorisation. Sender behaviour post-registration was treated as a separate, continuous signal.

The trade-off costs money. Manual review at volume is not cheap. It also means a compromised registered sender gets flagged before it sends a campaign wearing institutional clothes. Friction allocated at the point of risk, not at signup.

Why did this work when the other did not? Not because the people were better. Because the structural conditions were different. The fraud team saw the architecture before it was locked. Data flowed directly to the fraud function rather than through intermediate teams with competing priorities. Product and fraud were in the same room early enough that fraud input could change the design — not document a risk nobody had authority to act on.

---

#### The accountability gap

In the failure case: nobody. The investigator was advisory. The PM's review included no fraud metric. The fraud team's OKR covered detection on existing surfaces, not prevention on new ones. The product launched, the fraud compounded, and when it became visible it was treated as an operational problem for trust and safety, not a design problem for the PM who made the trade-off.

In the success case: the product leaders in the room when the design changed. They owned the trade-off. They understood the cost of manual review and accepted it because they understood the cost of not having it.

The structural fix is not a new process, a better risk assessment template, or "invite fraud to the alpha." It is one line on one performance review: the PM who ships a feature owns a fraud-adjacent metric for that feature for twelve months after launch.

The metrics already exist on every fraud team's dashboard. They are just on the wrong scorecard:

- Time from first fraud signal to detection and block
- Users contacted or transactions completed before the block lands
- False positive rate: legitimate users blocked then unblocked
- Verified-account abuse rate on features with coarse trust granularity
- Report and block rate as a proportion of new user interactions

On the fraud team's dashboard these are operational measures. On the PM's performance review they change behaviour. The PM whose review includes "time from first spam message to block" will ask, before launch: how will we detect this fast enough? That question pulls fraud into the design conversation not because a process mandates it but because the PM's incentive demands it.

This is a smaller intervention than a reorg and a larger one than a checklist. It does not require the fraud team to have authority they do not have. It does not require a shared OKR between Growth and Trust. It requires one decision by a VP or CPO: the PM who ships the feature owns the fraud number. That decision costs nothing to implement. It is the difference between a design that prices fraud at zero and a design that prices it honestly.

---

#### What this won't fix

This will not catch a novel fraud pattern nobody has seen before. It will not replace the risk engine. It will not work where the fraud team reports outside Product and shares no scorecard with anyone who ships features. In those organisations the prerequisite is structural: get Trust to the Product table first. Everything else is theatre until then.

What it does is move the fraud decision from a post-launch retrofit by a team that arrived too late to an explicit design choice owned by the team that made the trade-off. That is a smaller change than it sounds and a larger one than it looks.

---

## Closing — The compressed clock

Consumer messaging products who have not learned about friction debt are dead in 2026. Banks learned about it by being regulated.

The next class of platforms — the ones with wallets, with credit, with cross-border payouts, with verified seller markets — will learn faster than either, because EU has already named the seam in regulatory text. The next jurisdictions will follow

The product manager building a feature today is choosing, implicitly, what kind of platform they will become. Frictionless and short-lived. Frictionful and brittle. Or precise — friction allocated where the risk lives, trust evaluated at the granularity of the action, and an honest accounting of who pays the debt.

The third option is harder than the first two. It is also the only one that survives the next regulatory cycle.

The question for every feature review is not a checklist item. It is the question every feature owner should be able to answer before the feature ships:

*Which PM's name is next to the fraud metric for this feature twelve months from now? If the answer is "nobody," the debt is already being booked — silently, in the dark, compounding until someone builds the instrument that makes it visible. And then it will look like a sudden crisis. It won't be. It will be an inherited condition, and the design decision that created it will be sitting in a product spec that nobody has read twice.*

----------------------

## Appendix
<img src="/metrices.png" alt="metrics" />

