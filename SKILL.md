---
name: product-purchase-review
description: Analyzes the user's Amazon or Walmart cart and researches products, returning premium/value/budget tiers, a decisive pick, a buy-here-vs-elsewhere call, and an honest "don't buy this" verdict where warranted. Use this skill whenever the user mentions their cart, asks what to buy, asks you to research or compare products, says "is this worth it", wants options across price tiers, drops a product name and wants an opinion, or asks you to add/remove/clean up items in an online cart or shopping list. Also use it when they ask about gear for a specific need ("I need a backpack", "find me a good X") even if they never say the word "research". Always use this skill for purchase decisions — do not freehand product recommendations.
---

# Product & Purchase Review

## The premise this whole skill rests on

People often add the first search result to a cart as a **reminder that they need the category**, not as a considered pick.

> **Treat a cart item as a signal about the need, not a product to evaluate.**

So for every line, ask two questions: is this the right *product*, and is this the right *category*? In the August 2026 run, seven of sixteen items turned out to be the wrong product, the wrong category, or something not worth buying at all. A review that only compares alternatives within the category the user happened to land on will miss most of the value.

---

## Phase 1 — Read the cart, then stop

Read the **whole** cart, not just the selected items. Amazon shows the *selected* subtotal at the top of the page and the real one at the bottom; they differ, and it's an easy way to misreport what is actually being bought. Capture title, price, quantity, selected state, and the saved-for-later list.

Look for **duplicate intent** — several products solving one need is indecision, not several purchases. Three magnesium supplements meant "I can't decide," not "buy three."

---

## Phase 2 — Ask, and answer provisionally in the same turn

Clarifying questions are where the leverage is — in the August 2026 run, one round of them changed the answer on five items. But **questions alone make a dead-end turn.** If the user doesn't reply, they have nothing; if they do, they waited a round to learn anything.

So do both in one response:

1. **Ask the questions that would actually change the recommendation.**
2. **Give a provisional pick anyway**, clearly labelled as contingent — "based on what you've told me, X; here's what would change that."
3. **Say which answer flips which part**, so the user can see whether it's worth replying at all. Often one answer settles it and the rest don't matter.

This costs almost nothing and it respects their time. The user can act on the provisional pick immediately, or answer one question and get a better one.

The exception is when guessing would be actively misleading — where the plausible answers point at *different categories entirely*, not just different products. A "lockable container" that might be a truck box or a bedroom safe is not a case for a provisional pick; it's a case for one question. Say plainly that you're withholding a recommendation and why.

### Questions worth asking

Ask when different readings lead to materially different work:

- **What is this actually for?** A "storage ottoman" and a "truck tool box" turned out to be the same need — a lockable container. A "listening device" turned out to be assistive listening at events, not what it first sounded like.
- **Where will it live?** Temporary housing, someone else's home, and textured walls each eliminated entire product categories.
- **What do you already have?**
- **What's the spend posture right now?**

Use a multiple-choice question tool rather than prose when one is available; people answer those readily. Four questions maximum, real options, no filler.

### The ownership rule

**Never infer possession from purchase history.** Order history proves the need is real and recurring; it does not prove they still have the thing. People move, sell, lend, and give things away. Getting this wrong voided four recommendations at once in one real run. Ask.

---

## Phase 3 — Research

Per item, produce: **premium / value / budget**, a decisive pick, **where to buy**, and whether it's worth buying at all.

Keep "skip it" and "wrong category entirely" as live verdicts throughout. The most valuable output is often talking the user out of a purchase, or pointing at something free they already own — a venue's assistive listening receiver, the Find My already built into their laptop, pushing a curtain ring past its bracket.

### Survey the price range before naming a pick

Search the category and look at what it actually costs **before** committing to a recommendation — even when you think you already know the category. Name the tiers from what you just saw, not from memory.

This failed in practice: asked about auto-logging bathroom scales, I recommended a $129.95 Withings from earlier research, and only after being asked "is there a cheaper option?" found a $19.95 Etekcity that **synced to Apple Health for free and had twice the resolution.** $110 cheaper and better on the spec that mattered. The tiers were never built; a single remembered product was.

Two rules fall out of that:

- **When the question changes, re-search.** Prior research answers the question it was asked. Earlier work had concluded "don't buy a body-composition scale" — a different question from "what's the cheapest scale that auto-logs." Reusing tiers computed for one question to answer another is how the value tier goes missing.
- **If you name only one product, you have not done the job.** Premium/value/budget is not a formatting convention, it's the thing that surfaces the $19.95 option. A single pick pulled from memory looks like an answer and isn't.

### Source discipline

Use **manufacturer product pages** for specs and prices. Affiliate roundups dominate this space, are frequently stale, and are paid to recommend. Mark anything you cannot confirm as **UNVERIFIED** rather than asserting it, and never invent a SKU, part number, or price — that happened repeatedly and was caught only by the verification pass.

---

## Phase 4 — Verification, scaled to stakes

Run a pass whose job is to **refute** the research, not confirm it. In the August 2026 run, **four of eight** high-stakes checks came back refuted.

### Match the depth to what's at risk

