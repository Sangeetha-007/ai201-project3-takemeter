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
<!-- 
| Situation | Label |
|---|---|
| Interview with a strong storytelling frame | **interview** (guest still drives) |
| Solo episode where host uses "we" throughout | **solo** (one host's voice) |
| Two equal co-hosts, no guests | **panel** (multiple perspectives) |
| Narrative that weaves in interview clips | **narrative** (story arc dominates) |
| First-person personal story in past tense | **solo** if only the host's memory; **narrative** if built from external sources (documents, archives, others' interviews) |
                """


| Post | Failure mode | Agent response |
|------|-------------|----------------|
| search_listings | No results match the query | "I am sorry, your style requirements is beyond our reach! Please try a new search." |
| suggest_outfit | Wardrobe is empty | "Your outfit seems to be so good as it is! We can't suggest an outfit at the moment, please try again later"! |
| create_fit_card | Outfit input is missing or incomplete | "Outfit details unavailable — check back later!" |


-->

#### Data collection plan:


#### Evaluation metrics:


#### Definition of success: