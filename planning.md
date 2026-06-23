# TakeMeter Planning Document

## Community

For this project, I chose NBA online discussion communities, particularly NBA-related Reddit comments and discussion threads. NBA communities are a good fit for a classification task because they contain a wide range of discourse styles, including detailed basketball analysis, emotional reactions to games, and exaggerated hot takes. This variety creates meaningful distinctions that are recognizable to regular community members and suitable for supervised classification.

---

## Labels

### analysis

**Definition:** A comment that supports its basketball opinion with reasoning, evidence, statistics, tactical observations, or historical comparisons.

**Examples:**

* "The Thunder's defense works because their perimeter defenders force teams into difficult late-clock possessions."
* "Jokic remains the league's best offensive player because his passing creates efficient shots for everyone on the floor."

### hot_take

**Definition:** A bold or extreme basketball opinion expressed with confidence but lacking substantial evidence or reasoning.

**Examples:**

* "The Celtics are complete frauds and won't make it past the second round."
* "Anthony Edwards is already better than every shooting guard in NBA history."

### reaction

**Definition:** A short emotional response to a game, player, trade, injury, or event that expresses a feeling rather than making an argument.

**Examples:**

* "Refs sold this game."
* "This team is cooked."

---

## Hard Edge Cases

Some comments fall between analysis and hot_take.

Example:

> "LeBron is overrated because his playoff record against top-seeded teams is below .500."

This comment contains evidence but also uses a highly provocative claim. My decision rule is:

* If evidence is used as part of a broader basketball argument, label as **analysis**.
* If evidence is minimal, cherry-picked, or primarily used to support a provocative statement, label as **hot_take**.

Another difficult category occurs between hot_take and reaction.

Example:

> "Trade everyone. This roster has no heart."

If the comment primarily expresses frustration, it will be labeled **reaction**. If it makes a broader prediction or evaluation of a team without support, it will be labeled **hot_take**.

Difficult Annotation Examples:
Example: "The Celtics are just a coin-flip team."
Possible labels: hot_take or analysis
Decision: hot_take
Reason: The comment is about basketball strategy, but it does not provide enough evidence or reasoning. It makes a broad claim in a provocative way.
Example: "Some team will talk themselves into trading for Durant."
Possible labels: hot_take or reaction
Decision: reaction
Reason: The comment is more of a casual response to the discussion than a strong prediction or detailed argument.
Example: "Wembanyama will probably become the best player in the NBA soon."
Possible labels: hot_take or analysis
Decision: hot_take
Reason: The comment makes a bold future prediction without giving specific evidence or reasoning.
Example: "The Luka trade became especially damaging because Dallas failed to maximize the return through an open bidding process."
Possible labels: analysis or hot_take
Decision: analysis
Reason: Even though it criticizes the trade, it explains a specific reason: Dallas did not create a bidding war or maximize trade value.

---

## Data Collection Plan

I will collect comments from NBA discussion communities such as r/nba and team-specific NBA discussion threads. I will gather approximately 200 comments.

Target distribution:

| Label    | Target Count |
| -------- | -----------: |
| analysis |           70 |
| hot_take |           65 |
| reaction |           65 |

The dataset will be split into:

* Training: 160 examples
* Validation: 20 examples
* Test: 20 examples

If one label is underrepresented, I will deliberately search for discussion threads where that type of comment is more common. For example, post-game threads often contain more reactions, while discussion posts contain more analysis.

---

## Evaluation Metrics

Accuracy will be reported because it provides a simple measure of overall performance.

However, accuracy alone is not sufficient because a model could perform well by over-predicting the most common label.

I will also report:

* Precision for each class
* Recall for each class
* F1 score for each class
* Confusion matrix

These metrics are important because they reveal which labels are being confused with each other. Since analysis and hot_take can be similar, the confusion matrix will help identify whether the model has learned the intended distinction.

---

## Definition of Success

I would consider the classifier successful if:

* Overall test accuracy is at least 75%.
* F1 score for every class is at least 0.70.
* The fine-tuned DistilBERT model outperforms the zero-shot Groq baseline by at least 5 percentage points in overall accuracy.

For a real community moderation or discussion-quality tool, I would consider the model good enough for deployment if it achieves at least 80% accuracy and maintains balanced performance across all three labels.

---

## AI Tool Plan

### Label Stress-Testing

Before collecting the full dataset, I will use ChatGPT to generate 5–10 comments that intentionally sit between two labels. If I struggle to classify those comments consistently, I will revise my label definitions and decision rules before annotation begins.

### Annotation Assistance

I will not use an LLM to automatically label my dataset. All final labels will be assigned manually to ensure consistency with my taxonomy and to avoid introducing model-generated bias into the training data.

### Failure Analysis

After evaluation, I will provide examples of incorrect predictions to ChatGPT and ask it to identify possible error patterns. Potential patterns include confusion between analysis and hot_take, difficulty with sarcasm, or misclassification of very short comments. Any patterns suggested by the AI will be verified manually using actual prediction results before being included in the final report.