A full multi-lens adversarial pass is expensive. Under test, applying one to a single dash cam question took **20 minutes and 80k tokens against 2.3 minutes** for no verification — and the findings, while real, didn't change the recommendation. That's the wrong trade. In the original cart review the same machinery ran across sixteen items at once, so the cost amortized. Applied to one commodity item it's disproportionate.

**Full adversarial pass — several independent lenses, each told to refute:**
- Anything medical, legal, or safety-adjacent
- Items over roughly $100
- Anything hard to reverse, or where being wrong is expensive
- Multi-item cart reviews, where the cost spreads across the whole list

**Single sanity check — does the product exist, is the price real, is the seller legitimate:**
- Commodity goods under roughly $50
- Categories where being wrong means mildly annoyed, not out real money

**No verification pass at all:**
- When the recommendation is to buy nothing
- When the answer is something free they already own

### What never scales down

Three disciplines cost nothing and apply at every price point:

- Never invent a SKU, part number, or price
- Mark anything unconfirmed as **UNVERIFIED** rather than asserting it
- Still give tiers and a decisive pick

Scaling verification down is about skipping *redundant refutation agents*, not about lowering the bar on honesty or skipping the cheap structural work. Under test, the run that spent twenty minutes verifying also **forgot to give tiers and committed to a pick before asking what the product was for** — it did the expensive part and skipped the free parts. Don't trade one for the other.

What it caught that would otherwise have reached the user:

- A recommended product that **does not exist**
- A "best value" pick whose capacity was **too small for the job it was picked for**
- A **false legal claim** stated as fact
- Prices stamped "verified" that nobody could reproduce
- A shopping list that **specified regulated hardware** after explicitly promising to leave that choice to a professional

Give each verifier a **distinct lens** — price and availability, specs and physical fit, safety and scope, legal accuracy. Diverse lenses find more than redundant ones.

**Tell the user what got refuted.** Don't quietly patch and present a clean answer. The pattern of what failed tells the user how much to trust the rest.

---

## Recurring traps

| Trap | What to do |
|---|---|
| Interior vs exterior dimensions | Marketing quotes exterior. Demand interior. Advertised capacity runs ~13% inflated. |
| "Fits up to 16 inches" | Often boilerplate reused across an entire catalog — not a measurement. Find owner evidence, or buy on free returns and test it. |
| Specs unpublished below a price point | If nothing under $60 in a category publishes the number that decides whether it works, *that is the finding.* |
| Marketplace vs first-party | Walmart is cheapest on house brands (Sterilite, Mainstays) and marks up marketplace goods 2–3×. Amazon is often the reverse. Check both per item. |
| Warranty voiding | Some brands (Aer) void coverage bought through unauthorized sellers. Others (Darn Tough, Osprey) honor it anywhere, no receipt. Check before assuming direct is better. |
| Stock freshness | Some products degrade on a shelf. Buy direct for a fresh calibration or manufacture date. |
| Gated listings | "Available with a business account and healthcare license" means a consumer cannot buy it, however good the price. |
| Subscription paywalls | Check what sits behind a paid tier before recommending an app-connected device. |
| Listing self-contradiction | Titles and bullet points disagree more often than you'd think. Trust the manufacturer's own page. |

---

## Phase 5 — Executing cart changes

**Never purchase. Never check out. Never click "Buy Now."** On Amazon product pages those buttons sit adjacent — verify the target before clicking, and never click blind from a screenshot that may be stale. If you can't confirm the button, stop and hand it to the user.

- Prefer **Save for Later** over Delete when the user might reconsider.
- **Verify after acting.** Re-read the cart and report the real item count and subtotal. Clicks fail silently — one add-to-cart missed entirely and was only caught because the arithmetic didn't work.
- Flag anything you deliberately left alone, and say why.
- Watch selection state. Unselected items don't get purchased, and that state persists invisibly across sessions.

---

## Phase 6 — Capture durably as you go

Write findings into an artifact or file **during** the work, not at the end. Long research sessions hit context limits mid-task, and unrecorded findings are lost.

When a limit is approaching: say so early, write state down, then hand off remaining work with a **self-contained prompt that includes why each product is the right one**. Without the reasoning, the next session substitutes something wrong.

---

## Scope discipline

Where research touches health, legal, or safety questions, describe **options and tradeoffs** and route the decision to whoever owns it — prescriber, pharmacist, attorney.

The failure mode is subtle and worth guarding against specifically: the August run opened with an explicit refusal to specify medical hardware, then produced a shopping list that specified it anyway, complete with prices and links. **A disclaimer three sections above a recommendation does not survive a reader who skims to the list.** If you're deferring a decision, the buy list has to reflect that too.

Deliver instead a short list of concrete questions to bring to the professional. That's more useful than a guess, and it's the correct division of labor.

If a request falls outside what you'll help with, say so in a sentence, offer the nearest legitimate thing, and move on. Don't moralize.

---

## What good output looks like

- A decisive pick, not a menu of options
- Real prices with a date attached, and honest "approx" where unverified
- The reason each product is *that specific* product, so it can't be substituted wrong later
- What the user is **not** getting, stated plainly (no fire rating, no burglary rating, no warranty)
- Physical checks to run inside the return window
- The free or already-owned alternative, when one exists — it often does

Fewer, better-chosen items beat a long list, and "you don't need this" is frequently the most valuable sentence in the response.
