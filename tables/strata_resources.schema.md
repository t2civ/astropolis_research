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
- dispersion — Standard deviation of the lognormal distribution of the resource in log₁₀ units, simplified to cover all spatial scales. This is the principle factor that allows economical extraction in heterogeneous strata (e.g., for mining and drilling). For mostly homogeneous strata like atmospheres and oceans, dispersion is 0 for most resources but may be positive for specific resources (e.g., Water in Earth's atmosphere or Wild Fisheries in Earth's ocean).
- dispersion_sd — Epistemic uncertainty (SD) about the dispersion.

The table default value is "0,0,0,0". Hence, zero-abundance cells can be left empty.


## Notes

1. Consider `resources.descriptive.md` notes 2 and 3. It's important to note that relative_abundance here reflects total mass, not extractable mass; the latter is accounted for by dispersion and other factors. Some real-world composition substances may be borderline between stone, regolith, and other resource categories; in these cases, assign to the closest category by extraction context.
2. Cross-check: Always verify relative_abundance values against the stratum's total mass from strata.tsv. Multiply `relative_abundance / 100 * mass` to obtain the implied absolute mass in kg, then compare against known real-world estimates. Strata masses can be very large, so volatiles and biosphere resources will typically have very small relative_abundance values.
3. Keep significant digits of each value <= 2 with the following exceptions:
  - "Filler" resources (stone, regolith, and sometimes water) can have 3 significant digits to help normalize the total abundance to ~100.
  - Relatively homogeneous strata (atmospheres and oceans) can have any number of significant digits when abundance of a non-abstract resource (e.g., OXYGEN) is more precisely known.
 
 
## Discovered Resource Level
 
For targeted extraction operations, a `discovered` value between 0 and 1 is calculated for each resource in each stratum as follows:
 
effective_dispersion = dispersion × survey_level × 0.25, where survey_level is from strata.tsv.  
discovered = (relative_abundance / 100.0) × 10^effective_dispersion  
discovered = min(discovered, 1.0)

The resulting `discovered` value drives target resource production in extraction operations relative to energy requirement and overburden.
 