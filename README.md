# No Marks for Ambiance

Does Michelin's Guide really ignore ambiance and service when awarding stars — the way it claims to? A statistical test using facility data from ~19,400 restaurants in the Michelin Guide.

Part of the [somewhere-else.org](https://somewhere-else.org) weekly data science series.

## The question

Michelin's own guide states that inspectors do not factor decor, table setting, or service quality into star awards — recognition is meant to be about food alone. This project tests that claim empirically: do restaurant amenities associated with ambiance and service (wine list, terrace, view, garden, counter dining, valet parking) nonetheless show a measurable relationship with Michelin recognition, and does that relationship survive controlling for price and country?

## Headline finding

Of six ambiance/service facilities tested, **"Interesting wine list" stands alone** — it shows a real, independent relationship with Michelin recognition that survives controlling for both price tier and country (pooled odds ratio ≈ 2.04, Cochran–Mantel–Haenszel). Two other facilities (Terrace, Great view) actually point in the *opposite* direction. Once country is controlled for, the wine-list effect gets *stronger*, not weaker — but the initial "wine list matters more at higher price tiers" framing does not survive geographic controls and was revised accordingly.

## Method summary

- **Data:** [ngshiheng/michelin-my-maps](https://github.com/ngshiheng/michelin-my-maps), ~19,400 restaurants globally. See the notebook's reproducibility section for the exact commit hash and snapshot date used.
- **Price** converted from currency-symbol strings (e.g. `$$$$`, `¥¥¥¥`) to a validated 1–4 ordinal tier.
- **Facilities** one-hot encoded from a pre-specified set of six ambiance/service tags, chosen before looking at results.
- **Cochran–Mantel–Haenszel test** to check whether the wine-list/award association holds after stratifying by price tier.
- **Logistic regression** with a price-tier × wine-list interaction, including a documented correction after an initial continuous-coding specification produced a misleading result at the lowest price tier.
- **Geographic controls**: country added as a fixed effect after peer review raised a Simpson's-paradox concern; a likelihood-ratio test confirms country materially improves the model, and an omnibus LR test confirms the interaction terms matter jointly even where no single tier-level estimate is individually significant.
- **One-way ANOVA + Tukey HSD** on price by award category, to establish how price actually relates to award tier as background context.

## Repository contents
No_Marks_For_Ambiance.ipynb   — full analysis notebook, built cell by cell
wine_list_by_price_tier.png   — figure: wine-list effect by price tier
price_by_award_category.png   — figure: price distribution by award category
facility_gaps_comparison.png  — figure: all six facilities' raw award-rate gaps
requirements.txt              — Python package versions used
README.md                     — this file
The public-facing article (once published) will be linked here.

## Reproducing this analysis

```bash
git clone https://github.com/seanjaya/no-marks-for-ambiance.git
cd no-marks-for-ambiance
pip install -r requirements.txt
jupyter notebook No_Marks_For_Ambiance.ipynb
```

The notebook downloads its own data snapshot on first run. Because the source dataset is updated monthly, results may drift slightly if re-run after a future update — the notebook's reproducibility section pins the exact commit used for the reported results.

## Limitations

- The dataset's Cuisine field is populated for only ~2.7% of restaurants (almost entirely starred/Bib Gourmand listings) and is not used in this analysis.
- Country fixed effects capture between-country variation only — they cannot distinguish, for example, Paris from rural France, or Tokyo from Hokkaido.
- The 2-star/3-star price comparison is likely affected by a ceiling effect in the four-level price scale, not necessarily evidence that 3-star dining doesn't cost more.
- This analysis is correlational throughout; no causal claims are made about wine lists influencing award decisions.

## Peer review

This analysis went through three rounds of peer review, which surfaced a country-level confound (addressed by adding geographic controls) and several methodological refinements documented in the notebook itself.

## License

MIT — see [LICENSE](LICENSE).

## Data source

[ngshiheng/michelin-my-maps](https://github.com/ngshiheng/michelin-my-maps), scraped from the official Michelin Guide website.
