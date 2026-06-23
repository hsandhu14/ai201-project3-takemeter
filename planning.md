# TakeMeter: NBA Discourse Quality Classifier

## Project Overview

TakeMeter is a fine-tuned text classification system designed to evaluate discourse quality in NBA online communities. Using comments collected from NBA Reddit discussions, the model classifies posts into one of three categories:

- **analysis**
- **hot_take**
- **reaction**

The goal of the project is to determine whether a fine-tuned language model can distinguish between evidence-based basketball discussion, unsupported opinions, and emotional reactions.

---

## Community

I selected NBA Reddit communities, primarily r/nba, because they contain a large volume of discussion with varying levels of quality. Some comments provide detailed basketball analysis, while others are emotional reactions to games or bold opinions with little supporting evidence. These distinctions are meaningful to NBA fans and create an interesting classification problem.

---

## Label Taxonomy

### Analysis

A comment that supports its basketball opinion with reasoning, evidence, statistics, tactical observations, or historical comparisons.

**Examples:**
- "The Thunder's defense works because their perimeter defenders force teams into difficult late-clock possessions."
- "Jokic remains the league's best offensive player because his passing creates efficient shots for everyone on the floor."

### Hot Take

A bold or extreme basketball opinion expressed confidently but lacking substantial evidence or reasoning.

**Examples:**
- "The Celtics are complete frauds and won't make it past the second round."
- "Anthony Edwards is already better than every shooting guard in NBA history."

### Reaction

A short emotional response to a game, player, trade, injury, or event that expresses a feeling rather than making an argument.

**Examples:**
- "Refs sold this game."
- "This team is cooked."

---

## Dataset

### Data Sources

Comments were collected manually from public NBA discussion communities including:

- r/nba game threads
- r/nba post-game discussion threads
- r/nba serious discussion threads
- NBA trade discussion threads

### Dataset Size

**Total labeled examples: 229**

### Label Distribution

| Label | Count |
|---------|---------|
| Analysis | 95 |
| Hot Take | 54 |
| Reaction | 46 |

No label exceeded 70% of the dataset.

### Labeling Process

Each comment was manually reviewed and assigned exactly one label according to the taxonomy defined in planning.md. Comments were read individually rather than labeled in bulk to maintain consistency and reduce annotation noise.

### Difficult Annotation Examples

#### Example 1

**Comment:**  
"The Celtics are just a coin-flip team."

**Possible Labels:** analysis, hot_take

**Final Label:** hot_take

**Reason:** The comment makes a basketball evaluation but does not provide supporting evidence.

---

#### Example 2

**Comment:**  
"Some team will talk themselves into trading for Durant."

**Possible Labels:** hot_take, reaction

**Final Label:** reaction

**Reason:** The comment is more of a casual response to the discussion than a strong prediction or argument.

---

#### Example 3

**Comment:**  
"Wembanyama will probably become the best player in the NBA soon."

**Possible Labels:** hot_take, analysis

**Final Label:** hot_take

**Reason:** The statement is a bold prediction without supporting evidence.

---

## Fine-Tuning Pipeline

### Base Model

`distilbert-base-uncased`

### Training Platform

Google Colab (T4 GPU)

### Training Configuration

- Epochs: 3
- Learning Rate: 2e-5
- Batch Size: 16

I used the default hyperparameters because the dataset was relatively small and these settings are commonly recommended for DistilBERT classification tasks.

---

## Baseline Comparison

### Baseline Approach

The baseline used Groq's `llama-3.3-70b-versatile` model in a zero-shot setting.

The prompt included:

- Definitions for analysis, hot_take, and reaction
- Instructions to output only a single label

### Baseline Results

The baseline encountered output parsing issues within the notebook. While the model generated responses, the notebook failed to consistently map them back to the expected label format, resulting in zero parseable predictions.

Because of this issue, meaningful quantitative baseline metrics could not be generated.

---

## Evaluation Report

### Fine-Tuned Model Accuracy

**Accuracy:** 48.6%

### Per-Class Metrics

| Label | Precision | Recall | F1 Score |
|---------|---------|---------|---------|
| Analysis | 0.50 | 0.94 | 0.65 |
| Hot Take | 0.00 | 0.00 | 0.00 |
| Reaction | 0.33 | 0.12 | 0.18 |

### Confusion Matrix

