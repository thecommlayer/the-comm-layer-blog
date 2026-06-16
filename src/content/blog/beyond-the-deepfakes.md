---
title: "Beyond the Deepfakes"
description: "Why regulators are solving APP fraud at the wrong layer — and what Arup's $25M deepfake loss tells us about where the real chokepoint is."
pubDate: 2026-05-11
pinned: true
tags: ["APP Fraud"]
---

In February 2024, a finance employee at Arup's Hong Kong office joined a video call with what appeared to be the company's CFO and several senior colleagues.[^1] The call was convincing enough that, over the following weeks, the employee authorised fifteen separate wire transfers totalling $25.6 million.[^2] Every transfer was legitimate from the bank's perspective: the right customer, the right credentials, a properly authenticated instruction. The bank did exactly what it was designed to do.

The attack was complete before a single payment was made. The fraud happened on the call, not in the payment system. Yet the entire global regulatory response to this class of fraud — authorised push payment (APP) fraud — is focused on the payment layer. We are building the lock after the house has already been entered.

> **THE ARUP ATTACK — PASS 1: FACTS**
>
> **What:** AI-generated deepfake video conference. Multiple participants faked, including the CFO.
>
> **When:** January–February 2024, Hong Kong.
>
> **How much:** HK$200 million (~$25.6M USD) across 15 transfers to 5 bank accounts.[^2]
>
> **Primary sources:** Hong Kong Police public statement (February 2024);[^2] Arup spokesperson interview, CNN (May 2024);[^1] Financial Times (May 2024).[^3]
>
> **What the bank saw:** Legitimate customer. Authenticated session. Authorised instruction. No anomaly.[^4]

## Pass 2 — Attack Surface

### Where the kill chain actually ran

Map the attack stage by stage and the control failures become obvious — but not where most coverage locates them.

**Stage 1 — Reconnaissance.** Attacker identified the CFO, their direct reports, and the organisational chart. All publicly available from LinkedIn and press coverage. Control that should have caught it: none available at this stage. This is table stakes for any targeted fraud.

**Stage 2 — Pretext.** Victim received a phishing message appearing to be from the company's CFO requesting a "secret transaction," then an invitation to a video call.[^2] The invite appeared legitimate. Control that should have caught it: sender verification on the communications platform. Did it exist? No.

**Stage 3 — Execution.** Deepfake video and voice of multiple colleagues on a live call. The employee later said the people "looked and sounded just like colleagues he recognised."[^1] Control that should have caught it: real-time synthetic media detection on the video platform. Does it exist in enterprise conferencing tools? Not in 2024, not at scale.

**Stage 4 — Authorisation.** Fifteen transfers over several weeks to five bank accounts.[^2] Control that should have caught it: out-of-band confirmation above a threshold value; dual authorisation requirement for large transfers; callback verification to a pre-registered number — not a number provided in the meeting. Some of these controls existed in policy. They were not triggered.

**Stage 5 — Exit.** Funds moved to five accounts across multiple jurisdictions. By this point, the fraud was complete. Payment layer controls — Confirmation of Payee, transaction monitoring — fire here. They were irrelevant. The victim was already convinced.

> *"The deepfake was the least novel part of this attack. The real failure was fifteen authorised transfers over several weeks. That is a workflow and authorisation problem, not an AI problem."*

Every post-Arup media story focused on the deepfake technology. Understandable — it is the novel, frightening element. But the deepfake only needed to work once, for a few minutes, to establish a false premise. The consequential failure was the absence of any out-of-band verification before each of the fifteen subsequent payment instructions. A single callback protocol to a pre-registered number would have broken the chain at Stage 4, regardless of how convincing the deepfake was.

## Pass 3 — Counterfactuals

### What would actually have stopped it, ranked

**1. Out-of-band payment confirmation above a threshold (high impact, low friction).** Any transfer above — say — $100,000 requires confirmation via a separate channel not connected to the initial instruction. The separate channel must be a pre-registered number or hardware token, not a number provided in the suspicious communication. This is implementable today. It breaks the Arup attack chain entirely at Stage 4.

**2. Dual authorisation for large wire transfers (high impact, moderate friction).** The instruction to pay requires a second authoriser who was not present on the original call. Standard in many treasury controls; apparently not enforced here. This is process, not technology.

