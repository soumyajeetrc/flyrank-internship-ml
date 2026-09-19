# The State of AI-Driven SEO Triage

## Abstract
Can we use machine learning to identify declining SEO content before traffic fully decays? We analyzed a sample from the FlyRank ML Internship dataset, evaluating search visibility metrics. We trained a Decision Tree model using a strict time-aware validation split to predict low-traffic pages. After auditing the model and removing target leakage, the AI achieved 62.4% precision, which underperformed the manual baseline of 67.1%. Consequently, this pipeline is deployed strictly as a decision-support tool that requires human review rather than an automated publishing system.

## 1. Introduction / Problem Statement
**The Research Question:** Can we use machine learning to identify declining SEO content and prioritize human optimization efforts before search traffic completely decays? 
**Business Decision:** This model acts as a decision-support system, triaging the content backlog so the human strategy team knows exactly which pages to rewrite first for maximum ROI. Human editors at FlyRank have limited time and cannot manually review thousands of web pages. This project solves that bottleneck by using machine learning to predict which specific pages are at high risk of losing search traffic, allowing editors to prioritize their refresh queue and protect client visibility.

## 2. Data
**Data Source & Scope:** This study is built on a public-safe subset of the FlyRank ML Internship dataset, evaluating the `fact_content_daily_performance` table. We specifically excluded pages with zero search impressions to prevent unindexed or completely invisible content from artificially skewing our baseline traffic metrics.

## 3. Methodology
* **Label Definition:** A page was flagged for review if its daily search impressions fell below 10.
* **Features:** The model evaluated quantitative search metrics, specifically `gsc_avg_position` and `ctr` (Click-Through Rate).
* **Validation & Leakage Check:** An initial random split showed 100% precision, which our audit revealed as target leakage (using impressions to predict an impression-based label). To ensure honest results, we implemented a strict time-aware split (training on past data to predict future data) and completely removed the leaked feature.

## 4. Results (Model vs Baseline)
Under a strict time-aware validation split with target leakage removed, the manual heuristic (67.1% precision) outperformed the audited Decision Tree model (62.4%). Without the leaked impressions feature, the AI struggled to confidently isolate low-traffic pages using only CTR and Position. 

## 5. Limitations & Honest Framing
* **Model Performance:** We cannot claim this model automates or replaces human decision-making. The current AI underperforms a simple manual baseline.
* **Feature Deficits:** The model's logic is strictly limited to visibility and click metrics. It lacks qualitative context, such as content age or seasonality.
* **Scope:** Any output must pass through strict human review before content changes are made.

## 6. Ranked Recommendations (Action Playbook)
1. **Refresh Mature Pages:** Focus on older pages with declining clicks.
2. **Snippet Optimization:** Rewrite meta titles for pages stuck on Page 1 with low CTR.
3. **Guardrail:** Auto-publishing and auto-deleting are strictly prohibited. 

## 7. Reproducibility
The full code, data extraction logic, and audited notebooks used to generate this research can be found in the `work/notebooks/` directory of this repository.

## Acknowledgments & Data Credit
Built on the FlyRank ML Internship dataset. For more information, visit [https://flyrank.ai](https://flyrank.ai).
