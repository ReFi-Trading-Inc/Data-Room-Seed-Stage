# Founding Story

## The Problem That Started It All

In early 2022, Zeshan Ahmad and Daniel Oosthuyzen were independently experiencing the same frustration: they both knew *what* to do in the markets from a technical perspective, but consistently failed to execute due to emotional decision-making.

**Zeshan's Experience:**  
As a system architect and cryptography professor, Zeshan had built mass-scale consumer platforms and understood distributed systems deeply. He had been trading equities and crypto on the side for years, with a clear strategy and risk management rules. But he found himself breaking his own rules—holding losers too long, taking profits too early, revenge trading after losses. One particularly painful week at the onset of COVID, when he lost $120,000 because he panic-sold during a market dip, only to watch it recover within days. "I literally teach people how to build trustless systems," he recalls, "but I couldn't trust myself to follow my own trading rules."

**Daniel's Experience:**  
Daniel, a reinforcement learning researcher with a background in quantitative finance, had built ML systems at a fintech startup. He knew the math behind optimal trading strategies and had the coding skills to automate them. But like Zeshan, he struggled with execution. "I had backtested strategies with 2+ Sharpe ratios," Daniel explains, "but when real money was on the line, I'd second-guess the model. I'd see a losing streak and shut it down, or get greedy during a winning streak and over-leverage. Emotionally, I was sabotaging my own algorithms."

## The "Aha" Moment

The two met at a blockchain conference in Toronto in June 2022, where Daniel was presenting research on reward-centered reinforcement learning for sequential decision-making. During a coffee break, Zeshan asked a provocative question: "If RL agents can beat humans at Go and StarCraft, why are we still manually executing trades based on gut feelings?"

Daniel's response changed everything: "Because most trading bots are built on discounted reward models that assume finite time horizons. Markets are infinite-horizon environments. We're solving the wrong problem with the wrong math."

Over the next three hours, they sketched out the architecture for what would become ReFi.Trading:

1. **Average-reward RL agents** optimized for infinite-horizon market environments (not finite-episode models like most algo traders use)
2. **Non-custodial execution** via existing broker APIs (solving the trust problem that killed FTX and similar platforms)
3. **Transparent risk management** with cryptographic proof-of-risk (addressing the black-box critique of AI trading)
4. **Regulatory-first approach** (building compliance into the core architecture, not bolting it on later)

"We realized," Zeshan says, "that the combination of these four things didn't exist anywhere in the market. Everyone was either custodial (take my money, trust me) or non-compliant (use my script at your own legal risk) or using outdated RL frameworks. Nobody was building the *right* thing the *right* way."

## From Idea to Alpha (2022-2025)

**Phase 1: Validation (June-December 2022)**  
The co-founders spent six months validating the core technology:
- Built a prototype RL agent using RVI-Q (Relative Value Iteration Q-learning)
- Backtested on 3+ years of equity and crypto data
- Achieved 28.06% CAGR and 2.07 Sharpe ratio—significantly outperforming buy-and-hold benchmarks
- Validated that average-reward methods were indeed superior for market environments

"That first backtest result," Daniel recalls, "was the moment we knew this wasn't just an idea. The math worked. The question was: could we build a *product* around it?"

**Phase 2: Architecture (January-August 2023)**  
With technical validation in hand, they focused on product architecture:
- Designed the non-custodial broker integration via SnapTrade API
- Architected the risk management layer (VaR limits, progressive caps, kill switches)
- Built the paper trading environment for safe user testing
- Researched regulatory frameworks (UAE ADGM emerged as the ideal licensing jurisdiction)

**Phase 3: Incorporation & Seed Prep (September 2023-June 2024)**  
- Incorporated ReFi.Trading Inc. in Alberta, Canada (June 2024)
- Brought on a Product Lead with proven growth experience from D2C companies
- Built MVP web platform with React frontend, Python backend
- Integrated with SnapTrade for broker connectivity (IBKR, Questrade, Kraken, Coinbase)
- Created comprehensive investor data room and pitch deck

**Phase 4: Alpha Readiness (July-December 2024)**  
- Finalized paper trading MVP with risk controls
- Developed 90-day user acquisition plan targeting 150 alpha signups
- Prepared for UAE ADGM Category 3A license application
- Built go-to-market strategy focused on organic content and community

