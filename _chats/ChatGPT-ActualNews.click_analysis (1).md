# ActualNews.click analysis

#### You:
okay I'm fucking sick of news sites... Let's analyse my supposedly better idea...

It's a single site actualfuckingnews.com for everybody sick of bullshit news (not even fake news, just misleading and clickbaity titles, bullshit adds, and bullshit 'agendas' and recycled talking points.

The news site has no user accounts, everyone is anonymous and publishes by signing with their key, they can set an identity on each post, so we don't know who it came from, but we know if all stories from one author came from the same author. Readers can comment or upvote/downvote, again using the same signing system, and we track trust for all signers, if you write good, fair, interesting, articles, you get karma (like reddit). We publish post schemas of varius types, and you either fit a scheme and get placed in a feed, or you don't, and you don't, again, not dissimilar to reddit. Votes from users with low trust are worth less than votes from users with high trust. It's technically gameable, so we're going to need some reporting feature that invalidates everything from banned signers, we'll have to deal with further edge cases later.

Every reader can set the level of trust they have any interest in, and queries us for feeds, optionally with tags for the specific stories. We don't do full text indexing but can add a layer for it later.

News stories can have plain text (markdown) and I think we want to accept images too, but any other content will have to be via embeds. I also want a slightly elevated commenter type, 'pundit', which like comments is a sub-feed of the content block for the main story, I'm not sure how to class it in code, but pundits can compete for position on the feed, maybe we just let the trust level dtermine pundits and we make it a post assignment.

What did I miss?

#### ChatGPT:
You’ve got the core pretty tight already. Here’s what I see missing or underspecified if you want this to be robust:

- **Sybil resistance:**\
  Signing keys alone aren’t enough — people can generate infinite keys. You need some cost or friction to creating a signer identity (proof of work, stake, rate limits, proof-of-human, or just social trust bootstrap).

- **Key lifecycle / revocation:**\
  If someone loses a private key, or a key gets compromised, how do they migrate reputation without breaking trust accounting?

- **Moderation primitives:**\
  You mention banning signers, but you’ll need granular tools: per-post hides, temporary mutes, quarantine feeds, community voting to flag. Otherwise abuse and spam will eat the signal.

- **Feed hygiene:**\
  Sorting is trust-weighted, but what about recency vs trust tradeoff? Otherwise old high-karma stuff could clog the feed.

- **Schema governance:**\
  Who defines the schemas (types of posts)? You’ll need a way for schemas themselves to evolve without a central choke point.

- **Pundit class formalization:**\
  Needs explicit data model. Either:
  - `PostType: Comment | Pundit | Story` with extra fields.
  - Or just treat Pundit as a Comment subtype that only shows up if signer trust > threshold.

- **Media handling:**\
  Images = bandwidth + abuse vector (CSAM, gore). You’ll need hashing, deduplication, and some moderation pipeline or you risk legal takedowns nuking the site.

- **Legal exposure:**\
  Anonymous posting + zero accounts = harder to moderate. Need at least some ToS + jurisdiction plan or you’ll get wiped off the map with DMCA/illegal content.

- **Discovery/search:**\
  Tags are fine, but without full text, discovery is weak. Might be worth indexing plain text early (even if only naive inverted index) so readers can find niche stories.

- **Ranking manipulation edge cases:**\
  Coordinated brigading, circular upvotes among sockpuppets with modest trust. You’ll need anti-collusion heuristics.

- **Client trust filters:**\
  Important that filtering is client-side adjustable, so users can choose whether they want “raw chaos” or “high-trust only.” That’s powerful, but needs clear UX.

- **Persistence/archival:**\
  What happens to old posts? Are they immutable (signed ledger), or can they be edited/removed? If immutable, you’ll need versioning; if mutable, need signature strategy.

- **APIs:**\
  You mention feeds — formalizing them as signed, filterable JSON (like ActivityPub or AT Protocol) might give you free federation later.

Want me to sketch out a minimal **data model** (posts, comments, signatures, trust, feeds) so you can see if the design holds together structurally?


#### You:
Okay, let's address some of those...

I don't care if they over-post. Low trust lets us filter most spam, and if we get into like intentional racist floods we start with like ip filtering. I don't care about identity at all, good content rises, and as more content rises, shittier content just gets culled, as posts inherit trust, if you want to game the system, you're still going to have to gain trust. In theory we use tags to determine top levels (like the home page isn't JUST the most upvoted posts from the most trusted signers).

