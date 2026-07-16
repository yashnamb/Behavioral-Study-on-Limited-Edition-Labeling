# Does a "Limited Edition" Label Actually Sell More Sneakers?

A randomized online experiment measuring the causal effect of exclusivity ("Limited Edition") labeling on consumer purchase intent, willingness to pay, and resale intent.

> **Course:** BA830 – Business Experimentation & Causal Methods (Boston University, MSBA, Spring 2025)
> **Methods:** Randomized controlled experiment · Difference-in-means (ATE) · OLS regression · Cohen's *d* & power analysis
> **Stack:** Python · pandas · statsmodels · pingouin · Qualtrics

---

## Statistically significant findings

Out of seven outcomes tested, **two effects were statistically significant** (the rest were nulls — see [full results](#key-results)):

| Result | Estimate | Significance | **What it means, in plain words** |
|---|---|---|---|
| **"Limited Edition" label → resale intent** | −0.28 | *p* = 0.003 (survives multiple-comparison correction, *p*<sub>FDR</sub> = 0.02) | **The label makes people want to *flip* the product, not necessarily *use* it.** When the label was removed, far fewer people picked that sneaker to resell — so exclusivity's real pull is speculative/resale demand. |
| **Gender → purchase intent** | +0.23 | *p* = 0.015 | **Men leaned toward the featured sneaker more than women** — a baseline audience difference (not caused by the label), useful for knowing who to target. |

> **The headline null is itself the insight:** the label did **not** significantly move purchase intent or willingness to pay. In plain words — *a "Limited Edition" sticker alone doesn't make people buy; it makes a specific segment want to resell.* Marketers should treat exclusivity as a **targeting tool for collectors/resellers**, not a blanket lever for demand.

---

## Motivation

The sneaker market has shifted from functional footwear to a collector-driven, resale-fueled industry where "Limited Edition" drops command premium prices. This project asks a simple causal question that matters to any brand using scarcity marketing:

> **Does labeling a product "Limited Edition" actually change what consumers do — or just what we assume they do?**

## Hypotheses

| | Statement |
|---|---|
| **H₀ (Null)** | "Limited Edition" labeling has no effect on purchase intent, willingness to pay, or resale intent. |
| **H₁ (Alt.)** | Consumers show higher purchase intent, WTP, and resale intent for "Limited Edition"–labeled sneakers. |

## Experimental Design

Participants were recruited via social media and randomly assigned by Qualtrics to one of two arms. Both groups saw the **same two sneakers** and answered the **same 7 questions** — the only difference was the presence of the labels.

| Arm | What they saw | n (after cleaning) |
|---|---|---|
| **Control** | Sneakers explicitly labeled *"Regular Release"* vs *"Limited Edition Release"* | 56 |
| **Treatment** | Identical sneakers, **labels removed** | 55 |

Seven outcomes were measured: purchase intent, price sensitivity, past shopping behavior, availability influence, willingness to pay, resale intent, and same-price choice preference. Gender was collected to test for heterogeneous effects.

> **Coding note:** each outcome is coded `1 = Sneaker A (Regular)`, `2 = Sneaker B (Limited Edition)`. `treatment = 1` means the labels were *removed*, so a **negative** treatment coefficient means removing the label pushed people *away from* the (formerly) limited-edition sneaker.

## Methods

The [extended notebook](notebook/extended_analysis.ipynb) walks through a full causal-inference toolkit:

1. **Randomization checks** — proportions z-test for the ~50/50 split *and* a χ² test confirming gender is balanced across arms (*p* ≈ 1.0).
2. **Average Treatment Effects** — difference-in-means t-tests across all 7 outcomes, with a **Benjamini–Hochberg FDR correction** for testing multiple outcomes.
3. **Composite exclusivity index** — pooling the 7 items into one 0–7 score to recover signal the individual (underpowered) items miss.
4. **Effect size & minimum detectable effect (MDE)** — reframing the nulls in terms of what this sample size *could* have detected.
5. **Heterogeneous effects** — a treatment × gender interaction to test whether the label works differently by gender.
6. **A coefficient plot** summarizing every treatment effect with 95% confidence intervals.

> **On rigor vs. sample size.** With ~55 respondents per arm the study can only detect medium-or-larger effects (MDE ≈ *d* 0.54). Findings are framed as **directional evidence**, and the one robust result (resale intent) is shown to survive multiple-comparison correction.

## Key Results

**Average Treatment Effects** (OLS, *n* = 111)

| Outcome | Treatment coef. (SE) | Interpretation |
|---|---|---|
| Purchase intent | −0.027 (0.096) | No significant effect |
| **Resale intent** | **−0.279 (0.092)*** ** | **Removing the label lowers resale intent → the label drives resale demand (p < 0.01)** |
| Willingness to pay | +0.044 (0.095) | No significant effect |

**Heterogeneous Effects (Gender)**

| Term | Coef. (SE) | Interpretation |
|---|---|---|
| Male (vs Female) | +0.233 (0.094)** | Men significantly more likely to choose the sneaker (*p* < 0.05) |
| Treatment | −0.025 (0.094) | No significant label effect on purchase intent |

<sub>Significance: * p<0.1, ** p<0.05, *** p<0.01</sub>

**Purchase intent** — t = −0.28, *p* = 0.78, Cohen's *d* = −0.05, power ≈ 0.06.

## Business Implications

- **Don't rely on scarcity labels alone** to lift purchase intent — the direct effect is negligible.
- **Use exclusivity to target the resale/collector segment**, where the label demonstrably drives behavior.
- **Segment by demographics** (e.g., gender) rather than applying exclusivity cues broadly.

## Limitations

- **Underpowered:** ~55 respondents/arm and a tiny observed effect (Cohen's *d* = −0.05, power ≈ 0.06) mean subtle effects could be missed.
- **Sample bias:** skewed toward university students; limited generalizability.
- **Short window:** one week of collection misses broader temporal shopping variation.

See the [full report](docs/final_report.pdf) for the complete write-up and references.

## Repository Structure

```
.
├── README.md
├── requirements.txt
├── notebook/
│   ├── course_project_analysis.ipynb   # original Group 23 course submission
│   └── extended_analysis.ipynb         # solo post-course cleanup + added rigor
├── data/
│   └── survey_responses.csv      # de-identified Qualtrics export
└── docs/
    ├── final_report.pdf          # full written report
    ├── survey_instrument.pdf     # the 7-question survey shown to participants
    └── final_presentation.pdf    # summary slide deck
```

**Two notebooks, clearly attributed:**
- [`course_project_analysis.ipynb`](notebook/course_project_analysis.ipynb) — the original **Group 23** team submission.
- [`extended_analysis.ipynb`](notebook/extended_analysis.ipynb) — an **individual, post-course extension** by Yashna Meher: a reproducible cleanup plus added rigor (covariate balance test, FDR multiple-comparison correction, composite preference index, treatment × gender interaction, minimum detectable effect, and the summary coefficient plot).

## Reproduce

```bash
pip install -r requirements.txt
jupyter notebook notebook/extended_analysis.ipynb
```

The notebook runs top-to-bottom and loads the included `data/survey_responses.csv` automatically (falling back to the original Qualtrics export if run standalone). All outputs are pre-rendered, so it also reads cleanly on GitHub without being run.

## Tech Stack

`Python` · `pandas` · `numpy` · `statsmodels` · `stargazer` · `pingouin` · `scipy` · `matplotlib` · `seaborn` · `Qualtrics`

## Authors

**Original course project — Group 23:** Rebecca Bubis · Sanjal Desai · Yashna Meher · Mishil Trivedi
**Post-course extension & repository:** Yashna Meher

## References

1. Jang, W. E., Ko, Y. J., Morris, J. D., & Kim, S. Y. (2019). *Scarcity Message Effects on Consumption Behavior: Limited Edition Product Considerations.* Journal of Promotion Management, 26(4), 473–492.
2. Sharma, P., & Alter, A. L. (2012). *Financial deprivation prompts consumers to seek scarce goods.* Journal of Consumer Research, 39(3), 545–560.
3. Zhao, X., & Belk, R. W. (2007). *Live from eBay: The significance of resale value in consumer motivation.* Journal of Consumer Research, 34(2), 231–245.
