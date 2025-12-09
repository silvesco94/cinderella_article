# cinderella_article
I am **not** a sports bettor.
I will **never** be a sports bettor.
And I will absolutely, unequivocally, without hesitation, **never** tie myself to sports betting in any way.
Let’s be crystal clear: **nothing** in this article is meant to encourage anyone to place a wager, consult a bracket whisperer, commune with the basketball spirits, or otherwise tempt fate with their wallet.
*However…*
If you were to bet:
**What actually makes a Cinderella team a good bet?
And is it the same stats that make a blue blood team a good bet?**
This article isn’t here to help you bet — but it is here to explore why some underdogs capture our hearts, why some powerhouses feel inevitable, and what the numbers say about the beautiful chaos in between.
## How do we define Cinderella? 
I defined a Cinderella with these parameters: 
•	- **Double-digit seed (10–16)**
•	- **Reached at least the Sweet 16**
•	- **Beat expectations based on seed ** 
Most of these are not controversial. Double digit seeds makes sense, if you are in the top half of the seeds, then you probably are favorites to win at least game one. Beat expectations based on your seed, for example a 15 seed beating a 2 seed, duh. The only controversy I can see is the classification of having to get at least to the Sweet 16. This will not include the random 15 seed winning the first game (which admittedly is great). In my opinion though, a true Cinderella puts a scare into going really deep into the tournament and not one random win. This standard will also help come analysis, because it will eliminate some of the randomness. 

To get a clearer picture, I built a simple **Cinderella Index** using:

- **success probability** (from my model)  
- **upset score** (how far they beat seed expectations)  
- **seed difficulty** (how tough their draw was)

So lets rank the top Cinderella teams based on this index: 

![Top 10 Cinderella Index](attachment:top10_cinderella_index.png)
To help interpret this table, a few columns are worth outlining briefly. **Success probability** comes from the model and represents how likely a team’s statistical profile suggested a deep run *before* the games were played. 

**Upset score** measures how many rounds a team exceeded what their seed would normally predict—so a 15-seed reaching the Elite 8 earns a much higher score than an 11-seed making the Sweet 16.

**Seed difficulty** simply scales how tough a team’s starting position was, with higher seeds (like 15 or 16) receiving larger values. The final **Cinderella Index** blends these three ingredients into one number, highlighting the teams that were simultaneously strong, under-seeded, and able to significantly outperform expectations.
Without digging into the model’s features yet, a few patterns already stand out:

### **Saint Peter’s (2022)**  
A 15-seed with an upset score of **3** and one of the toughest seed paths possible.  
They didn’t have the highest predicted probability, but the *difficulty of their run* pushes them to the top.

### **Iowa State (2022)**  
They appear because of an unusually high **success probability (0.78)** for an 11-seed.  
This suggests they were dramatically under-seeded compared to their true strength.

### **Oregon (2019 & 2013)**  
Two different seasons show up — both with strong probabilities and meaningful upset scores.  
Some programs simply outperform their seed more often than others.

### **La Salle (2013) & FGCU (2013)**  
Both teams had memorable runs, and the index reflects their combination of upset wins and challenging seed lines.

### **What this tells us (without spoiling the next section):**  
Even before looking at deeper features like offense, defense, pace, or efficiency, the **historical Cinderellas that mattered** tended to share three things:

1. They weren’t as weak as their seed suggested.  
2. They outperformed expectations more than once.  
3. Their seed line guaranteed a difficult path — meaning their run required *real* overperformance.
We’ll come back to these patterns shortly, once we dig into the model itself and explore *which* team features tend to signal a Cinderella run before the tournament even starts.

## Building the Cinderella Model  

**Can we predict cinderellas before the tournament even starts?**

Therefore I created a simple logistical regression model to predict and see what makes these Cinderella teams great. 

### What the model is trying to predict
The model’s target is simple:

**Did a double-digit seed make a true Cinderella run?**  
(meaning: Sweet 16 or deeper *and* an upset-score above our threshold)