They do not migrate rep, don't lose your key. I don't need granular tools, no temps, no quarantines, no queues, if you lose trust, all your content is off our network. Someone else may back it up or mirror it on their own site if they want, you're basically throwing content onto the stack, if you act irresponsible you're out, make a new key and start over. In theory we can moderate spam with LLMs, to a point, then we have to find other solutions. Classes of downvote (rather than just up or down, but up or down with a reason from a set helps us hint responses)

Recent posts from signers with high karma should be easy enough to filter, especially if we determine active tags to provide submarkets.

Initial schemas are set in the github repo, github issues can be used to add or change schemas.

"just treat Pundit as a Comment subtype that only shows up if signer trust > threshold", I'm thinking we run a process that specifically identifies people with high trust on specific tags, so a pundit isn't just a generally high 'karma' but a high karma in the fields relevant to the story (an experts get visibility thing).

Hmm, I can live without images, if we allow them to be linked via alternate hosts (I don't think CORS applies to images right?)

Legal exposure is a risk, so I'm basically depending on the trust system, 1 strike and your out, and a grace period before new signers get general visibility (so only high trust/mods see brand new content from new signers.

On the rest you're about right.

#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
Added a concrete spec + code: data model (Postgres/Redis), signing, schemas, trust/ranking, feeds, APIs, workers, pundits, and example envelopes. If you want full backend listings (Fastify project skeleton + SQL migrations + docker-compose), say “full backend” and I’ll drop it.


#### You:
thanks for the code, but I'm more interested in the quality of the idea, bearing in mind I know it's unconventional (on purpose)

#### ChatGPT:
Got it — purely idea-level then:

**Strengths of your design:**

- **No identity / no accounts**: radical simplicity. Removes the entire mess of privacy, doxxing, logins, and also disincentivizes parasocial cults.
- **Trust-weighted feeds**: natural spam filter. You’re not trying to get to “perfectly fair,” just “signal rises above noise.”
- **Schema-first content**: keeps posts structured and consistent. The GitHub governance for schemas is an elegant way to keep it community-driven without bolting on user councils.
- **Pundits by *tag-specific* trust**: actually sharp — domain expertise without central designation.
- **Grace + 1-strike bans**: uncompromising, very low moderation overhead. You’re trading forgiveness for clarity — which might be right for the ethos of the site.

**Risks / weak spots (but maybe tolerable):**

- **Sybil wall of noise**: even if “low trust means low impact,” floods of garbage *still* cost bandwidth, storage, and mod attention. Grace periods help, but brigading from new keys is always possible.
- **Fragility of reputation**: “lose key, lose history” keeps it clean, but it’s punishing for legit users. Some good contributors will inevitably vanish. Maybe acceptable — depends how much you want permanence vs churn.
- **Legal bombs**: without proactive moderation, a single illegal image/link could bring takedowns. You’re relying heavily on community trust filtering and fast bans. That’s a brittle point if you grow big.
- **Dogpiling risk**: strong downvote categories might let high-trust users shape “consensus reality.” You’re depending on trust diversity (different domains/tags) to offset herd effects.
- **Discovery gap**: without search or recommendation, the feeds risk being dominated by a few well-trusted “authorities.” Could be boring/ossified unless you surface some long-tail randomness.
- **Cultural effect**: The “one strike, you’re gone” rule sets a tone — no rehabilitation, no second chances. That might align with the brand (“Actual Fucking News”), but it’ll also shrink the pool of contributors over time.

**Overall quality:**\
It’s a **strong, uncompromising experiment** — deliberately hostile to traditional growth/monetization metrics, but coherent as a signal-finding machine. Its viability depends on whether you want a niche, principled community (small but high-signal) or to scale big (where the moderation/legal issues start dominating).

Do you want me to stress-test it more against *adversarial actors* (brigades, state disinfo, organized harassment), or against *growth & sustainability* (can you keep the site alive if it catches on)?


#### You:
making it federated (I think I'm using that right) and a background p2p sync setup, that lets us mirror content, might help with the wall of noise, anyone can run a server at any address, and high trust servers can sync with popular servers. 

Fragility is intentional, don't lose your fucking key if you want to still exist. It's not hard. If they lose their key, bfd, the content still exists, join a fringe but trusted server if you want to build trust in a sandbox.

'Social media are publishers' is a bullshit legal position, that seems like it will be stopped soon anyway, especially with all the social media execs blowing trump and the uk refusing to chase real violations, as long as we kill csa and outright illegals, anything else is reportable, and we have an audit trail that proves we didn't actually write/upload it. That's literally the point. The p2pish nature helps keep troublemakers in their own gardens, not ours.

Dogpiling is a risk, but we're going to have to call it democracy and tolerate some of it, again, low trust downvotes are worth very little, high trust counters mitigate for the most part. 'Review bomb' style co-ordination campaigns happen, commentors and pundits have to just flag as necessary, a little like twitter's "disputed info" boxes.

Dominated by well trusted authorities is not a problem. Dig into the mire if you want, we won't stop you.

Cultural effect isn't a ban, it's a "you violated our trust, start over if you want", if we're mostly sending text it's cheap to run, and we stick a jimmy wales style "give us some money if you want" box, that is hopefully slightly less naggy. We're not a wiki, news only lasts so long, so old posts just get archived elsewhere, like archive.org

#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
Short take: the federation/P2P angle strengthens your thesis. It pushes noise and liability to the edges, makes capture harder, and lets you be opinionated without being gatekeepers. Here’s where it works and where it bites, plus the minimum you need to lock in.

### What federation fixes

- **Wall of noise:** shard by **server reputation**; only ingest from peers above a threshold. Let users opt-in to fringe peers.
- **Censorship/pressure:** abusive takedown? You can delist locally while mirrors keep history.
- **Culture separation:** each server can tune grace/bans/tag policies; the network stays plural.

### What it breaks / new attack surface

- **Sybil servers:** infinite new domains as spam hose. Solve with a **server trust graph** and cold-start friction (manual allowlist, proofs of work, invite tokens).
- **Eclipse/poisoning:** a malicious peer feeds you skewed subsets. Require **multi-peer quorum** before promoting items.
- **Federated brigades:** cross-server downvote storms. Cap **per-server voting weight** and detect **correlated timing**.

### Trust, now 2 layers (signer + server)

- **Signer trust `t_u(z)`**: per-tag trust for the key (you already have this).
- **Server trust `T_s`**: your view of a peer (0..1).
- **Federated weight**: `w = T_s ^ β * t_u(z) ^ α`, with β ≥ α early on to damp spammy servers.
- **Rate budget per peer**: sliding window of max posts/votes you’ll accept per hour *before* scoring.

### Sync model (simple + resilient)

- **Gossip pull**: periodically pull from peers’ **signed index** (Merkle frontier by day + tag).
- **Content addressing**: posts/comments/votes by **blake3(id)**; signatures inline.
- **CRDT-lite for votes**: last-write-wins per `(target, signer)`; dedupe by `(signer_pubkey)`.
- **Moderation tombstones**: signed “ban” or “hide” notes per server; don’t propagate bans globally unless you subscribe to that peer’s blocklist feed.

### Pundits in federation

- Compute **per-tag punditness locally** using only content seen from peers with `T_s >= τ_peer`.
- Don’t accept a remote “pundit” label; recompute from your own graph.

### Legal/ops stance (minimal but real)

- **Images**: hotlinking avoids storage, but you still transmit. Use an on-demand **strict proxy** with file-type/size caps and a **hash blocklist** (e.g., PhotoDNA-class, or at least PDQ/ perceptual hashing).
- **Audit trail**: keep signed envelopes + ingestion logs; publish **transparency pages** (recent bans, peer trust changes).
- **Jurisdiction**: publish a narrow ToS (illegal content ban, signature requirement, irrevocable moderation). Keep **operator discretion** explicit.

### Server discovery / growth without sludge

- Start with **manual allowlist** of peers.
- Later, add **web-of-trust peering**: a peer’s `T_s` rises if highly trusted peers vouch for it (signed “peer-endorse” notes), damped by diversity so one clique can’t boost each other infinitely.

### Brigading & dogpiles (acceptable but bounded)

- **Temporal dampening**: weight votes by **age of account** and **vote arrival distribution**; fast, coordinated spikes get discounted.
- **Per-story cohort caps**: limit any single peer to ≤X% of total weight on one story.
- **Reasoned downvotes** already help—use them to **penalize voters** who repeatedly end up anti-consensus on “spam/abuse/misleading”.

### UX that supports the ethos

- **Trust slider** with a **peer slider**: “only my server,” “my server + trusted peers,” “whole fediverse (⚠︎ noisy).”
- **Source badges**: show originating server + signer-trust + server-trust at a glance.
- **Fringe tab**: quarantines low-`T_s` content without banning it.

### Minimum viable federation contract (not code, just the contract)

- **Objects**: `Post`, `Comment`, `Vote`, `Ban`, `PeerEndorse`, all as signed envelopes (you have envelopes already).
- **Indexes**: per server, per day, per tag, a **Merkle root** over object ids; expose `/afn/index/{yyyy-mm-dd}/{tag}.cbor`.
- **Policies endpoints**: `/afn/policy.json` (grace window, max rates, reasons enum), `/afn/peers.json` (who they trust, with weights).
- **Transport**: HTTPS pull; optional **mTLS** for rate-privileged peers.

### Cheap anti-sybil levers (pick any 2–3)

- **Proof-of-Work header** on create (tunable, only for under-grace signers).
- **Invite tokens** per server for new signers to skip grace.
- **Rate price**: under-grace posts decrement a server-level quota faster; peers self-police or get de-weighted.

### Observability you’ll actually need

- Dashboards for: **ingest rate by peer**, **drop reasons**, **vote skew heatmaps**, **trust drift**, **brigade detectors** (burstiness, peer concentration).
- Public monthly **moderation report**: count of bans, reasons, appeals (even if appeals = 0).

### Funding that doesn’t rot the incentives

- **Reader donations** and **server operator patrons**.
- Optional **per-peer sponsorship**: peers can pledge for higher **rate allotments** (not ranking weight), visible to users for transparency.
- Never sell placement; only let trust move ranking.

### Where you’re strong

- Coherent values: harsh but clear.
- Federation matches the brand: if someone hates your policy, they can fork and you can defederate.
- Text-first keeps cost low, which makes the “start over” culture sustainable.

### Where to be stubborn vs flexible

- **Stubborn**: no key recovery, bans are absolute locally, trust-weighted ranking, schema-first posts.
- **Flexible**: server trust math, grace length, PoW/onboarding friction, whether to proxy images or hotlink only.

If you want, I’ll pressure-test this against three concrete adversaries next: **spam farm peer**, **state-scale disinfo peer set**, and **malicious high-trust insider**—and outline exactly which dials (β, rate budgets, quorum, temporal dampening) stop each without compromising your ethos.


#### You:
Just noting now... Temporal dampening is good. Anti-consensus is tricky, sometimes more people are wrong than are right, useful for output/analysis, but shouldn't be an input to anything. Trust slider with a peer slider is nice UI touch, makes it transparent what you're getting and where from. No proof of work, anyone can do work. You sign it you own it, and the consequences. Invite tokens 'might' work, but it's an exploit vector, maybe, with caution. Deleted posts keep their metadata, but not their content (like reddit again), at least for a while, so we can track activity against the service(s). Never sell anything (never!), this is for news, citizen led. If you run a server and want to sell anything, that's on you, it will not enter the network unless it earns trust in the conventional way. Ideally I'd never have money in the system at all, but even text servers cost money sometimes, so we compromise (as little as possible). Keys should depend on server keys, your key is locked to your server (maybe we could let signers sign with multiple servers, or re-sign for additional servers later, so they _can_ port 'ownership' elsewhere, but we will need to test this for exploits), this lets servers outright ban keys, and kick them out of their network, including federating that ban, without strictly needing to tell other servers and trust that it's obeyed.

We will also need writers and style guides, to encourage and guide the type of content we want, managed mainly via github issues. 

#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
Good. Here’s how to lock this in given your constraints.

### Trust/Ranking (no anti‑consensus penalty, with temporal dampening)

- **Vote weight** (no anti-consensus input):
  ```
  w_signer = t_u(z)^α            // α≈1.2
  w_server = T_s^β               // β≥α early on
  w = min_cap * w_server * w_signer
  ```
- **Temporal dampening** (applies to *votes*, not just items):
  ```
  vote_atten(Δt_hours) = 0.5 ** (Δt_hours / H)   // H=6..12
  effective_vote = w * vote_atten
  ```
  Prevents flash brigades without caring who’s “right”.
- **Item score**:
  ```
  raw = Σ_up effective_vote  -  Σ_down effective_vote * (1 + reason_w)
  score = raw * (0.5 ** (age_hours / 48))
  ```
- **Trust update** (only from *your own* item performance; no anti‑consensus over voters):
  ```
  Δt_u(z) = k * tanh(net_z / (1 + sqrt(total_weight_z)))   // small k≈0.02
  t_u(z) = clamp(t_u(z) + ΣΔt_u(z) + moderation_deltas, 0, 1)
  ```

### Server‑scoped identities (keys bound to a server, with portable attestations)

- **Identity tuple**: `(issuer_server_id, author_pubkey)`.
- **Author signs content. Issuer server co‑signs an attestation** that this key is allowed to post from it.
- **Portability** (opt‑in): other servers can issue additional attestations for the same `author_pubkey`. Reputation is still local; attestations only enable *posting*, not migrating trust.
- **Exploit guard**:
  - Hard cap on **active attestations per author** (e.g., 3).
  - **Cool‑down** after new attestation before votes count (e.g., 72h).
  - **Per‑issuer quotas** (posts/hour; votes/hour) before trust weighting.
- **Ban**: issuer revokes its attestation (signed `RevokeAttest`). Peers may choose to honor revocations from trusted issuers; not mandatory.

**Envelope fields update** (minimal):

```json
{
  "type": "post|comment|vote|tombstone|attest|revokeattest|ban",
  "schema": "afn.v2",
  "issuer": "did:web:news.example",     // server id
  "author": "ed25519:base64urlpubkey",
  "attestation_id": "blake3-hex",       // when present
  "sig_author": "base64url",
  "sig_issuer": "base64url",            // required for attest/revoke/ban; optional for content
  "body": { ... }
}
```

### Deletions & metadata retention

- **Content deletion**: emit `tombstone{ target_id, reason, redaction_hash }` signed by issuer or author.
- **Serve policy**: hide body, keep `(id, issuer, author, tags, timestamps, vote counts)` for N days (config).
- **Federation**: forward tombstones; peers decide whether to mirror.

### Federation rules (no PoW, invite tokens optional)

- **Discovery**: manual allowlist to start.
- **Ingest gating**:
  - Require **recent policy.json** and **peer-key** rotation schedule.
  - Peer budgets: `max_posts/hour`, `max_votes/hour`, `max_new_signers/day`.
  - **Quorum for promotion**: item must be observed from ≥Q distinct peers (Q=2–3) *or* originate from a high‑`T_s` peer to enter public feeds.
- **Rate fairness**: per‑peer contribution to a story’s total weight capped at `≤ p%` (e.g., 30%).

### UI: trust + peer sliders

- Two independent sliders:
  - **Content trust**: min signer trust threshold.
  - **Peer scope**: `{local only, local+trusted, all}`.
- Always show **badges**: origin server, server trust, signer trust, and “under grace” tag.

### LLM use (only as analysis, not as input to trust)

- Use for **explanations and labels** surfaced to readers (e.g., “likely clickbait/misleading” badges).
- Do **not** feed LLM output into trust or ranking, except as a soft *displayed* heuristic. Your scoring remains vote‑only.

### Style guides & writer program (GitHub‑driven)

- **Guides** (short, enforceable):
  - Claim ↔ Evidence linking (link out; quote length limits).
  - Headlines: no questions unless answer is in body; forbid “you won’t believe…”.
  - Corrections: append “Updates” section; no silent edits (new `content_id` + `prev` chain).
  - Sourcing: minimum 2 independent sources for contested claims, tag them (`sources:[]`).
  - Image use (if allowed): caption + provenance.
- **Lintable schemas**:
  - `afn.v2.post` gains:
    ```json
    { "sources": [{"url": "uri", "label": "string"}], "update_of": "id|null" }
    ```
  - CI bot comments on PRs proposing schema tweaks; issues track guide iterations.
- **Pundit expectations**:
  - Domain tags required; disclosure of affiliations when applicable (free‑text field).
  - Pundit comments rank only on posts matching ≥1 of their high‑trust tags.

### Ops toggles (fit your ethos)

- **Grace**: 72h for new signers and new attestations.
- **Never sell**: hard rule; code path for ads absent.
- **Donations**: reader‑client only; peers cannot propagate sponsor content.
- **Images**: hotlink by default; optional strict proxy with size/type cap + perceptual hash denylist.

### Minimal detectors (non‑normative; don’t affect trust, only visibility throttles)

- **Burst detector**: if `>X` votes arrive within `Y` seconds from `≤N` peers, damp their *per‑vote temporal weight* for that window.
- **Correlation detector**: flag stories where one peer supplies `>p%` of total weight repeatedly; surface to operators; do not auto‑ban.

### What this buys you

- **Clarity**: no anti‑consensus traps; truth isn’t decided by model, only by votes weighted by earned trust.
- **Accountability**: issuer‑scoped keys = “you sign it, you own it,” and servers can excise identities locally while federation stays optional.
- **Resilience**: quorum + temporal dampening tame brigades without central moderation creep.

If you want, I’ll refit the earlier spec to **afn.v2** (envelopes, trust math, federation endpoints, tombstones, attestation flows) in a compact doc you can hand to contributors.


#### You:
Minor note, we probably want to allow the post author (and _maybe_ consider support to let other signers) to make amendments, adding context, sources, supporting, related, etc, not just as a comment, but so 'developing stories' can be built by the crowd. 

#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
Added “Amendments & Developing Stories” to the spec:

- New envelopes: `amend` (proposals) and `merge` (accept/reject).
- Author can merge; fallback to tag‑pundit quorum (M-of-N) if author is absent.
- Append‑only sections: `Updates`, `Corrections`, `Context`, `Timeline`, `Sources`.
- Accepted amends affect proposer trust; rejected don’t (unless spam/abuse).
- Deterministic render endpoint: base post + accepted amends in order.
- SQL tables for `amend` and `merge_decision`, API routes, and auth checks.
- Federates both proposals and decisions; peers recompute quorum locally.
- Tombstones hide body but keep amend metadata.

If you want this refit as `afn.v2` envelopes across the whole doc, say “bump to v2” and I’ll update all types/routes accordingly.


#### You:
okay, no code, just gimme a README style project guide covering everything we've duscussed, more than a pitch, but less than a full implementation guide.

#### ChatGPT:
# ActualFuckingNews — Project Guide (README Style)

## 1. Purpose

**ActualFuckingNews** is an experimental news network for people tired of clickbait, agendas, and algorithmic sludge.\
Principles:

- **No user accounts**: identities are anonymous signing keys, not profiles.
- **Trust > popularity**: content rises by weighted trust, not raw votes.
- **Schema-first**: posts follow defined JSON schemas for consistency.
- **Decentralized**: servers federate, mirror, and filter as they choose.
- **Cheap + transparent**: text-first, donations if needed, no ads, no selling attention.

---

## 2. Core Concepts

### 2.1 Signers

- Anyone can generate a keypair.
- Content, votes, and comments are signed by the private key.
- Lose your key, lose your reputation. Content remains, trust does not migrate.

### 2.2 Trust

- Earned per key, per tag/domain, based on the reception of your posts/comments.
- Votes from trusted signers carry more weight.
- Downvotes include a reason (spam, misleading, etc.) for richer signals.
- New signers start in a **grace period** where their content/votes aren’t visible in public feeds.

### 2.3 Servers

- Anyone can run a server.
- Servers hold content, enforce their own trust thresholds, and federate with peers they trust.
- Servers assign **server trust** to peers; this weights how much content/votes from that peer count.
- A signer key is scoped to at least one server (issuer). Servers can co-sign to let a signer publish elsewhere, but attestation is capped and rate-limited.

### 2.4 Content Types

- **Post**: a news story (title, markdown body, tags, links, sources, images if permitted).
- **Comment**: discussion attached to a post.
- **Vote**: up or down with optional reason.
- **Pundit**: elevated comment by a signer with high trust in relevant tags.
- **Amend**: proposed updates to a post (new sources, corrections, timeline).
- **Merge**: acceptance/rejection of an amend (by author or tag-pundit quorum).
- **Tombstone**: deletion notice; metadata remains but content hidden.
- **Ban/Revoke**: server signals a signer is barred.

---

## 3. Ranking & Feeds

### 3.1 Vote Weight

- Each vote weight = signer trust^α × server trust^β.
- Temporal dampening reduces the effect of sudden brigades.
- Downvote reasons add multipliers (e.g., “spam” counts more heavily than “disagree”).

### 3.2 Item Score

- Net weighted votes decay with post age (half-life \~48h).
- Feed sorts by score × recency.

### 3.3 Trust Updates

- Trust grows/shrinks only from how your own posts perform.
- No anti-consensus penalties (the majority can be wrong).
- Tag-scoped trust identifies subject matter competence → “pundits.”

### 3.4 Feeds

- **Home**: top posts across active tags, trust-weighted.
- **Tag**: posts for a given tag.
- **Peer slider**: choose “local only,” “local+trusted peers,” or “all.”
- **Trust slider**: minimum trust threshold for content in your feed.

---

## 4. Amendments & Developing Stories

- Anyone can propose amends to a post (sources, updates, corrections).
- Post authors can merge/reject directly.
- If authors are absent, quorum of high-trust pundits (per tag) can merge.
- Accepted amends extend the post deterministically into sections like *Updates* or *Sources*.
- Rejected amends don’t hurt proposer trust unless flagged as spam/abuse.
- Amends and merges propagate through federation; each server applies its own trust view to recompute quorum.

---

## 5. Federation & P2P Sync

### 5.1 Model

- Servers gossip indexes of signed objects (posts, comments, votes, bans, amends).
- Content is addressed by hash; votes are idempotent by `(signer, target)`.
- Bans/tombstones propagate, but peers choose whether to honor them.

### 5.2 Server Trust

- Servers assign 0..1 trust values to peers.
- Rate budgets prevent flood: max posts/votes/hour per peer.
- Content may require quorum (seen via multiple trusted peers) before surfacing.

### 5.3 Portability

- A signer can hold multiple server attestations, capped and rate-limited.
- No reputation migration; trust is always local.

---

## 6. Moderation

### 6.1 Philosophy

- No temp bans, no queues, no gradual penalties.
- Abuse = immediate ban. New key required to start over.
- One-strike ethos: clarity over forgiveness.

### 6.2 Tools

- **Bans**: servers flip `is_banned` flag; content remains in DB but hidden from feeds.
- **Tombstones**: mark deletions; metadata stays for audit.
- **Spam checks**: optional LLM classifiers for triage; not used as trust input.
- **Rate limiting**: per IP, per signer, per peer.

---

## 7. Legal & Safety

- Outright illegal material (CSAM, etc.) → instant ban and tombstone.
- Audit trail of signed envelopes proves who uploaded what.
- Images hotlinked by default; optional proxy with size/type caps + hash deny-list.
- Servers operate under narrow ToS: illegal content forbidden, moderation is discretionary, no recovery of banned keys.

---

## 8. Governance & Culture

- **Schemas**: defined in GitHub repo, updated via issues/PRs.
- **Style guides**: community-maintained, encourage sourcing, neutral headlines, explicit corrections.
- **Writer program**: contributors can build guides, templates, and best practices for trustworthy reporting.
- **Never sell content or ads**: funding only via voluntary donations (like Wikipedia).
- **Servers are sovereign**: operators may fund themselves however they like, but trust weighting applies equally.

---

## 9. UX Principles

- **Transparency first**: always show source server, server trust, signer trust.
- **Reader control**: trust slider + peer slider decide what feeds show.
- **Fringe tab**: keep noisy/low-trust content visible but clearly separated.
- **Amends visible**: accepted updates integrated; pending ones shown separately.
- **No dark patterns**: no infinite scroll traps, no engagement farming.

---

## 10. Roadmap

1. **Core MVP**: posts, comments, votes, trust weighting, feeds.
2. **Grace + bans**: signer lifecycle, moderation primitives.
3. **Amends + merges**: crowd-sourced developing stories.
4. **Federation**: peer trust, indexes, syncing.
5. **UI polish**: sliders, badges, pundit highlighting.
6. **Schema expansion**: add new post types (fact check, timeline, interview).
7. **Community growth**: style guides, donation flow, server onboarding docs.

---

## 11. Ethos Recap

- **We don’t care who you are**; we care if your content earns trust.
- **Lose your key, lose your trust**. Content survives, reputation doesn’t.
- **Servers choose their own peers**; trust and filtering are explicit and user-controlled.
- **One strike**: violate trust, you’re out.
- **Never sell attention**: funding is voluntary.
- **Transparency beats algorithms**: feeds are openly weighted, no hidden ranking.

---

Do you want me to also draft a **sample CONTRIBUTING.md** (style guides + schema PR rules) so the GitHub workflow is crystal clear?


#### You:
Don't re-update now, just noting for reference so it's clear when coming to design/implement.

Score * recency is no good (we already discussed earlier). Score is independent of recency, recency is just a feed filter, probably on by default. Multiple filters are fine. 

We could probably use 'reactions' instead (where upvote/downvote is a type of reaction, which gives us vote types, without requiring the user to pick reasons). I suspect trust also comes from comments, not just posts, but will have to consider this more for implications, maybe commentors don't need trust, but I don't want to be dealing with millions of low value posts just because users want to earn trust for klout. Making trust part of the punditry requirement probably requires trust from comments anyway. 

I don't like 'anyone can propose amendments', I'd prefer groups or designated individuals, but _maybe_ just a simple rule settable by the poster "propose-amend if trust > $me.trust; amend if trust > $me.trust * 10" or similar meta rules. Quorum is a nice feature, but hard to manage, needs more consideration, I'd rather just stabilise content and accept someone else will write a follow-on that can be referenced, rather than add complex user-groups. Reactions also give us rating for amends (basically amends are child posts, subject to the same systems, and added to the tree). In theory a root post could be deleted, with amends retained, keeping context, and gives space for 'deleted because' from trusted signers. 

Ban kills key, but tombstone post keeps post, deleting content prop. Content is never retained under normal/official usage. I'd like to consider encrypted posts for private content, but might be too much of a vulnerability given UK govt attitude to e2ee, maybe future expansion. 

We'll support image uploads (with limits, it's news, we don't need 50 megapixel raws, like hd max jpg/webp only is good enough) but any server (even our own) could turn them off if abused. I don't even mind about LLM generated content, tho some servers could autodetect and autotombstone if they want, none of my business.

My earlier idea was to use this via a client-side p2p system, but it doesn't work for persistent content. Server only works, but in theory we could use webrtc for small client-side temp micro-servers (like sign multiple content offline, mirror to a server, and go offline, content stays available) for fringe use-cases (event specific news, everyone at an event, say a concert, posting stories, that can be mirrored by the local network operator, and kept on-device, but never hits the wider web, without large local infrastructure).

"Never sell attention" is not on me, sell it if you want, but you will not make it into my network, and every part of the network I have any influence over will reject you too, money breaks trust, but that's my opinion, if you can make money and keep trust, you do you.

#### ChatGPT:
Crystal clear — here’s the distilled notes to bake into design/implementation later, so you don’t lose the threads:

---

## Scoring & Feeds

- **Score ≠ recency.**
  - Score is computed from reactions/votes alone.
  - Recency is just a filter (default on, but toggleable).
  - Multiple filters (trust threshold, tags, recency windows) can combine arbitrarily.

---

## Reactions

- Replace “vote + reason” with **reaction types**.
  - Examples: 👍, 👎, 📰 (credible), 🚫 (spam), ⚠️ (misleading).
  - Keeps UX simple, but richer signal than binary up/down.
- Server maps reaction → weight multiplier.
- No reason-picking friction, but retains semantic info.

---

## Trust Sources

- Trust from **posts** definitely.
- Trust from **comments**:
  - Useful if punditry is comment-heavy.
  - Risk: users spam “low-value” comments just to farm trust.
  - Option: posts drive global trust; comments only drive *tag-local pundit trust*.
  - Needs tuning to avoid “klout chasing.”

---

## Amendments

- Don’t allow free-for-all amends.
- **Poster sets thresholds** for who can propose/merge:
  - Example: `propose if trust > me.trust`
  - Example: `amend if trust > me.trust * 10`
- Amends behave like **child posts**: subject to the same trust/reaction system.
- Quorum optional, but secondary — simpler to rely on “follow-on posts” that reference the original.
- Root deletion → amends persist, providing continuity/context.
- Trusted signers can issue “deleted because” notes.

---

## Content Lifecycle

- **Ban**: kills signer key, all their trust gone.
- **Tombstone**: metadata persists (id, tags, signer, timestamp, reactions), body deleted.
- **Content not retained** officially. Third parties may mirror/arch archive.
- **Encrypted posts**: possible future expansion, but risky in hostile jurisdictions (esp. UK).

---

## Media

- Support **image uploads** with limits (e.g. max HD JPG/WebP).
- Any server can disable images locally.
- LLM-generated content is acceptable; servers may auto-tombstone if they choose.
- Goal: flexible but cheap — no giant raw files.

---

## Federation / Client Architecture

- **Server-based persistence** is the main model.
- **Client-side ephemeral P2P (WebRTC micro-servers)**:
  - Temporary, offline-first signing and mirroring.
  - Good for event-specific use (concerts, protests, disaster coverage).
  - Mirrors keep availability after devices go offline.
  - Fringe use-case, not primary.

---

## Culture & Funding

- **Never sell attention**: core ethos for your network.
- Other servers may try — but trust system + federation filters will keep them out unless they genuinely earn it.
- **Donations fine**, voluntary like Wikipedia.
- “Money breaks trust” is a guiding principle for your slice of the network, even if others differ.

---

⚑ This boils down to: *Keep scoring clean and independent; keep amendments constrained; trust derives from meaningful contributions, not noise; federation + filters protect culture; money stays out.*

Do you want me to consolidate this into a **“Design Principles” section** (like a manifesto + non-negotiables) that sits at the top of the README, so contributors can’t miss these red lines?