| True \ Predicted | Analysis | Hot Take | Reaction |
|------------------|----------|----------|----------|
| Analysis | 16 | 0 | 1 |
| Hot Take | 9 | 0 | 1 |
| Reaction | 7 | 0 | 1 |

### Error Analysis

#### Example 1

**Text:**

> The Cavaliers have no chance of winning Game 7 if Mitchell keeps playing this way.

**True Label:** hot_take  
**Predicted Label:** analysis

**Analysis:**

The model interpreted the phrase "if Mitchell keeps playing this way" as basketball reasoning. However, the statement is ultimately an unsupported prediction and should be classified as a hot_take.

---

#### Example 2

**Text:**

> Wow.

**True Label:** reaction  
**Predicted Label:** analysis

**Analysis:**

This comment contains almost no contextual information. The model appears to struggle with extremely short comments and often defaults toward analysis when insufficient information is available.

---

#### Example 3

**Text:**

> The Luka versus Wembanyama discussion highlights the tension between proven production and future upside.

**True Label:** analysis  
**Predicted Label:** reaction

**Analysis:**

This comment contains abstract basketball reasoning without specific statistics or game references. The model appears to rely heavily on explicit evidence rather than broader conceptual analysis.

### Error Pattern Analysis

The confusion matrix reveals a clear pattern: the model heavily over-predicted the **analysis** label.

Nearly all hot_take and reaction examples were classified as analysis. This suggests that the model learned to associate basketball-related vocabulary with analysis regardless of whether evidence or reasoning was actually present.

The model almost never predicted hot_take, indicating that it struggled to learn the distinction between evidence-based arguments and unsupported basketball opinions.

To improve performance, I would collect additional hot_take examples and more short reaction examples. I would also strengthen the label definitions to further emphasize the difference between supported and unsupported claims.

---

## Sample Classifications

| Comment | Predicted Label | Confidence |
|----------|----------|----------|
| The Luka versus Wembanyama discussion highlights the tension between proven production and future upside. | reaction | 0.36 |
| People REALLY hate OKC huh? | analysis | 0.35 |
| We regret letting go of a generational talent in Kuminga. | analysis | 0.35 |
| We won something this year. | analysis | 0.35 |
| Triple double on one made field goal is insane. | analysis | 0.36 |

One prediction that is understandable is:

> "We regret letting go of a generational talent in Kuminga."

Although it was labeled as a hot_take, the comment evaluates a roster decision and references a specific player, which resembles analytical basketball discussion.

---

## Reflection

My intended goal was for the model to distinguish evidence-based basketball discussion from unsupported opinions and emotional reactions.

Instead, the model learned a simpler decision boundary. It frequently classified comments as analysis whenever they contained basketball-related reasoning language, even when they lacked evidence. It also struggled with very short comments that contained little context.

My definition of success was 75% accuracy and an F1 score of at least 0.70 for each class. The final model achieved 48.6% accuracy and did not meet those goals.

This suggests that the model captured surface-level basketball reasoning patterns rather than the deeper distinction between supported and unsupported arguments.

---

## AI Usage

### Example 1: Label Stress Testing

I used ChatGPT during the planning phase to generate edge-case examples that sat between analysis and hot_take. Several generated examples exposed weaknesses in my original definitions, which led me to refine the label boundaries before collecting data.

### Example 2: Error Pattern Analysis

After training the model, I used ChatGPT to analyze misclassified examples and identify common themes. The suggested pattern of over-predicting analysis matched my own manual review and was included in the final evaluation report.

### Annotation Disclosure

ChatGPT was used to help brainstorm examples and organize the dataset collection process. All final labels were reviewed and assigned manually before inclusion in the dataset.

---

## Spec Reflection

The project specification was most helpful in forcing careful label design before any model training occurred. Defining labels and edge cases early prevented major revisions later in the project.

One area where my implementation diverged from the original plan was the baseline evaluation. The project specification expected a quantitative comparison against a zero-shot baseline, but output parsing issues prevented reliable baseline metrics from being generated. The fine-tuned model evaluation was still completed successfully and provided useful insight into the classifier's strengths and weaknesses.

---

## Repository Structure

```text
ai201-project3-takemeter/
├── planning.md
├── README.md
├── data/
│   └── takemeter_dataset.csv
├── outputs/
│   ├── confusion_matrix.png
│   └── evaluation_results.json
```
