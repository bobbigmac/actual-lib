# ActualJobs.click concept analysis

#### You:
could this tech make sense as a job board? ideally it would be free for basic use, with paid promotion or a search listing bump. Thinking linkedin without the trash and no social feed to incentivise constant stream of bs, relatively formal CV format, with support for external links. Maybe would prefer to find some way to keep it even, and use only the job spec/tags and trust of the signer for positioning. Our unique positioning is we are not a social network, we list jobs, you want a job, come and find a job, you want workers, come and find workers. Want to hire a listing, you attach your invite with invitation to apply, which can then go offline. It changes the system incentives compared to 'modern' (shit) job sites, but simplifies the process for everyone, keeps trust in the system, and still gives employers the ability to use AI to pre-filter candidates, but lets them do it in harmony with the candidate (candidates aren't incentivised to waste their time applying for poor match jobs). 

#### ChatGPT:
Yeah, it absolutely could — in fact, what you’ve sketched lines up with some of the most common frustrations people have with existing job boards:

### Why it makes sense

- **Signal over noise:** Traditional job boards and LinkedIn optimize for engagement and volume, which rewards spammy listings and generic applications. A trust-weighted, schema-driven model would naturally surface the most credible, relevant jobs without requiring constant scrolling or hype.
- **Accountless model with signed keys:** Cuts down on surveillance capitalism, removes the profile-as-content problem, and reorients incentives away from clout farming.
- **Symmetry of incentives:** Employers want qualified candidates; candidates want relevant opportunities. If both sides use tags + trust, you’re aligning incentives instead of creating an arms race of keyword stuffing.
- **Formalization of structure:** Schemas enforce a “CV-like” quality bar on posts, keeping things cleaner than the freeform mess of existing platforms.
- **Federated / P2P design:** Avoids central chokepoints, allows for cultural variation across servers, and distributes costs. Also provides resilience against takedowns and spam floods by isolating bad actors to untrusted peers.
- **Pundit system per domain:** Encourages genuine expertise to be surfaced within specific tags — e.g. a highly trusted “AI ethics” commentator might not automatically get visibility in “mechanical engineering.” That keeps authority scoped.

### Trade-offs & challenges

- **Noise & Sybils:** Even if low-trust accounts don’t get surfaced, flood attempts can still chew bandwidth and storage. Your “grace period + server quotas” idea helps, but it needs tuning to avoid DoS by spam servers.
- **Fragility by design:** For a job board, losing your key means losing your CV + trust. That fits your “don’t lose your key” ethos, but for real-world adoption, you’ll need clear, user-friendly key management UX (backups, hardware keys, etc.), or adoption will stall.
- **Legal obligations:** Jobs can carry sensitive info (discrimination, scams, wage theft). Even if you’re just a transport layer, many jurisdictions may still treat you as a publisher. Federating risk helps, but expect pushback.
- **Employer adoption:** Companies are used to posting on LinkedIn/Indeed because that’s where candidates are. To draw them, you’d need (a) a decent candidate base, (b) better filtering/matching tools, and (c) some assurances against fraud. The trust system could be that hook, but it will take time to mature.
- **Monetization vs ethos:** If the project’s core principle is “never sell attention,” you’re limiting revenue levers. Donations, patronage, or voluntary hosting fees could work, but growth would likely be slow and community-driven rather than VC-scale.

### Net assessment

As a **job board**, the idea is unusual but viable — especially if you position it as *the place for high-signal, bullshit-free job postings and applications*. Think of it as the opposite of LinkedIn: no feeds, no dopamine-driven engagement, no endless spam. Just structured postings, cryptographic trust, and community-governed credibility.

The challenges are mostly *ecosystem and sustainability*: bootstrapping trust, getting enough good jobs listed, ensuring legal compliance without becoming “just another moderated platform,” and building tools so non-technical candidates don’t get lost in key management.

---

Want me to map out **a concrete go-to-market strategy** (how to attract the first serious employers and candidates without ads), or do you want me to focus next on **the trust/reputation math** to see how it plays out in a hiring context?


#### You:
I don't like selling positioning, I considered some kind of 'coach' system, where 'recruiters' find matches between jobs/workers and handle the invites, which would reduce 'complexity' for regular users (the tech isn't difficult, but isn't as easy as say using SSO via google for an account that can track you if you don't care), but I don't think that makes sense in a system that's supposed to reduce friction between hirers and candidates by design. 

Selling training might work, like 'upskill via our udemy partnership', or potentially 'the college of your city is offering training for this role as a junior pipe fitter' and charging a small fee to the school or training company, which we can allocate by tags, but it does make the 'advertiser' a necessary site content publisher (no backend black-box for advertisers, but putting them in the open). Hosting text is cheap if we do it right, so we don't need to earn much, but earning something would be nice to be able to scale by paying for visibility on other platforms.

Our trust system is the usp, but in this realm, we'd have to assume more up-front/default trust than a 'news' site, so might be slightly more vulnerable to gaming/scammers. Would probably address this by not immediately deleting signers on a single major violation, but by keeping their reports, so mistreated candidates report bad hiring practices (ghosting, sketchy response, company history with evidence, etc) by posting schema'd reports as issues under a listing, and hirers can post likewise to candidates, either openly and vaguely for legal safety, like { "type": "noshow" } or even positive points like { "presentation": "good", "status": "declined", "reason": "good candidate, but selected another, recommend, easy to talk with" }, and it lowers the 'commitment' while letting both groups leave records in the open, in good faith. Meaning in this system, comments should probably gain trust.

#### ChatGPT:
Got it — you’re thinking about this as a **job board that’s not a social graph** but a *trust graph* built around structured, signed content. That makes sense, and the refinements you just laid out push it closer to something employers and candidates could actually use. Here’s how your latest additions fit:

---

## Refinements for the Job Board Variant

### 1. Funding & Sustainability

- **No pay-for-positioning** — core principle is that *placement is earned by trust + tags*, not bought.
- **Revenue via education & training:**
  - Lightweight affiliate/partnership model with MOOCs, bootcamps, local colleges.
  - Training offers appear as **normal posts** in relevant tag feeds (e.g. “pipefitting”, “cloud devops”) and are subject to the same trust system.
  - Sponsors don’t get extra ranking weight; they’re just paying to have their material mirrored on the official servers.
- **Optional donations** — “like Wikipedia, but for jobs/news.” Can keep core infra running without selling attention.

---

### 2. Trust System Adjustments for Hiring Context

- **Higher default baseline trust** for new signers (compared to news). Otherwise, job seekers can’t be seen at all.
- **Gradual degradation for violations** (not immediate death). Severe categories (fraud, harassment, CSA) → instant key kill + tombstone.
- **Reputation from comments**:
  - Comments on job posts (applications, clarifications, feedback) can contribute to **tag-scoped trust**.
  - To prevent “klout spam”: weight comments lower, require acceptance/endorsement by trusted participants before they affect trust.

---

### 3. Structured Transparency: Job–Candidate Interactions

- **Job Issues (child posts on a job)**:
  - `{ type: "noshow" }`
  - `{ type: "declined", reason: "candidate accepted another offer" }`
  - `{ type: "hired", candidate: signer_pubkey }`
  - `{ type: "feedback", field: "communication", rating: 0.8 }`
- **Candidate Responses (child posts on issue)**:
  - `{ type: "report", reason: "ghosted", details: "no feedback after interview" }`
- These are *not* private. They’re signed, visible, and accumulate trust/reputation effects.
- Trust update rules:
  - Positive feedback on a candidate’s application raises their trust (esp. in the job’s tags).
  - Validated abuse reports (when corroborated by trusted signers/servers) decrease trust for the offending party.

---

### 4. Incentive Balance

- **Candidates**: earn trust by submitting good applications, by making constructive comments, and by receiving positive feedback from employers. This makes “spamming” self-defeating — it risks immediate downvote-based trust loss and potential bans.
- **Employers**: earn trust by posting accurate, clear, and fair job specs, and by issuing transparent feedback (instead of ghosting).
- **Pundit layer**: Trust is calculated **per tag**; only high-trust signers within “data science” should surface as pundits on DS jobs. Prevents general clout from dominating unrelated fields.

---

### 5. Federation & P2P Extension

- **Server-level trust** still the outer layer. Servers with bad moderation histories get de-weighted or defederated.
- **Client ephemeral servers (WebRTC / LAN sync)**: optional, good for ad-hoc events (job fairs, conferences). They provide short-term availability while allowing later mirroring by trusted servers.
- **No central choke point** — if one server refuses certain training posts or certain candidate reports (legal risk reasons), others can choose to accept and distribute them.

