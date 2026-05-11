# ai-social-good-project
Lazy Finance Ai 
# AI-Powered Personal Finance Assistant for College Students

> **Course:** Fundamentals of MIS | **SDG:** Goal 10 — Reduced Inequalities

---

## Problem

David is a 22-year-old student at San Jose State University who uses Apple Pay and credit cards for small daily purchases — coffee, food, Uber, and online shopping. Although he believes his spending is reasonable, he consistently runs out of money before the end of the month and cannot identify where his money is going.

The failure point is not a lack of financial data. David's banking app shows every transaction. The breakdown happens at the moment he tries to make sense of that data: the app returns raw numbers with no categorization, no pattern recognition, and no behavioral guidance. He can see that he spent $240 this week, but he cannot tell whether the problem is food, transportation, or impulse shopping — and so he cannot change anything.

This problem is common among college students in San Jose who have access to digital banking but lack the financial literacy tools to interpret what they see. Without meaningful interpretation, access to data does not translate into better decisions.

---

## AI Capability

This project uses **structured data extraction** (Lab 2) as its primary AI capability, supported by **image recognition** (Lab 3) as a secondary input.

Lab 2 directly addresses the failure point: it transforms unstructured spending descriptions — the kind a student might copy from a bank statement — into structured financial fields. The AI extracts total spending, main category, largest expense, spending level, and a personalized recommendation. In the lab, we observed that this capability could consistently produce the same output format across multiple runs when a schema was defined, which is essential for a tool a student would use repeatedly.

Lab 3 extends the system by connecting everyday visual behavior to financial habits. A photo of a coffee cup or a food court can prompt the AI to reflect on how repeated small purchases accumulate. This capability was chosen because financial problems for college students are often behavioral, not informational — the issue is not that David does not know coffee costs money, but that he does not see how daily coffee purchases add up across a month.

The combination of these two capabilities — structured extraction from transaction text and behavioral insight from images — is justified by what we observed in the labs: neither alone is sufficient. Extraction without behavioral context produces data the student may ignore. Image analysis without structured data produces advice that feels vague.

---

## Workflow

**Input**
The student provides two types of input: (1) a text description of recent transactions, written the way they might appear in a bank statement or be recalled from memory, and (2) an optional image related to a daily spending behavior, such as a photo of a coffee shop, a food delivery app, or a shopping bag.

**AI Processing**
- Lab 2 structured extraction parses the transaction text and returns a five-field JSON object: `total_spent`, `main_category`, `largest_expense`, `spending_level`, and `recommendation`.
- Lab 3 image recognition analyzes the uploaded image and generates a behavioral observation connecting the visual to a spending pattern.

**Output**
The system produces a categorized spending summary with a spending-level label (Low / Moderate / High), a behavioral insight connecting the image to financial habits, and a personalized recommendation the student can act on.

**Who Acts on It**
David reviews the AI output after each spending period. He compares the AI's category breakdown against his own perception of where his money went, identifies discrepancies, and uses the recommendation to adjust one behavior before the next period. The AI supports his decision — it does not make the decision for him.

> **Screenshots of prototype output are included in the repository notebook (see `milestone3_prototype.ipynb`).**

---

## Failure Case

**The Input**
During edge case testing in Milestone 2, the following prompt was submitted to the Lab 2 extraction system:

> *"I spent $28 at Target, $35 on Amazon, $12 at Starbucks, and $18 on Uber. The Target purchase included groceries, school supplies, and a phone charger. The Amazon purchase included both a textbook and a hoodie. I am not sure which category these should go into."*

**What the AI Returned**
The AI correctly calculated the total spending ($93) but assigned a single `main_category` to the entire input — collapsing a purchase that was simultaneously food, school supplies, electronics, clothing, and transportation into one label. In one run, it returned `Shopping`. In another, it returned `Clothing`. The category changed across runs without any change to the input.

**The Real-World Consequence**
For David, this means the budgeting advice he receives is tied to the wrong problem. If the AI labels his main category as `Shopping` when his real overspending is in `Food` and `Transportation`, he may reduce discretionary purchases — and still run out of money — because the actual behavior driving his shortfall was never identified. The Lab 2 output demonstrated that this kind of inconsistency is not hypothetical; it was observed in a real prototype run.

---

## Oversight and Tradeoff

**Oversight Position**
A human review step sits between the AI's category output and any financial decision David makes. Specifically: before David acts on the spending level label or the recommendation, he manually confirms that the main category assigned by the AI matches his own recollection of the purchase. This review step is placed at the output stage, not the input stage, because the lab showed that the failure occurs in classification — not in data entry.

This position is justified by the Lab 2 edge case output: the same input produced different category labels across runs, which means the AI's confidence in its classification is not reliable enough to act on without confirmation.

**The One Change**
The change that would most reduce the harm identified in the failure case is replacing the single `main_category` field with a multi-category breakdown — for example, `categories: {food: $40, school: $20, transportation: $18, shopping: $15}` — so that mixed purchases are not forced into one label.

The tradeoff is real: multi-category extraction is harder to implement, requires a more complex schema, and produces output that is less immediately readable for a student who wants a quick answer. It also increases the chance that the AI will disagree with itself about how to split a purchase — introducing a different kind of inconsistency. The simpler schema is faster and cleaner, but it systematically misrepresents purchases made at stores like Target or Amazon, which are the purchases most likely to cause budgeting confusion in the first place.
