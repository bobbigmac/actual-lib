# Actual... keys, not accounts

Actual builds everyday apps (news, jobs, feedback, tickets, debates, Q\&A) that work without accounts, ads, or tracking. You hold a *key*, not an account. Your key is your identity. Your data stays with you.

---

## Why “accounts” are the root problem

* **Accounts leak power.** Email + password puts the platform in charge. They can read, reset, ban, sell, and profile. If they get hacked, *you* get hurt.
* **Tracking is built-in.** Accounts stitch your actions together across products and devices. That’s great for ad-tech; terrible for privacy.
* **Resets = backdoors.** “Forgot password?” means someone else can be you. That’s a feature for support, and a gift for attackers.
* **Single point of failure** The org, or even just a single dev got hacked or social engineered? The leaked data is just unencryptable noise.

## Why federation isn’t the rescue you think

* **Same account model, different landlord.** You still trust a server admin with the keys to your world.
* **Many single points of failure.** Each server is a jackpot for attackers. One phish. One misconfig. One breach.
* **Still incentive-driven.** Federated hosts drift toward the same growth and data incentives as the big platforms, often user-data exploitation, leading to grasping and garden-walling.

---

## The Actual way: e2eDe (end-to-end *Data* encryption)

* **Your key, your rules.** You create a cryptographic key on your device. There’s no username. No email. No account to reset.
* **We can’t read your stuff.** Data is encrypted *before* it leaves your phone or laptop and can only be opened by the people you choose. The platform cannot read it, by design.
* **Crypto = policy.** What can be shared (or published later) is decided at the moment you send it, by how it’s encrypted. Not by toggles we could flip later.
* **No cryptocurrency.** No tokens, no mining, just boring, proven cryptography to keep everyday data safe.

Think of it like a house key: if you keep it safe, nobody else gets in. Lose it, and there’s no landlord with a master key. That’s the trade you choose for real privacy.

---

## Low-risk, low-cost, disposable by design

* **Small units, small blast radius.** Our apps are built from tiny pieces (messages, tickets, feedback packets) that stand alone. If one thing breaks, the rest keep working.
* **Built-in disposability.** Keys and links can be single-use or short-lived. Posters with a QR can open a pop-up mailbox for a weekend event, then vanish.
* **Cheap to run.** Servers store encrypted blobs and serve public pages. They don’t do surveillance or heavy personalization. This keeps costs—and temptations—low.

---

## Trust only when you need it

* **Most of the time, no trust layer.** Ask a question, leave feedback, buy a ticket—your key is enough.
* **When needed, add proofs.** For age-gated spaces or real-person checks, you attach a *verifiable credential* from someone you already trust (e.g., a certified age-check provider). We verify the signature; we don’t keep your personal details.
* **Reputation is portable and optional.** Some apps (like news/jobs) may show a track record for a key. Others don’t need it. Nothing is tied to a real-name account unless you choose.

---

## Some early ideas for v1 examples

The core of this approach is to replace platforms that are bloated with dark-patterns and invasive networked tech, and provide simple/easy tools that are accessible to people, low-risk, but high-value, and can demonstrate that the principles can work in practice. 

* **ActualFeedback:** A shop prints a QR. You scan and send private feedback. They can act on it, or—if you allowed it—publish a snippet. Works for research via collection schemas too.
* **ActualTickets:** An indie venue issues 200 tickets. Your ticket is a signed pass bound to your key. No per-ticket fees, no overselling, no scalping chaos.
* **ActualDebates:** Arguments are claims + evidence, not dunks + likes. Observers mark “agreed,” “contested,” or “needs evidence” to build shared understanding.
* **ActualAnswers:** Ask. Answer. Cite. Readers tag answers as clear/unclear or supported/needs sources. Usefulness beats popularity.

Across all of them, there’s no account to make, nothing for us to mine, and nothing valuable for attackers to steal.

---

## Why this is safer for everyone

* **People:** No profiles. No inbox full of “we lost your data” emails. Your key can even live in your password manager, wrapped with a passphrase, or even a physical dongle.
* **Organisations:** Lower liability. Regulators can demand content, but they still need the key from the owner to read it. The org gets utility without risk: fair ticketing, real feedback, structured discussion.
* **The internet:** Fewer honeypots. Fewer incentives to extract and manipulate. More tools that do one job well without a hidden cost.

---

## What we *don’t* do

* No ad networks, no profiling, no tracking.
* We don’t sell or share behavioural data.
* We don’t do crypto-coins, tokens, or pay-to-play.

---

## The pitch to creators, organisers, and communities

* **Own your audience, not an account.** Your key is your channel across every Actual app. No platform tax, no lock-in.
* **Prove fairness, publicly.** Issue 500 tickets; show the world you issued 500 tickets. Publish aggregate feedback without exposing anyone.
* **Start small, grow clean.** Launch in minutes. If it works, keep it. No data landfill to find you later.

---

## Why now

People already understand “we can’t read your messages.” We’re extending that to *everything else you do online*. End-to-end **Data** encryption (e2eDe) makes safe defaults the norm, not a premium feature.

If you want a web that isn’t vulnerable by design—where even the operator can’t exploit you—this is how we build it: **keys, not accounts; crypto, not promises.**

## Progress

This is the start of a project to build a library of tools to support building services with this approach, with best-practice and standards-compliant tech, that is portable and accessible. This 'pitch' is just a starting to point to ask for feedback and concerns I may have skipped over in focusing on the tech first.
