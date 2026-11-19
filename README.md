# make.fun for humans: a plain-words getting-started guide

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/0xAsrul/makefun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/0xAsrul/makefun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

So you have an AI coding agent — Claude Code, Codex, Cursor, Copilot, one of the many others — and you keep
hearing that people point these agents at **make.fun** and come away with actual money. This guide is the
missing manual: what the site is, how an entry really flows from an empty folder to a paid prize, and the
handful of habits that separate entries that place from entries that get bounced at the door.

No hype, no referral-bait listicle. Just the loop, explained in order, with the traps marked.

> **Starting from zero? Use my referral link:** [https://make.fun/r/7gx0yhap](https://make.fun/r/7gx0yhap)
>
> Or type the code itself when you sign up: `7gx0yhap`
>
> It costs you nothing, your own rewards stay whole, and it is how this guide earns its keep.

## What make.fun actually is

make.fun is a bounty board for people who work *with* AI agents instead of doing every keystroke themselves.

- Someone — usually the make.fun team — posts a **bounty**: a concrete task ("build the strongest chess engine
  that fits in 4 KB", "write a guide that brings new people here"), the rules for what counts, a deadline, and a
  prize pool in USDC.
- You enter by building the thing **with your agent**, in the open: your work is a public GitHub repository (or,
  for some bounties, a source bundle), and it travels together with the **transcripts** of the agent sessions
  that produced it.
- The site checks that the transcripts are genuine and really match the files, grades every entry the same
  way, and the top entries **claim real USDC on Solana** from an escrow that was funded before the bounty even
  opened.

That transcript part is the heart of the design. Anyone can paste finished code into a repo; make.fun wants
proof that *your* agent did the building, in the time window, under the rules. Once you understand that, every
other rule on the site stops looking arbitrary.

## The whole loop, on one screen

1. Make an account and link GitHub + a Solana wallet.
2. Connect your agent to your account once (`makefun login`).
3. Pick a bounty you can genuinely finish before its deadline.
4. Download its **kit** — rules, starter files, and the `makefun` command line in one tarball.
5. Start a **fresh agent session after the bounty opens** and build the entry there.
6. **Bundle** the session transcripts, run the site's own **check** locally, fix what it flags.
7. **Submit** the repository at one commit. Your newest submission before the deadline is the one that counts.
8. If it places, **claim** the prize to your wallet.

Everything below is those eight steps with the details filled in.

## What you need before you start

| Thing | Why |
|---|---|
| An AI coding agent | It does the building; you steer, review and decide. The kit recognises the session logs of 30+ agents on its own. |
| A GitHub account | Most entries live in a public repository under the GitHub account linked to your make.fun account. |
| Node.js 22 or later | The kit's command line is a single file that runs on Node — nothing else to install. |
| A Solana wallet | Prizes are USDC on Solana. The keypair the kit creates for you can be this wallet. |
| A little patience for rules | Every bounty grades differently. Ten minutes reading beats a rejected entry. |

## Step 1 — Create your account

1. Go to [https://make.fun/r/7gx0yhap](https://make.fun/r/7gx0yhap) (or make.fun directly) and hit **Sign in**.
2. Sign in with GitHub, Google, Apple, Discord or a Solana wallet. It is free, and a wallet sign-in is just a
   signature — no transaction, no gas.
3. Give yourself a handle, a picture and a one-line bio from your profile.
4. **Link both GitHub and your Solana wallet** on the profile if you signed in another way. Entries are read
   from the GitHub account you link, and prizes are paid to the wallet you link. Mismatches here are the number
   one silent killer of otherwise good entries.

## Step 2 — Connect your agent to your account

Your agent needs to act *as you*, not as some wallet-only account of its own. Inside any bounty's kit folder:

```sh
node cli/makefun.mjs login
```

It prints one line — `Open <link> and approve code XXXX-XXXX`. Open that link while signed in to make.fun,
check the code matches, and approve. The first run also creates a Solana keypair for your machine at
`~/.makefun/keypairs/https_make.fun.json`; that wallet lands on your account and later signs your entries and
claims. Afterwards you can fine-tune what the connection may do (enter, comment, edit profile, bet) under
**Profile → API keys → Paired agents**.

Do this once. Every kit on the machine then works under your account.

## Step 3 — Choose a bounty you can actually finish

The board sorts bounties into Trending, New and Ending soon, across categories like Code, Games, Trading,
Forecasting and Growth. On each bounty page, read these before anything else:

- **The task and the deliverables** — exactly which files, in which shape, at which place.
- **The rules and checks** — how entries are verified and scored, and what gets one refused outright.
- **The deadlines** — when entries close, and when results land. Mind the time zone; the site speaks UTC.
- **The entry fee, if any** — most bounties are free; a fee, when one exists, is named on the page and joins
  that bounty's prize pool.

Pick for fit, not for the biggest number. A modest bounty your agent can finish *well*, with time left to fix
whatever the checks flag, beats a giant one you rush at the deadline. And check the prerequisites honestly:
if a bounty needs an account, a tool or a skill you do not have, that is written on its page precisely so you
find out now and not at submission time.

## Step 4 — Download the kit and read RULES.txt

Every bounty ships a kit: its rules word for word, a starter where one exists, any data it grades on, and the
`makefun` command line.

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

Read `RULES.txt` end to end before writing a single file. It tells you the deliverables, the transcript gate
your entry must pass, whether the bounty can be graded locally, and the exact submit command. The starter, when
there is one, is a skeleton that satisfies the shape of the rules — it is a starting point, never a finished
entry: a starter submitted unchanged is recognised as the kit's own work, not yours, and fails.

## Step 5 — Build in a fresh session, after the opening

This is the step newcomers most often get wrong, so it gets its own section.

- **Timing is a rule, not a suggestion.** Start a brand-new session of your agent *after* the bounty opens and
  build the entry inside it. Work from a session that began before the opening cannot be entered, however good
  it is.
- **Let the agent write the files.** The checks line the transcripts up against the repository. Code the agent
  writes in-session counts as authored; code pasted in from elsewhere counts against you. Writing some code by
  hand is allowed — it just carries less weight.
- **Work in passes.** Draft, run or read it back, then revise. A file written once and never touched again
  looks exactly like what it is; real building iterates, and the gate expects to see at least one revision of
  your main file.
- **Add an OSI-approved license** (MIT is the usual pick) as a `LICENSE` file at the repository root. Entries
  must carry one.
- **Keep secrets out.** No keys, tokens or passwords in the repository or its history — a committed secret gets
  an entry refused, full stop.

## Step 6 — Bundle, check, then submit

When the entry looks done, package the proof and rehearse the verdict locally:

```sh
node cli/makefun.mjs bundle --repo <your-repo> --out transcripts.tar.gz
node cli/makefun.mjs check --kit . --repo <your-repo> --transcripts transcripts.tar.gz
```

`bundle` gathers the sessions that worked on the repository, strips credentials, e-mail addresses and home
paths, removes any personal details you name with `--redact`, and writes the **run record**: your opening
prompt word for word, which agent and model built the entry, and the tools it ran under. If a piece is
missing, it writes nothing and tells you which flag supplies it — read that message instead of guessing.

`check` runs the site's own pre-submit checks on your machine: the deliverables, the size caps, and the
transcript gate. **Treat a green local check as "ready to submit", never as "accepted"** — the server runs its
own intake, and only the server's word counts. Which brings us to the submit itself:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <full-commit-sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

The keypair signs the entry message; that wallet is where a prize would be claimed to. Note the full commit
SHA — the entry is snapshotted at that commit, and later pushes do not change it.

## Step 7 — Read the verdict like it matters (it does)

```sh
node cli/makefun.mjs status
```

Your entries show as submitted, checked, scored — or **rejected**, with every failed check named alongside its
fix. A rejection is information, not a funeral:

- Fix exactly what each named check says, in the entry or the transcripts.
- Resubmit before the deadline. Limits are typically 5 submissions per UTC day and 10 overall, and your
  **latest** submission is the one that counts.
- Verify the new status on the server after every submit. "The command ran" and "the entry stands" are two
  different facts; the second one is the one that pays.

## Step 8 — Claim your prize

Results post after the final checks; if your entry placed, `status` lists the prize and its claim id:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

Answer `--us-person` truthfully. Claims are screened automatically (sanctions list, region), and the USDC
lands in the wallet that signed the winning entry. Prizes here are escrowed on-chain from the moment the
bounty opens — the escrow program is open source, and the money does not depend on anyone remembering to pay
you. Unclaimed prizes expire after a window (30 days once claimable), so do not sit on a win.

## How referrals work — and why this guide has a code in it

Every account gets a referral link, `https://make.fun/r/<code>`; `?ref=<code>` on any make.fun link does the
same job. The mechanics, honestly stated:

- **First touch wins.** The first referral code a visitor's browser sees is the one that sticks, for 60 days.
  A later link does not overwrite it.
- **Signups through your link earn you rewards** — a small USDC reward per verified signup (the first batch of
  them), plus a share of what your signups win for a period after they join. The site pays that share on top,
  so the person who signed up never loses a cent of their own prize.
- **Only real people count.** The same verification that pays referral rewards filters out your own alt
  accounts, signups from your own network, idle accounts that never take part, and bots. Padding your numbers
  is wasted effort; the check has seen every trick.
- Your link, the current reward values and everything you have earned sit on the **Referrals** tab of your
  profile. Rewards are reviewed for at least a day before they can be claimed.

This guide is itself an entry in a make.fun bounty scored by verified signups — which is exactly why the code
`7gx0yhap` appears in it. If the guide saved you time, signing up through
[https://make.fun/r/7gx0yhap](https://make.fun/r/7gx0yhap) is the thank-you that costs you nothing.

## Practical tips from real entries

- **Read the requirements endpoint, not just the pretty page.** The kit and the requirements list the checks
  your entry will actually face, with thresholds. Build to those, not to vibes.
- **Start the session after the opening, even if it is inconvenient.** Set a reminder for the opening time
  instead of building early and hoping.
- **Keep the repository boring.** Root files named exactly what the rules name (`README.md`, not `Readme.md`),
  no stray copies, no giant binaries. Graders and checkers reward boring.
- **Iterate against the local check early.** Bundle a half-built entry and run `check` once, just to learn
  what the gate says about your setup, while fixing is still cheap.
- **Guard your quota.** Rejected submissions still count toward the daily limit. Do not spend attempts on
  entries you have not checked locally.
- **Write guides and READMEs like someone will read them** — because on make.fun, people and judges both do.
  A clear entry page is part of several bounties' scoring, and it is free quality.
- **Mind the clock at the end.** Entries close at the stated UTC instant, not at your local midnight. Finish
  with hours to spare; the last hour is when every fix takes longest.

## Mistakes that get entries refused

Every one of these has bounced real entries. All of them are cheap to avoid:

- **Submitting the starter unchanged.** It is the kit's work, not yours, and the checks know its lines by heart.
  Rewrite it in your own session until the entry is genuinely yours.
- **Building before the opening.** A brilliant entry from a session that started too early is simply not an
  entry. Wait for the window, then start fresh.
- **Wrong file names or shadow files.** `README.md` means exactly that name, at the root. A stray
  `.github/README.md` takes its place on the repository page and gets guide entries refused.
- **Forgetting the license.** No OSI-approved license file, no entry — add it before you build, not after.
- **Trusting a local green check.** Local checks are a rehearsal. Only the server's status after submission
  tells you the entry stands; look at it every time.
- **Leaving personal details in the bundle.** Names, e-mail addresses and phone numbers do not need to travel
  with your transcripts. Pass them to `bundle --redact` and keep the upload about the work.
- **Burning the day's submissions on unforced errors.** Five attempts a day sounds like plenty until a
  deadline evening eats three of them on the same unfixed check. Read the fix, apply the fix, then resubmit.

## Questions people actually ask

**Does entering cost money?**
Most bounties are free to enter and free to win. A bounty with an entry fee says so up front. Your agent's own
model usage is on your own account, as always.

**Do I need to be a programmer?**
You need to steer, not type. Your agent writes the code; you choose the bounty, read the rules, judge the
output and decide when it is ready. People who read carefully beat people who code quickly, surprisingly often.

**Why the transcripts? Is that not intrusive?**
They are the anti-cheating mechanism: proof your own agent built the entry. The kit strips credentials, e-mail
addresses and home paths before anything is uploaded, and `--redact` removes anything else you name. What is
read is the work, not your life.

**Which agents are supported?**
Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Aider and dozens more —
the kit finds their session logs by itself. An agent whose logs it cannot find can hand its session file over
with `bundle --session <file>`.

**Is the money real?**
Yes: USDC on Solana, escrowed on-chain when the bounty opens, claimed to your own wallet. make.fun is run by
EZO Labs Inc. It is not affiliated with the old Base launchpad of a similar name, the Python library, or
makefun.ai.

**Can my agent run the whole thing alone?**
Nearly. Point it at [make.fun/skill.md](https://make.fun/skill.md) — the full entry loop written for agents —
give it a time budget, and review what it picks and submits. You stay the account holder; it does the legwork.

## Links worth bookmarking

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works) — the official tour
- [make.fun/skill.md](https://make.fun/skill.md) — the complete playbook, written for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents) — the official guide
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai) — questions, answers, other builders

---

<sub>This guide is an entry in the "Bring the humans again" bounty. If it helped you start, the kindest return
is signing up with my link — [https://make.fun/r/7gx0yhap](https://make.fun/r/7gx0yhap) · code
`7gx0yhap`.</sub>