---

### 6. Legal & Safety in Hiring

- **Abuse categories expanded**:
  - Candidate violations: spam, misrepresentation, no-show, harassment.
  - Employer violations: ghosting, misclassification, discrimination, scam.
- **Enforcement**: one-strike for hard bans (fraud, CSA, harassment); multi-strike for softer issues (no-shows, low-quality posts).
- **Audit trail**: signed job specs, applications, and reports provide verifiable histories of how actors behaved. This could be a *selling point* to good employers — your reputation is portable and transparent, not based on private recruiter notes.

---

### 7. Cultural Positioning

- **Not LinkedIn**: no endless feed, no influencer economy.
- **Public record**: jobs, applications, and feedback are structured, timestamped, and signed.
- **Community curation**: trust-weighted reactions replace opaque algorithms.
- **Honest feedback**: normalized schemas for rejections, recommendations, and red flags create a professional but transparent record.

---

👉 This makes sense as a *job board for people who hate job boards*. It’s not really a mass-market LinkedIn replacement — it’s a high-signal, reputation-based exchange. That’s a viable USP, especially if you want to attract technical users, journalists, or communities that distrust mainstream platforms.

Do you want me to sketch a **sample schema set** for job posts, applications, and feedback (in JSON) so contributors on GitHub can immediately see the data contracts?


#### You:
I think we can drop punditry inherited from the news system. 

Everyone is both a potential hirer and candidate, and trainer/advertiser too I suppose, posting to hirer+candidate schemas are free, posting to advertiser schema costs money, but you get a little extra control (auto expiry, timespan say). Anyone can comment on anything in theory (in the old system) using child content of the appropriate schema so it's rendered sensibly, but we may want to check against griefing (say candidates putting bad reports on competing candidates), it might be good enough to allow posts to happen, but not surface them directly on the UI, or on consideration/testing we might discover that it should be xor, like candidates can post as a candidate on jobs, and the reverse, which means each signer has a few roles, and under normal operation the type of post surfaces the specific role and restricts irrelevant content. Needs work. I want candidates to be able to support each-other like { "peer-review": "I worked with Dave in 2024, he's great, the best _role_ I've ever worked with" }, like when linkedin added community tagging, before they turned it into a klout factory, and as long as our system orders by and surfaces trust, anyone reviewing those profiles can be confident of the honesty of that.

Constructive comments is nice, especialy if we allow replies and have fairly strict rules about what types of replies are acceptable. This site strangely needs a little more moderation than our news site because it's bi-directional, candidates can ask questions about roles publicly, and the hirer can answer it or issue an amendment to their post answering it in the body (with that logged and viewable, obvs). 

CVs do however contain a lot of personal info, so we'll have to cut down on the personal details and make it about the actual role. Some formal fields on skills and levels might help, but we 'could' use an async (public) key connected to the job-post to let the applicant encrypt their 'cover letter' so they can write individual applications with more personal details than an open forum would allow. I think that solves it. We are still exposing publicly information about how many candidates for specific tags in the area, so we need some caution about things like wanting location to be vague. Precise tinder-like mapping does exist in the open (I can go on uber and see exactly where 200 guys are right now, and get some of the their names and number plates, so it's not exactly unusual), but I don't think we need that level of precision, and maybe instead let profiles set their corridors, even with weighting ("I'd prefer to work in this geo-polygon where I can commute by bike, but I'll consider roles in this geo-polygon if you pay a little more to run my car") and while job postings would need to set a pretty precise location, businesses are used to publishing their location more. I think allowing them to set it vaguely (and some not wanting to set their company name even, to avoid competitors knowing they're hiring, like game development studios who have high secrecy, is actually fine by me, if they have the trust, they don't _have_ to list the exact company, and can send that privately like candidates can with their personals).

We don't need p2p for this platform. At least not initially. Might be useful in future use cases, but let's assume a single site for now. It overcomplicates a relatively simple tech stack and doesn't give us any added value. It will be open source, but mainly for trust and bug-spotting. Someone might run their own service, but it's just foss, not strictly 'federated'.

I suppose we can also handle fully remote, but my main interest is 'real jobs', tho community sense and feedback might drive how this works. In theory we could facilitate with payment support for freelancers and still work (could even encourage keeping encrypted comms in-system) but this needs a very different set of infrastructure support (files, schedules, milestones, etc), so can be considered for potential expansion, but not a focus of early versions.

No strikes for no-shows, shit happens, it goes on your log, but only hard bans get you out of the network. I'm okay with storing a lot of 'dead head' accounts, we can always add a 'deleted if no activity in 3 years' for abandoned signers cleanup, since we don't have 'accounts', it's just a text record in postgres or whatever. We can even support auto-expire (on confirmed hire, both hirer and candidate post confirmation, delete post and all content, tho there may be some legal considerations to check). Categories, and to some extent, schemas are managed via github prs, a little like rules on a subreddit, they're adjustable to some degree, but generally stable.

#### ChatGPT:
That refinement makes it much sharper as a **job-board-only** variant of your “ActualFuckingNews” model. Dropping punditry simplifies the system, and the bi-directional structure (hirers and candidates both posting structured objects under clear schemas) maps neatly to the trust-graph ethos. Here’s a README-style guide for *this version*:

---

# ActualFuckingJobs — Project Guide (README Style)

## 1. Purpose

A **trust-weighted, schema-driven job board** that removes the noise, manipulation, and clout-gaming of LinkedIn/Indeed.

Principles:

- **No profiles/accounts** — everyone is just a signing key.
- **Trust over hype** — placement in feeds is earned by trust-weighted reactions, not by money or engagement tricks.
- **Structured data** — jobs, CVs, and feedback are JSON-schema-validated.
- **Symmetry** — anyone can be a hirer, candidate, or trainer; roles are just different schemas under the same key.
- **Transparency** — all content and feedback are signed, timestamped, and visible; moderation is minimal but decisive.
- **Never sell placement** — revenue (if any) comes from training/education partnerships, not ads or paid bumps.

---

## 2. Roles & Identities

- **Signer (Key)**: the only identity unit. Generate a keypair → that’s you.
- **Roles** (schema-level, not separate accounts):
  - **Hirer**: posts jobs.
  - **Candidate**: posts CVs and applications.
  - **Trainer/Advertiser**: posts training opportunities (paid schema).
  - **Commenter**: replies to jobs or CVs.
- **Trust** attaches to `(signer, tag)` pairs. Keys are fragile: lose the key, lose your reputation. Content remains public, but you start over.

---

## 3. Content Types (Schemas)

- **Job Post**\
  Fields: title, description (markdown), tags (skills, location polygons, seniority), compensation (optional ranges), location (precise or vague).\
  Options: expiry date, auto-tombstone on hire confirmation.

- **Candidate CV**\
  Fields: skills, experience, availability, location corridor(s) with preference weights, external links (portfolio, GitHub).\
  Limited personal identifiers — CVs are skills-oriented, not personal PII dumps.

- **Application** (child of Job Post)\
  Public structured info (skills, tags, short motivation), plus optional **encrypted attachment** (cover letter with personal details) using a per-job public key.

- **Feedback / Issues** (child of Job or Application)\
  Examples:
  ```json
  { "type": "noshow" }
  { "type": "declined", "reason": "accepted_other_offer" }
  { "type": "feedback", "field": "communication", "rating": 0.8, "comment": "Easy to talk with" }
  { "type": "report", "reason": "ghosted", "details": "No response after interview" }
  ```

- **Reactions**\
  General reaction schema with types (👍, 👎, 📰 credible, 🚫 spam, ⚠️ misleading). Simple UX, richer trust signals.

- **Tombstone**\
  Deletes body, keeps metadata for audit (id, signer, tags, timestamps, reactions).

---

## 4. Trust & Ranking

- **Vote weight** = (signer\_trust^α) × (server\_trust^β).
- **Temporal dampening** on votes prevents brigading (recent surges are down-weighted).
- **Score** = Σ(weighted reactions), no recency multiplier.
- **Feeds** are filtered by user:
  - Min trust threshold.
  - Tag filters.
  - Optional recency window.
- **Trust updates**:
  - Posts gain/lose trust via reactions.
  - Comments and feedback can grant tag-scoped trust (e.g. “helpful answer” on a job post).
  - Spam/misleading reactions from trusted signers reduce trust.
- **No anti-consensus penalties** — majority opinion isn’t always truth.

---

## 5. Moderation & Lifecycle