**3. Trusted-call indicator on enterprise conferencing platforms (medium impact, medium effort — with an important caveat).** Enterprise video platforms could cryptographically sign calls originating from verified corporate tenants, similar to how DKIM signs email. A visual indicator — "this call is verified as originating from Arup's tenant" — would raise the cost of external spoofing substantially. This does not yet exist in any major enterprise conferencing product. However, this control has a critical blind spot: it does nothing if the attacker has already compromised the legitimate credentials of an executive or trusted vendor. If a fraudster is hosting a deepfake call from *inside* a legitimately authenticated corporate tenant — using the CFO's real account — the platform would mark the call as verified, potentially creating a false sense of absolute security that makes the victim less likely to question what they are seeing. Trusted-call indicators address external spoofing. They do not address Business Email Compromise or account takeover, which are increasingly the entry point for high-value attacks.[^11]

**4. Real-time synthetic media detection on video calls (lower impact near-term, high effort).** Detection of deepfake video in real-time is an active research area but not yet reliable enough to be a primary control. Flagging possible synthetic media is useful as a warning, not a blocker. Invest here for 2026–2027, not as a primary 2024 control.

## Pass 4 — The Comms Layer Lens

### What this attack tells us that banking fraud writers miss

The standard framing of APP fraud is a payments problem. A victim is tricked into sending money; the bank should have stopped it; regulators should make banks pay. This framing is not wrong — but it is incomplete in a way that is becoming dangerously expensive.

UK Finance's own data shows that **70% of APP fraud cases in 2024 originated online, and 16% originated via telecommunications networks.**[^5] Add those together and you have 86% of APP fraud cases starting outside the financial system entirely. The bank sees the last five seconds of an attack that began on a social media platform, a phishing SMS, a spoofed phone call, or — as in Arup's case — a video conference.

Regulators have built elaborate machinery at the payment layer. The UK's mandatory reimbursement scheme (October 2024) requires banks to reimburse victims up to £85,000, with the cost split 50/50 between the sending and receiving bank.[^6] The EU's Verification of Payee (VoP) went live in October 2025, requiring IBAN-name matching before any SEPA credit transfer executes.[^7] These are meaningful controls. They are also controls that fire after the fraud is, in most cases, already complete.

> **THE REGULATORY LANDSCAPE — WHERE COMMS LIABILITY SITS TODAY**
>
> **UK:** Banks pay. Telcos and platforms have a voluntary Online Fraud Charter. No legal liability for either.[^6] 70% of APP fraud originated online and 16% via telecoms in 2024.[^5] Social media platforms bear zero financial liability.
>
> **EU (PSD3/PSR, agreed November 2025):** Payer bears the loss by default. Two exceptions: PSP impersonation fraud, and — importantly — the European Parliament successfully argued for liability to extend to electronic communication service providers where they fail to remove fraudulent content.[^8] Not yet in force. Scope still being defined.
>
> **Singapore (SRF, live December 2024):** Waterfall model. Banks first, then telcos if banks complied. Telco duties are specific and binary: only connect to authorised SMS aggregators, implement anti-scam filter. No liability cap. Full payout expected.[^9]
>
> **Australia (SPF, passed February 2025):** Banks + telcos + digital platforms all in scope. Up to AU$50M in fines. Sector-specific codes not yet published. Implementation stalled.[^10]

## The Structural Problem Nobody Wants to Name

There is a reason the UK did not make telcos and platforms liable, and it is not that Parliament forgot they existed. It is that building a bank-only liability regime was tractable in 2024. Banks are supervised, concentrated, and already have legal obligations to customers. Extending liability to Meta, Google, or telecoms operators would have required primary legislation, new regulatory mandates, cross-authority coordination between the PSR, Ofcom, and the FCA, and a political fight with some of the most powerful lobbying operations in Westminster. The PSR chose to ship a working bank-only regime rather than wait years for a theoretically better multi-sector one.

This is understandable. It is also wrong in its long-run implications.

The UK's 50/50 bank liability model is, structurally, a subsidy for telco and platform failure. When a fraudster uses a spoofed SMS to initiate an APP scam, the bank pays out. The mobile operator that delivered the spoofed message bears no cost. When a fraudster runs a fake investment ad on a social media platform to recruit APP fraud victims, the bank pays out. The platform that monetised that ad bears no cost. The incentive structure is backwards: the party with the least visibility into the attack chain is bearing the financial consequences.