## The "Why Now?" Inflection Point

Three macro trends converged in 2024-2025 that made this the perfect time to launch:

**1. Post-FTX Trust Crisis (November 2022)**  
The collapse of FTX and similar custodial platforms created unprecedented demand for non-custodial solutions. Users now understand that "not your keys, not your coins" applies to trading automation too.

**2. AI Democratization (2023-2024)**  
GPUs, RL frameworks (Stable Baselines3, RLlib), and cloud compute became accessible to retail developers. What once required hedge fund infrastructure can now be built with $10K and open-source tools.

**3. Regulatory Clarity (2024-2025)**  
UAE ADGM published clear licensing pathways for non-custodial platforms. Canada updated securities guidance for AI systems. The "regulatory wild west" of 2020-2022 gave way to frameworks that favor compliant innovators.

"We spent 2022-2023 building in relative obscurity," Zeshan explains, "because the market wasn't ready. By late 2024, suddenly everyone was asking for exactly what we'd built: transparent, non-custodial, regulatory-compliant algo trading. The timing aligned perfectly."

## The Vision: Democratizing Quant Trading

The founding team believes we're at the beginning of a massive democratization wave in quantitative finance—similar to how Robinhood democratized commission-free trading, but far more profound.

**The Old World (2010-2020):**  
- Algo trading was exclusive to hedge funds with $10M+ budgets
- Required quant PhDs, proprietary data, co-located servers
- Retail traders were locked out or forced into black-box custodial bots

**The New World (2025+):**  
- RL frameworks are open-source and accessible
- Broker APIs allow non-custodial automation
- Cloud compute is cheap and scalable
- Regulatory frameworks exist for compliant platforms

"We're not trying to build another trading bot," Daniel emphasizes. "We're building the infrastructure layer for the next generation of retail quants. The platform that lets a software engineer in Lagos or a grad student in Mumbai compete with Goldman Sachs on equal footing—without needing $10M in capital or a PhD from MIT."

## Mission Statement

**We exist to give every retail trader the tools, transparency, and trust they need to automate trading like a hedge fund—without surrendering custody, compromising on performance, or operating in regulatory gray zones.**

We believe:
- **Trust is earned through transparency,** not marketing promises
- **Custody belongs to users,** not platforms
- **Automation should augment human judgment,** not replace it entirely
- **Regulation is a feature,** not a bug—it protects both users and the ecosystem
- **Financial tools should be accessible to all,** not just the wealthy and connected

## What Drives Us

**Zeshan:** "I want my 15-year-old daughter to be able to invest and trade with the same sophistication as a $1B hedge fund, when she's ready. Not in 20 years—now. That's the world we're building."

**Daniel:** "Every time I see someone lose money because they panic-sold or revenge-traded, I'm reminded why this matters. We're not just building software—we're giving people the tools to make rational decisions when their brains are screaming irrational things."

**The Team:** "We're here to solve the problem we *had* as traders, not the problem we imagined other people might have. That's why every feature, every design decision, every line of code comes from lived experience and pain. This isn't a Silicon Valley 'we're disrupting X' vanity project. This is us building the tool we desperately needed and couldn't find anywhere else."

## What's Next

As we enter 2025, ReFi.Trading is at the beginning of our journey:
- **Q4 2024:** Alpha launch, paper trading, 150 users
- **Q1 2025:** UAE ADGM license filing, compliance buildout
- **Q2 2025:** Live trading launch, first revenue
- **Q3 2025:** APAC expansion, marketplace beta
- **Q4 2025:** $3M ARR, Series A fundraising

The founding team is betting the next decade of their lives on this vision. We're here for the long haul—not to flip the company in 18 months, but to build the definitive platform for democratized quantitative trading.

"In 10 years," Zeshan predicts, "people will look back at 2024 as the year retail algo trading went mainstream—the way we look back at 2013 as the year commission-free trading became standard. And ReFi.Trading will be the infrastructure that made it possible."

**Join us.**

---

**Founded:** June 2024 (incorporated)  
**Founders:** Zeshan Ahmad (CEO), Daniel Oosthuyzen (CTO)  
**Headquarters:** Alberta, Canada  
**Licensing:** UAE ADGM (in progress)  
**Stage:** Seed fundraising ($2.45M round)
