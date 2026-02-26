# Extractable Resources

Notes:
1. "Extractable resources" are simplified model abstractions that are (at the same time):
   * Output resources from extraction operations such as mining, drilling, and air separation, including beneficiation where applicable. 
   * The substances that completely define the composition of any natural body stratum by total mass. In this context, stone and regolith are usually the main "filler" material for solid strata. Ores represent total concentrated mass (usually much greater than what could be extracted economically) and volatiles represent what might be separated from solids in the extraction operation (sometimes a target product).
   * Trade commodities priced by weight.
   * Inputs to refining (to produce refined resources) or other industrial processes. Note that refined resources don't come from only one resource stream. For example, in an asteroid mining context, "precious metals" are refined mainly from "iron/nickel" since there are no significant deposits of "precious metal ores".
2. Listed compositions are elemental weight percent (summing to 100) followed by valuable co- or by-products in ppm (or main product in the case of precious metals). **For composite items, these are representational only!** Different strata will necessarily have different "industrial metal ores", different "industrial minerals", different "stone", etc. This issue is especially acute for "industrial metal ores" and "industrial minerals". The simulation simplifies by treating composition within an abstract resource item as entirely fungible.
3. Distribution of each extractable resource within a stratum is modeled by four internal parameters:
   * `abundance` (μ) — Total mass in the stratum (normalized to achieve correct total stratum mass).
   * `abundance_error` (μ_err) — Epistemic uncertainty (SD) about the abundance.
   * `dispersion` — (σ) — Standard deviation of the lognormal distribution of the resource in log₁₀ units (simplified to cover all spatial scales). This is the principle factor that allows economical extraction in mining and drilling. For homogeneous strata like atmospheres, dispersion is 0. 
   * `dispersion_error` (σ_err) — Epistemic uncertainty (SD) about the dispersion.
4. Use CONSTANT_CASE for the "name" field in data table construction. Replace non-alphanumeric characters with underscores. E.g., IRON_NICKEL, IRON_ORES, etc.

List:

