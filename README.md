<h1 align="center">Hamzah Punjabi</h1>

<p align="center">
  <b>Software Engineer</b> · Doha, Qatar<br>
  I build the unglamorous half of the stack — the APIs, caches and data pipelines<br>
  that have to be right at 3am when a match kicks off on the other side of the world.
</p>

<p align="center">
  <a href="https://hamzahap.github.io/hapunjabi/"><img alt="Portfolio" src="https://img.shields.io/badge/portfolio-terminal-4ade80?style=for-the-badge"></a>
  <a href="https://www.linkedin.com/in/hamzahpunjabi/"><img alt="LinkedIn" src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge"></a>
  <a href="mailto:hamzahqatar123@gmail.com"><img alt="Email" src="https://img.shields.io/badge/Email-D14836?style=for-the-badge"></a>
  <a href="https://www.credly.com/badges/1769a415-88da-4445-9447-f92b9ae73337/public_url"><img alt="AWS Solutions Architect Associate" src="https://img.shields.io/badge/AWS-Solutions%20Architect-FF9900?style=for-the-badge"></a>
</p>

```console
$ whoami --verbose

  role      Software Developer @ AllThingsRugby / Bin Yousef
  shipped   The Rugby App — 50k+ downloads, live scores & stats
  shipped   Breakline & EdgeBloom — two mobile games, published
  building  Majlis — social multiplayer platform, TypeScript top to bottom
  building  four games, Unity and Godot
  stack     .NET · SQL · Next.js · Flutter · TypeScript · Firebase
  certs     AWS Certified Solutions Architect – Associate
```

---

### 🏉 What I actually do all day

