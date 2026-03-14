# Schema for strata_resources.tsv

This table defines resource masses and distributions in each strata.





Consider extractable_resources.md note 1, 2nd bullet point, about filler, ores and volatiles. It's important to note that `relative_abundance` here reflects total mass, not extractable mass; the latter is accounted for by `dispersion` and other factors. Some real-world composition substances may be borderline between stone, regolith, and other resource categories; in these cases, assign to the closest category by extraction context.





Rows are strata names (`name` column in strata.tsv). Columns are extractable resource names (see extractable_resources.md, note 5). Each data cell contains 4 numbers separated by commas. These values are used (2 after conversion) to model resource distribution in each natural stratum (per extractable_resources.md, note 3):

* `relative_abundance` — This is %wt of the resource in the stratum. It will be used to generate resource `mass` (as described in extractable_resources, note 3). The sum of all values in a stratum should be 100 ± 1. It doesn't need to be exact because these will be normalized by code before conversion to mass. Note: the simulation can handle **very** small numbers here, so be sure to include volatiles in accordance with extractable_resources.md note 1.
* `mass_cv` — Epistemic uncertainty (CV) about the mass abundance.
* `dispersion` — See extractable_resources.md, note 3.
* `dispersion_sd` — Epistemic uncertainty (SD) about the dispersion. (We want SD in the table; it will be converted to CV internally.)

Keep significant digits of each value ≤ 2 with the following exceptions: 1) "Filler" stone and regolith can have 3 significant digits to help normalize the total abundance to ~100. 2) Relatively homogeneous strata (e.g., atmosphere, ocean) can have more significant digits where precise values are known and the higher precision is warranted.