- **Iron/Nickel** — Native Fe-Ni metal; ref. M-type asteroid alloy (Psyche-class). Fe 91, Ni 8, Co 0.5, P 0.2, S 0.15, C 0.1, Cr 0.05. Byproducts (ppm): Ge 150, Cu 150, Ga 20, Pt 15, Ru 5, Pd 3, Os 3, Ir 2, Au 1.5, Rh 1.5, W 1.5.
- **Iron Ores** — Fe oxide/hydroxide; ref. blend of Earth magnetite-hematite concentrate and lunar ilmenite + Martian hematite. Fe 55, O 29, Ti 7, Si 2.5, Mg 1.1, Al 1.1, S 0.6, Ca 0.6, Mn 0.5, Cr 0.2, Cl 0.15, P 0.05, other 2.2. Byproducts (ppm): V 300, Zr 100, Ni 50, Co 20, Sc 15, Hf 2.
- **Aluminum Ores** — Ref. blend of Earth beneficiated bauxite and lunar highland anorthosite concentrate. Al 22, O 50.5, Si 11.5, Ca 6.5, Fe 5.5, H 1.5, Ti 1, Mg 1, Na 0.2, other 0.3. Byproducts (ppm): Sr 100 (off-Earth fraction), Ga 30 (Earth fraction), Eu 8 (off-Earth fraction).
- **Industrial Metal Ores** — Ref. blend of Earth production-weighted Cu-Mn-Cr-Zn-Ni-Li sulfide-oxide-silicate concentrates (Li pegmatite fraction forward-weighted for projected demand growth; Li from brines enters via water/brine processing) and asteroidal chromite-sulfide + Martian Mn-oxide + lunar chromite-spinel. Fe 17.5, O 19.8, S 12.2, Mn 8.9, Cr 8.5, Si 5.8, Cu 4.1, Al 4.1, Mg 3.8, Zn 3.4, Ti 2.4, Ni 1.8, Ca 1.15, Co 0.3, Ba 0.25, Li 0.2, Pb 0.2, other 5.6. Byproducts (ppm): V 350, Mo 75, Sn 60, Rb 30, W 30, Ag 25, Se 18, Ta 9, Cd 9, Nb 6, In 6, Cs 6, Ge 3, Bi 3, Be 2.5, Te 2, Re 1.2, Au 1, Pt 1, Pd 0.7, Ru 0.3, Rh 0.2, Ir 0.1, Os 0.1.
- **Precious Metal Ores** — Mainly present in Earth crust; ref. production-weighted blend of gold sulfide concentrate (78%), PGM chromite-sulfide concentrate (16%), and primary silver sulfide concentrate (6%). Fe 36, S 30, O 12, Si 5, Cr 3, As 2.3, Pb 2, Cu 1.4, Mg 1, Al 1, Zn 0.6, Ni 0.3, Sb 0.2, other 5.2. Precious metal content (ppm): Ag 760, Au 65, Pt 18, Pd 15, Rh 5, Ru 5, Ir 2, Os 2. Byproducts (ppm): Se 150, Co 100, Te 20, Bi 10.
- **Rare Earth Ores** — Ref. blend of Earth bastnäsite-monazite flotation concentrate and lunar KREEP merrillite-apatite mineral separate. Total REE 28.5 (Ce 12.5, La 7, Nd 3.8, Pr 1.5, Y 1.25, Sm 0.55, Yb 0.50, Gd 0.4, Dy 0.25, Tb 0.20, Ho 0.18, Er 0.17, Tm 0.08, Lu 0.07, Eu 0.05), O 31.5, Ca 15, P 9, F 3.9, C 2.5, Si 2, Fe 1.25, Mg 1.25, Al 0.75, Ba 0.5, Na 0.5, Th 0.35, K 0.1, other 2.9. Byproducts (ppm): Nb 500, U 175, Zr 150, Sc 25, Ta 13.
- **Uranium Ores** — Mainly present in Earth crust; ref. gravity-flotation pre-concentrate from unconformity-type and IOCG deposits, beneficiated but not yet leached. U 6, O 38, Si 20, Fe 17, S 3.5, Al 3, Ca 3, Mg 2, K 0.5, Ti 0.5, Na 0.5, V 0.3, Cu 0.3, H 0.3, C 0.3, Th 0.2, Pb 0.2, Ni 0.1, total REE 0.1 (Ce 0.044, La 0.025, Nd 0.013, Y 0.005, Pr 0.005, Sm 0.002, Gd 0.001, Dy 0.001, Eu 0.001, Er 0.001, Yb 0.001, Tb 0.001), other 4.2. Co-product elements at percent scale (in main composition): V 0.3, Cu 0.3, Th 0.2, total REE 0.1. Byproducts (ppm): Mo 100, Se 20, Sc 20, Ra 10, Au <1 (some deposits).
- **Sulfur** — Ref. blend of Earth recovered elemental sulfur (from hydrocarbon desulfurization) and elemental sulfur from asteroidal troilite thermal decomposition + Martian anhydrite concentrate. S 91, O 4.7, Ca 2.5, Fe 0.55, Si 0.4, Mg 0.15, C 0.08, H 0.03, other 0.59. Byproducts (ppm): Se 200 (primarily from off-Earth troilite), Te 25.
- **Industrial Minerals** — Ref. blend of Earth production-weighted construction, agricultural, and chemical minerals and off-Earth devolatilized regolith-phosphate-salt (water, nitrogen compounds; organics removed and tracked separately). O 44.5, Si 18, Ca 13.5, Fe 5.2, Al 4.2, Mg 2.8, P 2.1, Na 2.1, Cl 2.1, C 2, K 0.95, S 0.5, Ti 0.5, total REE 0.35 (Ce 0.154, La 0.086, Nd 0.047, Pr 0.018, Y 0.015, Sm 0.007, Gd 0.005, Yb 0.006, Dy 0.003, Tb 0.002, Ho 0.002, Er 0.002, Eu 0.001, Lu 0.001, Tm 0.001), F 0.2, H 0.05, Mn 0.05, other 0.9. Note: total REE 0.35 in main composition is primarily from off-Earth KREEP fraction and may be a co-product. Byproducts (ppm): B 500 (Earth borate/evaporite fraction), Th 400 (off-Earth KREEP fraction), Br 100 (Earth evaporite/brine fraction). He-3 at ppb level in lunar regolith fraction (potential fusion fuel).
- **Organics/Tholins** — Ref. blend of Earth kerogen concentrate (Type I/II, demineralized from oil shale and organic-rich marine mudstone; excludes coal, petroleum, gas, and biomass) and off-Earth carbonaceous chondrite insoluble organic matter (IOM), Titan tholins, and cometary CHON particles. C 73.4, H 7.4, N 4.5, O 10.5, S 3.6, other 0.6. Byproducts (ppm): V 150 (Earth kerogen porphyrins), Ni 50 (Earth kerogen porphyrins), Mo 15 (Earth kerogen, chelated in organic matrix).
- **Coal** — Earth only; ref. washed, air-dried thermal coal, production-weighted global blend (dominated by bituminous and sub-bituminous). C 66, O 18, H 5.5, Si 2.3, S 1.5, N 1.4, Al 1.3, Fe 0.7, Ca 0.35, K 0.17, Mg 0.12, Na 0.07, Ti 0.06, P 0.02, other 2.51. Byproducts (ppm): total REE 50 (concentrated 5–10× in coal ash — some coal ashes exceed 1000 ppm total REE; Ce 22.5, La 10, Nd 7, Y 4, Pr 2.5, Sm 1.5, Gd 1, Dy 0.5, Er 0.3, Yb 0.3, Eu 0.2, Tb 0.1, Ho 0.05, Tm 0.03, Lu 0.02), Ge 10, Ga 10, Se 3, U 3.
- **Oil** — Earth only; ref. desalted, dewatered crude, production-weighted global average. Co-product: recovered elemental sulfur. C 84.5, H 12, S 1.5, N 0.4, O 0.6, other 1.0. Byproducts (ppm): V 150, Ni 50.
- **Stone** — Bulk rock, suitable for dimension stone when not molten; ref. primitive mantle pyrolite (McDonough & Sun 1995). O 44, Mg 23, Si 21, Fe 6, Ca 2.5, Al 2.3, Na 0.3, Cr 0.3, Ni 0.2, other (Ti, Mn, K, P) 0.4.
- **Regolith** — Surface fines and debris (includes overburden and gangue from mining operations); ref. generic basalt (Moon/Mars/MORB average). O 44, Si 22, Fe 11, Ca 7, Al 7, Mg 5, Na 1.5, Ti 1.5, other (K, Mn, Cr, P) 1.
- **Water** — H₂O. H 11.2, O 88.8.
- **Hydrogen** — H₂. H 100.
- **Oxygen** — O₂. O 100.
- **Nitrogen** — N₂. N 100.
- **Carbon Dioxide** — CO₂. C 27.3, O 72.7.
- **Carbon Monoxide** — CO. C 42.9, O 57.1.
- **Ammonia** — NH₃. N 82.2, H 17.8.
- **Methane** — CH₄. C 74.9, H 25.1.
- **Ethane** — C₂H₆. C 79.9, H 20.1.
- **Sulfur Dioxide** — SO₂. S 50.1, O 49.9.
- **Helium** — He. He 100.
- **Argon** — Ar. Ar 100.
- **Heavy Noble Gases** — Kr, Xe, and Ne as co-product. Ref. Earth atmospheric mass ratio. Ne 77, Kr 20, Xe 3.
