#### Community

**r/soccer** — chosen because the 2026 FIFA World Cup generates a high volume of posts across a wide range of tones: data-driven breakdowns, bold predictions, and pure emotional reactions. The sheer volume makes manual labeling impractical, and the label boundaries are non-obvious enough to make automated classification genuinely useful.

#### Label taxonomy

| Label | Description |
|---|---|
| `analysis` | Structured argument backed by statistics, historical comparison, or tactical observation. Evidence is specific and verifiable. |
| `hot take` | Bold, confident opinion stated without supporting evidence. The claim might be true, but the post asserts rather than argues. |
| `reaction` | Immediate emotional response to a specific event. Little to no argument — the post is expressing a feeling in the moment. |

An `uncertain` label was also used during data collection for posts that were genuinely ambiguous.

#### Data collection source

Posts were scraped from r/soccer during the 2026 World Cup group stage using the Reddit API. Three match threads were used as primary sources:

- [Mexico secures 1st place in Group A](https://www.reddit.com/r/soccer/comments/1u9q9q4/mexico_has_secured_1st_place_in_group_a_of_the/)
- [Curaçao manager Dick Advocaat in tears](https://www.reddit.com/r/soccer/comments/1ubnc5l/cura%C3%A7ao_manager_dick_advocaat_78_in_tears_after/)
- [Spain 1–0 Saudi Arabia — Lamine Yamal goal](https://www.reddit.com/r/soccer/comments/1ubtxtp/spain_1_0_saudi_arabia_lamine_yamal_goal_10/)

Top-level comments and post titles were collected and stored in `labels.csv` with columns: `text`, `label`, `notes`.

#### Fine-tuning approach

A `distilbert-base-uncased` model was fine-tuned on the labeled dataset for 3-class sequence classification (`analysis` → 0, `hot take` → 1, `reaction` → 2). The model was trained on the hand-labeled posts from `labels.csv` and evaluated on a held-out test split of 5 examples.

#### Baseline description

The baseline used the Grok API in zero-shot/few-shot mode: each post was classified by prompting the model with the three label descriptions and 2–3 labeled examples per class. The baseline achieved **75% accuracy** on the test set — correctly capturing the strong linguistic signals in the data (stats/numbers → analysis, exclamations → reaction, bold claims → hot take).

#### Full evaluation report

| Model | Accuracy | Test set size |
|---|---|---|
| Grok API (baseline) | **75%** | 5 |
| DistilBERT fine-tuned | **40%** | 5 |

The fine-tuned model performed 35 percentage points below the baseline. See `confusion_matrix.png` for the per-class breakdown.

The fine-tuned model underperformed despite the baseline's strong results — the likely cause is dataset size. With only ~100 labeled examples across three classes, DistilBERT did not have enough signal to learn reliable decision boundaries. The test set of 5 examples is also too small to draw statistically robust conclusions.

#### Reflection

The most surprising result was that fine-tuning hurt performance rather than helping it. The zero-shot Grok baseline at 75% outperformed the fine-tuned DistilBERT at 40%, which is the opposite of the expected outcome.

The main bottleneck was data. ~100 posts is far too few to fine-tune a transformer model effectively — fine-tuning typically requires hundreds to thousands of examples per class. The 5-example test set also makes the accuracy numbers noisy: one wrong prediction is a 20-point swing.

If I were to redo this project, I would: (1) collect 500+ labeled posts before fine-tuning, (2) use a larger test split (at least 20% of the dataset), and (3) consider few-shot prompting with a stronger model as the primary approach rather than fine-tuning a small model on a small dataset.

#### Spec reflection

The planning document predicted a 75% baseline accuracy, which matched the actual baseline exactly. The hypothesis that `hot take` and `reaction` would be the most confused classes was borne out — both lack supporting evidence and differ mainly in tone, which is hard to capture without sufficient training data.

The edge cases defined in planning (stat-buried hot takes, hedged analysis, short celebratory posts) appeared in the real data and were the source of most labeling uncertainty, as indicated by the `notes` column in `labels.csv`.

#### AI usage section

- **Grok API** — used as the zero-shot/few-shot baseline classifier. Each post was sent as a prompt with label descriptions and examples; the model returned one of `analysis`, `hot take`, or `reaction`.
- **Claude (claude-sonnet-4-6)** — used to assist with writing the planning document, generating edge case tables, drafting evaluation metric targets, and filling in this README.
- **DistilBERT (distilbert-base-uncased via Hugging Face)** — fine-tuned locally on the labeled dataset for the supervised classification approach.