So the model is trained only on these eligible teams, learning the patterns that separated:

- **Actual Cinderellas** → the Saint Peter’s, FGCU, La Salle types  
- **Pretenders** → teams that got the seed but fizzled immediately  

### What data we fed into the model  
To keep things grounded and interpretable, I used a logistic regression model trained on core team statistics:

- Efficiency metrics (AdjOE, AdjDE, BARTHAG)  
- Shooting stats (eFG%, 2P, 3P)  
- Turnover rates (TOR, TORD)  
- Rebounding (ORB, DRB)  
- Pace (AdjT)  
- Resume-style metrics (WAB, Wins, Games Played)  
- Seed information  
- Year (to control for era differences)

These are the same stats predictive analysts have relied on for years — we’re just reframing them through the lens of *underdogs*.

---
## How well does the model work?

Before looking at which features matter most, it’s worth checking whether the model performs reasonably on past tournaments.

👉 **[Figure: Cinderella model metrics table goes here]**

Because true Cinderella runs are rare, overall accuracy is less important than whether the model can **identify the kinds of teams that historically surprise us**. Strong recall and precision for the Cinderella class show that the model isn’t just guessing — it’s finding repeatable statistical patterns.

👉 **[Figure: Confusion matrix heatmap goes here]**

The confusion matrix answers two practical questions:

- Does the model **miss** too many teams that actually became Cinderellas?  
- Does it **overpredict** Cinderellas among teams that fizzled?

A balanced result tells us we are modeling a real signature — the “Cinderella profile” — rather than just noise around it.

---

# ⭐ **What makes a good Cinderella? (According to the model)**  

We begin with the **Top 10 Cinderella Index chart**, which blends strength, upset performance, and path difficulty.

👉 **[Figure: Top 10 Cinderella Index chart goes here]**

At first glance, Saint Peter’s (2022) may look lower than teams like Oregon or La Salle. But the Index blends *several* components. Oregon and La Salle rate higher because they entered the tournament with elite efficiency metrics. Saint Peter’s did not look like an Oregon-level juggernaut — but **relative to other 15-seeds**, they were something close to unprecedented. They are the top Cinderella team, not because their stats were emensly better than other teams that were cinderellas, but in comparison to people in their own seed. Remember Oregon and the other teams were higher seeds to start with, 15 was certainly an underseeded team, and this next section will prove that. 

To understand this deeper, we zoom in to compare Saint Peter's in comparison to all other 15 seeds in the dataset. 

👉 **[Figure: Saint Peter’s vs. all 15-seeds percentile comparison chart here]**

This chart makes it clear why the model viewed Saint Peter’s as extraordinary **within their seed line**, even if they weren’t as strong as the top Cinderella teams overall.

---

## **1. SEED — Yes, being “worse” actually helps**

Seed appears high in the feature-importance chart because lower seeds have **more runway to outperform expectations**.

Historically, most true Cinderellas sit between **11 and 13**.  
Saint Peter’s, as a **15-seed**, sits at the extreme end of this curve — maximum opportunity for upside.

But what really stands out is this:

- Compared to all historical 15-seeds, Saint Peter’s ranks in the **84th percentile** in strength (BARTHAG).  
- Nationally, their BARTHAG (0.6786) places them around the **71st percentile**.  
- Typical 15-seeds live between the **5th and 40th percentiles** nationally.

So while their seed gives them upset *potential*, their underlying strength made them *capable* of cashing it in.

---

## **2. BARTHAG & efficiency metrics — the backbone of every Cinderella**

BARTHAG — KenPom’s neutral-court win probability metric — is one of the strongest features in the model because it captures how good a team truly is.

Here’s how some of the best Cinderella teams stack up:

| Team              | Seed | BARTHAG | Percentile (National) |
|-------------------|------|---------|------------------------|
| Saint Peter’s 2022 | 15   | **0.6786** | **71st percentile** |
| Oregon 2019       | 12   | **0.8687** | **91st percentile** |
| Oregon 2013       | 12   | **0.8728** | **90th percentile** |
| La Salle 2013     | 13   | **0.8516** | **88th percentile** |

