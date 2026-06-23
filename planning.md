# TakeMeter — Project Planning

## 1. Project Overview

TakeMeter will classify the discourse quality of public comments from the r/nba community. The project focuses on how basketball opinions are expressed rather than whether the opinion itself is correct.

r/nba is a good community for this task because its discussions include several distinct forms of discourse: statistical analysis, tactical explanations, reasoned opinions, unsupported hot takes, and emotional game reactions.

The goal is to train a classifier that assigns each comment to exactly one of four labels:

1. `evidence_based_analysis`
2. `reasoned_opinion`
3. `unsupported_take`
4. `game_reaction`

---

## 2. Community

### Chosen community

The selected community is **r/nba**, a public Reddit community focused on NBA games, players, teams, trades, coaching, statistics, and league news.

### Why this community is appropriate

The community is text-heavy and contains a wide range of comment styles. A single discussion thread may include detailed statistical arguments, tactical observations, general opinions, exaggerated predictions, jokes, and immediate emotional reactions.

This variation makes r/nba appropriate for a classification task because the model must learn differences in how a claim is supported and presented. The task is not simply positive versus negative sentiment. It requires distinguishing between evidence, reasoning, assertion, and emotional reaction.

### Scope

The dataset will contain English-language public comments about NBA basketball. I will collect comments from several types of threads:

* Game and post-game threads
* Player comparison and award discussions
* Trade and roster discussions
* Coaching and tactical discussions
* General league news discussions
* Statistical analysis threads

I will not include usernames or unnecessary personal information in the dataset.

---

## 3. Label Taxonomy

### Label 1: `evidence_based_analysis`

**Definition:** A comment that supports a basketball claim using specific statistics, historical comparison, lineup information, film-based observation, or tactical reasoning.

The evidence must contribute meaningfully to the argument. Mentioning one number without explaining its relevance does not automatically make a comment analysis.

**Clear example 1:**

> The defense struggled because the center was repeatedly pulled away from the rim, which left no reliable help defender when the guards were beaten.

**Clear example 2:**

> His scoring average increased, but his efficiency declined because his true shooting percentage fell and his turnover rate increased after the All-Star break.

**Primary signals:**

* Specific statistics
* Tactical cause-and-effect reasoning
* Historical comparison with context
* Discussion of schemes, rotations, matchups, or lineups
* Evidence that could be checked or evaluated

---

### Label 2: `reasoned_opinion`

**Definition:** A comment that presents a clear basketball opinion and gives at least one meaningful reason, but does not provide detailed statistics, historical evidence, or tactical analysis.

The reasoning must explain why the commenter holds the opinion. A repeated claim, insult, or vague accusation does not count as meaningful support.

**Clear example 1:**

> I would start the younger guard because his energy and off-ball movement fit better next to the team’s primary scorer.

**Clear example 2:**

> They should keep the current coach because changing systems again would make it harder for the young players to develop.

**Primary signals:**

* A clear opinion
* At least one understandable reason
* General basketball logic
* No detailed or verifiable evidence required

---

### Label 3: `unsupported_take`

**Definition:** A confident judgment, prediction, comparison, or criticism that is stated without meaningful evidence or explanation.

The claim may eventually be correct, but the comment asserts a conclusion rather than supporting it.

**Clear example 1:**

> He is easily the most overrated player in the league.

**Clear example 2:**

> This team is definitely winning the championship next season.

**Primary signals:**

* Strong or absolute wording
* Player or team rankings without support
* Predictions without reasoning
* Insults presented as basketball conclusions
* Claims that simply restate the opinion

---

### Label 4: `game_reaction`

**Definition:** An immediate emotional response to a specific play, referee decision, game result, or live basketball moment, with little or no argument.

The main purpose of the comment is to express emotion rather than make a lasting basketball claim.

**Clear example 1:**

> WHAT A BLOCK!

**Clear example 2:**

> That fourth quarter nearly gave me a heart attack.

**Primary signals:**

* Excitement, frustration, disbelief, or celebration
* Short comments tied to a live event
* Capitalization or exclamation marks
* Little or no reasoning
* References such as “that play,” “this game,” or “what a shot”

---

## 4. Annotation Decision Rules

Each comment must receive exactly one label.

When a comment appears to match more than one category, I will use the following priority and decision rules.

### Rule 1: Reaction versus broader claim