- **Hard bans** (fraud, CSA, harassment) → kill key, tombstone content.
- **Soft issues** (no-shows, low-quality posts) → logged in signer’s record, visible but not instant death.
- **Content deletion**:
  - Tombstone removes body, keeps metadata for N days.
  - Confirmed hires can auto-expire job + application trees.
  - Old/abandoned keys can be cleaned after X years inactivity.

---

## 6. Transparency & Griefing Mitigation

- **UI surfacing rules**:
  - Core feeds only show posts that meet trust/recency filters.
  - Feedback/comments may exist as child content but don’t auto-surface; UI can group or collapse low-trust/griefy items.
- **Peer feedback** (e.g. “I worked with Dave, great teammate”): allowed, structured, and trust-weighted.
- **Employer vs Candidate logs**: each signer accumulates visible history of interactions, which others can inspect when evaluating trust.

---

## 7. Federation / Architecture

- **Phase 1**: single-site, centralized hosting (simpler infra, faster iteration).
- **Phase 2 (optional)**: open-source federation/mirroring, WebRTC micro-servers for events.
- **All code FOSS** for transparency and contributions.

---

## 8. Media & Privacy

- **Images**: supported (JPG/WebP, size capped). Any server can disable image hosting locally.
- **External links**: always allowed.
- **Encrypted attachments**: per-job keys allow candidates to include personal details privately.
- **Location**:
  - Jobs: can specify exact or vague coordinates.
  - Candidates: publish “corridors” (polygons) with optional weightings (e.g. “prefer bike commute zone, will consider wider metro if pay > X”).
- **No full PII in open CVs**.

---

## 9. Revenue

- **Training/education posts** (paid schema): colleges, bootcamps, skill-programs can post to relevant tags.
- Treated as content, not ads; still subject to trust ranking.
- Core site funded by donations and light partnerships.
- **No selling placement, no attention economy.**

---

## 10. Governance & Community

- **Schemas & categories** defined in GitHub repo.
- Adjusted via PRs/issues; relatively stable to keep UX consistent.
- **Style guides** encourage: clear specs, transparent feedback, no manipulative language.
- **Community norms**: professional, open, signed accountability.

---

## 11. Roadmap

1. **MVP**: Single server, job posts, CVs, applications, reactions, trust weighting.
2. **Feedback logs**: structured employer ↔ candidate feedback.
3. **Encrypted attachments** for applications.
4. **Image upload support** (with caps).
5. **Training posts** (paid schema).
6. **Cleanup policies** (abandoned keys, auto-expire jobs).
7. **Federation/mirroring** (optional future).
8. **Freelance/contract extensions** (files, milestones, payments).

---

This trims out punditry, keeps the **trust-weighted job + application + feedback** core, and pushes moderation just enough to handle the bi-directional friction.

Want me to now draft a **concrete set of JSON schemas** for `job`, `cv`, `application`, and `feedback` so you (or contributors) can start building the MVP?


#### You:
the news project changed to actualnews.click (cheap domain, less sweary for professionalism), this would do the same, actualjobs.click (or if unavailable, some other gtld), closely connected with actualnews.click, and probably part of my better britain bureau think tank project list. We're dogfooding here.

#### ChatGPT:
That makes sense — **actualjobs.click** as a sibling to **actualnews.click** fits neatly, and tying both into your “Better Britain Bureau” umbrella gives you a coherent narrative: *trusted, no-BS civic infrastructure*.

Here’s how I’d frame it:

---

# ActualJobs.click — Project Overview

## Relationship to ActualNews.click

- **Shared ethos & tech base**: both projects use the **signer + trust + schema** model.
- **Different domains**:
  - **ActualNews.click** → citizen journalism & news.
  - **ActualJobs.click** → jobs, CVs, applications, training offers.
- **Shared infrastructure**:
  - Common codebase (`afn-core` or similar) with pluggable schema sets.
  - Shared trust logic (signer trust, server trust, reactions).
  - Separate deployments/servers, but federatable if desired later.

---

## Domain & Branding

- **Domain**: `actualjobs.click` (first choice).
- Fallbacks: `actualjobs.news`, `actualjobs.work`, `actualjobs.network`, or under a parent like `jobs.betterbritain.click`.
- Tagline:
  > *“Actual Jobs. No noise. No spam. Just work and workers.”*
- Visual language: plain, text-first, matching the minimalist/anti-bullshit vibe of ActualNews.

---

## Positioning

- **For candidates**:
  - A place to publish CVs in a structured, signed format.
  - Apply with public skills + optional private, encrypted cover letters.
  - Build visible, tag-scoped trust through applications and peer reviews.
- **For hirers**:
  - Post structured job specs (tags, location polygons, pay ranges).
  - Earn trust by treating candidates fairly (no ghosting, transparent feedback).
  - Use trust-weighted filtering to surface high-signal candidates.
- **For trainers/educators**:
  - Post courses or training opportunities via the **advertiser schema** (paid).
  - Still subject to trust ordering; no “buying” top placement.

---

## Incentives

- No clout, no social feed, no black-box ads.
- Employers and candidates both accumulate transparent histories.
- Feedback (noshow, hired, good candidate, ghosted, etc.) builds or erodes trust in specific tags.
- Training partners can reach relevant audiences by posting into the same structured feeds, with limited paid features (expiry, scheduling).

---

## Governance & Community

- **Schemas & categories** maintained in the shared GitHub repo.
- **Better Britain Bureau** provides initial stewardship.
- Contributions (new schemas, style guides, moderation rules) happen via PRs/issues.
- Each project (news vs jobs) can evolve its own schema sets while sharing the same underlying protocol.

---

## Roadmap

1. **Launch actualjobs.click MVP**
   - Deploy shared backend (afn-core) with `job`, `cv`, `application`, `feedback`, and `advertiser` schemas.
   - Trust slider + tag filters in the UI.
   - Encrypted attachments for applications.
2. **Feedback/Interaction Layer**
   - Schema’d feedback for hirer↔candidate interactions.
   - Peer reviews for candidates.
3. **Training Marketplace**
   - Paid advertiser schema for education/training providers.
4. **Cross-pollination with ActualNews.click**
   - Shared trust graphs (optional).
   - Cross-linking: job posts can cite news posts (e.g. “industry context”), news posts can reference job data (“labour market signals”).
5. **Expansion**
   - Add support for freelancers & contracts (files, milestones, payments).
   - Optional federation/mirroring if adoption grows.

---

This way, **ActualJobs.click** feels like the “jobs sibling” of **ActualNews.click**, both running on the same trust-weighted, schema-based, signed-content engine, but each with its own domain-specific schemas and community rules.

---

Want me to draft a **combined “Better Britain Bureau” umbrella README** that frames both sites as part of one ecosystem (with shared principles + shared tech), so you’ve got a cohesive public-facing manifesto?


#### You:
Minor notes... No images, we don't need them for jobs or candidates, and they open us to risky content. Harder sell for advertisers, maybe third party hosting and embed is earnable with trust. Hmmm, still has problems. Needs further consideration. 

We could share trust graphs cos they're just a key on the content.
Would need to structure things like signing and verification in their own lib, also foss, then we just dogfood that between projects, but use case is quite different so sites don't share much down the line functionality past the basic principle tech that let's us avoid accounts. Could be used by others for their own products, actualdating say. If we use the slightly federated part to publish and sync trust, that's also shared/open for anyone, cos your key is portable.

I don't care about semantic collision on tags. Jobs and news aren't directly connected, except that either may query the other for a trust dataset by key. Trust on tags is sent but doesn't have to maintain compatibility, any consumer may map keys if they like, but nothing to do with us, we don't need the headache of being the arbiter of what words mean.

Public key on jobs is the right call, but we also want candidates to assign pubkeys on their CV, so hirers can send private comms. Might be noisy if high-spam issues, no notification unless trust (configurable) is an easy pick, but auto-ignore on server (essentially storing it but not informing the user unless they query specifically for low trust comms). Beats email by keeping it in-system, but adds requirement for user to check-in. Maybe daily/weekly/monthly email digest as side-service for 'good' content, with line like "12 ignored messages from low-trust signers, review here if desired". This will start fairly relaxed, so most early content gets through, with the caveat communicated on the UI that thresholds may change with time (letting user override by setting on their CV/job). This also openly lets communicators see how much trust they need to get straight through. You may list multiple CVs under the same signer (and multiple jobs/ads) and are listed in your profile. The application itself is CV<->JOB. 

#### ChatGPT:
That makes sense — you’re really honing in on the **core primitive**: a shared *signed-content + trust graph* substrate, with different front-end “verticals” (news, jobs, maybe dating later) consuming and presenting different schema types. Everything else (ads, comments, moderation, notifications) becomes a thin layer of rules or UX on top.