There is one actor this article has underweighted: the receiving bank. For a $25.6 million fraud to succeed across fifteen transfers, receiving banks had to allow those funds to land in — and quickly exit from — fraudulent accounts and money mule networks. The UK's 50/50 liability split was specifically designed to force receiving banks to invest in identifying and closing mule accounts. By arguing that liability should move upstream to the communications layer, I risk inadvertently letting the receiving banks — who harbour the criminals' infrastructure — off the hook entirely. The honest answer is that the receiving bank failure is a separate, parallel problem that the 50/50 split partially addresses. Fixing the communications layer does not make the receiving bank problem disappear. Both need solving simultaneously.[^6]

Singapore's waterfall model corrects the comms-layer gap. Liability attaches to whoever failed. If the telco delivered a sender-ID SMS from an unauthorised aggregator, and that SMS initiated the fraud, the telco pays.[^9] This is not punitive — it is accurate. It puts the cost of failure where the failure occurred, which creates the right incentive for each party to invest in their own layer of the defence.

> *"The UK's reimbursement scheme is a victim compensation mechanism. It has been mistaken for a fraud prevention strategy."*

## The Tension I Cannot Resolve

Here is the uncomfortable assumption underneath my argument: sender verification and platform-layer controls are technically feasible and commercially viable at scale. I am not sure this is true.

> **THE STRUCTURAL CONFLICT**
>
> The same product logic that makes communications platforms easy to adopt — fast onboarding, frictionless provisioning, low barrier to sending — is precisely what makes them exploitable. A telco or platform that introduces meaningful sender verification at scale will slow activation rates, reduce message volume, and create friction that competitors without those controls can exploit. The commercial incentive runs directly against the security requirement. This is not a problem that voluntary charters or good intentions can solve. It requires mandatory baseline standards that apply to all players equally, removing the competitive penalty for doing the right thing.

This tension runs through every major communications platform. Fast onboarding, frictionless provisioning, low barrier to sending — these are features that product teams ship deliberately, because they drive adoption. Sender verification and provisioning friction are costs that the same product teams are incentivised to minimise. The industry has consistently resolved that tension in favour of ease of onboarding, because the competitive penalty for adding friction unilaterally is real. The regulatory pressure now building in Singapore, Australia, and slowly in the EU is the external forcing function that the market has not provided for itself.

## Pass 5 — My Take

### Where I land, and the counter-arguments I take seriously

My position: **liability for APP fraud should sit where the failure occurred, not where the money moved.** In most APP fraud cases, that failure occurred at the communications layer — a spoofed SMS, a fake call, a fraudulent platform interaction — before any payment was instructed. Bank reimbursement treats the symptom. Sender verification at the communications layer treats the cause.

But sophisticated APP fraud is almost never a single-point failure. It is an ecosystem failure. The Arup attack involved a compromised communications channel, a failure of internal authorisation controls, and receiving banks that processed fifteen outbound transfers without triggering meaningful scrutiny. Blaming one node in that chain is analytically incomplete. Which brings me to what regulators are actually choosing between — and what each choice costs.

