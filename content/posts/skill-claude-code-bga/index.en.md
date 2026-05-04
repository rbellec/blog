---
title: "A Claude Code skill that ships, tested across four games"
date: 2026-04-29
draft: false
description: "Building a Claude Code skill through four game adaptations on Board Game Arena: design choices, test loop construction, silent pitfalls, and what an agent decides when the spec stays silent."
tags: ["Claude Code", "Skills", "Board-Game-Arena"]
categories: ["Claude Code"]
---

How do you build a skill that actually ships? How do you build a test loop that closes? What does an agent decide on your behalf when the spec stays silent? Adapting board games on Board Game Arena gave me a case study — bounded scope, constrained framework, instant validation. One example: April 10, 2026, with the help of this skill, I let Claude start from a game's rules (Quantum Tic-Tac-Toe) and reach a complete game in under three hours, without writing a single line of code by hand. Four games later, here's what I took away.

After the post-discovery rush around Claude Code, I asked myself the same questions as many others: how do you steer it to automate development and tests as much as possible? What method should one use to write skills and other steering tools? When should you lean on tooling versus on the model's own capabilities? And if the code writes itself — what's left, plumbing or permaculture?

To make headway on these questions, adapting board games seemed an excellent test ground: short projects, fast feedback, a heavily constrained framework, products generally far better defined than any SaaS service (rules). On top of that, in 2023 I had promised a friend to adapt his game to Board Game Arena "within ~1 month" — the tension was becoming palpable...

**Goal:** write a skill that lets Claude Code deliver a board game adaptation directly from the rules. Understand the design and iteration choices that go into a skill aimed at producing a finished product.

## First steps

Lucky me, BGA (Board Game Arena) uses PHP, a language I have absolutely no affinity for. Ideal terrain for developing "without touching or even glancing at the code".

The first attempts are interesting: questions are automatically extracted and sent to the authors, a first version of the code is produced, a first board is auto-generated as SVG, everything looks fine, first deployment...

Nothing works! It's not even possible to start a game!

The error messages flash for a few seconds in the browser, and trying to copy them before they vanish quickly turns into an unpleasant race against the clock.

Annoyed by this repetitive, dull, unsuccessful step, I take a moment to reflect and recall the three cardinal virtues of the programmer: Laziness, Impatience, and Hubris. That gets me to immediately work on automating the entire chain from deploy to test using Claude in Chrome. The error itself was unpleasant, but the loop "deploy → test → capture the error message → fix" is something I would have long dreamed of, had I considered the possibility! I later used the same principle to test all the rules through complete games.

Yet the following hours did not unblock the situation: the project had been started on an obsolete version of the BGA framework. Since I hadn't laid a hand on the code, I hadn't seen this. The unblocking came from a change of approach: rather than try to make this code work, I picked up an example codebase, verified it could spawn a game, and asked Claude to gradually "port" the game's code over to that example, testing as it went. That took ~20 minutes, with no intervention from me.

In its current state, the skill therefore makes the following choices:
- We start by downloading the "base" code as our starting point. If the framework changes, the skill always has the latest version.
- The test loop is built in and designed to run via DOM events rather than screenshots analyzed by Claude. As much for speed as to reduce Claude Code usage.

## First lesson: start smaller than you think

