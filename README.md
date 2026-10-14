<!-- solve-problem:7c24f21b-9c04-4d1a-9527-7802fb84a825 -->
# Decentralized resale of execution rights

A [Tangle Solve problem](https://solve.tangle.tools/problems/7c24f21b-9c04-4d1a-9527-7802fb84a825).

## What is the problem?

In modern Ethereum (and similar protocols), the right to order transactions in a block — the "execution right" — is allocated via auctions, e.g.,PBS-style proposer-builder separation, where a block proposer sells the block to the highest-bidding builder, with an off-chain ordering auction (e.g., a first- or second-price auction) inside the builder's block. This problem asks: **can execution rights be made resellable in a decentralized secondary market without conferring a persistent advantage on incumbents?**

Formal setting: agents compete for a single (or per-slot) indivisible execution right in repeated auction rounds. After winning, the holder may resell it (possibly with a reserve price it sets). The design goal is a mechanism (primary auction + resale protocol) such that:

1. **Efficiency:** the right ends up with the agent with the highest valuation (which may include MEV extraction ability).
2. **No persistent incumbent advantage:** a winner in round $t$ cannot leverage that win (e.g., via accumulated MEV wealth, reserve-price setting power, or durable-monopoly dynamics) to win round $t+1$ at terms unavailable to entrants.
3. **Credible commitment:** the mechanism's rules are enforceable/trustless on-chain or via commit-reveal, so resale promises cannot be reneged on.

The precise open question is existence and characterization of such mechanisms — in particular, whether a resale right compatible with (2) exists at all. The statement is informal in the census; the most reasonable formalization is a repeated-game mechanism-design question where "centralization incentives" means equilibrium dynamics where value concentrates in one agent with probability → 1.

## Why does it matter?

If incumbents can parlay MEV winnings into durable dominance of block production, Ethereum's builder/relay market collapses into an effective monopoly, undermining censorship resistance and credible neutrality — the chain's core value propositions. A resale mechanism that preserves competitive access would let execution rights flow to the highest-value user each slot while preventing winner-take-all dynamics, directly informing the design of execution tickets, APS (ats-proposer separation) variants, and inclusion lists. Conversely, a clean impossibility result would tell protocol designers resale must be curtailed or paired with other tools.

## What's already known?

- **Pai and Resnick (2024)** modeled attester-proposer separation and showed centralization pressure persists: even when proposers are rotated/randomized among attesters, wealth accumulation by efficient producers leads to concentration (buying stake, better technology), so formal decentralization at the consensus layer doesn't prevent economic centralization. This prompted the present question: does adding a resale/secondary market for execution rights change the picture?
- **Related literature (context, not direct results):** classic durable-monopoly theory (Coase conjecture: a monopolist selling a durable good cannot sustain monopoly profits if it can commit to nothing — resale/self-competition erodes markups); endogenous secondary-market reserve pricing (a reseller posting reserve prices interacts dynamically with primary auctions). These suggest resale *can* discipline incumbents — but also that incumbents set reserve prices, which is itself an advantage lever.
- The census states a 2024 analysis found "resale alone does not eliminate centralization incentives" — i.e., a naive resale extension is insufficient; the open question is what additional mechanism structure is needed.

Treat the exact statements of the 2024 analysis as needing verification from the cited paper.

## What would count as solving it?

Any of:
1. **Positive construction:** an explicit mechanism (primary auction + on-chain resale with specified pricing/reserve rules) with a proof that in the repeated game (a) allocation is efficient in every equilibrium (or the unique equilibrium is efficient) and (b) the stationary distribution over winners has non-vanishing entropy / no winner's winning probability → 1, plus credibility (strategyproofness or credible-commitment guarantees à la commit-reveal).
2. **Impossibility:** a theorem that no credible, efficient mechanism with resale avoids persistent incumbent advantage under mild conditions (e.g., values correlated with wealth) — with a precise statement of which condition fails.
3. **Quantitative trade-off:** bounds showing exactly how much efficiency must be sacrificed to cap incumbency advantage, or how resale revenue/reserve-setting power degrades concentration.

## How to start

1. Read Pai and Resnick (2024), especially their centralization results and any discussion of execution rights/resale — identify their exact model so you can extend it minimally rather than rebuilding.
2. Formalize the repeated game: agents with types (MEV ability), budget dynamics where round-$t$ winnings increase round-$(t{+}1)$ bidding capacity (this budget feedback is the engine of centralization — model it explicitly).
3. Import Coase/durable-goods intuition: check whether the incumbent's inability to commit to future reserve prices helps entrants, and whether on-chain commitment *reverses* this by strengthening the incumbent — a key subtlety, since blockchain mechanisms typically *add* commitment power.
4. Main obstacle: separating "advantage from setting reserve prices in resale" from "advantage from wealth dynamics" — you likely need to shut one channel down to analyze the other.

## Key references

- Pai, M., & Resnick, P. (2024). *Centralization in Attester-Proposer Separation*. arXiv:2408.03116. Models proposer selection among attesters and shows centralization pressures persist despite protocol-level decentralization; the motivating work for this problem. https://arxiv.org/abs/2408.03116
- Coase, R. (1972). "Durability and Monopoly." *Journal of Law and Economics* 15(1). Classic result that a durable-good monopolist without commitment power loses monopoly rents — the core economic intuition for why resale might discipline incumbents. (verify relevance to your formalization)
- Any paper on "execution tickets" or APS proposals in Ethereum's roadmap (e.g., Ethereum research forum posts by the Ethereum Foundation) — practical context for how execution rights are actually proposed to be sold. (verify; no canonical citation)

## Warnings

- The census record is unusually thin; the phrase "2024 analysis showed resale alone does not eliminate centralization incentives" may refer to content *within* Pai–Resnick or to unpublished work — verify before relying on it.
- "Execution rights" could mean PBS block-production rights, intra-block ordering (MEV-Boost auction), or proposed execution-ticket systems; pin down which object before proving anything.
- Naive solutions to avoid: (a) randomizing winners ignores efficiency; (b) capping budgets requires identifying agents (sybils break it); (c) second-price resale doesn't fix reserve-setting power.
- "Credible commitment" on-chain may *help incumbents* relative to Coase-style commitment failure — don't assume more commitment is always better.

---
*`BCGT-000305` · flagged for review*

## Acceptance guidance

Any of:
1. **Positive construction:** an explicit mechanism (primary auction + on-chain resale with specified pricing/reserve rules) with a proof that in the repeated game (a) allocation is efficient in every equilibrium (or the unique equilibrium is efficient) and (b) the stationary distribution over winners has non-vanishing entropy / no winner's winning probability → 1, plus credibility (strategyproofness or credible-commitment guarantees à la commit-reveal).
2. **Impossibility:** a theorem that no credible, efficient mechanism with resale avoids persistent incumbent advantage under mild conditions (e.g., values correlated with wealth) — with a precise statement of which condition fails.
3. **Quantitative trade-off:** bounds showing exactly how much efficiency must be sacrificed to cap incumbency advantage, or how resale revenue/reserve-setting power degrades concentration.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/7c24f21b-9c04-4d1a-9527-7802fb84a825?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000305.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000305)
