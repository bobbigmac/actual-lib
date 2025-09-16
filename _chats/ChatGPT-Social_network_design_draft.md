# Social network design draft

#### You:
You'll find these files about ActualNews/ActualJobs and other supporting/related ideas under BBB. Take a look at their technique and function, and we're going to consider how they they might help us build a new social network, that works like an old social network, think ActualSocialNetwork (probably not the name, maybe ActualSpace or something like that). Read the chats, don't guess, if you can't read the chats, don't make stuff up.

I'm thinking the chronological feed of early twitter, the contacts list of linkedin, the lists of things you like from early facebook/myspace, a customisable landing page/profile _kinda_ like geocities,  maybe even the poke mechanic, just for fun, maybe brought up to date a little, maybe some basic web game, tho more like words with friends or wordle, maybe some jackbox-inspired family games built in, nothing complicated and zero scams or data farming. The simple chat of old-school chat rooms, with a little of the supportive features of discord and maybe even basic zoom (via webrtc).

None of this is mapped to a mineable graph or other tracking system, literally no backend systems, all content e2ee (maybe games and video wouldn't work for that, but whatever we can practically transmit in private, will be in private and untracked, literally no metrics). This would use a slight tweak to the ActualNews backend semi-federated/semi-p2p server model, where if you want your content to stay around and your server to be performant, you might want to pay for a micro-server to host it for you and your friends, that acts more like a package storage proxy, than it does a content server.

The trust system would certainly need tweaks (in code, not on-network), but all content sets permissions about who can push to it (ignoring or even flushing subcontent that gets applied, like a comment, if it doesn't meet the rules), and because everything is locked down via your encyrption keys, even public data, and involves a handshake to even get it, you can still block a key you know is bad, meaning the person/bad-actor trying to harass you has to lose their trust to have any engagement with you (it's not quite a ban, but is a tool to help).

#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
Awesome. I read through your ActualNews/ActualJobs chats and the BBB tool notes and pulled the techniques that matter. Below is a tight “design spine” for **ActualSpace** (placeholder name) that borrows what works, drops what doesn’t, and keeps your “no metrics, no tracking, E2EE where possible” ethos intact.

# What we’re reusing (proven bits)

- **Keys, not accounts.** Everything is signed by a user key; lose the key, lose reputation (content may persist, but trust doesn’t migrate). fileciteturn1file1
- **Trust-weighted interactions, per tag.** Votes/reactions carry weight by signer trust^α; trust grows/shrinks only from how *your own* posts perform; no anti-consensus penalties. Client has **trust slider**; and a **peer scope slider** for federation exposure. fileciteturn1file9
- **Servers are first-class actors.** Anyone can run one; each server assigns trust to peers; rate budgets/quorum tame floods; bans/tombstones propagate but are up to each server to honor. Keys can be scoped to an issuer server; no reputation migration. fileciteturn1file9
- **Schema-first content + GitHub governance.** Post types are JSON-schema’d; changes via PRs/issues (style guides encouraged). fileciteturn1file11
- **Amend/Update & Tombstone model.** Authors (and possibly trusted others) can amend; merges are explicit; deletions keep metadata, content hidden. fileciteturn1file9
- **Privacy for sensitive comms.** Jobs shows the pattern: public post with an *extra* public key to accept encrypted attachments; high-trust comms notify, low-trust are silently stored until the user asks. We’ll reuse that for DMs/invites. fileciteturn1file8
- **Separation of datasets across products.** ActualNews and ActualJobs keep their own trust/data; you *may* request/replicate based on a specific key, but there’s no shared global dataset by default. Also: avoid PII in public content. fileciteturn1file8

# What “ActualSpace” needs (mapped to your feature list)

## Core objects (schemas)

- **Note** (early-Twitter style): short text, optional tags. Chronological by default; reactions are trust-weighted, but *feed is not engagement-sorted*.
- **List** (things you like): a signed collection of links/tags/items. Can be public, friends-only, or private-E2EE.
- **Profile Page** (Geocities-lite): markdown + safe CSS subset (no JS), optional theme tokens. Public bits are still request-gated (handshake); PII discouraged by linting rules.
- **Poke**: minimal “signal” object (no text, just intent), throttle-limited and trust-gated.
- **Room** (chatroom): invite-link + rotating keys (from ActualFamily thinking), optional passphrase to join; messages via WebRTC DataChannels with server as fallback relay. fileciteturn1file4
- **Call**: WebRTC session descriptor; keys derive from room/session; video optional (SRTP).
- **Game Session**: turn/state CRDT over WebRTC DataChannels; built-ins kept *simple* (Wordle/WWF/Jackbox-lite).
- **Event**: beacon on a map with “now / later / recurring”; ephemeral by default; export ICS; discovery by tag + distance. fileciteturn1file7

*All of the above are signed envelopes with optional encrypted child payloads (for private replies/attachments).*

## Permissions & safety (fits your “no mineable graph”)

- **No auto-graph.** There isn’t a global follow graph to mine. “Contacts” are just keys you’ve explicitly handshaked (mutual allowlists).
- **Handshake before even public fetch.** Public content can require a lightweight, rate-limited request/accept handshake; once accepted, you can fetch the author’s “public” feed. (You can still publish truly open posts, but default UX nudges toward request-gated.)
- **Blocklists at two layers.** User-level key blocks, plus server-level peer de-trust. Harassers must burn trust (and keys) to re-engage. (Jobs/News already use this posture.) fileciteturn1file9
- **Notifications respect trust.** High-trust sends ping; low-trust lands in a quiet “requests” mailbox (Jobs pattern). fileciteturn1file8
- **Edits & vanish.** Authors can amend or tombstone at will. Server can invalidate a key on request and nuke attached content; sub-content becomes “detached/flushable” (visible only if navigated directly). (This mirrors your News/Jobs deletion stance.) fileciteturn1file9

## Feeds & discovery

- **Home** = *your* rooms + handshaked profiles + events you opted into, purely chronological by default.
- **Explore** = tag timelines with trust & peer sliders (identical UI concept to News). fileciteturn1file9
- **No metrics.** No view counts, no time-on-post, no “people you may know.” Only transparent, signed reactions.

## Hosting model

- **Micro-servers for persistence & relay.** Users can rent a tiny “space server” that acts like a package proxy for their signed blobs and as a TURN/relay for calls/games. (Same “semi-federated/semi-p2p” idea you floated for News; Jobs stayed single-site initially.) fileciteturn1file9 fileciteturn1file6
- **Federation is optional, scoped.** You may peer with other space servers (street groups, clubs). Server trust and rate budgets apply, just like News: peers can be allow-listed, weighted, or ignored. fileciteturn1file9

## Moderation (minimal, decisive)

- **Hard bans only for hard abuse** (fraud/harassment/illegal) → key tombstoned; start over if you must. (Your one-strike ethos.) fileciteturn1file16
- **Soft friction for everything else.** Low-trust content de-surfaced by default; users can opt-in to see it. (Jobs pattern.) fileciteturn1file8
- **Style guides.** Short, enforceable guidelines (“Be human; don’t dox; no PII in public posts; use rooms for private chatter”), versioned in GitHub like News writers’ guides. fileciteturn1file11

# How the old-school vibes map cleanly

- **Chronological feed** → default timeline is strictly time-ordered; trust just gates *visibility*, not sort order. (Slider exposed.) fileciteturn1file9
- **Contacts list (LinkedIn minus surveillance)** → explicit handshakes (mutual key allowlists), not a mined graph.
- **Lists of things you like** → the **List** schema; public/friends/private-E2EE modes.
- **Customizable page (Geocities-ish)** → Profile Page with safe markdown/CSS; templates to keep it cute but non-exploitably cute.
- **Poke** → “signal” object; deliverable only if trust threshold passed (or it lands quietly in requests).
- **Simple games** → WebRTC DataChannels; state machine + CRDT; E2EE by room key.
- **Chat rooms** → Room schema + rotating keys; invite links; optional passphrase to unlock the shared key (ActualFamily concept). fileciteturn1file4
- **Basic Zoom** → the Call object over WebRTC with server TURN; E2EE where practical (SRTP).
- **Events (Meetup-ish)** → Event schema with map beacons and ephemerality (ActualEvents notes). fileciteturn1file7

# Guardrails that match your constraints

- **No shared “global trust” across products by default.** Each product keeps its own trust store; they *may* request by key to bootstrap, but no single dataset. fileciteturn1file8
- **Data minimisation as policy + code.** Linters reject obvious PII in public fields; encrypted fields exist for “must-share” details. (Jobs pattern.) fileciteturn1file8
- **Images/media**: default to text-first public posts (like Jobs’ caution), allow media in E2EE rooms; hotlinking only for public embeds if you decide to enable them later. fileciteturn1file6
- **Federation defenses (if enabled):** server-trust weights, per-peer rate budgets, quorum before surfacing, temporal dampening to blunt brigades. fileciteturn1file9

# MVP slice (shippable without boiling the ocean)

1. **Profiles + Notes + Handshakes**

- Keys + issuer servers; profile page; chronological home; trust/peer sliders. fileciteturn1file9

2. **Rooms (chat) + DMs**

- Invite links with rotating keys; low-trust message requests; digest email for trusted senders only (opt-in, Jobs pattern). fileciteturn1file8

3. **Events (map beacons) + Lists**

- Ephemeral events + ICS export; lists of links/interests.

4. **Pokes + Reactions**

- Minimal signals; trust-weighted reactions (not used for sort, only visibility).

5. **Calls + One simple game**

- WebRTC calls; a word game using the room key for E2EE.

# Risks & how the News/Jobs playbook answers them

- **Harassment & spam** → handshake gates, trust-gated notifications, user/server blocklists, temporal dampening, per-peer rate budgets. fileciteturn1file9
- **PII leakage** → schema linting, style guide, encrypted replies; tombstones on request. fileciteturn1file9
- **Federated sludge** → peer slider defaults to “local+trusted,” not “all”; server trust weights + quorum before surfacing. fileciteturn1file9
- **Key loss pain** → consistent rule from News/Jobs: “don’t lose your key.” No migration, clear UX warnings. fileciteturn1file1

---

If you want, I can turn this into a **concrete schema set** (Note/List/Profile/Room/Event/Poke/Call/GameSession) and a one-page **protocol brief** (envelopes + trust/peer math) so contributors don’t guess.