If the primary purpose is to express immediate emotion about a specific game moment, label it `game_reaction`.

Example:

> That was embarrassing. This team has no heart.

If this appears directly after a specific play or game result and functions mainly as an emotional response, it is `game_reaction`. If it appears as a general claim about the team without immediate game context, it is `unsupported_take`.

### Rule 2: Evidence-based analysis versus reasoned opinion

A comment is `evidence_based_analysis` only when it contains specific evidence or explains a basketball mechanism.

Example:

> They defended badly because the weak-side defender kept leaving the corner too early.

This is `evidence_based_analysis` because it identifies a specific tactical problem.

Example:

> They need to defend with more effort.

This is `reasoned_opinion` only if it supports another recommendation. By itself, it may be too vague and could become an `unsupported_take`.

### Rule 3: Reasoned opinion versus unsupported take

A reason must add meaningful explanatory information.

Example:

> They should trade him because his playing style does not fit beside their ball-dominant guard.

This is `reasoned_opinion`.

Example:

> They should trade him because he is terrible.

This is `unsupported_take` because “he is terrible” only restates the negative judgment.

### Rule 4: Statistics do not automatically equal analysis

A comment containing one statistic is not automatically `evidence_based_analysis`. The statistic must be connected to the conclusion and used with enough context to form an argument.

---

## 5. Hard Edge Cases

### Edge case 1: Provocative claim with one statistic

Example:

> LeBron is overrated because his playoff record against top-seeded opponents is below .500.

Possible labels:

* `evidence_based_analysis`
* `unsupported_take`

**Decision:** Label it `unsupported_take` if the statistic appears mainly decorative or cherry-picked and the commenter does not explain why it fairly measures the claim. Label it `evidence_based_analysis` only if the comment explains the relevance of the statistic and addresses necessary context.

---

### Edge case 2: Emotional language with reasoning

Example:

> That rotation was awful because they left three non-shooters on the floor together.

Possible labels:

* `game_reaction`
* `evidence_based_analysis`

**Decision:** Label it `evidence_based_analysis`. Although “awful” is emotional language, the comment identifies a specific lineup problem and explains why the rotation failed.

---

### Edge case 3: Prediction with a general reason

Example:

> They are making the conference finals because their young players will improve.

Possible labels:

* `reasoned_opinion`
* `unsupported_take`

**Decision:** Label it `reasoned_opinion` if improvement is connected to a specific roster or development argument. If “the young players will improve” is the only vague justification, label it `unsupported_take`.

---

### Edge case 4: Short tactical observation

Example:

> They keep hunting him in the pick-and-roll.

Possible labels:

* `evidence_based_analysis`
* `game_reaction`

**Decision:** Label it `evidence_based_analysis` if it identifies a repeated tactical pattern. The comment does not need to be long to contain analysis.

---

### Annotation process for unresolved cases

When I encounter an unclear example, I will:

1. Compare it to the written label definitions.
2. Apply the annotation priority rules.
3. Record the example and my decision in an `annotation_notes` column.
4. Review similar examples together to maintain consistency.
5. Revise a decision rule if the same ambiguity appears repeatedly.

I will not create an “other” label. Comments that cannot reasonably fit the taxonomy, such as comments containing only usernames, links, bot messages, or context-free jokes, will be excluded before annotation rather than included as a catch-all class.

---

## 6. Data Collection Plan

### Data sources

I will collect public English-language comments from r/nba.

I will use a mixture of:

* Game threads
* Post-game threads
* Trade and free-agency discussions
* Player ranking and award discussions
* Coaching discussions
* Statistical posts
* Tactical or lineup discussions
* General NBA news threads

Using different thread types will reduce the risk that each label becomes associated with only one source type.

### Target dataset size

The minimum dataset size is 200 examples.

My initial target is:

| Label                     | Target count |
| ------------------------- | -----------: |
| `evidence_based_analysis` |           50 |
| `reasoned_opinion`        |           50 |
| `unsupported_take`        |           50 |
| `game_reaction`           |           50 |
| **Total**                 |      **200** |

I may collect approximately 220–240 candidate comments so that unclear, duplicated, extremely short, or unusable examples can be removed while still retaining at least 200 labeled examples.

### Collection strategy

I will first read and collect approximately 30–40 comments without labeling the full dataset. I will use this sample to verify that the taxonomy applies to real r/nba discourse.