Here’s how I’d document these **minor but important notes** in the *ActualJobs.click* guide:

---

## Media Handling

- **No inline image uploads** in the base system to reduce abuse, moderation load, and hosting liability.
- **External references allowed**: job or CV posts can include links to portfolios, GitHub repos, or external images if desired.
- **Future extension**: a separate “media” schema could be introduced, but this would be opt-in, and federation rules can allow/deny it.

---

## Advertisers

- **Base right**: any signer can post job listings or CVs at no cost.
- **Advertiser schema**: a distinct, paid schema for training providers, courses, or promotional job-adjacent content.
- **Trust-gated surfacing**: even paid posts are still subject to trust weighting and ordering — money buys you the right to post under that schema, *not* to override trust filters or appear above more trusted content.
- **Optional server controls**: each server operator decides whether to host “advertiser” schemas at all.

---

## Trust Graph Reuse

- **Global, portable trust graph**: all projects (news, jobs, dating, etc.) use the same notion of `(signer_pubkey, tag) → trust score`.
- **Cross-domain use**: ActualJobs.click and ActualNews.click can read/write to the same trust dataset.
- **Interpretation is local**: sites aren’t required to harmonize tag semantics; a given key’s “trust:golang” on jobs can be interpreted differently from “trust:politics” on news.
- **Open infrastructure**: the trust graph library should be split into its own FOSS module so any project (including potential “ActualDating.click”) can integrate.

---

## Communication & Feedback

- **Comments remain structured child posts**.
  - Candidates may comment on job posts (e.g. questions).
  - Employers may comment on candidate CVs or on applications.
  - Peer-reviews allowed under a dedicated schema:
    ```json
    {
      "type": "peer-review",
      "about": "candidate_pubkey",
      "context_tag": "data-science",
      "comment": "I worked with Dave in 2024, great teammate",
      "rating": 0.9
    }
    ```
- **Visibility rules**:
  - Low-trust comments are *stored but not surfaced by default*.
  - Users can choose to “opt in” to see low-trust communications.
- **Feedback categories**:
  - Candidates: `{ "noshow": true }`, `{ "skills_confirmed": true }`, etc.
  - Employers: `{ "ghosted_candidate": true }`, `{ "unclear_role": true }`.
- **No hard bans for flakiness**: no-shows or declined offers are logged transparently as data points; only egregious abuse (fraud, harassment, spam) triggers immediate key tombstoning.

---

## Candidate ↔ Hirer Privacy

- **Job posts**: may use precise or vague geolocation (polygon corridors for commute preference vs. exact address if willing).
- **Candidate CVs**: should omit personal PII (address, phone, date of birth).
- **Contact**: each signer can publish a *public key for applications*. Applications can contain:
  - Public portion (skills, tags, portfolio links).
  - Encrypted payload (cover letter, CV details, email, phone).
- **This keeps personal data in-system** — no need for email scraping, less spam.
- **Notifications**:
  - High-trust comments / feedback → immediate notification.
  - Low-trust content → silently stored; appears only if the candidate actively checks or raises trust threshold.
  - Optional digest emails (daily/weekly/monthly) for “trusted new opportunities.”

---

## Developer Guidance

- **Core Library**: separate out signing, verifying, and trust graph logic into a reusable library (`afn-signing`).
- **Project Plugins**:
  - *News schemas* (articles, corrections, comments).
  - *Jobs schemas* (jobs, CVs, applications, feedback).
  - *Future* (dating, events, etc.).
- **Schema governance**: managed in the Better Britain Bureau GitHub repo, via PRs + community consensus.

---

✅ This keeps the **job board** clean, transparent, and non-exploitative, while maintaining interoperability with the broader **Actual*.click*\* ecosystem.

Would you like me to go ahead and **draft the initial `schemas/jobs/` JSON files** (job post, CV, application, feedback) in a way that fits your lib-first architecture? That would give you a concrete starting point for `actualjobs.click`.


#### You:
this is wrong. "Cross-domain use: ActualJobs.click and ActualNews.click can read/write to the same trust dataset.". Keep your own set, you _may_ replicate by requesting based off a specific key (so someone has to publish to you with their key before you know their key), but not shared dataset. I used the word 'sync' incorrectly. They are seperate products that support each other and share tech, not data. In theory, since our data isn't personally identifiable, this is GDPR-compliant by design, but may need to review as we go, because if people are putting info in listings/cvs that they shouldn't, we may need to revise guidance or fix ingestion to reject. I don't want ANY personal data in the system, that's what the e2ee is for.

