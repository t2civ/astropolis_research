*Claude-Opus-4.6 (output effort = high). Feb, 2026.*

---
---

I need information for a simulation that focuses on resource streams: extraction, commodity trading, and downstream usage in industrial/economic activities. The simulation starts at present-day technology and operations on Earth, but runs forward in time to include highly speculative (yet physically plausible) future technologies and operations in the wider solar system. Abstractions and simplifications are necessary. However, please tell me if my instructions seem too oversimplified or have erroneous assumptions. Ask for clarification if it may help in subsequent queries. Summarize simplifications and important omissions in your analyses. State references and show your work.

Use estimations or speculative values where needed. Use units km, km², km³, kg, and g/cm³ as appropriate. Write large numbers (≥1e6) and small numbers (<0.001) using "e" exponent notation with up to 3 significant digits (e.g., 5.97e24). Use estimations or speculative values where needed. Use existing table data if it helps for cross-calibration.

# Context files include:
* strata.md — Descriptive list of simulation "strata".
* extractable_resources.md — Descriptive list of simulation "extractable resources".
* strata.tsv — Under construction data table defining strata (model input).
* strata_resources.tsv — Under construction data table defining resource distributions in the strata (model input).

# strata.tsv

This table defines strata in the simulation.

Note: It's not important that adjacent strata "fit" perfectly at boundaries. In fact, this is impossible in cases where a boundary is an "area-weighted mean" adjacent to layers of different thickness, e.g., Moon mantle upper boundary adjacent to maria and terrae crust. This is OK. What matters here is consistency **within** the stratum, such that `mass` = `volume` × `density` and spatial values are internally consistent given the simplified geometries. Use thin-shell approximations where appropriate. Atmospheres will have tiny mean densities given the volume construction; this is OK.

Don't worry too much about getting survey_level exactly right; these will be cross-calibrated after we have all strata.

Columns:

* `name` — See strata.md, note 8.
* `body` — See strata.md, note 8.
* `reference_r` — Reference radius for the body. Surface or sea level radius. Mean radius for irregular bodies/bulk strata.
* `territory` — See strata.md, note 8.
* `strata_group` — See strata.md, note 8.
* `inner_r`  — Inner boundary radius of the specific stratum. 0 for core or bulk strata. 0 for generic strata.
* `thickness` — Thickness of the stratum. For bulk strata, this should be the same as reference_r. 0 for generic strata.
* `area_fraction` — Fraction of whole body spherical area for strata that include regional boundaries (e.g., Earth territories, Moon terrae versus maria, Mars polar cap, etc.). Use 1 when not applicable (or generic strata).
* `area` — Surface area of the stratum (at outer radius boundary) treating all bodies as spherical. 0 for generic strata.
* `volume` — Volume of the stratum. 0 for generic strata.
* `density` — Mean density of the stratum. Mean expectation density for generic strata.
* `mass` — Total mass of the stratum. 0 for generic strata.
* `survey_level` — A value from 0 to 10 (decimals allowed) that indicates our relative knowledge about the stratum composition and deposit locations in it. Construct the scale considering all strata listed in strata.md. 0 for generic strata. For calibration:
   * 0. No knowledge.
   * 0.5 Theoretical models with moderate uncertainty.
   * 2.5. Lunar regolith (excluding polar).
   * 5. Earth near-surface areas that have been extensively explored for resource extraction.
   * 7. Earth surface that is fully surveyed and explored.
   * 10. Earth atmosphere — near perfect knowledge of a mostly homogeneous composition (no uncertainty about deposits).
* `survey_note` — A very brief qualitative note that justifies the value provided in `survey_level`. "NA" for generic strata.

# strata_resources.tsv

This table defines resource masses and distributions in each strata. Consider extractable_resources.md note 1, 2nd bullet point, about filler, ores and volatiles. It's important to note that `relative_abundance` here reflects total mass, not extractable mass; the latter is accounted for by `dispersion` and other factors. Some real-world composition substances may be borderline between stone, regolith, and other resource categories; in these cases, assign to the closest category by extraction context.

Rows are strata names (`name` column in strata.tsv). Columns are extractable resource names (see extractable_resources.md, note 5). Each data cell contains 4 numbers separated by commas. The numbers are:

* `relative_abundance` — This is %wt of the resource in the stratum. It will be used to generate resource mass (`abundance` as described in extractable_resources, note 3). The sum of all values in a stratum should be close to 100 but doesn't need to be exact; these will be normalized by code before conversion to mass. Note: the simulation can handle **very** small numbers here, so be sure to include volatiles in accordance with extractable_resources.md note 1.
* `abundance_cv` — This is coefficient of variation of the total resource mass, representing epistemic uncertainty. It will be used to generate `abundance_error` (after conversion of `relative_abundance` to `abundance`).
* `dispersion` — See extractable_resources.md, note 3.
* `dispersion_error` — See extractable_resources.md, note 3.

# Instructions

Modify the tables as needed. Provide either specific table rows or entire tables as needed, formatted as code. These are the changes and additions I need right now:

* Subdivide continental ice and water bodies into the 8 territorial regions. For simplicity, keep the global mean thickness the same when subdividing water bodies and adjust `area` and `area_fraction` as needed. However, for ice bodies give separate attention to Antarctica and Other (the latter containing Greenland); these should probably have their own thickness. Per notes, the important thing is that `mass` = `volume` × `density` and spatial values are internally consistent within the stratum.
* Per notes, exact normalization of `relative_abundance` in strata_resources.tsv isn't needed. However, some rows are excessively far from 100. Normalize `relative_abundance` in all rows so the total is 100 ± 1. When doing so, keep significant digits of each value ≤ 2 except for "filler" stone and regolith, which can have up to 3 significant digits. Also allow more precision when it is warranted in bulk air and water compositions.

Also provide feedback so I can improve the prompt and/or context files for subsequent analyses.

---
---