After confirming the taxonomy, I will collect comments in batches. Each batch will intentionally include several thread types rather than taking all comments from one game or one discussion.

### Data fields

The dataset will include at least:

```text
text
label
source_type
source_url
annotation_notes
ai_prelabel
```

Possible meanings:

* `text`: cleaned comment text
* `label`: final human-reviewed label
* `source_type`: game thread, trade thread, analysis thread, and so on
* `source_url`: original public thread URL
* `annotation_notes`: explanation for difficult examples
* `ai_prelabel`: optional AI-suggested label before human review

Usernames will not be stored.

### Duplicate and leakage prevention

I will remove exact duplicates and obvious copied comments. I will also avoid collecting many near-identical comments about the same event.

The notebook will create a stratified 70/15/15 train-validation-test split. I will inspect the data before training to ensure that duplicated or nearly identical comments do not appear across different splits.

### Underrepresented labels

If one label is below 20% of the dataset after initial collection, I will not fill the remaining dataset with the majority labels.

Instead, I will conduct targeted collection:

* For `evidence_based_analysis`, collect from tactical, statistical, lineup, and long-form discussion threads.
* For `reasoned_opinion`, collect from roster, trade, coaching, and player-role discussions.
* For `unsupported_take`, collect from ranking, prediction, award, and comparison discussions.
* For `game_reaction`, collect from live and post-game threads.

If a label remains very difficult to find, I will reassess whether its definition reflects naturally occurring community discourse. I may revise or merge labels before finalizing the dataset, but I will document any taxonomy change.

---

## 7. Data Splitting Plan

The notebook will create a stratified split:

* 70% training
* 15% validation
* 15% testing

With 200 examples, this should produce approximately:

* 140 training examples
* 30 validation examples
* 30 test examples

Stratification will keep the four labels represented proportionally in each split.

The test set will remain locked after splitting. I will not revise labels or tune hyperparameters based directly on individual test predictions.

---

## 8. Model and Training Plan

The fine-tuned classifier will start from:

```text
distilbert-base-uncased
```

The initial training configuration will use:

* 3 epochs
* Learning rate: `2e-5`
* Training batch size: 16
* Evaluation batch size: 32
* Maximum sequence length: 256 tokens
* Weight decay: 0.01
* Stratified train-validation-test split

I will use the starter notebook’s default settings for the first run. If the validation performance shows clear underfitting or overfitting, I may adjust one hyperparameter at a time and document the change.

The fine-tuned model will be compared with a zero-shot baseline using Groq’s:

```text
llama-3.3-70b-versatile
```

Both models will be evaluated on the same locked test set.

---

## 9. Evaluation Metrics

### Overall accuracy

Accuracy measures the percentage of test examples classified correctly. It provides a simple overall summary but is not sufficient by itself because a model can achieve acceptable accuracy while performing poorly on one label.

### Macro F1 score

Macro F1 will be the primary evaluation metric.

It calculates F1 separately for each label and then gives every label equal weight. This is appropriate because all four discourse categories are important, and I do not want strong performance on an easy class such as `game_reaction` to hide weak performance on a harder class such as `reasoned_opinion`.

### Per-class precision

Precision shows how often predictions for a label are correct.

This is especially important for `evidence_based_analysis`. Low precision would mean the classifier frequently treats unsupported or shallow comments as analysis.

### Per-class recall

Recall shows how many actual examples of each class the model successfully identifies.

This matters for `unsupported_take` and `game_reaction` because low recall would mean the model misses many examples of those discourse styles.

### Confusion matrix

The confusion matrix will show which labels the model mixes up most often.

The most important anticipated confusion pairs are:

* `evidence_based_analysis` versus `reasoned_opinion`
* `reasoned_opinion` versus `unsupported_take`
* `game_reaction` versus `unsupported_take`

### Baseline comparison

I will calculate the same metrics for:

1. Fine-tuned DistilBERT
2. Zero-shot Groq baseline

This comparison will show whether task-specific fine-tuning improves performance over a general language model prompted only with the label definitions.

### Error analysis

I will examine at least three incorrect test predictions. For each one, I will document:

* Comment text
* True label
* Predicted label
* Model confidence, if available
* Why the example was difficult
* Whether the error came from ambiguous labeling, missing context, sarcasm, short text, or a weakness in the model

---

## 10. Definition of Success

### Minimum project success

