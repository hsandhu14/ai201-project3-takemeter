# ai201-project3-takemeter

# Milestone 1: Community and Label Taxonomy

## Community Chosen

For TakeMeter, I chose NBA online discussion communities, especially public NBA subreddit-style comments and posts. NBA discourse is active, opinion-heavy, and full of different types of takes, ranging from thoughtful basketball analysis to emotional overreactions and low-effort jokes. These distinctions matter because basketball fans often debate whether a comment is actually adding insight or just reacting dramatically to a recent game, player, or controversy.

## Label Taxonomy

### Label 1: analysis

**Definition:** A post or comment that makes a clear basketball argument using reasoning, statistics, matchup details, historical comparison, or tactical observation.

**Clear examples:**

1. “OKC’s defense works so well because their guards pressure the ball early and force teams into late-clock decisions.”
2. “Jokic is still the best offensive player in the league because he creates efficient shots for everyone, not just himself.”

**Uncertain example:**
“LeBron is overrated because his playoff record against top-seeded teams is worse than people think.”

**Decision rule:** If the comment gives specific, verifiable evidence and uses that evidence to support a basketball argument, I will label it analysis. If it only uses one vague or cherry-picked point to make a bold claim, I will label it hot_take.

---

### Label 2: hot_take

**Definition:** A bold or extreme basketball opinion stated with high confidence but little supporting evidence.

**Clear examples:**

1. “The Celtics are frauds and are going to get exposed in the playoffs.”
2. “Anthony Edwards is already better than every shooting guard in NBA history.”

**Uncertain example:**
“Luka is amazing, but Dallas will never win a title with him dominating the ball this much.”

**Decision rule:** If the comment makes a strong claim but does not explain it with enough evidence or reasoning, I will label it hot_take. If it explains the claim with specific basketball logic, I will label it analysis.

---

### Label 3: reaction

**Definition:** A short emotional response to a player, team, call, trade, or game moment with little or no argument.

**Clear examples:**

1. “Refs sold this game.”
2. “This team is cooked.”

**Uncertain example:**
“Trade everyone, this roster has no heart.”

**Decision rule:** If the comment is mainly expressing frustration, excitement, or disappointment in the moment, I will label it reaction. If it makes a broader confident claim about a player or team, I will label it hot_take.

## Why These Labels Matter

These labels reflect a real difference in NBA fan discourse. Fans often separate serious basketball discussion from emotional game-thread reactions and exaggerated hot takes. A useful TakeMeter model should not just decide whether a comment is “good” or “bad,” but should identify what kind of basketball discourse the comment represents.