"It is faster to make a four-inch mirror and then a six-inch mirror than to make a six-inch mirror." [source](https://wiki.c2.com/?TelescopeRule=&utm_source=chatgpt.com)

The first game I picked, **Go On Rasalva** (a game I co-authored, not yet released), was fairly complex: a hexagonal board with a tricky color distribution to describe, resource management, changing turn order, placement and transformations, and so on. I had simply chosen the game I had promised back in 2023 for plain reasons of social pressure...

But considering the real goal — automating this kind of development — I quickly realized I'd probably move faster by starting with something genuinely simpler. So I switched the work over to tic-tac-toe, with the goal of having a game playable and tested end-to-end by Claude Code.

The approach proved its worth, and I continued with other available games while gradually adding features (a deck of cards, different user interactions...): Quantum Tic-Tac-Toe ([here](https://boardgamegeek.com/boardgame/171143/quantum-tic-tac-toe)), Royal Visit ([here](https://boardgamegeek.com/boardgame/22245/royal-visit)), Duelly ([GitHub](https://github.com/rbellec/bga_duelly), completed end-to-end — publication pending a licensing issue). Each adaptation let me incrementally improve the skill by going through the full process end-to-end, until eventually I returned to the first game with a process that just needed polishing.

Whether this transfers to other domains remains an open question. For projects that are bounded with a similar process (like game adaptations), it seems to fit. For other domains — for instance, evolving and maintaining a SaaS service in production with a wide functional surface, full infrastructure, and a lot of legacy code — I'm not sure this approach is feasible.

## When AI hits a wall / is biased

Go On Rasalva had a hexagonal board with a particular coloring scheme I couldn't manage to describe. Whether to Claude Code or to ChatGPT, I systematically got an "alternating" coloring, like a checkerboard. I tend to call these "10:10 problems" — a nod to image generators that only know how to depict watches at 10:10.

In this case, I found no other method than supplying code as the spec. I had solved this problem in Haskell a few years ago, and that was enough for it to be transcribed straight into PHP. I plan to keep the idea of "code as part of the spec" for cases like this in the future, though I wonder how many problems of this type actually exist.

The other side of the same difficulty: when Claude Code falls into a trap specific to the framework, getting out without domain knowledge is tricky. It can spin on the same error for a long time without making progress. What's striking is that it sometimes finds its own way out through a lateral path I wouldn't have thought of — but it's still on the pilot to spot the stall signal before the wasted effort becomes the real problem.

## Hallucinations / interpretations

During the first human play-tests, one behavior surprised us strongly: every game produced has "default behaviors" that are technically valid given the rules, but unexpected. For example, the generated code automatically moves one or several pieces in a way that looks like the best in most cases — but that should remain the player's decision, since they may have a more elaborate strategy.

While reading rulebooks, I also ran into several "hasty interpretations", even on rules that were perfectly well defined.

This drift toward generalization seems to be an asset for moving fast. But it amounts to handing structuring choices over to the AI. It shows up quickly when it's a game; it looks like an entire field of work for more complex software: which "automatic" choices can we accept on a SaaS that processes payroll, for instance? How do we tell apart what should be specified from what we can let the AI decide? I don't have a settled view at this stage; I plan to treat the AI's output as a first draft to feed product discovery.

A related behavior, less critical but recurring: Claude Code sometimes offers advice that would be perfectly sound for a human team — *"better to ship this stable version and iterate from there"*, *"this refactor represents several days of work"*. These recommendations make little sense when the feature in question takes five to ten minutes and the value is real. My sense is that this bias shows up especially when the request *looks like* serious team work — a refactor or an architecture task — rather than exploration. Others have noticed the same thing.

## When the rules say nothing

Key difference with the previous section: here, the AI isn't inventing, the code isn't at fault. Plainly, **the rules say nothing**. They're sometimes incomplete or ambiguous, and that's normal — a rulebook was never written to serve as a spec. It's a teaching document aimed at human players who can hold ambiguity in their heads, ask around the table, or improvise a compromise. The code has none of those options: each case has to be decided explicitly.

So I adopted a simple workflow, refined as the games went on. The skill asks Claude to produce three distinct files rather than one:

- `RULES.md` — a faithful reformulation of the PDF, no interpretation. This is what we re-read to verify nothing has been deviated from.
- `ASSUMPTIONS.md` — each interpretation choice gets an `[Hx]` identifier, referenced from the code (`// [H11] Win checked before blocker action`).
- `AUTHOR_QUESTIONS.md` — each remaining ambiguity becomes an explicit question, with a status (OPEN, ASSUMED, CLOSED) and a category (RULES-AMBIGUOUS, RULES-MISSING, RULES-IMPLICIT...).

The `AUTHOR_QUESTIONS.md` document is sent to the authors. Each round of feedback flips a status, sometimes drops an `[Hx]` hypothesis, and sometimes wipes out an entire subsystem before it has even been implemented.

**Concrete example on Duelly.** The rulebook mentions "Joker" cards that could apply in several situations — including on a winning move. A simple question: *"Does the Joker apply to a winning Move?"* The authors' answer was unexpected: *"Actually, to keep things simple, we're scrapping Jokers altogether."* Immediate effects: a game state, a database column, a UI indicator, and a branch of the win condition — all removed before being coded. A few hours of work saved by a question written in two minutes.

This part of the workflow isn't AI-specific: a human developer adapting a game asks the same questions. The difference is that the AI workflow *forces* explicitation. A human can hold ambiguity in their head, waiting for the situation to come up; an agent can't, and that constraint is what produces the written, auditable trace that ends up being useful for the authors too.

## Building the skill incrementally

The skill's construction broadly happened in two parts:
- A general architecture with behaviors, links to relevant docs, and "processes" (like starting by transcribing the rules from PDF to markdown).
- A "reactive" construction, in the **registry of regrets** style, where each rule comes from fixing an incident.

I'm undecided about whether to offer end-users a way to add their own rules automatically: when Claude hits an error that could be resolved by adding a rule, the skill could suggest amending it and opening a PR.

To pre-empt context-size issues, I split the skill into several files. For now I assume the race between adding new rules and the growth of model context windows leaves us some margin before this needs optimizing.

A word on git worktrees: they're genuinely pleasant to use, but they run up against the small size of these projects. Each task touches a lot of shared code — worktrees end up waiting on each other, which undercuts the point of working on several things in parallel.

### Example of a pitfall

While building Quantum Tic-Tac-Toe (QTTT), Claude spent more than an hour on a single error — silent, lurking, treacherous... (we so rarely get the chance to use dramatic prose in this profession; I'm taking the opportunity).

When BGA Studio handles the database model file `dbmodel.sql` (a single file, no migrations concept), it starts by stripping all newlines from inside statements before sending the result to MySQL as a single string.

```sql
-- Before newline stripping:
CREATE TABLE `t` (
  `a` INT NOT NULL,   -- first column
  `b` INT NOT NULL,
  PRIMARY KEY (`a`)
) ENGINE=InnoDB;
```

becomes, for MySQL:
```sql
CREATE TABLE `t` (  `a` INT NOT NULL,   -- first column  `b` INT NOT NULL,  PRIMARY KEY (`a`)) ENGINE=InnoDB;
```

MySQL therefore cuts the statement off at the start of the comment, creates the first column, returns `success` — without even a warning making it back to us. The first symptom appears at runtime as `Unknown column 'b' in 'field list'` when the code tries to read that column.

The BGA documentation states "if you use comments, then you must not do it in the same line as the code" — the kind of line you typically find AFTER you've understood the error. Humans probably still have an edge over AIs here, since I don't know any of them who comment individual lines of their SQL queries! Others will hit this problem very quickly.

Two further problems in the same register — silent, hard to diagnose — came up during the journey:

- `getCollectionFromDb($sql)` indexes the returned PHP array by the first selected column. If that column isn't unique, later rows silently overwrite the previous ones. `SELECT square1, square2 FROM q_moves` lost half the entanglement graph. Technically a model (and probably a developer) should recognize this as a PDO (PHP Data Objects) API where the framework takes its own interpretation.
- A collision with an internal framework table name. The official documentation lists 4 reserved tables (`global`, `stats`, `gamelog`, `player`, plus any `bga_` prefix). Empirically testing this while writing the article, I found at least a fifth: `replaysavepoint`, no `bga_` prefix, created by the framework at boot. If you redeclare that name in `dbmodel.sql`, your schema is silently overwritten by BGA's — exactly the same behavior as for the 4 documented tables. *On a related note, while running this test, I also realized that `moves` — which I had thought reserved and preemptively renamed in QTTT — is actually fine. The bug was simply the inline SQL comments described above. The `moves → q_moves` rename served no purpose, but it did inspire half of the `[Hx]` "don't redeclare an internal table" rule in the skill.*

**Another silent pitfall of the same kind, but at a different layer.** Claude Code had tested every rule of a game through the DOM loop — placement, movement, win, all green. On the first human test, surprise: *no piece responded to a click*. The test mechanism, which goes through `gameui.ajaxcall` to trigger framework actions, had never exercised the mouse→piece path. The test loop certifies what it tests, and only that: the business logic was sound, the interaction wasn't. The human test remains, for now, the only thing that catches this category of bug.

## Graphical adaptation

I expected to spend time supervising the integration of the graphical assets — that's often where BGA adaptations become tedious (for me at least; it's not my favorite part). Reality was different: Claude Code handled all of Duelly's graphical adaptations, including analyzing source files via OCR, without me having to touch anything. I couldn't put an exact number on it, but I'd say roughly 3 to 4 hours from start to finish including back-and-forths with the author — 80% of the work done in under 30 minutes.

![Duelly on BGA Studio](images/DuellyOnBgaStudio.webp)

A useful calibration: with a well-tuned tool, a particularly responsive author who provided all the assets and answered every rules question, and a reasonably complex game, the full adaptation — code and graphics — took about four full days. For teams that regularly do this kind of adaptation and have their own process, the gain should be substantial. For someone starting without prior landmarks, the tools genuinely ease the code and rules-analysis work, but I don't think you can comfortably get below a week — especially if back-and-forths with the author or graphical assets take time.

One limit I hadn't anticipated: on BGA, translations only become available once a game reaches the alpha stage — there's no way to work on localization ahead of time. And more fundamentally, Duelly's card illustrations are entirely in French. To truly internationalize the game, the assets would need to be redone — either with neutral or localizable text, or through actual pictographic design work. That's outside my scope as an adapter, but it's a concrete lesson for the author: a game designed for international play from the start avoids this illustration rework entirely.

## A few numbers

Audit of the Quantum Tic-Tac-Toe repo on 2026-04-28:

- **~1080 lines of code** (PHP 679, JS 271, SQL 23, CSS 105) — excluding the BGA scaffold and JSON config files.
- **~49 minutes between the initial commit and the first fix**, which corrected two of the three bugs. The third bug — the `getCollectionFromDb` first-column indexing — surfaced the next morning, ~11h later. The "**first game in ~3h**" figure is therefore plausible if you include the setup time before the initial commit, but it should be clear that this is the **first end-to-end playable game**, not the first game without any bugs.
- **3 bugs caught and fixed via the test loop**: silent SQL comments, table-name collision, and `getCollectionFromDb`. All traceable in the commits, so verifiable.
- **~15 deploy → test → fix cycles** and **~5 human interventions** (SSH key setup, Express Stop on stuck tables, browser extension reconnect).

These numbers measure a **simplified** variant of Goff's rules (resolving simultaneous wins by "lowest maximum subscript" rather than by the point-sharing described in the original article), with a skill that hadn't yet absorbed the lessons from later games. This is the measurement of an on-ramp, not of a sustained throughput, and certainly not an announcement that "you can build any board game in 3 hours". These numbers still give me pause.

Audit of the Duelly repo on 2026-05-04 — a complete game with cards, animations, statistics, and internationalization:

- **~3,025 lines of code** (PHP 1,811, JS 923, CSS 271, SQL 20) — roughly 3× the volume of QTTT, for a significantly richer feature scope.
- **94 commits across 8 active days** spread over ~3 calendar weeks (April 15 → May 4), with gaps due to back-and-forths with the author and human testing sessions.
- **Breakdown**: 20 fix, 14 feat, 7 refactor, 15 ui/i18n, 24 chore/docs — a picture of a game finished end-to-end, not just "running".

These numbers include the PHP quality pass and the full graphical adaptation. QTTT measured the on-ramp; Duelly measures what the process delivers once it's dialed in.

## Conclusion

A first piece of work that's genuinely interesting: long enough for the pitfalls to emerge from the scenery and for good practices to settle in.

Board games on BGA Studio turned out to be a surprisingly valuable training ground: small functional scope (the rules), an imposed framework, instant validation (the game runs or it doesn't). Everything that makes a SaaS service painful to tame disappears. What's left — the spec, the framework's pitfalls, the test loop — is sharp and visible. It let me see Claude Code-specific behaviors that would probably have slipped past me on a larger project.

A common question, still without an answer: **what is the value of code quality when no human reads the code anymore?** I spent time on it during the project — refactors, naming, debt reduction — without being certain it makes subsequent AI development any easier. The question remains open.

Of the five games I started, only one is done end-to-end: Duelly, in about four full days — code, graphics, and animations. The game's author was particularly responsive throughout the project, and that made a real difference. The public release, though, is running into an administrative deadlock: a BGG listing is required to apply for a BGA license, but BGG wants to keep its pages to physical games, and the potential publisher is waiting to see whether the game does well on BGA before considering a print edition.

The bottleneck shift is real: code is no longer the problem; it's the authors' availability and the coordination around human testing that slows the chain down.

The [`claude-code-bga` skill](https://github.com/rbellec/claude-code-bga) is published. I'm hoping for feedback, cases that break it, rules to add, and more broadly comments on the approach itself — that's exactly the kind of input that has moved the skill forward so far.