The classifier will meet my minimum success criteria if:

* Fine-tuned test accuracy is at least 70%.
* Macro F1 is at least 0.68.
* No class has recall below 0.55.
* The fine-tuned model performs at least as well as, or within 0.03 of, the Groq baseline.
* The confusion matrix shows that the model learned distinctions beyond predicting the most common label.

This level would demonstrate that the pipeline works and that the taxonomy contains a learnable signal, even if the classifier is not yet ready for real deployment.

### Strong project success

I will consider the result strong if:

* Fine-tuned accuracy is at least 78%.
* Macro F1 is at least 0.75.
* Every class has F1 of at least 0.65.
* The fine-tuned model exceeds the Groq baseline by at least 0.05 accuracy or macro F1.
* The model does not show systematic collapse into one or two labels.

### Deployment-level success

For deployment in a real community tool, I would require:

* Accuracy of at least 82%.
* Macro F1 of at least 0.80.
* Recall of at least 0.70 for every label.
* Stable results across a larger and more recent test set.
* Human review of low-confidence predictions.
* Additional testing for sarcasm, slang, changing player context, and comments that depend on the surrounding thread.

A real deployment should not automatically remove or punish comments based solely on the classifier. It should be used for optional sorting, research, moderation assistance, or human review.

---

## 11. AI Tool Plan

### 11.1 Label stress-testing

**Tool:** Claude

**Input I will provide:**

* All four label definitions
* Annotation decision rules
* The four hard edge cases
* Two clear examples per label

**Task for the AI:**

Generate 5–10 realistic r/nba-style comments that sit near the boundary between:

* `evidence_based_analysis` and `reasoned_opinion`
* `reasoned_opinion` and `unsupported_take`
* `unsupported_take` and `game_reaction`

The AI should not provide labels immediately. I will classify the examples myself first.

**Verification process:**

If I cannot assign an example consistently using the written rules, I will revise the definition or add a more specific decision rule before collecting the full dataset. I will not use generated examples as part of the final real-world dataset.

---

### 11.2 Annotation assistance

**Tool:** Groq using `llama-3.3-70b-versatile`

**Decision:** I may use an LLM to produce preliminary labels, but every final label will be reviewed and approved manually.

**Process:**

1. Collect raw public comments.
2. Give the LLM the exact taxonomy and ask for one label only.
3. Store the proposed label in `ai_prelabel`.
4. Review each comment personally.
5. Store the final human decision in `label`.
6. Record difficult decisions in `annotation_notes`.

The LLM’s suggestion will not automatically become the ground-truth label.

**Disclosure:**

The README’s AI Usage section will report how many examples were AI-pre-labeled and that all final annotations were manually reviewed.

---

### 11.3 Failure analysis

**Tool:** Claude

**Input I will provide:**

* Wrong test predictions
* True labels
* Predicted labels
* Confidence scores
* Relevant confusion matrix counts
* Label definitions

**Task for the AI:**

Identify possible recurring error patterns, such as:

* Sarcasm being mistaken for a literal hot take
* Short tactical comments being mistaken for reactions
* Emotional wording causing analysis to be misclassified
* Player names or statistical vocabulary acting as shortcuts
* Lack of surrounding thread context
* One-stat claims being confused with full analysis

**Verification process:**

I will verify each proposed pattern directly against the test examples. I will only report a systematic pattern if it appears in multiple independent errors. I will not accept an AI-generated explanation that is unsupported by the actual predictions.

---

## 12. Anticipated Limitations

The labels depend partly on context. A short comment may be a reaction inside a live game thread but an unsupported take when shown alone.

Sarcasm, memes, community-specific references, and player nicknames may also be difficult for the classifier. DistilBERT may learn surface-level shortcuts, such as associating numbers with analysis or capitalization with reactions.

The dataset will also be relatively small. Results from approximately 200 examples should be treated as a learning experiment rather than proof that the model generalizes to all NBA discussions.

---

## 13. Planned Project Outputs

The repository will contain:

```text
planning.md
README.md
data/labeled_dataset.csv
outputs/evaluation_results.json
outputs/confusion_matrix.png
```

The Colab notebook will contain:

* Label map
* Dataset loading and validation
* Train-validation-test split
* DistilBERT fine-tuning
* Fine-tuned model evaluation
* Groq zero-shot baseline
* Model comparison
* Wrong-prediction analysis