Clearly, the Oregon runs came from teams that were playing like **top-40 teams**.  
La Salle 2013 was a **top-12% team nationally**.

Saint Peter’s wasn’t as strong as Oregon — but that’s only half the story.

To understand why their BARTHAG still matters, we compare them not to Oregon, but to their true peers:

### **How Saint Peter’s compares to other top-performing 15-seeds**

Among all 15-seeds in the dataset:

- The **best-ever** BARTHAG values for 15-seeds fall in the **65th–75th percentile** nationally.  
- Saint Peter’s (71st percentile) sits right in that historic top tier.  
- Their BARTHAG is **higher than most 15-seeds that *didn’t* win a game**, and similar to (or better than) several 15-seeds that *did* pull off an upset.

Put differently:

> **Saint Peter’s had one of the strongest efficiency profiles of any 15-seed in modern March Madness.**

This matters much more than comparing them to Oregon.  
A 15-seed in the **top third** of the country is a neon sign that something unusual is lurking.

This is exactly why the model boosts them:  
they were not strong in an absolute sense — they were **historic relative to their seed**.

---

## **3. The “true seed strength” effect — finding the real sleepers**

Cinderella magic happens when **a team is much stronger than its seed suggests**.  
The model captures this via the interaction between seed and efficiency.

This effect identifies teams whose résumé and seed line create a statistical mismatch:

- Oregon 2019 → **2nd-highest** BARTHAG among all double-digit seeds  
- Oregon 2013 → **5th** among the same group  
- Saint Peter’s → **18th** among 32+ double-digit Cinderellas, but…

…when comparing **only 15-seeds**, Saint Peter’s jumps to the **top 16%** in strength.

So although they weren’t an Oregon-level team, they were **Oregon-like relative to their seed**.

That’s the entire Cinderella formula:

> **Strong team + low seed = structural chaos.**

Saint Peter’s didn’t just break expectations.  
They broke the *mathematical logic* of what a 15-seed is supposed to be.

---

## **4. Defense & disruption — the traits that actually travel in March**

Efficiency gets a team in the conversation; defense often carries them deeper.

Saint Peter’s shines brightest here — especially compared to other 15-seeds:

| Metric              | Saint Peter’s Value | Percentile vs 15-Seeds | Percentile Nationally |
|---------------------|----------------------|--------------------------|------------------------|
| 2P% Defense          | 44.2%               | **100th percentile**      | **98th percentile**     |
| eFG% Defense         | —                   | **100th percentile**      | mid-90s                |
| Turnover Creation    | 20.5%               | **84th percentile**       | **82nd percentile**     |

This is not normal.  
Not for a mid-major.  
Definitely not for a 15-seed.

Oregon 2019 and Oregon 2013 had similarly strong defensive traits, though not at the same seed-relative extremes. La Salle used ball pressure and shot-making to compensate for weak paint defense.

The feature-importance chart confirms:

> **Efficiency builds the platform.  
Defense and disruption ignite the upset.**

---

# ⭐ Putting it all together

When you combine both charts — the **Cinderella Index** and the **Saint Peter’s vs 15-seeds comparison** — a clear, data-driven picture emerges:

- Saint Peter’s 2022 wasn’t just a fun narrative.  
- They were **one of the strongest 15-seeds ever measured**, with top-tier defensive numbers and a BARTHAG in the historic high range for that seed.  
- Oregon and La Salle look stronger in absolute efficiency, but Saint Peter’s looks far stranger — and more dangerous — relative to their seed line.

This explains why:

- They appear high in the Index,  
- They consistently broke elite teams’ offensive systems, and  
- The model flagged them despite a modest absolute BARTHAG.

And now the stage is set for the next question:

If *this* is what a great Cinderella looks like,  
**what does a great blue blood look like — and how different is their statistical DNA?**








