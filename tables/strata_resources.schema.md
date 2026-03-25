# Schema for strata_resources.tsv

This Entity x Entity table defines resource abundance and distribution in each strata. 


## Entity Descriptions 

For complex tasks involving strata and their resource compositions, it's important to understand what these simulation entities are supposed to represent. Read these files in the `descriptions` directory:

- strata.md — This file describes strata (rows).
- extractable_resources.md — This file describes extractable resources (columns). Only these resources need to be considered when working with strata composition.


## Table Data

The data type is VECTOR4. Each cell contains the following 4 float values delimited by commas:

- relative_abundance — This is %wt of the resource in the stratum. The sum of all values in a stratum should be 100 ± 1; we don't need exact summation to 100 because values are normalized internally. IMPORTANT: Include resources at very low abundance if their extraction might be economically significant. E.g., we include Helium in Earth subsurface strata because it is an important byproduct in gas drilling.
- mass_cv — Epistemic uncertainty (CV) about the mass abundance.
- dispersion — Standard deviation of the lognormal distribution of the resource in log₁₀ units, simplified to cover all spatial scales. This is the principle factor that allows economical extraction of low `relative_abundance` resources in heterogeneous strata (e.g., for mining and drilling). For mostly homogeneous strata like atmospheres and oceans, dispersion is 0 for most resources but may be positive for specific resources (e.g., Water in Earth's atmosphere or Wild Fisheries in Earth's ocean).
- dispersion_sd — Epistemic uncertainty (SD) about the dispersion.

The table default value is "0,0,0,0". Hence, zero-abundance cells can be left empty.


## Notes

1. Consider `resources.descriptive.md` notes 2 and 3 when setting `relative_abundance`. It's important to note that `relative_abundance` here reflects total mass, not extractable mass; the latter is accounted for by dispersion and other factors. Some real-world composition substances may be borderline between stone, regolith, and other resource categories; in these cases, assign to the closest category by extraction context.
2. `dispersion` should be set with strong consideration of its practical effect on extraction operations such as mining (see Practical Effect of Dispersion below).
3. Cross-check: Always verify relative_abundance values against the stratum's total mass from strata.tsv. Multiply `relative_abundance / 100 * mass` to obtain the implied absolute mass in kg, then compare against known real-world estimates. Strata masses can be very large, so volatiles and biosphere resources will typically have very small relative_abundance values.
4. Keep significant digits of each value <= 2 with the following exceptions:
  - "Filler" resources (stone, regolith, and sometimes water) can have 3 significant digits to help normalize the total abundance to ~100.
  - Relatively homogeneous strata (atmospheres and oceans) can have any number of significant digits when abundance of a non-abstract resource (e.g., OXYGEN) is more precisely known.
 
 
## Practical Effect of Dispersion

Target resource removal in extraction operations is directly proportional to an internal parameter `discovered` (0 < discovered <= 1.0), calculated from `relative_abundance`, `dispersion` and `survey_level` (the latter is a stratum parameter from `strata.tsv`). The `discovered` cap at 1.0 provides a natural cap on the effect of `survey_level`. E.g., Stone typically has the maximum `discovered` value 1.0, so increasing `survey_level` has no marginal benefit for Stone Quarrying. Note that `survey_level` is set internally to a minimum value of 2.5 for any stratum that has active extraction operations.

When setting `dispersion` values for unsurveyed or poorly surveyed strata, calculate `discovered` *as if* the stratum has been surveyed with `survey_level = 4.0`. This represents a moderately high survey level and simplifies calculations below such that `effective_dispersion` equals `dispersion`.

Calculate `discovered` as follows:

```
effective_dispersion = dispersion + (0.25 * (survey_level - 4.0)) 
discovered = (relative_abundance / 100.0) * (10 ^ effective_dispersion) 
discovered = min(1.0, discovered)
```
 