I own the backend and platform for **[All Things Rugby](https://allthingsrugby.com/)** and **The Rugby App** — live scores, stats, highlights and editorial for 50k+ downloads.

The interesting problem isn't any one endpoint. It's that upstream providers disagree with each other, go stale, and occasionally just lie — while a user staring at a live match expects a correct scoreline *right now*. So the platform is built around that:

- **High-throughput REST APIs** over multiple third-party providers — 900%+ throughput gains from in-memory caching, aggregation and concurrency-safe refresh, not from bigger servers.
- **Feed validation upstream of the cache.** If a bad payload gets past it, that error is aggregated, cached and served to everyone until the TTL expires. Catching it early turns a user-visible defect into an alert nobody outside the team sees.
- **An internal CRM** with role-based access that lets editors override anything a provider gets wrong.
- **[OpenClaw](https://openclaw.ai/) workflows** — I run the open-source assistant, I didn't write it; what I built are the workflows on top. One drafts the daily rugby brief (pull, dedupe, rank, hand it to an editor). The other watches match feeds through the weekend and alerts us when provider data disagrees with what the match state says is possible.

---

### 🛖 Majlis — the big one

A social multiplayer platform built around persistent rooms: a community makes a room and it stays theirs to chat in, hang out in, and play casual games in. A TypeScript monorepo — Expo mobile app, Fastify API, React admin dashboard, shared packages between them.

The decision I'm happiest with is that **game logic lives in its own package**, behind a generic `GameEngine<State, Action>` that knows nothing about HTTP, sockets, React, or the database. It validates a move, applies an action, decides a winner, and derives *the public state a given player is allowed to see*. That last part matters: the server computes what each client may know instead of shipping full state and trusting the client not to look. Get it wrong and a card game is trivially cheatable. Adding Chess or Dominoes later means implementing an interface, not touching the server.

Realtime and durability are split on purpose — Socket.IO and Redis hold what's true *right now* (presence, sessions, in-flight match state) so the API stays stateless; PostgreSQL via Prisma holds everything that must survive a restart.

Deliberately gambling-free. No loot boxes, no randomized purchases, no wagering — which rules out the easiest revenue model in social gaming, and is exactly why it's worth stating up front.

`TypeScript` · `React Native` · `Expo` · `Fastify` · `PostgreSQL` · `Prisma` · `Socket.IO` · `Redis` · `Docker`

---

### 🎮 Games

**Shipped** under the **hkinggames** name — end to end, including the unglamorous part: ad mediation, UMP consent gating, IAP, privacy policies and store review.

| | |
|---|---|
| **[Breakline](https://hamzahap.github.io/breakline/)** | One-touch precision runner. 20 chapters, 600 levels, second-wind revives. |
| **[EdgeBloom](https://hamzahap.github.io/edgebloom/)** | Line-capture strategy. 2–8 player pass-and-play, solo *Pulse AI*, 5 board topologies. |

**In progress**

| | |
|---|---|
| **Graveyard Shift** | Co-op roguelite where you're the dungeon janitors. Filth and cleanliness are real mechanics, not set dressing. `Godot` |
| **Arcabeasts** | Grid-based fantasy tactics. Four academy houses, shrine control, mana management. `Godot` |
| **Snakes** | Grid arena survival — every elimination makes the hunting snake longer. `Unity` |
| **Block Arena** | Quick chaotic PvP where the arena is the weapon. `Unity` |

---

### 🤖 AI tooling

Built mostly because I got tired of coding agents that stop three-quarters of the way through the job.

| Project | What it is |
|---|---|
| **[LLMContextOptimizer](https://github.com/hamzahap/LLMContextOptimizer)** | CLI + local proxy that ranks, compresses and bin-packs context to fit a token budget. Protection tiers keep stack traces and test output verbatim — the packer can drop context, never the trace you're debugging. `3.7k LOC · 63 tests` |
| **[OrbitDesk](https://github.com/hamzahap/OrbitDesk)** | Electron app running a plan → assets → execute → review pipeline, each stage routable to a different provider. Hardened IPC, Zod schemas shared across processes. *MVP — runs from source.* |
| **[VibeCodeMax](https://github.com/hamzahap/VibeCodeMax)** | Wraps a coding agent in a verify-audit-retry loop. Hashes a git snapshot to catch "the agent changed nothing" stalls; a run is only *complete* when an auditor says so. *v0.1.* |
| **QueryPilot** | Natural language → SQL, with the security boundary in the database rather than in a string parser. Started as a hand-written guard over the SQL text; adversarial review broke it three times, so the boundary moved to a read-only connection plus SQLite's authorizer, which runs inside the parser after name resolution. The string guard survives as a fast advisory filter and is documented as *not* the boundary. `739 tests` |

---

### 🧱 Backend

| Project | What it is |
|---|---|
| **TaskTracker** | .NET 8 task API where the architecture rules are enforced by the test suite — reflection-based tests fail the build if Domain or Application takes a dependency on EF Core or ASP.NET, or if an entity grows a public setter. Lifecycle invariants live in the aggregate, not the controller. `120 tests` |
| **Book Management System** | Spring Boot 3 + MongoDB, modelled as documents rather than tables in disguise. Search and paging execute in the database; sort fields are allow-listed so an arbitrary property can't be injected into the query. Tests run with no MongoDB and no Docker installed. `94 tests` |

---

### 🛠 Tech

```
Languages    C#  ·  Python  ·  TypeScript  ·  JavaScript  ·  SQL  ·  Dart  ·  Java  ·  C/C++  ·  GDScript
Backend      .NET  ·  ASP.NET Core  ·  Entity Framework  ·  Node.js  ·  Fastify  ·  Spring Boot  ·  Laravel
Frontend     React  ·  Next.js  ·  React Native  ·  Expo  ·  Flutter  ·  Vite  ·  Electron
Data         SQL Server  ·  PostgreSQL  ·  Prisma  ·  MongoDB  ·  Firebase  ·  Redis  ·  pandas
Cloud        AWS  ·  Azure  ·  Docker  ·  Kubernetes  ·  CI/CD
Games        Unity  ·  Godot  ·  SFML  ·  AdMob
```

---

<p align="center">
  <a href="https://hamzahap.github.io/hapunjabi/"><b>The portfolio boots as a terminal.</b></a><br>
  Type <code>help</code>, or go straight to <code>ps</code> — every project listed as a running process.
</p>