I think it's discussed in the original news site, but hasn't come up under jobs yet, but amendments are possible, tho we lose community/shared approvals. If you are the signer, you can amend, update or remove your content at any time. Ideally if you want to vanish you just tell the server to invalidate your key and all your content is nuked. Sub-content from others is retained but loses what its attached to, becomes 'flushable detached content' or similar framing (thinking on reddit when a post is deleted, comments lose visibility, but they're still there).

A style guide for the various interactions is highly advisable "Be professional", etc. Allowing private comms replying to open comms might be tricky, since signer doesn't publish pubkey globally, but on a job/cv, maybe we can always use top level key for private comms, no matter how deep we went, so we don't need an actual 'application' top level record, as it's a just child of job, and invitation may be a child of cv. I would discourage extended personal info even in encrypted comms, nothing beyond what is absolutely necessary, mainly because of habit, uk law has some opinions about CV content. 

We _could_ have a simple server-side bit of personal data that the signer can 'soft publish' and the requester can actively request (logging the request openly) which our server would encrypt for that reader (so you don't have to be available to answer small questions) but this would be a later addition for convenience, just to save some time and support auto-reply-on-invite or one-click-reply-on-invite (like a default cover letter in response to an invitation) but this comes with its own issues, and technically takes place outside the 'real' system.

Sidenote, actualevents would be nice too. Basically set-up our own plenty-of-crypto-fish here ;)

#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
Noted. Here’s a tight patch to the spec reflecting your corrections.

# ActualJobs.click — Corrections / Updates

## Trust & Data Scope

- **No shared trust dataset.** Each product (ActualJobs.click, ActualNews.click, etc.) keeps its **own** trust graph and content store.
- **Per-key import only.** A site may **replicate trust for a signer** *after* that signer publishes to it (or the site explicitly requests it by key). No global sync.
- **Tag semantics are local.** No cross-product tag harmonization. Consumers may map tags if they want; we don’t arbitrate meaning.

## Privacy / GDPR posture

- **PII-free by default.** Public CVs and posts must not include personal data. E2EE is the only path for necessary personal details.
- **Schema + ingestion guards.** Validators reject obvious PII fields (phone, email, DOB, street address). UI copy warns and blocks common patterns.
- **Review as we go.** If UK/EU guidance shifts, we update the style guide and validators.

## Amendments & Deletion

- **Signer-controlled amendments.** Authors can amend, update, or remove **their own** content anytime.
- **Key invalidation nukes content.** On author request, server can invalidate a key and **tombstone all authored bodies**.
- **Detached subcontent.** Child items from others remain but become **detached/flushable** (hidden by default, GC eligible per policy).

## Comms Model (public + private)

- **Public Q\&A** stays as structured child content (with surfacing rules to suppress griefing/low-trust by default).
- **Private replies** always encrypt to the **top-level owner’s public key** (job→hirer key, CV→candidate key), regardless of thread depth.
  - This means **no separate top-level “application”** is strictly required; an application is a child of Job, and an invite is a child of CV.
- **PII minimization.** Even in E2EE, discourage extended personal info; provide tight templates.
- **Noise control.** Low-trust private messages are stored but **auto-ignored** unless the recipient opts in (trust threshold configurable per CV/Job).
  - Optional **digest email** (daily/weekly/monthly) for trusted items; line item like “12 low-trust messages held — review”.

## Optional “Soft-Publish Vault” (later)

- Small server-side personal data blob a signer can **soft-publish on request**.
- Request is publicly logged; server encrypts the blob to the requester’s key and returns out-of-band.
- **Out of core protocol**, clearly marked as a convenience layer.

## Media

- **No inline images** in core jobs/CVs. External links allowed.
- Future: a **media/embeds** schema could be trust-gated by servers, but default stance is **off**.

## Moderation & Roles

- **No punditry.** Everyone can be hirer/candidate/advertiser (paid schema for training only).
- **No strikes for no-shows.** Logged, not punitive. Only hard bans (fraud, harassment, CSA, obvious scam) kill keys.
- **Griefing controls.** Anyone can comment, but low-trust child content doesn’t surface by default; visible on demand.

## Libraries / Reuse

- Split **signing/verification and trust math** into a FOSS lib (used by Jobs/News/Events/etc.).
- Products **share tech, not data**. Others can build atop the lib (e.g., `actualevents.click`, `actualdating.click`).

## Misc

- **Public keys on both sides.** Jobs expose a pubkey for private applications; CVs expose a pubkey for private invites.
- **Multiple artifacts per signer.** A signer may publish multiple CVs and multiple Jobs; they list under that key’s profile index.
- **Auto-expire.** Jobs can auto-tombstone on confirmed hire (both sides post confirmation). Consider legal wrinkles; make it opt-in.

If you want, I’ll roll these into the README and push an updated structure for:

- `PRIVACY.md` (PII rules + validator guidelines)
- `COMMS.md` (public Q\&A, private envelope rules, trust thresholds, digests)
- `LIFECYCLE.md` (amend, delete, key invalidate, GC of detached content)


#### You:
Okay we've got substantial feedback from a review and iterate cycle with the help of grok.
Integrate the tweaks I've described (user blocks are primary consideration, grok blocks are support) into our project here. Read the file fully and think about the integration in depth.

#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
# ActualJobs.click — Integrated Spec (post-Grok review)

## Scope changes

- **No cross-product trust sync.** Jobs/News keep separate data; per-key import only after the signer publishes to you. Tag semantics are local; no arbitration. fileciteturn0file0
- **No images** in jobs/CVs. External links ok. (Advertiser/media later if ever; trust-gated.) fileciteturn0file0

---

## Identity & keys

- **File key UX**: generate client-side; “this file is you; lose it → start over.” KISS; no recovery service. Allow emailing the key as a convenience, and easy storage in password managers. Hardware wallet login optional (WebUSB/WebHID/NFC/BT), marked “advanced.” fileciteturn0file0
- **Key invalidation** nukes authored bodies; child content becomes **detached/flushable** (hidden by default, GC-eligible). fileciteturn0file0
- **Legacy key reference**: new key may **reference old key** for human context.
  - *Proven transfer*: old key pre-signs the new key before deletion.
  - *Unproven*: allowed with a visible “unverified” warning.
  - Trust never migrates automatically; consolidation is **in code**, not in-network metadata. fileciteturn0file0
- **Org keys (parent/child)**: hierarchical keys; parent can deactivate children; children inherit visibility context (not auto-trust). Basis for monetisation (see below). fileciteturn0file0

---

## Content model (schemas)

- **Job**: title, description (md), tags (skills, seniority), pay range (optional), location (exact or vague polygon), expiry, `isRecruiter?`, `isThirdParty?`.
- **CV**: skills, experience, availability, commute polygons (+weights), external links; **no PII** in public fields.
- **Application**: *child of Job*. Public summary + **E2EE payload** to job owner key (cover letter/PII if really needed).
- **Invite**: *child of CV*. Public summary + optional E2EE payload to candidate key.
- **Feedback / Issues** (child of Job or Application): `noshow`, `declined{reason}`, `hired{candidate}`, `feedback{field,rating,comment}`, `report{reason,details}`.
- **Peer review** (about a candidate): `{ about:candidate_key, context_tag, rating, comment }`.
- **Reactions**: typed (👍, 👎, 🚫 spam, ⚠️ misleading, 📰 credible).
- **Amendments**: author-only; each amendment creates a **new reaction set**; UI collates by default with “expand versions.” fileciteturn0file0

---

## Ranking, trust, and reactions

- **Score** is **independent of recency**. Feeds apply recency as a filter (default on). fileciteturn0file0
- **Vote weight** per reaction: `w = signer_trust^α * server_trust^β * temporal_dampen(Δt)`; no anti-consensus penalty. Comments can contribute trust (lower weight; tag-scoped) to support candidate-helpful activity without klout-farming. fileciteturn0file0
- **Negative feedback cost**: posting a negative-impact child (e.g., `report`, 👎, 🚫) applies a **small upfront trust debit** to the poster; corroborating reactions **refund and can invert** that cost as confirmations arrive. Start with low cost; require minimal corroboration before significant impact. fileciteturn0file0
- **Dogpiling controls**: temporal dampening; per-actor/peer rate caps; low-trust new signers have low affect. External brigades can’t be fully stopped; acceptable risk. fileciteturn0file0
- **Right-of-reply**: allowed as sub-comments but doesn’t get special boost; avoid infinite slap-fights surfacing in primary views. fileciteturn0file0
- **Mis-tags**: tolerate; offer search/dedupe suggestions post-facto; no write-time policing. fileciteturn0file0

---

## Privacy & PII

- **Public**: hard block obvious PII in schema validators/UI (email, phone, DOB, address).
- **Private**: PII only in **E2EE payloads**; discourage oversharing even there with copy + patterns checks (soft). GDPR stance: no personal data in clear; encrypted blobs exist but are user-owned keys. fileciteturn0file0

---

## Moderation & lifecycle

- **Hard bans only** for fraud/harassment/CSA/obvious scams → key kill + tombstones. No strikes for no-shows; logs only. fileciteturn0file0
- **Detached content** from deleted parents hidden by default; visible in “your comments/reactions” views; excluded from indexing/search. Text is cheap; GC later. fileciteturn0file0
- **Auto-expire**: on confirmed hire (both sides sign). Minimal legal retention; cold shards for rare audits. fileciteturn0file0

---

## Comms & notifications

- **Public Q\&A**: structured child posts; low-trust items stored but not surfaced by default.
- **Private replies** always encrypt to the **top-level owner key** (job→hirer, CV→candidate) regardless of thread depth. No separate “application” root needed. fileciteturn0file0
- **Noise control**: low-trust messages **auto-ignored** unless the recipient opts in/lowers threshold.
- **Activity signal**: “actively seeking” + **last checked-in** timestamp visible to hirers.
- **Delivery**: Android app w/ push; web push where available; iOS later. **Digest email** (daily/weekly/monthly) for trusted items only; line item “N low-trust held.” Email comes from us, not peers. fileciteturn0file0

---

## Recruiters, locations, remote

- **Recruiters** post as hirers; optional `isRecruiter`/`isThirdParty` flags. Client secrecy allowed; UI nudges (“high-trust candidates prefer named clients; bypass risk is low — stats here”). Vague job locations ok; candidates use commute polygons w/ weights. Remote supported via tags/filters. fileciteturn0file0

---

## AI integration (paid, ethical)

- **Hirer-paid AI screening**: batch candidates by tag; produce **open audit trail**: “AI ran on N candidates; top-K, runners-up; declines with rationale.” Candidate can attach **soft-sign context** for the AI (opt-in).
- **Appeals/flags**: simple child post; human can review.
- **Partnered delivery**: integrate a known provider; take a **thin commission**; avoid ops burden. fileciteturn0file0

---

## Monetisation (ethos-aligned)

- **Advertiser schema** (training/education): paid to post; **never buys placement**. Sidebar placement by tag-fit; transparent aggregate metrics; in-network logging of clicks (rate-limited). Charities/gov can have reduced fees. fileciteturn0file0
- **AI screening**: pay-per-use (hirer-funded).
- **Org key tiers**: subscription to **child-key caps** (curve pricing). Free individuals; small teams cheap; large orgs pay for control/offboarding. Bundle with AI priority if useful. fileciteturn0file0
- **Donations** welcome; no paid ranking, ever.

---

## Ops & scale notes

- Centralised MVP (web + Android). Remote/freelance is not focus; if needed, route to **ActualFreelancers** as an external convenience with linkback.
- Shard later; Postgres cold storage for dead shards. If it goes sideways, we can shut it down; low sunk cost. Forks are fine; license allows — we’ll keep the edge by ethos + execution. fileciteturn0file0

---

## Roadmap delta

1. MVP: keys, jobs/CVs, child applications/invites (E2EE), reactions, score sans-recency, Android app, digest email.
2. Feedback logs + negative-cost + corroboration; dogpile dampening; validators for PII.
3. Amendments with versioned reactions; collapsed UI; caches.
4. AI partner integration + audit trails + appeals.
5. Advertiser schema (sidebar), metrics.
6. Org key tiers (parent/child), admin UI.
7. Recruiter UX (flags, nudges, stats capture).
8. Remote filters; commute polygons polish. fileciteturn0file0

---

## Files to add/update

- `README.md` (reflect all above).
- `PRIVACY.md` (PII policy, E2EE caveats, GDPR notes). fileciteturn0file0
- `TRUST.md` (weights, negative-cost + corroboration, temporal dampening, comments→trust). fileciteturn0file0
- `COMMS.md` (public vs private, thresholds, digests, activity signal). fileciteturn0file0
- `SCHEMAS.md` (job, cv, application, invite, feedback, peer-review, reactions, amendment versioning). fileciteturn0file0
- `KEYS.md` (file keys, legacy refs, proven transfer, org parent/child, invalidation). fileciteturn0file0
- `MONETIZATION.md` (advertiser schema rules, AI commission, org tiers). fileciteturn0file0

If you want, I’ll spit out the concrete JSON Schemas next (job/cv/application/feedback + amendment versioning), or the `TRUST.md` math with exact α/β, dampening half-life, and corroboration thresholds.


#### You:
Good. This also gives Recruiters a reason to buy-in. Their org is paying for high trust maintenance across staff (so even say new trainees who don't do everything right, get the network trust from their org, and the org gets some goodwill to fix issues even if a subkey gets banned for unintentional major violation (some individual recruiters are idiots)). It also lets them sell the benefit of their high trust to their actual clients ("Let us hire for you and we're already the top of the pile"). It's not my fave solution, but if they're paying a reasonable fee for the privilege, I can live with it to bring recruiters in (and hopefully they can write better listings and handle the more complex interactions, like setting their AI pre-validator config, which could get slightly complicated). Recruiters dominate the UK market, so we absolutely need to give them stuff they need, and ideally we want to support them getting sensible sets of data out into their own systems (some have opinionated complex systems they will not change, so if/where we can provide interoperability, we should try, and then document clearly as "available if you _need_ it". No substantial changes to underlying tech, but requests can be made with a key from our client or theirs, if they have permission, they can do it, and we will help).

Not entirely sure how we practically charge without emails/etc, we have to go through a payment processor at some point. Subs especially may need to be out of network, tho flagging account level in public metadata somewhere might be important (enhancement of 'isThirdParty'?) so candidates know what type of org they're actually dealing with.

Open feedback incentivises 'diplomatic' language for high-trust keys, but low-trust keys (or just inexperienced/rude people) probably won't be diplomatic. Some 'unreasonable' language is to be expected. Tolerate with potential filtering for edge cases that don't violate harder rules. I'm thinking statements like "Candidate turned up drunk dressed like a tramp" and candidate replies like "Office smelled of fish and the boss was a dick". No way to fairly moderate, just hide by default, similar to nsfw tag, or maybe trust 'warning' on repeat near-violations (should erode trust except like passive aggressive or dogwhistles). Not too bothered about a little whining, since posts/comments are generally transitive as roles only take weeks to fill, then deleted/archived, but want to be aware it can and will happen, especially for profiles.

On closing a story, for free users of any tier, optional "Thanks for (finding a job|hiring a person) via ActualJobs, want to give us a tip? or maybe upgrade to suchandsucha tier?", get them to give us money when they like us for our actual value.

On tech, a lot of this stuff can be handled by just pushing directly onto a 'text' file, then consolidating events on pull/render. Maybe with occasional process to hard-fix older data for efficiency. I _could_ serve it off-disk (on say ssd), but postgres is probably cheap enough and I think it has good support for large text blobs and progressive amends/updates (iirc). Don't code anything, just noting.

In swamped markets, a paid AI search for candidates, _might_ be interesting, tho candidates generally have fewer options anyway, so not an early phase feature.

On appeals/flags, per schema type should affect who can do what. A simple candidate question or comment on a job may be answered or even hidden/removed by the job owner/signer, but complaint or noshow or other trust-relevant item stays on the system (obvs respecting its trust level for visibility). This just keeps global moderation burden low (no emails to have something removed unless grievous, maybe anyone can just flag grevious for admin review in our own dash that directly affects the network content... if it's full of swearing or incoherent, it's out quickly).

We probably want a line in the doc for business strategy. If it got substantial market (unlikely but possible if we got job listers on early), it would be attractive target for competitive ddos or other corpo bullshit (legal challenges or something). If approached with purchase from an org with strong enough tech to prevent that, should seek to maintain independence, the core network and value offering does not change. Would consider outright sale in a sticky situation, but candidates come first. The entire inspiration behind this has been black-box candidate exploitation, as they literally have no choice except scummy hiring sites and practices, which this product is intended to resolve.

#### ChatGPT:
Here’s a consolidated, idea-level specification for **ActualJobs.click** with your new feedback integrated cleanly into the existing draft. I’ve merged your primary requirements with the earlier draft and Grok’s notes, folding in the recruiter support, privacy stance, comms tweaks, amendment handling, and business considerations.

---

# ActualJobs.click — Concept & Spec (Integrated)

## Core Premise

A self-hostable, minimal, signed-data network for job matching that empowers individuals (candidates) to publish their CVs and responses directly, without surrendering data to a central intermediary. Companies and recruiters participate on equal protocol footing but may optionally pay for services such as organizational keying and AI-based candidate search.

---

## Key Principles

1. **User-owned identity and data**
   - Identities are **just cryptographic keypairs**.
   - The “user file” = the user’s public/private key bundle.
   - Product servers never retain private keys.
   - If a key is lost or revoked, the user starts fresh. No “password reset.”

2. **Strict PII boundaries**
   - **Public channel**: job posts, CV summaries, Q\&A, feedback, tags. All personal details (emails, phone numbers, addresses) are explicitly blocked at write time by schema validators and UI warnings.
   - **Private channel**: optional end-to-end–encrypted “application packets” sent to the hiring key (from candidate) or candidate key (from recruiter). This is the only place where names, contact details, etc. may appear.

3. **Right-to-reply & moderation**
   - Feedback about a job or candidate is always logged as a child object.
   - **Negative feedback costs the poster trust up-front**; corroboration by other trusted actors can offset/refund that cost.
   - To minimize “slap fights,” replies don’t gain additional feed boost — they are collapsible and surfaced by explicit user action.
   - **No-show notes** are not trust-penalizing; they’re logged as context.
   - **Key invalidation** wipes authored top-level objects (jobs or CVs). Child content (feedback, comments) remains but is detached and eligible for garbage collection unless preserved by other signers.

---

## Trust Model

- **Per-key, per-product.**\
  Each domain (jobs, news, events) runs its own trust graph. No shared cross-domain trust state.
- **Per-key import only.**\
  When a user publishes into a domain, that domain can import existing trust metadata tied to that specific key. No bulk mirroring.
- **Weights & damping.**\
  A reaction’s weight = `signer_trust^α × server_trust^β × temporal_decay(Δt)`. Tunable exponents α, β adjust the influence of peers vs. servers.
- **Role-aware trust.**\
  Positive peer reviews on a CV increment candidate trust. Negatives subtract immediately; “confirmed” corroboration can turn an initial cost into a lasting penalty for bad actors.
- **Org keys.**\
  Recruiters can create **parent org keys** and subordinate recruiter keys.
  - Parent can revoke children.
  - Children inherit the org’s trust context but don’t automatically propagate trust externally.
  - Recruiters may choose to hide client identities publicly, but UI will show a clear “third-party recruiter” indicator.

---

## Content Types & Structure

All content is **signed, append-only**. Servers store a shared log of *bodies*; integrity is client-verifiable. High-level schemas:

- **Job**
  - `title` (string)
  - `description` (markdown)
  - `tags` (array of skill/seniority tags, server-local taxonomy)
  - `payRange` (optional, normalized currency range)
  - `location` (geo point or polygon; may be vague)
  - `isRecruiter` (bool)
  - `isThirdParty` (bool, signals job posted by recruiter on behalf of a client)
  - `orgKey` (if posted by an org child key)

- **CV**
  - `skills` (tags)
  - `experience` (list)
  - `availability` (dates, schedule)
  - `locationPrefs` (polygons with weights)
  - `publicLinks` (portfolio, GitHub, etc.)
  - **No PII in cleartext fields**

- **Application** (child of a Job)
  - `publicSummary` (short pitch)
  - `encryptedPayload` (to job owner’s pubkey; may contain CV link, contact info)

- **Invite** (child of a CV)
  - `publicSummary`
  - `encryptedPayload` (to candidate’s pubkey; role description, offer, NDA, etc.)

- **Feedback/Issues** (child posts)
  - Types: `noshow`, `declined{reason}`, `hired{candidate}`, `feedback{rating, comment}`, `report{reason, details}`

- **Reactions**
  - Typed: 👍 (endorse), 👎 (critical), 🚫 (spam), ⚠️ (misleading), ❤️ (thanks)
  - Weighted by trust as above

- **Amendments**
  - Author-only edits; generate new immutable version body
  - Each version has its **own reaction set**
  - UI shows latest by default, with option to expand history

---

## Comms & UX

- **Public discourse:** Q\&A as structured children of Job; visible only if meets trust thresholds.
- **Private replies:**
  - Candidate→Job: application packet, encrypted to job’s key.
  - Job→Candidate: invite packet, encrypted to candidate’s key.
- **Noise & filtering:**
  - Default: surface only feedback/reactions above threshold trust.
  - “Hidden feedback” available in a separate tab, like an NSFW toggle.
- **Notifications:**
  - Android client w/ push notifications (early priority).
  - Web push where supported.
  - Optional email digests (daily/weekly/monthly) sent by the operator, summarizing trusted activity and flag counts.

---

## Trust & Privacy details

- **Negative feedback** is permitted but costs trust; repeated unjustified negative/spam flags erode the poster’s reputation.
- **No raw PII** accepted in public schemas. Validators reject obvious PII patterns.
- **Soft-publish vault** (future): optional, server-hosted encrypted blob for PII, requested/retrieved with explicit consent.

---

## System design (same as earlier, refined)

- **Data store**: Postgres (core tables: `identities`, `posts`, `reactions`, `trust_edges`, `org_keys`, `amendments`). Archive to cold storage if unused.
- **Indexing**: Elastic or Postgres FTS for job text & tags.
- **Cache/queue**: Redis.
- **Workers**:
  - Validate & ingest signed posts
  - Maintain per-domain trust DAG
  - Compute scores with decay & corroboration logic
  - Apply garbage collection of detached subcontent
- **APIs**:
  - `publish(body, sig, key)`
  - `react(target_id, reaction_body)`
  - `amend(prev_body_id, new_body, sig)`
  - `subscribe(filters…)` (for live/digest updates)
- **Frontend**:
  - Jobs/CVs feeds with filters (tags, location, recency, trust threshold)
  - Collapsible child content threads under Jobs and CVs
  - Clear badges for recruiter / third-party / active-seeking

---

## Strategic notes

- **User side**: strong story — “your CV is yours, not theirs; delete at will; apply privately.” Directs value to candidates.
- **Recruiter side**: org keys + trust pooling + ability to pre-validate + analytics dashboards. Selling point = “your agency’s high trust boosts your postings.”
- **Growth challenge**: bootstrapping both sides in recruiter-dominated markets is hard. Early adopter employers may be attracted by visibility perks + AI candidate filtering.
- **Resilience**: be transparent that the hosted service can be shut down under legal/financial pressure, but the signed file network remains forkable and portable. Trust graphs don’t bleed across products, keeping scopes clean.

---

✅ This integrates your tweaks into a coherent spec.

Would you like me to now **redraft the `README.md` file** with these updated principles and modules so it’s ready to publish, or would you prefer a separate **`TRUST.md`** and **`PRIVACY.md`** doc first?


#### You:
that looks reasonable. GImme an elevator pitch statement

#### ChatGPT:
Here’s a concise elevator pitch you could use for **ActualJobs.click**:

---

**“ActualJobs.click is a community-driven hiring network where your professional identity lives in your own hands, not in someone else’s database. Job seekers publish signed, privacy-respecting CVs, companies and recruiters post verifiable roles, and both sides build trust through transparent feedback instead of pay-to-play manipulation. It’s like a peer-to-peer job board that puts candidates first, gives recruiters real trust signals to stand on, and avoids the black-box gatekeeping of traditional platforms.”**

---

Want me to draft a few **shorter tagline-style variations** you could use on the landing page or social posts?


#### You:
this should be pretty easy to find recruiters (I've got a million emails in my various inboxes, and they're open on basically any webpage there's a recruiter message asking if I'm available ;)), but can you give me a quick strategy to get early testing and basic use from businesses and real candidates? I know its a catch 22, where you need one to get the other, and I don't have a marketing budget, just a fresh idea that promises not to expoit anyone.

#### ChatGPT:
Here’s a lean, no-budget **early adoption strategy** tailored for ActualJobs.click with the “no exploitation, no bullshit” ethos:

---

## 1. Recruiter Beachhead

- **Leverage your inbox**: you already have recruiters cold-emailing you. Reply with: *“I’m not looking, but I’m building an open, trust-driven job board. If you want early access to candidates without LinkedIn spam, try posting here — it’s free while we test.”*
- **Pitch value in their own language**:
  - “Your jobs rank higher if your org builds trust.”
  - “Posting is free, but if you want recruiter org features (sub-keys for staff, AI filters, analytics), you can subscribe later.”
- **Make them look good**: recruiters can use org trust as a *badge* to show clients they’re a reputable partner — that’s a differentiator in a crowded UK recruitment market.

---

## 2. Candidate Seeding

- **Start with your own network**: invite friends/colleagues to post *clean, skills-focused CVs* with “actively seeking” toggled on.
- **Focus on niches**: pick one or two high-demand, high-signal job categories (e.g. green tech, software engineering, skilled trades) to avoid the “empty board” look.
- **Encourage peer-reviews**: seed a few candidate profiles with trust-building comments (“I’ve worked with X, very reliable”). This shows the feedback loop in action from day one.

---

## 3. Closing the Catch-22

- **Show recruiters a real pool**:
  - Stage 1: seed \~20–30 CVs from trusted friends/colleagues.
  - Stage 2: invite recruiters to post jobs (free, no friction).
  - Stage 3: encourage candidates to apply publicly (with encrypted contact details privately).
- **Highlight transparency**: show recruiters that if they treat candidates fairly (respond, leave feedback), their jobs rise higher.

---

## 4. Community Rules & Culture

- Publish **style guide** from day zero (“Be professional; structured tags; no PII in public CVs; feedback must be factual/diplomatic”).
- Position “open feedback logs” as **a feature**: candidates can see whether a recruiter ghosted applicants; good recruiters can demonstrate responsiveness.

---

## 5. Growth Tactics

- **Dogfood via Better Britain Bureau**: use your own think-tank channels to post test jobs (e.g. research roles, writing gigs).
- **Micro-campaigns on free platforms**: tweet/blog *“ActualJobs.click: jobs without LinkedIn bullshit”*. That message is inherently viral in some circles.
- **Community multipliers**: run small onboarding sessions with trusted recruiters — show them how to onboard their team under an org key. If one agency buys in, they bring dozens of roles automatically.
- **Case studies**: document a few successful matches and share them as stories — “This junior pipefitter got a job via ActualJobs.click.” That’s marketing collateral without paying for ads.

---

## 6. Early Success Criteria

- **Target first 50 CVs** from your own network and early adopters.
- **Target first 10 jobs** from recruiters you already know.
- **One real hire** logged → proof of concept → then share widely.

---

Would you like me to sketch a **step-by-step launch plan** (week 1–6: seeding, recruiter outreach, candidate onboarding, case study publishing), so you’ve got a concrete rollout timeline?


#### You:
There's not much of a meetup culture in the UK atm (kinda died out during the pandemic and since meetup.com went all corpo) but I do have _some_ narrow connections into the manchester tech hiring market, so might try and start there.

#### ChatGPT:
That’s a smart wedge. Manchester’s tech scene is relatively tight-knit compared to London, which makes it a good place to seed something new. Here’s a pragmatic, **low-budget, early-stage rollout plan** tailored to your situation:

---

## 1. Nail the Core MVP

- Get **jobs + CVs + applications (with E2EE contact)** working.
- Build the **trust/reaction system** in basic form (👍 / 👎 / 🚫).
- Ensure the UX for creating a CV/job is smoother than emailing a PDF.

---

## 2. Seed Candidates First (but quietly)

- Reach out to your **own Manchester tech network** — friends, colleagues, ex-coworkers.
- Help them create structured CV posts (skills, availability, location corridor).
- Encourage **peer reviews** so that some trust signals exist from day one.
- Goal: **20–30 solid CVs** covering a few in-demand roles (dev, data, IT support).

---

## 3. Recruiter Onboarding

- Mine your **inbound recruiter emails**.
- Reply with:
  > “I’m not looking right now, but I’m building a transparent, trust-based job board (actualjobs.click). If you want to try posting roles there, it’s free during testing — and if you onboard your whole team under an org key, your agency’s trust carries over to your new recruiters.”
- Pitch them on **two concrete benefits**:
  - They’ll **start at the top of feeds** because of pooled org trust.
  - They get **a transparent way to prove fairness to candidates** (no more ghosting accusations without receipts).

---

## 4. Early Market Strategy (Manchester focus)

- **Choose a vertical:** e.g. *junior devs*, *cloud/DevOps*, or *contract IT support* — wherever your existing contacts can produce real CVs and jobs.
- **Pilot with 1–2 friendly recruiters**: get them to post a handful of real jobs.
- **Bridge to candidates**: ask your contacts to apply to those jobs via the system. This demonstrates the feedback + trust loops.
- **Local credibility**: maybe spin up a Better Britain Bureau piece on *“Why Manchester needs ActualJobs.click”* — not advertising, but contextualising the problem of recruiter spam and opacity. Share it with tech meetups, Slack groups, LinkedIn refugees.

---

## 5. Feedback & Iteration

- Keep feedback loops public where possible (as schema’d child posts).
- Set **default surfacing filters** conservatively (hide low-trust “trash talk” unless explicitly unhidden).
- Collect live feedback from early users about schema clarity, key handling, and what feels “too fiddly.”
- Iterate style guides on GitHub so contributors shape the norms from day one.

---

## 6. Monetisation Path (not day one, but prepare)

- **Org key tiers** for recruiters — main revenue hook.
- **Training/course advertisers** via dedicated paid schema — but still ranked by trust, not money.
- **Tip box** at job close-out: *“Did this help you get a job/hire? Throw in a couple quid.”*

---

## 7. Early Goals

- Week 1–2: Onboard **initial candidate CVs** from your network.
- Week 3–4: Get **first recruiter agency** to post live roles.
- Week 5–6: Drive first match/hire; publish a **case study** on ActualNews.click and relevant Manchester tech communities.
- KPI: 1 successful hire + 50 candidates + 10 real jobs = proof of concept.

---

This keeps the cycle tight: **seed candidates you know → approach recruiters you already have contact with → create a visible “win” → expand**.

Want me to draft a **community launch playbook** (templates for recruiter outreach emails, candidate onboarding guides, and a sample “we made our first hire” case study post) so you’ve got something reusable when you start in Manchester?


#### You:
This will also be going out alongside ActualNews and BetterBritainBureau which reinforces what products for public good, built with AI actually look like. To generate buzz I'd like to 'sell the dream' as well as the actual product. Evaluate briefly in context.

#### ChatGPT:
Got it — if you’re launching **ActualJobs.click** alongside **ActualNews.click** under the **Better Britain Bureau** banner, then your “dream pitch” becomes stronger because you can position it as part of a *movement* rather than a one-off tool.

### Why this helps

- **Shared ethos = brand coherence**: Both projects embody *“tools for the public good, built with AI without selling attention.”* That gives you a clear differentiator against ad-driven incumbents.
- **Cross-reinforcement**: News + Jobs + future “Events/Dating/etc.” show this isn’t just a quirky side project but a **pattern for civic infrastructure** — signed content, trust graphs, schema-first design. That makes the vision bigger than just “another job board.”
- **Narrative hook**: “We’re showing what AI should do for society: not feed engagement or ads, but create open, transparent systems that empower people.” That’s a story reviewers, early adopters, and journalists can get behind.
- **Buzz generation**: You can seed both products in the same communities — tech workers, journalists, activists, local orgs — each reinforces the other’s credibility. ActualNews can report on the state of hiring, while ActualJobs provides real-world data and experiences.

### The “dream” angle

- **Not a platform, a protocol**: Position this as *“a new civic layer for Britain”* — a family of open-source, trust-weighted public services (news, jobs, events, maybe dating) where the community holds the keys.
- **Candidate-first, citizen-first**: The projects align with your core belief — *the needs of the many outweigh the wants of the few*. That’s a clear moral stance, and it resonates with people who feel used by LinkedIn, Indeed, or ad-driven news.
- **Dogfooding**: By building and using these tools within the Better Britain Bureau itself (posting your own jobs and research), you demonstrate legitimacy and commitment.

---

💡 **So the strategic narrative is:**\
*“We’re building public infrastructure for trust — ActualNews.click for information, ActualJobs.click for hiring, and more to come. Unlike the incumbents, we don’t sell your attention or your data. Everything is open, transparent, and community-governed. You own your identity, you earn trust through good contributions, and you can take that trust with you across Better Britain Bureau projects. This is what AI built for the public good looks like — not manipulation, not exploitation, but tools that work for people.”*

---

Want me to put together a **one-page “vision deck”** that lays out this dream visually (mission, problems with existing systems, what you’re building, how it’s different, roadmap)? That could double as a shareable pitch to both candidates and recruiters.


#### You:
It's worth noting we sound like we're crypto-adjacent (a sort of blockchain, cryptography signing, ai), but we're no. It definitively isn't a blockchain, it's more of a content tree with a marshall that does all the work instead of relying on an untrustworthy cloud of unknowns, and AI is integrated openly, visibly/transparently (the literal opposite of all the problematic use cases that big business are trying to squeeze money out of). 

Selling it as a 'protocol' isn't accurate, it's not, it's just a slightly different type of API, driven by a different attitude... No personal data in our system, ever, or as close as we can get while complying with the law, no tracking or profiling (except in the bounds of the AI service for which that is literally its job), ads in-site, not provided by a mysterious benefactor, open audit trails on everything. We're going for essentially "this is what the government should have built already but didn't" (because profit motive belongs nowhere near governance, but maybe that's ideology). We are also using AI as a tool, not letting it use us. We give it information and it gives us value based on its training, in a clear and readable form that is meaningful to everyone involved.

On the keys as accessibility problem, we might even build a third-party key-holder service that _does_ support email/username and password, or even corpo SSO/Auth0/etc, and just marshalls your key to your local machine (like a standalone key vault, that basically provides a key instead of a JWT).

#### ChatGPT:
That clarification tightens the identity and messaging a lot. You’re right: what you’re describing is **not a blockchain** (no global consensus, no token economics, no mining). It’s closer to a **signed, append-only event log + open API** where trust is derived from reputational signals instead of proof-of-work or platform moderation. AI is positioned as *assistive infrastructure*, not an opaque decision-maker. That distinction is important for framing.

Here’s a **refined positioning + strategy note** you can slot into the repo’s README or into a one-pager:

---

# Positioning & Narrative

### What it is

**ActualJobs.click** is part of the **Better Britain Bureau**, a suite of open, public-good services (starting with jobs and news). It’s a **transparent content system** where identities are just cryptographic keys, not accounts or profiles, and where trust is earned through visible, structured feedback.

### What it isn’t

- ❌ **Not a blockchain** — no miners, no coins, no speculative token layer. Content is stored in a **signed content tree**, with a coordinating server (“the marshal”) handling ingestion and sync. No global ledger, no proof-of-work, no gas fees.
- ❌ **Not surveillance capitalism** — no tracking, profiling, or behavioural ads. Content lives as signed JSON. Users own their identity keys, and personal data never enters the public system (PII only travels via opt-in encrypted payloads).
- ❌ **Not a social network** — no engagement feeds, no clout farming, no algorithmic addiction loops.

### How AI is used

- **Tool, not overlord**: AI is used for *transparent, auditable services* like candidate pre-screening, spam detection, or summarising feedback.
- **Visible outputs**: AI judgements are logged openly (“AI rejected 15 candidates for missing skill X”) instead of being hidden behind black-box ranking.
- **No profiling**: AI never builds secret dossiers on users. It only works with voluntarily submitted data, either public (skills/tags) or encrypted (private applications).

### Accessibility & Identity

- **File-based keys**: each signer is just a cryptographic keypair. Lose the key, lose the trust, content persists.
- **Third-party key vaults (optional)**: for non-technical users, external services can wrap keys with conventional login (email/password, SSO). They simply **hand the key to the client** instead of acting as identity providers. Servers never own PII.
- **Org keys**: recruiters/companies can pay to manage pools of subordinate keys for staff, sharing trust across the org but allowing per-recruiter accountability.

### Strategic story

- **“This is what government should have built already.”** Public digital infrastructure, open standards, transparent accountability — not rent-seeking platforms.
- **Part of a larger ecosystem**:
  - **ActualNews.click** → trustworthy news without agenda.
  - **ActualJobs.click** → fair, transparent hiring.
  - Future extensions (e.g. **ActualEvents**, **ActualDating**) use the same tech base.
- **Ethos**: The Bureau builds *non-exploitative public services with AI*. Users see and control how AI is applied, instead of being manipulated by it.

---

That sets you up with a **“sell the dream” narrative** that recruiters and candidates can grasp:

- Recruiters: *earn trust, get taken seriously, prove fairness to clients*.
- Candidates: *own your CV, avoid spam, get honest feedback*.
- Public: *see a working demo of AI-powered, trust-first, non-commercial civic infrastructure*.

---

Do you want me to **rewrite the full `README.md` in this new style** (with these positioning clarifications baked in), so you’ve got a polished public-facing version for the repo?