> **THREE REGULATORY PATHS — WHAT EACH ONE COSTS**
>
> **The UK Path:** Bank-only liability. Fast victim reimbursement. Clean, binary, enforceable. The cost: a structural subsidy for telco and platform failure. 86% of fraud originates outside the financial system; 0% of liability flows there.[^5] Bad long-term incentives dressed up as consumer protection.
>
> **The Singapore Path:** Waterfall liability by sector. Binary duties, binary accountability. Clean and enforceable. The cost: rigidity. It works well for SMS phishing with clear duty definitions. It does not yet cleanly cover RCS, enterprise video, or AI voice — the channels where the next generation of attacks is running.[^9]
>
> **The Dynamic Split Path (Australia's ambition):** Liability apportioned across banks, telcos, and platforms based on which control failed in the specific attack. The most accurate reflection of how fraud actually works. The cost: an arbitration nightmare. If Meta, a telco, a sending bank, and a receiving bank are fighting over percentage fault, the victim waits. Passed in February 2025 in Australia; sector-specific codes still not published.[^10]

The dynamic split is the right target state. Treat each fraud investigation like an airplane crash: pull the telemetry from the telco, the platform, and both banks to determine where control failed and apportion loss accordingly. A telco that allowed a spoofed sender-ID from an unauthorised aggregator absorbs a larger share. A receiving bank that allowed funds to land in a two-day-old account and wire immediately to a crypto exchange absorbs their share. A platform that hosted a flagged deepfake ad absorbs theirs. Liability follows the failure, not the nearest regulated pocket.

The fatal flaw is operational, not conceptual. Cross-sector data sharing at the velocity required — fraud reported, telemetry submitted, liability apportioned, victim paid, all within days — does not exist yet. Building that infrastructure requires a centralised API-driven arbitration clearinghouse that no single regulator currently has the mandate or the technical capacity to operate. The UK chose bank-only liability not because it was right, but because it was fast. That is understandable. It should not be permanent.

The strongest near-term answer is Singapore's binary-duty model, extended progressively to new channels as those channels are defined and their controls specified. Start with SMS. Define RCS duties next. Then enterprise video. Then AI voice. Each channel gets specific, auditable duties. Liability attaches to breach of those duties, not to probabilistic outcomes. That is enforceable today and extensible tomorrow.

The EU is slowly moving in this direction with PSD3/PSR's extension toward electronic communications providers.[^8] The UK is not moving at all — it remains committed to bank-only liability, with a voluntary charter for everyone else.[^6] Given that 86% of UK APP fraud originates outside the financial system, that is an increasingly indefensible position.[^5]

<div align="center">— ◆ —</div>

### Open Questions — I Don't Have Answers To These

**1.** As fraud migrates from SMS to RCS, Teams, WhatsApp, and cloud communications voice, none of the existing sender verification frameworks cleanly apply. Singapore's SRF explicitly carves out RCS because it is not delivered via the SMS channel.[^9] Who owns the verification obligation for a deepfake call placed over an enterprise communications platform? Nobody has answered this yet.

**2.** The UK's mandatory reimbursement scheme covers consumers, micro-enterprises, and small charities only. Large organisations — including every enterprise customer of a communications platform — are entirely outside scope.[^6] If Arup had been a UK company, no reimbursement scheme would have applied. Is there a coherent argument for why B2B APP fraud victims deserve less protection? Or is this simply a political economy outcome?

**3.** VoP fires at the IBAN-name matching layer. CoP fires at the Faster Payments layer. Both are payment-layer controls.[^12] The Arup attack would have passed both without triggering either — the payment instructions were to real accounts, with real names, that matched. Is there any payment-layer control that could have caught this? Or is the payment layer simply the wrong layer for this class of attack?

**4.** Right now, the bank sees the payment and the communications platform sees the manipulation. Neither can see the other half of the kill chain. The logical solution is a real-time data bridge — an API that allows a bank's risk engine to query whether a user is currently in a high-risk concurrent session before executing an anomalous large transfer. The comms platform would not bear financial liability; it would provide a deterministic signal. Is there any major bank or platform currently building this? And would bank compliance departments accept a real-time third-party session signal as grounds for blocking a payment — or is that third-party reliance too legally exposed?

**5.** Enterprise communications platforms measure success in Daily Active Users. A deepfake attacker who joins one call and a legitimate CFO who joins the same call are identical in that metric. If platforms moved from DAU to Verified Active Users — sessions where identity has been cryptographically attested — the product incentive to invest in verification would shift fundamentally. Will any major enterprise platform make that metric change voluntarily? Or does the liability forcing function have to come first before the measurement changes?

---

## Sources & References

[^1]: **CNN Business (May 2024).** "Arup revealed as victim of $25 million deepfake scam involving Hong Kong employee." Includes statement from Arup Global CIO Rob Greig and confirmation that fake voices and images were used. [cnn.com](https://www.cnn.com/2024/05/16/tech/arup-deepfake-scam-loss-hong-kong-intl-hnk)

[^2]: **South China Morning Post (May 2024).** "UK multinational Arup confirmed as victim of HK$200 million deepfake scam." Includes Hong Kong Police account of the phishing message, video call, and 15 transfers to five accounts. [scmp.com](https://www.scmp.com/news/hong-kong/law-and-crime/article/3263151/uk-multinational-arup-confirmed-victim-hk200-million-deepfake-scam-used-digital-version-cfo-dupe)

[^3]: **Financial Times (May 2024).** First publication to name Arup as the victim. Described by FT as "one of the world's biggest known deepfake scams." Referenced via Dezeen's contemporaneous reporting: [dezeen.com](https://www.dezeen.com/2024/05/17/arup-victim-deepfake-video-scam/)

[^4]: **PRMIA Case Study (2025).** "The Arup Deepfake Fraud." Confirms that Arup's IT environment remained fully intact: no malware, no intrusion, no compromised credentials, no data loss. All traditional cybersecurity layers were operating effectively. [prmia.org](https://prmia.org/common/Uploaded%20files/eCyber/PRMIA%20Case%20study%20-%20ARUP.pdf)

[^5]: **UK Finance Annual Fraud Report 2025 (May 2025).** 70% of APP fraud cases started online; 16% started through telecommunications networks. APP fraud losses £450.7M in 2024. Personal losses £365.7M; non-personal (B2B proxy) £84.9M. [ukfinance.org.uk](https://www.ukfinance.org.uk/policy-and-guidance/reports-and-publications/annual-fraud-report-2025) — Press release: [ukfinance.org.uk/press-release](https://www.ukfinance.org.uk/news-and-insight/press-release/fraud-report-2025-press-release)

[^6]: **UK Payment Systems Regulator — APP Fraud Reimbursement Scheme (October 2024).** Mandatory reimbursement via Faster Payments and CHAPS, capped at £85,000, cost shared 50/50 between sending and receiving PSP. Applies to consumers, micro-enterprises, and small charities only. Replaces the voluntary Contingent Reimbursement Model Code. [psr.org.uk](https://www.psr.org.uk/our-work/app-scams/)

[^7]: **EU Instant Payments Regulation — Regulation (EU) 2024/886 (March 2024).** Mandates Verification of Payee (VoP) for all SEPA credit transfers. Eurozone deadline: 9 October 2025. Non-euro area deadline: 9 July 2027. Four possible outcomes: match, close match, no match, NOAP. Published in the Official Journal of the EU. [eur-lex.europa.eu](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32024R0886)

[^8]: **European Parliament — PSD3/PSR Political Agreement (November 2025).** Trilogue agreement on Payment Services Directive 3 and accompanying Payment Services Regulation. Extends liability toward electronic communication service providers for failure to remove fraudulent content. Payer remains default liable except in impersonation cases. [europarl.europa.eu](https://www.europarl.europa.eu/news/en/press-room/20251121IPR31540/payment-services-deal-more-protection-from-online-fraud-and-hidden-fees)

[^9]: **MAS & IMDA — Shared Responsibility Framework Guidelines (December 2024).** Joint announcement and guidelines implementing the SRF for phishing scams. Waterfall liability model: FI first, then telco if FI complied. Telco duties: authorised aggregators only, block unauthorised sender-ID SMS, implement anti-scam filter. No liability cap. Effective 16 December 2024. [mas.gov.sg](https://www.mas.gov.sg/regulation/guidelines/guidelines-on-shared-responsibility-framework)

[^10]: **Australian Parliament — Scams Prevention Framework Act (February 2025).** Amends the Competition and Consumer Act 2010. Designates banks, telcos, and digital platforms as regulated sectors. Civil penalties up to AU$50M. Sector-specific codes and reimbursement mechanisms to be published by Treasury. [legislation.gov.au](https://www.legislation.gov.au/Details/C2025A00013) — Treasury explainer: [treasury.gov.au](https://treasury.gov.au/sites/default/files/2025-01/p2025-623966.pdf)

[^11]: **MAS & IMDA — SRF Consultation Response (October 2024).** Confirms that the SRF is strictly limited to phishing scams resulting in unauthorised transactions. Authorised push payment fraud — including CEO/CFO impersonation, BEC, and deepfake-induced transfers — is explicitly outside the SRF's scope. The framework rejects industry feedback to expand coverage to authorised fraudulent payments. [mas.gov.sg](https://www.mas.gov.sg/news/media-releases/2024/mas-and-imda-announce-implementation-of-shared-responsibility-framework-from-16-december-2024)

[^12]: **UK Payment Systems Regulator — Confirmation of Payee (CoP).** Technical guidance on how CoP checks whether the name entered by the payer matches the registered name on the destination account. Alerts notify the payer of match, close match, or no match. CoP operates at the Faster Payments layer only — it does not intercept or assess the communications interaction that preceded the payment instruction. [psr.org.uk/confirmation-of-payee](https://www.psr.org.uk/our-work/app-scams/confirmation-of-payee/)

[^13]: **UK Online Safety Act 2023.** Section 34 covers fraud offences. Ofcom granted power to impose fines of up to 10% of qualifying worldwide revenue for platforms that fail to implement systems preventing fraudulent content. Important distinction: these fines flow to the regulator, not to fraud victims. Platforms have regulatory exposure under OSA but zero direct financial liability to APP fraud victims. [legislation.gov.uk](https://www.legislation.gov.uk/ukpga/2023/50/enacted)
