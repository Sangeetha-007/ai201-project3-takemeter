#### Community:

The community I have chosen is: r/soccer. The reason I chose this is because FIFA World Cup is happening so there will be a lot of posts. 
The reason this labeling is useful is because with so many games happening, we can't tell if certain comments are factual, or just reactions/hot-takes. 

#### Sources:

https://www.reddit.com/r/soccer/comments/1u9q9q4/mexico_has_secured_1st_place_in_group_a_of_the/
https://www.reddit.com/r/soccer/comments/1ubnc5l/cura%C3%A7ao_manager_dick_advocaat_78_in_tears_after/
https://www.reddit.com/r/soccer/comments/1ubtxtp/spain_1_0_saudi_arabia_lamine_yamal_goal_10/
#### Labels:

LABEL_DESCRIPTIONS = {
    "analysis": "the post makes a structured argument backed by statistics, historical comparison, or tactical observation. Evidence is specific and verifiable.",
    "hot_take": "a bold, confident opinion stated without supporting evidence. The claim might be true, but the post asserts rather than argues.",
    "reaction": "an immediate emotional response to a specific event. Little to no argument — the post is expressing a feeling in the moment."
}

Examples:
"The last WC game England played in the Azteca was vs Argentina in 86!" : analysis
"Curaçao manager Dick Advocaat (78) in tears after helping Curaçao, the smallest nation to ever make the World Cup, earn a point against Ecuador. 
He is the oldest manager in World Cup history by four years. This is Advocaat’s 27th managerial stint, not counting repeats" : analysis
"The hand of Bellingham in bound to send Mexico into absolute Chaos." : hot_take
"Oyarzabal is incredibly underrated" : hot_take
"To be fair Korea in LA as a second place team will still be huge for them" : reaction
"I'd really like us to go for him as a backup plan for a bigger name striker. I don't really get why we've never been linked to him." : reaction
"His poor performance in game 1 didnt help his cause" : uncertain

#### Edge cases

| Situation | Label |
|---|---|
| Post cites a stat but buries it in emotional language ("Messi literally carried the whole team, 8 key passes btw") | **hot_take** (stat is incidental; the claim is asserted, not argued) |
| Short celebratory post that quotes a specific match minute or score ("87' GOAL!!!") | **reaction** (emotional, event-triggered, no argument) |
| Post that starts with an opinion but then provides historical comparisons or data to back it up | **analysis** (structure and evidence determine the label, not the opening tone) |
| Post uses uncertain hedging ("I think", "maybe", "not sure but...") followed by a reasonable argument | **analysis** if reasoning is present; **hot_take** if no supporting evidence |
| Highlight or news-dump post with no commentary (just a link or game score) | **reaction** (informational but no argument — closest to an emotional share) |
| Post that mixes analysis and a strong emotional conclusion | **analysis** if the body is evidence-driven; label by the dominant mode |

#### Data collection plan:

- Scrape 150–200 top-level post titles from r/soccer using the Reddit API (PRAW) during the 2026 World Cup group stage.
- Target a mix of match threads, general discussion, and news posts to capture all three label types.
- Filter out mod/automod posts, removed posts, and posts under 5 words.
- Manually label a sample of ~60 posts (20 per class) to serve as ground truth for evaluation.
- Store collected data as a CSV with columns: `post_id`, `title`, `post_text`, `label`.

#### Evaluation metrics:

- **Accuracy** — overall fraction of correctly classified posts on the labeled sample.
- **Per-class F1** — precision and recall broken out per label (analysis / hot_take / reaction) to catch class imbalance issues.
- **Confusion matrix** — to identify which pairs of labels the model most often confuses (e.g., hot_take vs reaction).
- Target: macro-average F1 ≥ 0.70 on the hand-labeled sample.

#### Definition of success:

The classifier is considered successful if it achieves a macro F1 ≥ 0.70 on the 60-post hand-labeled evaluation set, with no single class falling below F1 = 0.60. Qualitatively, the model should correctly handle the edge cases above and not systematically collapse into predicting one dominant label.

#### AI Tool Plan:

- **Grok API** — zero-shot or few-shot classification of each post title using the label descriptions and examples already defined above. Each post is sent as a prompt; the model returns one of `analysis`, `hot_take`, or `reaction`.
- **Prompt construction** — include the three label descriptions and 2–3 labeled examples per class in the system prompt to anchor the model's judgment.
- **Batch processing** — iterate over collected posts, call the API per post, and write predictions to the CSV alongside ground-truth labels.
- **Evaluation script** — compute accuracy, per-class F1, and confusion matrix using scikit-learn against the hand-labeled sample.