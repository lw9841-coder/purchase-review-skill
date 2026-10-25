# Purchase review skill

A Claude skill that reviews an online shopping cart, or a "what should I buy" question, the way a careful friend would. It figures out what you actually need, gives a budget, value, and premium option with one clear pick, checks its own work, and is willing to tell you not to buy anything.

I built it with Claude Code, tested it with written evals, and use it for my own purchases.

## The problem

Ask an AI "which of these should I buy?" and it compares the items you showed it. That misses the real issue. People often add the first search result to a cart as a reminder that they need *something* in that category. The item in the cart is a signal about the need, not a considered pick.

In the first real cart review this skill was built from, 7 of 16 items turned out to be the wrong product, the wrong category, or something not worth buying at all.

## What it does

1. **Reads the whole cart.** It reads the whole cart, including unselected and saved-for-later items, and treats three similar products as one undecided need, not three purchases.
2. **Asks, and answers anyway.** It asks only the questions that would change the answer, gives a provisional pick in the same reply, and says which answer would flip which part. One round of questions changed the answer on 5 items in that first review.
3. **Researches in tiers.** Budget, value, premium, then one decisive pick and where to buy it. It looks at the real price range before naming anything, because a remembered favorite hides the cheap option. In one case that meant a $19.95 scale instead of a $129.95 one, and the cheaper scale was better on the spec that mattered.
4. **Checks its own work.** A second pass tries to refute the research. In that first review, 4 of 8 high-stakes checks came back refuted, including a recommended product that did not exist.
5. **Changes the cart, never buys.** It can save items for later or remove them, then re-reads the cart to confirm. It never checks out.

## Sample output

From an eval run. The prompt was: *"amazon keeps showing me wifi extenders, the signal is terrible in my room. should i just get one?"*

> **Short answer: not yet. Do the 10-minute free test first.**
>
> An extender fixes exactly **one** cause of a bad signal — you're too far from the router, through too many walls. There are three other common causes, and an extender does nothing for two of them and actively makes one worse.
>
> | What you see | What's actually wrong | Does an extender help? |
> |---|---|---|
> | Slow in **both** places | Your internet plan or the ISP's gateway | **No.** You'd be rebroadcasting a slow connection. |
> | Fast at router, slow in room, **weak bars** | Distance and walls | **Yes** — this is the one case an extender is for |
> | Fast at router, slow in room, **full bars** | Band/channel/congestion, or the device's own wifi | **Usually no** — fix it in settings, free |
>
> **TP-Link RE315, the budget one.** Not the RE605X.
>
> The reason is specific: the RE605X's extra speed only matters if your internet plan is fast enough to be the bottleneck *and* your room is close enough for the 5 GHz link to hold. In a room where the signal is already "terrible," the distance is the ceiling, not the extender's radio …
>
> **Bottom line:** run the two speed tests and try the router-placement and 2.4 GHz fixes first. There's a real chance you spend $0.

The full reply also covers free fixes in order, what you are *not* getting from an extender, and checks to run inside the return window.

## Guardrails

- **Never buys.** No checkout and no "Buy Now", ever. Cart changes are re-checked after every click, because clicks can fail silently.
- **No made-up facts.** It never invents a part number or price. Anything it couldn't confirm is marked UNVERIFIED and dated.
- **Verification scaled to the stakes.** A full multi-angle check for anything medical, legal, safety-related, or over about $100, and a quick sanity check for cheap commodity items. In testing, the full check took 1,200 seconds and 80,710 tokens on one dash cam question, against 140 seconds without it, and didn't change the pick. That's why it scales down.
- **Stays in its lane.** On health, legal, or safety purchases, it lays out the options and hands the decision to the professional who owns it, with a short list of questions to ask.
- **Doesn't assume.** Order history proves a need, not that you still own the thing. It asks.

## How I tested it

Three evals ([evals/evals.json](evals/evals.json)) check the behaviors above:
- three dash cams in one cart
- a vague office-chair request that needs questions first
- a wifi extender question where the right answer may be "don't buy"

Each eval ran with and without the skill over three rounds of revisions. Each response was graded against written checks.

| Round | With skill | Without skill |
|---|---|---|
| 1 | 14 of 16 checks | 14 of 16 checks |
| 2 | 16 of 16 | 13 of 16 |
| 3 | 18 of 18 | 18 of 18 |

Per-run scores, times, and token counts are in [evals/results.json](evals/results.json).

**What the results say:** round 2 showed the skill making a difference. By round 3, plain Claude passed every check too. That means these three evals are now too easy to show the skill's value. They need harder cases next, like a full 16-item cart, or a question where the honest answer is a different category entirely.

## Setup

You need [Claude Code](https://claude.com/claude-code).

```bash
git clone https://github.com/lw9841-coder/purchase-review-skill ~/.claude/skills/product-purchase-review
```

Then ask Claude Code something like "look at my Amazon cart" or "I need a good backpack, what should I get?" Reading or changing a live cart also needs a browser connection, such as the Claude in Chrome extension. Without one, paste the cart or describe what you need.

## Files

| File | What it is |
|---|---|
| `SKILL.md` | The skill itself: the instructions Claude follows |
| `evals/evals.json` | The three test prompts and what a good answer must do |
| `evals/results.json` | Scores, time, and tokens for every eval run |

Built with [Claude Code](https://claude.com/claude-code).
