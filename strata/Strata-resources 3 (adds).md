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

This table defines resource masses and distributions in each strata. Consider extractable_resources.md note 1b. It's important to note that `relative_abundance` here reflects total mass, not extractable mass. The latter is accounted for by `dispersion` and other factors.

Rows are strata names (`name` column in strata.tsv). Columns are extractable resource names (see extractable_resources.md, note 5). Each data cell contains 4 numbers separated by commas. The numbers are:

* `relative_abundance` — This is %wt of the resource in the stratum. It will be used to generate resource mass (`abundance` as described in extractable_resources, note 3). The sum of all values in a stratum should be close to 100 but doesn't need to be exact; these will be normalized by code before conversion to mass. Note: the simulation can handle **very** small numbers here, so be sure to include volatiles in accordance with extractable_resources.md note 1.
* `abundance_cv` — This is coefficient of variation of the total resource mass, representing epistemic uncertainty. It will be used to generate `abundance_error` (after conversion of `relative_abundance` to `abundance`).
* `dispersion` — See extractable_resources.md, note 3.
* `dispersion_error` — See extractable_resources.md, note 3.

# Instructions

Modify the tables as needed. Provide either specific table rows or entire tables as needed, formatted as code. These are the changes and additions I need right now:

* Add Earth strata: atmosphere, ocean, ocean floor, ocean crust, mantle, and core.

Also provide feedback so I can improve the prompt and/or context files for subsequent analyses.

---
---

*Thinking...*

> The user wants me to add several Earth strata to both `strata.tsv` and `strata_resources.tsv`. Let me work through each one carefully.
> 
> ## Earth Strata to Add
> 
> 1. **EARTH_ATMOSPHERE**
> 2. **EARTH_OCEAN**
> 3. **EARTH_OCEAN_FLOOR**
> 4. **EARTH_OCEAN_CRUST**
> 5. **EARTH_MANTLE**
> 6. **EARTH_CORE**
> 
> Let me work through each one.
> 
> ### Reference values
> - Earth mean radius: 6,371 km
> - Earth surface area: 4π × 6371² ≈ 5.10e8 km²
> 
> 
> - Earth surface area: 4π × 6371² ≈ 5.10e8 km²
> 
> For the atmosphere, I'm calculating the outer radius by adding the 65 km thickness to Earth's surface, which gives me the area of the outer boundary at roughly 5.21e8 km². The atmosphere covers the full planet with a unit area fraction, and I need to compute its volume based on these dimensions.
> 
> Now I'm working through the volume calculation using the spherical shell formula, computing the difference between the outer and inner radii cubed to find the atmospheric volume. Now I'm finishing those calculations and getting a total atmospheric volume of about 3.35e10 km³. When I convert that to cubic meters and divide the total atmospheric mass by this volume, I'm getting a density that seems way too low—around 1.54e-13 g/cm³—which suggests I've made an error somewhere in my unit conversions or calculations.
> 
> Let me recalculate with the right unit conversions: converting the volume to cubic centimeters and the mass to grams gives me a density of 1.54e-4 g/cm³, which is much more reasonable. It's still quite low, but that makes sense since most of the atmosphere is concentrated in the lower layers. The instructions even note that atmospheres will have unusually low mean densities due to how the volume is constructed, so this checks out. I'll set the mass at 5.15e18 kg, use a survey level of 10 as a calibration reference, and note that this is a near-perfect survey.
> 
> Now I'm working through the atmospheric composition by mass, converting the various gases from their typical measurements. Nitrogen and oxygen dominate at 75.5% and 23.1%, with argon making up most of the remainder. For the trace gases, I need to convert from parts per million by volume to mass fractions—carbon dioxide at 420 ppmv converts to about 640 ppmw or 0.064% by mass, and I'm doing similar conversions for helium, neon, and methane. Now I'm working through the heavy noble gases composition—neon, krypton, and xenon together make up about 16 parts per million by mass, which is 0.0016%, and I'm also accounting for the smaller trace gases like helium, methane, and carbon monoxide as I build out the full atmospheric breakdown for the simulation.
> 
> For the atmosphere data structure, I need to set dispersion and dispersion_error to zero since it's well-mixed and homogeneous, while keeping abundance_cv very small for the stable components like nitrogen and oxygen but slightly higher for variable ones like water vapor and carbon dioxide.
> 
> Now I'm assigning coefficient of variation values for each atmospheric component based on their stability and measurement uncertainty—the major gases get very tight ranges around 0.001 to 0.005, while the trace gases and more variable components get progressively wider ranges up to 0.2 for the least stable ones. I should also verify the total mass accounts for everything properly. I'm finishing the calculation for CO₂ and water vapor, then realizing that water vapor actually displaces the other gases rather than adding to them—dry air composition is what we typically reference, with nitrogen, oxygen, argon, and CO₂ accounting for essentially 100%, while water vapor varies and reduces the percentages of the other components proportionally. Now I'm verifying the atmospheric composition adds up correctly with the water vapor adjustment, and then setting all the ore resource values to zero across the different strata categories. I'm finishing out the atmospheric composition data and verifying the percentages add up correctly to around 100%, which they do.
> 
> For the simulation purposes, I'm going to treat dust and aerosols as negligible since they don't fit cleanly into the gas categories. I'm also reconsidering sulfur dioxide—while it does exist in Earth's atmosphere at trace levels around 0.1-1 ppb, it's highly variable depending on volcanic activity, so I'll just set it to zero for practical simulation purposes.
> 
> Now I'm moving on to defining the ocean layer. According to the reference data, Earth's oceans cover about 361 million km² (roughly 70.8% of the surface) with an average thickness of 3.69 km. I'm setting up the ocean stratum with the body as Earth, a reference radius of 6371 km, and calculating the inner radius by subtracting the ocean thickness from the surface radius. Now I'm calculating the ocean's volume using the thin shell approximation with the area and thickness I found, which gives me about 1.33 billion cubic kilometers—matching the actual ocean volume remarkably well. Then I'm converting the seawater density and multiplying through to get the total mass, which comes out to roughly 1.36 × 10²⁴ grams.
> 
> For the survey level, I'm settling on 8.0 since we have extensive oceanographic data on bulk chemistry, though there's notable spatial variation in dissolved minerals and salinity across different regions. Now I'm mapping out the composition: seawater is mostly water with about 3.5% dissolved salts, dominated by sodium chloride but also containing magnesium chloride, sodium sulfate, and other minerals.
> 
> For the simulation, I need to categorize these components as extractable resources. Water makes up 96.5% by mass, so that's clearly the WATER category. The dissolved salts—sodium, chloride, magnesium, calcium, sulfur compounds—should map to INDUSTRIAL_MINERALS since they're industrially valuable and extractable. I'm accounting for the CO₂ as mostly bicarbonate ions that are already part of the mineral fraction, so I'll treat it as trace, and similarly N₂ is negligible. The trace elements like uranium and lithium are so dilute they're effectively captured within the industrial minerals category. For the simulation, I'm setting water at 96.5% with low variability and no dispersion, while industrial minerals at 3.5% will also have low variability and uniform distribution.
> 
> Actually, I should reconsider—ocean salinity does fluctuate between 3.3% and 3.7% globally, so the coefficient of variation isn't quite zero, though the dispersion across the bulk ocean should remain essentially zero since it's well-mixed as a liquid. There might be minor exceptions like brine pools or hydrothermal vents, but those are localized anomalies. I'm also factoring in the dissolved gases—oxygen, nitrogen, and CO₂—which are all extremely small percentages, so I'll include them as separate components even though they're negligible.
> 
> Now I'm working through the composition table with these values and checking that everything sums to roughly 100%, then considering whether to add trace methane from hydrothermal vents and methanogenic organisms. The real question is what resources ocean extraction would actually yield—desalinated water, salt, minerals like sodium and magnesium, and potentially other compounds.
> 
> For the simulation I'll keep it straightforward by having INDUSTRIAL_MINERALS represent the salt and mineral fraction. Then I'm moving on to define the ocean floor stratum, which covers the deep seafloor beyond the continental shelf down to 100 meters below the surface, spanning about 336 million km² and containing manganese nodules, polymetallic sulfides, and rare earth element-bearing muds. I need to calculate what fraction that represents of Earth's total surface area. So I need to reconsider how the ocean floor layer is structured—it's the top 100 meters of sediment below the deep seafloor, which sits at roughly 4 km depth on average. Working through the geometry, the outer radius is about 6367 km at the seafloor surface, the inner radius is 6366.9 km accounting for the 100-meter thickness, and I'm calculating the surface area by taking the ocean fraction of Earth's total surface area at that radius.
> 
> Now I'm computing the volume by multiplying the area by the thickness, then estimating the mass using a density of about 2.0 g/cm³ for the mixed sediment and rock composition of the top 100 meters. This gives me a total mass around 6.72 × 10¹⁹ kg. For the survey level, I'm thinking this should be around 2.0 since we have reasonable knowledge of ocean floor composition from various drilling and sampling programs, though the estimate is still somewhat incomplete.
> 
> Looking at what actually makes up the ocean floor, I'm considering the major components: the sediments and basalt form the bulk of the material, with unconsolidated sediment as a secondary layer, and various mineral deposits like manganese nodules and polymetallic sulfides scattered throughout. The composition is heavily weighted toward stone materials, with significant industrial mineral potential. proportion — ocean floor sediments rich in clays, zeolites, phosphorites, salts — maybe 20%
> - WATER: Pore water — maybe 5% (saturated sediments)
> - INDUSTRIAL_METAL_ORES: Mn nodules, polymetallic sulfides, cobalt crusts — these are significant but still a small mass fraction overall. Mn nodules cover about 30% of the Pacific floor. If we estimate their contribution to the top 100 m average... Nodule density on the seafloor is about 5-15 kg/m², concentrated in the top few cm. Over the whole 100 m column, this is tiny. But the question is about total ore-grade material. Let me estimate:
>   - Mn nodules: ~5e11 tonnes = 5e14 kg distributed across maybe 30% of Pacific floor, plus some in other oceans. As a fraction of total ocean floor mass, that's only about 0.00074% — essentially negligible. Polymetallic sulfides and other concentrated deposits are similarly small by mass, though economically valuable. I'd estimate industrial metal ores at around 0.01% of the ocean floor average, with most concentrated at specific locations rather than evenly distributed.
> 
> Let me step back and think about the actual composition more carefully. The ocean floor is dominated by pelagic sediments and basalt as the bulk material, while the economically interesting resources like Mn nodules, sulfides, and rare earth muds represent tiny mass fractions despite their high value. So I should probably structure this as roughly 50% stone...
> 
> I'm now working through a composition breakdown for the top 100 meters of ocean floor, assigning percentages to different material types based on their actual distribution and abundance. the upper portion):
>    - Siliceous ooze
>    - Calcareous ooze
>    - Red clay (pelagic clay)
>    - Terrigenous sediments
> 
> 2. Where sediment is thin, basaltic ocean crust is within 100 m
> 
> Average composition of pelagic sediments (rough):
> - SiO₂: ~50%
> - Al₂O₃: ~15%
> - Fe₂O₃: ~7%
> - CaO: ~3%
> - MgO: ~3%
> - Na₂O: ~2%
> - K₂O: ~2%
> - MnO: ~1%
> - TiO₂: ~0.7%
> - P₂O₅: ~0.3%
> - H₂O: ~5-10%
> - Organic C: ~0.5-2%
> 
> For the simulation, I need to map these sediment types to resource categories. The exposed basalt would count as STONE, maybe 10-20% of the column, while the clay and sediment material dominates as the unconsolidated fines that fit better under REGOLITH rather than INDUSTRIAL_MINERALS.
> 
> Now I'm working through the specific resource breakdown for the top 100 meters of deep seafloor. The sediment layer is mostly unconsolidated material that maps to REGOLITH, with some basalt exposed as STONE. There are also trace concentrations of economically interesting materials like manganese nodules, iron oxides, phosphorites, and rare earth element muds that would fall into their respective ore and mineral categories. I'm estimating REGOLITH at around 55% as the dominant unconsolidated sediment, with STONE at roughly 20% for the basaltic portions.
> 
> But I'm realizing I've significantly underestimated the pore water content. Deep sea sediments near the surface have porosity levels of 60-80%, and even when I average that down over the full 100-meter column, the water fraction by mass is probably closer to 20-40% rather than the 10% I initially assigned. I'm recalculating using grain density and water density to get a more accurate mass fraction.
> 
> For the ocean floor stratum, I should set the bulk density to around 1.8 g/cm³ to account for the high porosity and pore water saturation, then redistribute the composition to reflect roughly 20% water, 45% regolith, 15% stone, and the remainder as industrial minerals.
> 
> Now I'm working through the specific percentages for the mineral components—iron ores at about 3%, aluminum-rich clays at 3%, rare earth elements in muds at around 0.3%, plus smaller fractions for polymetallic nodules, sulfides, and other trace minerals—checking that everything sums to 100%.
> 
> I'm realizing the main bulk of ocean floor composition should be sediment and basalt, similar to how continental land is mostly stone and regolith, so I need to recalibrate the percentages with regolith as the dominant filler component for the top 100 meters. I'm finishing out the composition percentages for the deep ocean, allocating the remaining fractions to organic carbon, iron and aluminum ores, and industrial metal ores like polymetallic nodules and sulfides—though I'm realizing the nodule mass alone is negligible relative to total ocean floor, so I need to think about whether "industrial metal ores" should capture the broader category of metalliferous sediments around hydrothermal vents rather than just the nodules themselves. Looking at the continental land data, I see 0.015% for industrial metal ores works out to about 296 billion tonnes for the USA, which tracks with known Cu, Zn, and Ni deposits. For the ocean floor, I'm trying to calibrate the percentage—0.01% gives 6.72 trillion tonnes which feels too high, while 0.001% at 672 billion tonnes seems more defensible given how sparse deep-sea mineral deposits actually are.
> 
> The key insight is that land ore deposits are highly concentrated in specific locations (hence the high dispersion value), whereas ocean floor metalliferous sediments like manganese nodules and hydrothermal sulfides are more thinly spread across vast areas. So I'm settling on reasonable percentages for ocean floor composition and moving forward with those baseline estimates rather than getting caught in the precision trap. I'm realizing methane hydrates are actually a massive resource—estimates range from 5e14 to 5e16 cubic meters globally, though most concentrations are on continental margins rather than the deep ocean floor itself. For the actual ocean floor, I'll estimate around 1e14 m³ of methane in solid clathrate form, which puts it at about 0.01% abundance. Now I'm thinking through other dissolved components like nitrogen in pore water and carbonate sediments.
> 
> Carbonate ooze is a dominant sediment type on the ocean floor, so that's a significant contributor to industrial minerals rather than gaseous CO₂. Let me lock in my final composition estimates across all the major categories. Now I'm assigning dispersion values for each material type on the ocean floor, ranging from low for stable features like stone and regolith to very high for concentrated phenomena like hydrothermal vents where precious metals cluster.
> 
> Adding organics and methane to the list with moderate to high dispersion, then accounting for the ocean floor's limited exploration by setting abundance uncertainty in the 0.3-0.6 range.
> 
> Moving on to the ocean crust layer itself, which extends from about 100 meters below the seafloor down to the Moho boundary around 7 kilometers deep—this is where the deep sediment and basaltic crust exist, and it's largely speculative for resource harvesting purposes.
> 
> Now I'm working through the geometry: the ocean crust should cover the deep ocean floor area, which is roughly 336 million km² after accounting for continental land and shelf coverage. There's some ambiguity in how the continental layers relate to the ocean crust—whether ocean crust extends under the continental shelf or only covers the true deep ocean floor—but based on the simulation's structure, it seems ocean crust corresponds specifically to the ocean floor area outside the continental regions. near-surface, subsurface, etc.) cover land + shelf, while ocean crust covers just the deep ocean floor area. This is a simplification.
> 
> Wait, let me re-read: "total surface = ... = continental near-surface (or deeper 'continental') + ocean crust"
> 
> Continental area (land + shelf): ~174 million km²
> Ocean crust area: total - continental = 510 - 174 = 336 million km²
> 
> Yes, ocean crust area ≈ 3.36e8 km², same as ocean floor.
> 
> Thickness: from 100 m below seafloor to Moho at ~7 km sub-ocean.
> Thickness = 7 - 0.1 = 6.9 km
> 
> Deep ocean floor average depth: ~4.0 km
> Ocean crust outer_r: 6371 - 4.0 - 0.1 = 6366.9 km (bottom of ocean floor stratum)
> Inner_r: 6371 - 4.0 - 7.0 = 6360.0 km (Moho under ocean)
> 
> Now I'm calculating the volume of the oceanic crust layer using the area fraction of 0.659 and thickness of 6.9 km, which gives me roughly 2.32 billion km³. For the mass calculation, I need to account for the density of oceanic crust—basalt and gabbro at about 2.9 g/cm³.
> 
> Now I'm working through the mass calculation and then documenting the composition of oceanic crust. The material is primarily basaltic in the upper layers and gabbroic below, with a known elemental composition from MORB (mid-ocean ridge basalt) samples. My confidence level here is moderate since we have ophiolite analogues and some deep drilling data, but direct sampling of the full oceanic crust column remains limited.
> 
> For the resource distribution across the crust, I'm breaking down the composition into simulation categories: the bulk of it is solid basalt and gabbro rock, with smaller fractions of sediments, hydrothermal minerals, iron and aluminum-rich zones, and trace amounts of sulfides and other economically relevant materials.
> 
> I'm verifying the percentages add up correctly—they total around 99.42%, so I'll bump the stone fraction slightly to 85.6% to reach 100%. Since this is tagged as a speculative planet with low survey confidence, I should keep in mind these numbers carry significant uncertainty.
> 
> Now I'm moving on to characterizing the mantle layer beneath the Moho discontinuity.
> 
> For the mantle geometry, I need to account for the fact that the Moho sits at different depths depending on whether it's under ocean or continental crust. I'm calculating an area-weighted average depth—about 66% of Earth's surface is ocean with a shallow Moho around 7 km down, while 34% is continent with a much deeper Moho around 35 km. That weighted average comes to roughly 4.6 km, which gives me an outer radius for the mantle of about 6366 km. The inner boundary extends down to 2,890 km depth, putting the inner radius at 3,481 km. The mantle is a complete spherical shell, so I don't need perfect boundary alignment—I can just use the area-weighted mean Moho depth of 6354.5 km as the outer radius. Now I'm calculating the volume using the inner radius of 3481 km and working through the arithmetic for the spherical shell formula. Now I'm working through the mantle's composition based on seismic and gravity models, constrained by xenolith samples and exposed ophiolites since we can't directly sample it. For the simulation, I'm treating the mantle as almost entirely silicate rock—olivine, pyroxene, and garnet—with trace amounts of water, sulfides, and other volatiles mixed in, following the standard McDonough & Sun reference model. I'm realizing the mantle composition should be almost entirely stone since that's what defines stone in the first place. For a speculative harvest scenario, I'd get mostly bulk silicate rock with trace amounts of water trapped in minerals, some sulfur from sulfide pockets, and negligible quantities of everything else. I'm adjusting the stone percentage to get closer to 100, then setting dispersion values for the mantle layer—keeping most resources fairly uniform since the mantle is relatively homogeneous at large scales, though diamonds and metal ore pods show more regional concentration. Now I'm moving on to define the Earth's core, which is primarily iron-nickel composition at depths beyond 2,890 km. Now I'm calculating the core's density from its mass and volume, which comes out to about 11 g/cm³ — matching well with seismic estimates for the outer and inner core. Since we can't directly sample the core, I'm relying on seismic data and geochemical models, which gives a survey level of 0.3. For the composition, the core is primarily an iron-nickel alloy at around 88%, with lighter elements like sulfur, oxygen, and silicon making up the rest. I'm working through how to map Earth's core composition to the resource categories. The light elements dissolved in the metal are tricky—they're not separate phases, so I need to think about what extraction would actually yield. I'll go with IRON_NICKEL at 88 and SULFUR at 5 to keep it realistic for this speculative stratum.
> 
> Now I'm considering the trace elements: cobalt and chromium would go into INDUSTRIAL_METAL_ORES, silicates would separate as STONE during processing, and platinum group elements are already accounted for within the Fe-Ni alloy. The question of hydrogen is interesting—there's likely a significant amount in the core, but there's no clean resource category for it. Carbon's the same problem; it doesn't fit neatly into the existing categories like ORGANICS_THOLINS or METHANE. So I'm simplifying this down to what actually makes sense: Earth's core is fundamentally a massive Fe-Ni alloy with some impurities.
> 
> Looking at the final breakdown, I'm settling on IRON_NICKEL at 88, SULFUR at 5, STONE at 5 for the silicate component, and INDUSTRIAL_METAL_ORES at 1.5 for the other siderophile elements. The carbon in the core exists as dissolved carbon in metallic iron or as iron carbide, not as CO₂, so it doesn't warrant its own category—I'll just fold it into the IRON_NICKEL composition since it's already there in trace amounts. I'm realizing the oxygen dissolved in the core would separate out as silicates and oxides during extraction, so I should consolidate that into the STONE category. Adjusting the composition to account for this and bringing the total to 100, I get IRON_NICKEL at 85.5, SULFUR at 5, STONE at 7, INDUSTRIAL_METAL_ORES at 2, and HYDROGEN at 0.5.
> 
> The precious metals like platinum and palladium are already factored in as byproducts of iron-nickel processing, so I don't need a separate category for them. I'm also adding a trace amount of primordial helium trapped in the core. Now I'm moving on to think through the composition of continental ice and water bodies.
> 
> I'm setting up the data tables now and need to focus on the six main regions the user asked for: atmosphere, ocean, ocean floor, ocean crust, mantle, and core. Let me start calculating the precise surface areas and volumes for each stratum, beginning with the atmosphere layer. Now I'm checking the ocean layer parameters—the density calculation confirms around 1.54e-7 g/cm³ for the crust, and I'm verifying the ocean's area using its surface radius of 6371 km with a 70.8% coverage fraction, which gives roughly 3.61e8 km². I need to reconsider whether the area should be calculated at the outer surface rather than averaged across the layer thickness. surface area.
> area = 0.708 × 4π × 6371² = 0.708 × 5.10e8 ≈ 3.61e8
> 
> - volume: thin shell V ≈ area × thickness = 3.61e8 × 3.69 = 1.33e9 km³
>   
>   More precisely: for a partial sphere shell:
>   V = area_fraction × (4/3)π × (6371³ - 6367.31³)
>   But thin shell approximation is fine: 1.33e9 km³
> 
> - density: 1.025 g/cm³
> - mass: 1.33e9 × 1e15 × 1.025 = 1.36e24 g = 1.36e21 kg
> 
> Good, actual is ~1.4e21 kg. Close enough.
> 
> Now I'm setting up the ocean floor stratum with a reference radius of 6371 km, an outer boundary at 6367 km (accounting for average deep ocean depth of 4 km), and a thin 0.1 km layer representing the seafloor itself. The area fraction is 0.659, but I need to reconsider whether I'm correctly excluding the continental shelf—the definition should be all ocean depths except the continental shelf down to 100 m below the seafloor, and I'm trying to figure out if my current parameters align with that. be consistent with continental land.
> 
> mass = 3.36e7 × 1e15 × 2.0 = 6.72e22 g = 6.72e19 kg
> 
> Now I'm setting up the ocean crust layer with survey metadata noting that most of the ocean floor remains unexplored beyond DSDP/ODP/IODP sampling sites. For the structural parameters, I'm defining the outer radius at 6366.9 km (accounting for the 0.1 km sediment layer) and the inner radius at 6360 km, which puts the Moho discontinuity about 7 km below the ocean floor surface.
> 
> Working through the volume calculation using the thin shell approximation, I get roughly 2.32e9 km³ for the oceanic crust. With a density of 2.9 g/cm³ typical of basaltic and gabbroic material, that gives a total mass around 6.73e21 kg, and the survey confidence level is relatively low since we have limited direct sampling of the deep ocean crust.
> 
> Now I'm recalculating the Moho depth more carefully—I need to account for the actual ocean and continental coverage percentages rather than just the surface fractions. The ocean floor and continental shelf make up about 70.8% of Earth's surface, while continental land is 29.2%, and I'm working out what the mean radius to the Moho boundary should be based on these proportions. Now I'm calculating the area-weighted average radius to the Moho at about 6352 km, which gives me a mantle thickness of roughly 2871 km when accounting for the core-mantle boundary at 3481 km. I'm working through the surface area and volume calculations for the mantle layer using these radii.
> 
> Finishing the volume computation and then deriving the mantle's average density of about 4.47 g/cm³ from the known Earth mantle mass. Now I'm setting up the core parameters with a reference radius of 6371 km and an inner radius of 0 at the center.
> 
> For the core itself, I'm calculating its thickness at 3481 km, then working through the surface area and volume using spherical geometry. The mass breaks down into inner and outer components totaling roughly 1.94e24 kg, which gives a bulk density around 10.96 g/cm³. Let me double-check this against Earth's total mass of 5.97e24 kg to make sure the accounting is consistent.
> 
> Subtracting out the mantle, crust, oceans, and atmosphere from Earth's total mass leaves about 1.93e24 kg for the core, which confirms my density calculation of roughly 11 g/cm³. Now I'm organizing the resource composition data for each stratum, starting with the atmosphere which has zero abundance for all metal and ore resources.
> 
> Generating resource table... I'm looking at the atmospheric composition data and verifying the percentages add up correctly. Nitrogen and oxygen dominate at 75.4% and 23.1%, with argon, water vapor, and carbon dioxide making up most of the remainder, totaling just over 100% which accounts for rounding. That checks out. Now I'm converting the trace gases to mass percentages—hydrogen's around 3.8e-6%, methane should be closer to 1e-4% rather than 1.2e-4%, and carbon monoxide comes in at about 1e-5%.
> 
> For the ocean resources, I'm setting up a table with water as the dominant component and industrial minerals as a minor fraction. The key question is how much dispersion to account for—the ocean's pretty well-mixed overall, but salinity does vary between 33-37 ppt depending on upwelling and trade wind patterns, so I'll give industrial minerals a small dispersion value of 0.05 while keeping everything else at zero. I should also note dissolved oxygen as a trace component.
> 
> Now I'm working through the oxygen concentration—it's typically around 6 mg/L in surface waters but varies significantly with depth, ranging from near zero in the deep ocean to about 8 mg/L at the surface. Converting this to weight percentage, 6 mg/L works out to roughly 5.85e-4%, so I'll use 5e-4% as a reasonable average that accounts for this natural variation.
> 
> For nitrogen, dissolved N₂ averages around 12 mg/L, which converts to about 1.2e-3% by weight. The carbon dioxide situation is more nuanced—while total dissolved inorganic carbon is quite high at around 2000 µmol/kg, most of that exists as bicarbonate and carbonate ions rather than free CO₂ gas. The actual dissolved CO₂ available is only about 10 µmol/kg or roughly 4.4e-5%, so for the simulation I should focus on that free CO₂ portion since the bicarbonate and carbonate are part of the mineral system rather than the gas phase.
> 
> During seawater processing, the dissolved inorganic carbon would mostly stay dissolved or precipitate as calcium carbonate, which gets classified as industrial minerals. The free CO₂ that would actually degas is minimal, so I'll set carbon dioxide at 0.01% to account for the small portion that would release as gas during extraction operations.
> 
> Now I'm setting up the resource composition for ocean water with water as the dominant component, industrial minerals from dissolved salts, and trace amounts of dissolved gases like oxygen, nitrogen, and carbon dioxide that could be extracted through various processes. The percentages check out to roughly 100%, which is good. I should also account for dissolved organic carbon in the ocean—that's about 4.85e-5% of total ocean mass, which is negligible—and dissolved sulfate, which similarly doesn't meaningfully change the composition.
> 
> Now I'm thinking through the ocean floor composition more carefully. The deep seafloor is dominated by pelagic sediments—mostly calcareous and siliceous oozes along with red clay—with some basalt exposure at ridges where sediment cover is thin.
> 
> For the top 100 meters, I need to account for the fact that sediment thickness varies widely, so roughly 15-20% of the area might expose basalt within this stratum. For the sediment portion, I'm calculating an average composition based on the three main types: clays contribute silica and alumina, calcareous ooze adds calcium carbonate, and siliceous ooze adds more silica, giving me an overall pelagic sediment average of roughly 50% silica, 20% calcium carbonate, with the remainder being clay minerals and trace elements like manganese.
> 
> Now I'm mapping this to the simulation's resource categories. The unconsolidated sediment—clays and oozes—maps to regolith with a coefficient of variation of 0.15 since bulk sediment composition is fairly well constrained, though regional variation pushes the dispersion to 0.3. The basalt and any indurated sediment go into the stone category with higher uncertainty at 0.25 coefficient of variation, but since basalt is a discrete layer rather than dispersed, I'm setting dispersion to zero.
> 
> For the remaining components, I'm accounting for pore water at 15% with moderate uncertainty, then the authigenic minerals like phosphorites and zeolites as industrial minerals with much higher variability since their distribution is patchy and composition varies widely. The iron-manganese oxide crusts and metalliferous sediments are sparse but highly variable, so they get the highest uncertainty values, and aluminum-rich phases round out the minor constituents.
> 
> But I'm realizing this is getting unwieldy—the manganese nodule estimates alone suggest a total around 5×10¹¹, which means I need to reconsider how I'm allocating these percentages across the different material types. tonnes = 5e14 kg. As fraction of ocean floor mass (6.72e19 kg): 7.4e-6. So actual Mn nodules are ~0.0007%.
> 
> But "industrial metal ores" isn't just Mn nodules — it's all metalliferous concentrations. Metalliferous sediments near ridges, polymetallic sulfides, cobalt crusts... These could add up to more.
> 
> Still, 0.5% seems too high. Let me use 0.05% (which would be 3.36e16 kg = 33.6 billion tonnes).
> 
> Hmm, global manganese nodule resources (including all grades) might be on the order of 500 billion tonnes (5e14 kg). Polymetallic sulfide deposits: maybe 1e12 kg. Cobalt crusts: maybe 1e13 kg. Adding metalliferous sediments and other enriched material, I'm getting a total around 1.5e15 kg, which works out to about 0.0022% of the ocean floor.
> 
> So I'm thinking INDUSTRIAL_METAL_ORES should be around 0.003 with high dispersion. But I need to reconsider this — the continental crust uses 0.01-0.03% for all concentrated metallic material, so for the ocean floor I should probably use something like 0.005 to be consistent.
> 
> Now for RARE_EARTH_ORES: the REE-rich muds in the Pacific are estimated at 1e14 to 1e15 kg total, with maybe 1e11 to 1e12 kg of actual REE oxide. The enriched mud fraction containing most of the REE could be around 1e13 to 1e14 kg, giving a fraction of 1e-6 to 1e-5. I'll set RARE_EARTH_ORES at 0.001%.
> 
> PRECIOUS_METAL_ORES from hydrothermal vents are negligible — probably 1e-5% or less. SULFUR is more abundant in sulfide minerals and dissolved sulfate, maybe around 0.3%. URANIUM_ORES are essentially trace amounts.
> 
> Now I'm compiling the final composition breakdown with all these percentages accounted for, including methane hydrates at 0.005% and dissolved carbon dioxide at 0.02%. I'm realizing the simulation treats ocean floor material as regolith even though its composition differs significantly from lunar regolith—calcareous ooze versus basalt fines. I'll accept this abstraction and map the ocean floor resources to the available categories: regolith for unconsolidated sediment, stone for lithified rock, and industrial minerals for economically valuable deposits. I'm working through the porosity-to-mass-fraction conversion for deep-sea sediments. With typical porosity around 60% in the top 100 meters, the water mass fraction comes out to about 38% rather than the 18% I initially estimated—though that assumes a grain density of 2.5 g/cm³, which conflicts with my earlier bulk density assumption of 2.0 g/cm³.
> 
> Let me recalculate using quartz grain density of 2.65 g/cm³ instead. If the bulk density is 2.0 g/cm³, that implies a porosity of only 40%, which gives a water mass fraction of about 20.5%—more consistent with my initial estimate. But I'm second-guessing whether 2.0 g/cm³ is realistic for the average top 100 meters of the ocean floor, given how porous the upper layers are.
> 
> Looking at the reference data, continental land at 2.0 g/cm³ only has 4% water, which makes sense since it's much less porous than deep ocean sediment. So I should either lower the ocean floor density or increase the water fraction. Let me try 1.7 g/cm³ instead, which better reflects the high porosity of pelagic sediments. That gives me a porosity of about 58.5% and a corresponding water mass fraction I'm calculating now.
> 
> Actually, that shifts the total mass significantly. Let me compromise at 1.8 g/cm³ instead—that gives me a porosity around 52% and roughly 30% water by mass, which feels more reasonable for ocean floor sediment.
> 
> Now I'm settling on the final composition breakdown, adjusting the resource percentages to reflect a more realistic distribution across regolith, stone, water, and various ore types, with the rarest elements like precious metals and uranium at trace levels. I'm revising the composition breakdown to account for the basalt-heavy mid-ocean ridge areas, shifting more mass to stone while reducing regolith and adjusting the trace components accordingly to maintain the total around 100. I'm filling in the trace elements and adjusting the regolith percentage to make everything sum to 100—bumping it up to 37 gives me a clean total across all the major and minor components.
> 
> Now I'm moving on to ocean crust composition, which is dominated by basalt and gabbro with some sediment layers. I'm setting stone at 90 percent as the bulk material, then allocating smaller percentages to industrial minerals like zeolites and iron oxides found in the basalt formations, along with their respective variance parameters. Now I'm looking at the abundance and uncertainty parameters for the rarer components—organics and trace elements like rare earths, precious metals, and uranium, each with their own coefficient of variation and dispersion metrics reflecting how sparse and variable these materials are in the system.
> 
> I'm verifying the composition adds up correctly by summing all the abundance values, which comes to about 99.92, so I'll adjust the stone fraction slightly to 90.08 to reach exactly 100.
> 
> For the mantle itself, stone dominates by definition at 99 percent, with only trace amounts of other materials mixed in.
> 
> Now I'm looking at the detailed breakdown of those trace components — industrial minerals like diamond and garnet make up about 0.2 percent, while water content is minimal at 0.05 percent on the surface, though I'm reconsidering this given that the transition zone between 410 and 660 kilometers depth might actually contain enormous quantities of water locked within crystal structures, potentially equivalent to multiple ocean masses.
> 
> Let me work through the math on this. If the transition zone holds around 1 percent water by weight and comprises roughly 10 percent of the mantle's total mass, that's about 4 times 10 to the 21 kilograms of water — roughly 3 ocean masses. But as a fraction of the entire mantle, that's only about 0.1 percent. The upper mantle above the transition zone probably has much less, around 0.01 percent, while the lower mantle might range from 0.01 to 0.1 percent. Taking a weighted average across all these layers, I'm settling on roughly 0.08 percent for the total water content in the mantle, and I'm setting sulfur at 0.025 percent.
> 
> Now I'm working through the remaining compositional parameters. Sulfur exists mainly in sulfide phases with the mantle containing around 200 to 250 parts per million. For industrial metal ores like chromite and platinum-group element sulfides, I'm using 0.05 percent since chromium is fairly abundant at around 2500 parts per million and gets concentrated in certain pods. Rare earth elements are too dispersed throughout the silicate minerals to form concentrated ore deposits, so that's zero. Precious metals like platinum-group elements do occur in sulfide segregations but at very low concentrations, so I'm setting that at 0.001 percent. I'm assigning uncertainty parameters to these estimates based on how variable these phases are throughout the mantle.
> 
> For carbon dioxide, the mantle contains roughly 100 to 300 parts per million of carbon, mostly locked in carbonates and diamond, which could potentially be released as CO₂. Helium is trickier—primordial helium-3 escapes at mid-ocean ridges, and while helium-4 gets produced through radioactive decay, pinning down the total helium concentration in the mantle is difficult. The estimates range somewhere between parts per billion and parts per million, so I'm working with a conservative value for now.
> 
> Now I'm finalizing the mantle composition by adjusting the trace elements to sum to 100, with stone making up the bulk at around 99.66 percent, and the remaining constituents—industrial minerals, water, sulfur, metal ores, carbon dioxide, precious metals, and helium—accounting for the rest.
> 
> Moving to the core, I'm working through the iron-nickel composition. The core is primarily an iron-nickel alloy with smaller amounts of silicon, oxygen, sulfur, carbon, and hydrogen. For the simulation, I need to represent this accurately, with the iron-nickel component making up roughly 90 percent of the core's total composition.
> 
> When extracting the core, the materials would separate during processing—the iron-nickel alloy as the main product, sulfur as a byproduct, silicates forming slag, and hydrogen and carbon as minor components. Following the same pattern used for the moon core, I'm setting iron-nickel at 85 and sulfur at 5 as the primary extractable resources.
> 
> Now I'm working through where the oxygen in the outer core goes—it's thought to be dissolved in significant amounts, and during extraction it would combine with metals to form oxides or slag. I'm adjusting the stone fraction upward to 7.5 to account for the silicon, oxygen, and other light elements that would end up in the slag, with industrial metal ores capturing the siderophile trace elements like cobalt and chromium.
> 
> The percentages add up to 100, and since the core is essentially homogeneous within its liquid and solid phases, I'm setting very low dispersion values across all components—iron-nickel gets the tightest clustering at 0.05, while sulfur and stone get slightly higher dispersion at 0.1 to reflect their minor compositional variations.
> 
> Now I'm finishing the hydrogen parameters and moving on to compile the final tables, but first I should clarify that I'm only including the strata the user requested: atmosphere, ocean, ocean floor, ocean crust, mantle, and core. I'm calculating the radii for Earth's atmosphere with an inner radius of 6371 km and an outer radius extending 65 km upward. Now I'm calculating the ocean's density and mass, then moving on to model the ocean layer itself with its reduced radius accounting for depth, and computing its surface area and volume using the thin shell approximation. Now I'm calculating the ocean crust layer, which sits beneath the ocean floor sediment at about 6.9 km thick with a density of roughly 3.0 g/cm³ for basaltic rock, giving it a mass around 6.05e19 kg.
> 
> For the mantle, I need to account for the varying depth of the Moho discontinuity—it's shallower under the oceans at roughly 11 km below sea level, but much deeper under continents at around 35 km, so I'm working out the weighted average radius where the mantle actually begins. rust.
> 
> Under ocean: bottom of ocean crust at r = 6360
> Under continent: Moho at ~35 km depth → r = 6336
> 
> Area-weighted: 0.659 × 6360 + 0.341 × 6336 = 4191.2 + 2160.6 = 6351.8 ≈ 6352
> 
> Let me recalculate the area fractions more carefully. The continental subsurface fractions sum to 0.341, which includes both land and shelf, while just the land portion is 0.292. That means the continental shelf alone accounts for about 0.049 of the total area. Now I need to account for the different crustal depths: land has the Moho at roughly 35 km depth, but the shelf is shallower at around 25 km.
> 
> For the ocean crust, the seafloor sits at about 4 km depth, and the crust extends another 7 km below that, putting the Moho at roughly 11 km below the surface. Calculating the area-weighted average radius to the Moho across all three regions gives me approximately 6352 km.
> 
> Now I'm computing the mantle's volume by finding the difference between the outer sphere (radius 6352 km) and the core (radius 3481 km), which works out to about 8.97 × 10¹¹ km³. Using the known mantle mass of roughly 4.01 × 10²⁴ kg, I can derive the average mantle density at about 4.47 g/cm³.
> 
> Now I'm verifying this calculation checks out, and then moving on to the core itself—calculating its volume as 1.77 × 10¹¹ km³ with an estimated mass around 1.94 × 10²⁴ kg. I'm summing up the continental subsurface masses across all regions and the ocean floor contribution, which gives me a total crustal mass of about 2.6×10²² kg when I combine the continental and oceanic portions.
> 
> Now I'm working backward from Earth's total mass to find the core mass by subtracting the mantle, crust, and other layers, which gives me roughly 1.94×10²⁴ kg. Using this mass with the core's volume, I can calculate its density at about 11.0 g/cm³.
> 
> Now I'm assigning survey confidence levels to each Earth layer, starting with the atmosphere at near-perfect knowledge and decreasing as I go deeper into less accessible regions like the mantle and core.
> 
> Let me finalize the data by double-checking my calculations and preparing the output rows for both files. I need to verify the ocean's surface area calculation using the outer radius and area fraction. I'm finishing out the resource percentages for the ocean crust—rare earth ores and precious metals are trace amounts, uranium even tinier. The total comes to just under 100, so I'll lock stone at 90 to keep everything balanced.
> 
> Now I'm reconsidering the density assumption for the ocean floor. The top 100 meters is mostly marine sediment in most areas, but near ridges there's exposed basalt. Blending those together—roughly 80% sediment at around 1.7 g/cm³ and 20% basalt at 2.9 g/cm³—gives me a weighted average closer to 1.94 g/cm³, which is a bit higher than my initial 1.8 estimate.
> 
> I'll round that to 2.0 g/cm³ for consistency with the continental layer and set water content at 20%. That gives me a total ocean floor mass of about 6.72×10¹⁹ kg, and I can now distribute the resource composition across stone, regolith, water, industrial minerals, iron ores, aluminum ores, and organics.
> 
> Now I'm accounting for the trace elements and adjusting the regolith percentage to make sure everything adds up to 100%.
> 
> Let me reconsider the composition though—the ocean floor is mostly soft sediment rather than solid stone, so I should increase regolith to around 50% and reduce stone to about 12%, which better reflects the predominantly sedimentary nature of deep ocean environments.
> 
> Now I'm adjusting the industrial minerals fraction downward to 5% since phosphorites and authigenic minerals, while present, shouldn't take up as much of the composition as I initially estimated.
> 
> Finishing out the ocean floor composition with iron ores at 4%, aluminum ores at 3%, organic-rich sediment layers at 3%, and then smaller fractions for sulfur, various metal ores, and dissolved gases—everything sums to 100.
> 
> 03. But I'm second-guessing the aluminum percentage since bauxite and anorthosite are really terrestrial resources, not typical ocean floor deposits. Aluminum does exist in clays down there, but not in extractable ore concentrations, so I should dial that back to 1% and recalibrate the regolith accordingly. The iron-rich sediments seem reasonable at 4% though, given how prevalent they are in deep ocean environments.
> 
> Let me finalize the composition with these adjustments: I'm settling on a regolith base of 52%, keeping iron ores at 3% to reflect their dispersed nature, dropping aluminum ores to 1%, and maintaining the rest of the trace elements as calculated. I'm filling in the remaining trace components—iron and manganese oxide deposits, aluminum-rich clays, organic matter, sulfides, and various rare ore concentrations scattered throughout the ocean floor sediments.
> 
> Now I'm completing the inventory by adding the dissolved gases in pore water and calculating the total to reach 100 percent, adjusting the regolith fraction to balance everything out. I'm working through the Earth's structural layers, cataloging data for each stratum from the atmosphere down through the ocean, crust, mantle, and core—tracking their radii, thicknesses, densities, surface areas, masses, and the confidence levels based on available observational constraints like seismic data, drilling samples, and geophysical models. Verifying the mass calculations for each layer checks out—the ocean crust, mantle, and core all multiply correctly. Now I'm summing everything together to get Earth's total mass, which is dominated by the core and mantle contributions.
> 
> I'm noticing the calculation gives about 5.97e24 kg, which matches Earth's known mass, but I realize I haven't accounted for the continental crust properly. Looking at the existing continental land and subsurface masses, they only add up to around 7e20 kg, but the total continental crust should be closer to 2.6e22 kg—so there's a significant gap from missing layers like the continental shelf, deep subsurface, and lower crust that I need to factor in. The total comes to about 5.99e24 kg, which is slightly higher than the standard value of 5.97e24 — likely due to my simplified mantle geometry, but that's acceptable given the instruction allows for some variation. Now I'm moving on to format the strata_resources.tsv file, setting up each row with relative abundance, coefficient of variation, dispersion, and dispersion error for different Earth layers like the atmosphere, ocean, and ocean floor.
> 
> Generating resource table... Filling in the atmospheric composition data for Earth, with the key values being water at 0.25%, hydrogen at 3.5e-6%, oxygen at 23.1%, and nitrogen at 75.4%, along with trace amounts of carbon dioxide and other gases. Now I'm looking at the resource composition for Earth's oceans, mapping out the abundance and distribution parameters for various ore types, minerals, and organic compounds across different concentration levels. Now I'm looking at the atmospheric composition data and realizing I need to account for dissolved argon in the ocean. Let me calculate that conversion from milliliters per liter to the proper concentration units. Let me verify this with the equilibrium concentration instead—at around 13 micromoles per kilogram, that gives roughly 0.52 parts per million, which aligns better with what I'd expect from atmospheric equilibrium.
> 
> Now I'm checking the dissolved gas concentrations in seawater more broadly. Oxygen averages about 4.5 milliliters per liter, which converts to roughly 6 parts per million by mass. For nitrogen, the typical range is 9 to 14 milliliters per liter, so I'm working through the mass calculation for that as well.
> 
> For the carbon dioxide system, I need to account for the fact that most dissolved inorganic carbon exists as bicarbonate and carbonate ions rather than free CO₂. The total DIC concentration is around 2100 micromoles per kilogram in deep water, but only about 1% of that is actually dissolved CO₂ gas. Though if the ocean were to acidify significantly, most of that DIC could theoretically be released as CO₂.
> 
> For practical extraction purposes, I'll use the total DIC value since it represents what's actually available to convert. That gives me roughly 0.009 or about 0.01% for carbon dioxide.
> 
> Now I'm checking dissolved helium concentrations, which turn out to be extraordinarily small—around 6.6 to 7 parts per trillion by mass. It's so negligible that I'm wondering whether it's even worth including. I'm converting the dissolved noble gas concentrations to mass fractions—neon at 7 nanomolar gives about 1.4×10⁻⁷ g/L, which is still negligible compared to seawater density, so helium stays at zero but I should track the heavier noble gases like neon, krypton, and xenon since they have slightly higher concentrations. The simulation can handle very small numbers, so I should include these volatiles. I'm setting HEAVY_NOBLE_GASES to 5e-8 and HELIUM to 7e-10 with their respective extraction parameters. Now I'm considering dissolved hydrogen in the ocean—it's extremely dilute at around 1 nanoMolar in deep water, which works out to about 2e-10 as a fraction. I'm filling in the rest of the ocean composition with trace gases and setting negligible components to zero—argon, ammonia, and methane get small but non-zero values with their respective dispersions, while things like carbon monoxide, sulfur dioxide, and solid materials are essentially absent from the ocean.
> 
> The percentages add up to about 100%, which checks out. Now I'm preparing the final output with two data blocks: one for the strata table additions and one for the resources table, making sure to handle the territory fields correctly—leaving them blank for non-territorial Earth strata like the atmosphere and ocean, just like the existing format shows.
> 
> Let me verify the atmosphere area calculation using the radius of 6436 km: that gives me roughly 5.21 × 10⁸ km², and I'll apply the same approach to calculate the ocean surface area.
> 
> Now I'm working through the surface areas for each layer—ocean floor, mantle, and core—using their respective radii and the sphere surface area formula. Then I need to check the volume calculations, starting with the atmosphere by finding the difference between the outer and inner sphere volumes.
> 
> Computing the cubic values for the atmosphere layer... Now I'm calculating the mantle volume by finding the difference between the outer radius cubed and inner radius cubed, then applying the spherical volume formula to get approximately 8.97e11 km³.
> 
> For the core, I'm using the same approach with the core radius to get about 1.77e11 km³.
> 
> Both calculations check out, so I'm ready to compile the final output. I should stick with the six layers the user specifically requested—atmosphere, ocean, ocean floor, ocean crust, mantle, and core—and note which strata still need to be calculated. I'm also reconsidering some of my resource abundance percentages for the ocean floor, particularly the iron ore composition. Looking at the distribution of metalliferous sediments and Fe-Mn nodules across the ocean floor, they represent only a tiny fraction of total sediment mass—maybe 0.01-0.1% when accounting for their limited coverage and thin surface layers. This means my initial 3% allocation for iron ores is far too generous, since most iron is actually dispersed throughout clay rather than concentrated in extractable form, so I'm scaling that down to 0.1% and doing the same for aluminum. I'll need to compensate by increasing the regolith and stone categories to capture those clay sediments with their dispersed metal content.
> 
> Now I'm working through a revised composition for the ocean floor that better reflects this distribution, with regolith as the dominant component and ore deposits scaled down to their actual concentrations. Now I'm reconsidering the ocean floor composition—the Fe-Mn crusts are geographically widespread but represent such a thin layer that their contribution to the total mass in a 100-meter column is actually quite small, so 0.05% for iron ores seems more realistic than my earlier 3% estimate. INDUSTRIAL_METAL_ORES should capture manganese nodules since they're primarily manganese concentrations with economically valuable copper, nickel, and cobalt — even though they contain significant iron too, their composition aligns better with industrial metal ores than iron ores based on the reference definitions.
> 
> Now I'm adjusting the ocean floor resource values to be more consistent with continental subsurface benchmarks. Iron ores at 0.005 and industrial metal ores at 0.01 seem reasonable given the USA subsurface has 0.002 and 0.004 respectively, and I'm also factoring in rare earth element-bearing muds at 0.005.
> 
> Let me verify the complete ocean floor composition adds up correctly—stone at 12, regolith at 63, water at 20, industrial minerals at 3, organics and tholins at 1.5, plus the trace metals and gases, which totals to 99.8, so that's essentially balanced.
> 
> Actually, I should reconsider industrial minerals more carefully. On the ocean floor, the main deposits are phosphorite nodules on continental shelves, but the deep ocean floor has limited industrial mineral resources—mostly just dispersed zeolites in sediments. I'll reduce industrial minerals to 1% and bump regolith up to 65 to maintain the total around 100. I'm realizing the normalization will handle minor rounding discrepancies anyway, so I'll stick with these round numbers for the ocean floor composition rather than obsessing over decimal precision.
> 
> Now I'm reconsidering the ore concentrations in the oceanic crust itself—iron and aluminum percentages seem too high for basalt and gabbro, which are mostly just bulk rock. I need to think through what economically relevant mineral deposits actually form there, like volcanic massive sulfide deposits at mid-ocean ridges. Iron and aluminum are already accounted for within the stone component since they're bound in silicates and oxides, so those get zero as separate ore categories. I'm adding regolith from altered zones, trace amounts of rare earth and precious metals from the VMS systems, uranium, and organics, then verifying the percentages sum correctly.
> 
> Now I'm working through the mantle resources separately—the mantle is fundamentally stone, but there are some concentrated materials worth tracking: diamonds from deep mantle formation, chromite in peridotite layers, platinum group elements in sulfide phases, and water trapped in the rock structure.
> 
> Revising the mantle composition to account for these specifics: stone dominates at 99.6, with small allocations for water in the transition zone, industrial minerals like diamonds and spinels, sulfide phases, chromite and other metal concentrations, and carbon dioxide equivalent from diamond and carbonate phases.
> 
> Now I'm adding the remaining trace elements—platinum group elements in sulfide segregations, rare earth elements dispersed throughout silicates at much lower concentrations than initially estimated, and primordial helium. The sum comes to about 99.916, so I'm adjusting stone down to 99.7 to reach 100. Though I'm second-guessing the rare earth ore percentage—mantle REE concentrations are typically just a few parts per million, so 0.01% seems too high. REE. Total REE maybe ~10 ppm = 0.001%. But REE "ores" require concentration. In the mantle, REE aren't concentrated. Let me reduce to 0.001%.
> 
> Actually, looking at the resource definition: "Rare Earth Ores — Ref. blend of Earth bastnäsite-monazite flotation concentrate and lunar KREEP merrillite-apatite mineral separate. Total REE 28.5..." So 0.001% of the stratum would be ore with 28.5% REE, giving 28.5 × 0.001/100 = 2.85e-6 = ~3 ppm REE in the mantle. That's about right for total mantle REE.
> 
> The mantle's REE is dispersed throughout silicates rather than concentrated into exploitable deposits. I should set the dispersion parameter to near-zero for rare earth ores in the mantle — this reflects how the REE is essentially uniform across the rock, with no high-grade zones to mine. Though the mantle does have some heterogeneity from enriched domains with higher incompatible element concentrations, so I'll use very low dispersion like 0.1 to capture that slight variation while keeping it mostly homogeneous.
> 
> I'm overthinking this. Let me just finalize the resource table and move on. I'm setting all dispersion values to zero for the atmosphere since those resources are uniformly distributed there.
> 
> For the ocean, most resources are well-mixed with minimal dispersion, except for things like methane concentrated near hydrothermal vents and organic matter in specific zones. The ocean floor is where things get interesting—mineral resources cluster at specific locations like mid-ocean ridges and nodule fields, so they need higher dispersion values, while bulk materials like regolith and stone vary more moderately depending on whether you're near a ridge or in a barren area.
> 
> Now I'm looking at the ocean crust layer, which is mostly uniform basalt with low dispersion overall, though metalliferous deposits around hydrothermal systems show more variation. Now I'm assigning coefficient of variation values to capture our uncertainty about total abundances—well-documented reservoirs like the atmosphere get very tight estimates, while poorly constrained regions like the mantle and core warrant much higher uncertainty. The ocean and its floor fall somewhere in between, with dissolved components fairly well-known but ore concentrations much more speculative.
> 
> I'm ready to compile the final tables now, making sure the formatting and column alignment are precise. I'm also noticing that sulfate is a major dissolved component in seawater that should be included in the ocean inventory—
> 
> but since it's an oxidized form of sulfur rather than elemental sulfur, it belongs in the industrial minerals category rather than as a separate sulfur resource. Now I need to decide whether to include dissolved metal ores like iron and aluminum in the ocean data, or if those should remain zero since they're not economically viable resources in their current form.
> 
> Dissolved metals in seawater exist at trace concentrations—parts per billion to parts per trillion—which means they don't constitute actual ore deposits. These trace elements are already accounted for within the industrial minerals category as dissolved constituents. The same applies to the atmosphere, where mineral dust particles are negligible by mass. So all ore resources remain zero for both the ocean and atmosphere.
> 
> Now I'm ready to compile the final output tables and cross-check my values for Earth's core against the Moon's core to ensure consistency.
> 
> Generating the data tables... I'm realizing the core composition needs to account for dissolved oxygen in the iron, which would combine with silicon to form silicate slag during extraction. Since the core has roughly 6% silicon and 5% oxygen, these light elements together represent about 11% that would form stone-like compounds, but I need to be careful about how this translates to the STONE reference weight rather than just adding the raw percentages.
> 
> Let me finalize the composition by checking my totals: if IRON_NICKEL is 85%, OXYGEN is 5%, STONE is 7%, and MAGNESIUM_OXIDE is 2%, that leaves 0.5% unaccounted for. I could either bump IRON_NICKEL to 85.5% or consider whether the carbon content in the core should be represented as a separate category like carbon dioxide or carbon monoxide during processing. I'm realizing the carbon calculations are getting too complicated, so I'll just incorporate it into the stone category to keep things simple. Now I'm setting up the final core composition with iron-nickel as the dominant component, followed by sulfur, stone, industrial metal ores, and hydrogen, each with their associated variance parameters.
> 
> The percentages add up correctly, so I'm ready to format the output as TSV tables with the proper column structure for the strata resources data. Continuing through the atmospheric composition data, filling in the abundance values for each element—water makes up a quarter percent, hydrogen is trace amounts, oxygen dominates at over 23%, nitrogen is the bulk at 75%, and carbon dioxide and other gases are minimal. Now I'm looking at the resource composition data for Earth's ocean, mapping out the abundance and distribution parameters for various material types from industrial metals through to water content. Now I'm looking at the composition data for ocean floor materials, including various ore types with their respective concentration and property values. I'm looking at the rest of the resource distribution table for atmospheric and gaseous compounds, then realizing I need to recalculate the totals since I adjusted the regolith value to 56. I'm getting 91.85%, which is way too low—I must have made an error when I reduced those ore values without adjusting the filler amounts to compensate. Let me start fresh and recalculate the composition properly, accounting for the main components like stone, water, and organics, then figure out what should go into the remaining slots.
> 
> Once I add up all the specific components, I get about 35.86%, which means REGOLITH needs to fill the gap at around 64% to reach 100. That checks out. Now I'm moving on to Earth's ocean crust composition, which has a much simpler breakdown of stone, industrial minerals, and water. Now I'm adjusting the composition for Earth's mantle, adding water and various minerals while keeping the total near 100, then moving on to define the core with iron-nickel as the dominant component along with sulfur and stone.
> 
> I'm reconsidering the ocean layer's dispersion values—water should have zero dispersion since oceans are essentially uniform saltwater bodies, but I'm keeping a small nonzero value for industrial minerals to account for salinity gradients in coastal areas and estuaries, even though the main ocean body is well-mixed. Sulfur compounds in the ocean are already accounted for in the industrial minerals category, and elemental sulfur near hydrothermal vents is negligible, so I'll set sulfur to zero across all columns. For the strata file, the ocean's inner radius should be 6367.31 kilometers—that's Earth's radius minus the average ocean depth of 3.69 kilometers, which applies across all ocean areas including continental shelves.
> 
> Let me verify this makes sense by checking the volume and mass: with an ocean surface area of 3.61×10⁸ km² and average depth of 3.69 km, I get roughly 1.33×10⁹ km³, which matches the known ocean volume within a couple percent. The corresponding mass works out to about 1.37×10²¹ kilograms, very close to the accepted value of 1.385×10²¹ kilograms.
> 
> As another check, the dissolved salt content should be around 3.5% of the total ocean mass, giving me about 4.76×10¹⁹ kilograms, which aligns well with the known total of roughly 4.9×10¹⁹ kilograms.
> 
> Now I'm thinking through the survey_level calibration more carefully—how to assign confidence levels based on the type and quality of data available for each region, from purely theoretical estimates down to direct observations. These values feel consistent with what we know about Earth's accessibility and sampling depth. The atmosphere and ocean are well-monitored globally, the ocean floor has limited direct sampling similar to lunar crust knowledge, and the deeper layers follow the same pattern as before. Now I'm formatting this into the TSV data structure, being careful with the tab separations.
> 
> Generating the data table... I need to verify the territory field is blank for Earth strata, matching the Moon data format with consecutive tabs. Also recalculating the ocean floor inner radius—if the seafloor sits at 6367 km and I need 100 m below that, the inner radius should be 6366.9 km.
> 
> Now I'm double-checking the area fractions: ocean floor coverage is about 361 million km² out of Earth's total 510 million km², which gives 0.708. I'm ready to compile the final output with both the strata.tsv and strata_resources.tsv rows, showing my calculations and reasoning for the density values I've chosen. Let me verify the composition breakdown one more time. The deep ocean floor sediments vary by region—calcareous ooze dominates about 60% of the area, red clay covers roughly a quarter, siliceous ooze takes up 15%, with exposed basalt making up the remainder—and all of these are water-saturated with around 50% porosity. Working through the mass fractions with that 1.8 g/cm³ bulk density, the pore water content comes out to roughly 28% by mass when accounting for seawater density versus the grain material density.
> 
> For the solid fraction, I'm mapping out how the remaining 71.5% breaks down across the different sediment types and minerals, then translating that into resource categories: water at 28%, regolith materials like clay and biogenic sediment at 45%, stone including basalt at 8%, and industrial minerals like the calcium carbonate from calcareous ooze at 15%.
> 
> Now I'm reconsidering where calcareous ooze actually fits—it's calcium carbonate, which is fundamentally different from silicate rock. Looking at the resource definitions, it doesn't match STONE (which is silicate-based) or the basaltic regolith reference, but it clearly fits INDUSTRIAL_MINERALS since it's used in construction and as a chemical feedstock. This means I need to recalculate the resource distribution with CaCO₃ moved to industrial minerals instead.
> 
> If calcium carbonate comprises about 30% of the ocean floor solids and solids make up roughly 72% of total mass, that puts industrial minerals at around 22% of the total—a significant portion. The real tension here is that the simulation treats all industrial minerals as interchangeable, but ocean floor chalk is compositionally distinct from terrestrial phosphate rock or gypsum. I'm stuck between putting it in industrial minerals (which misrepresents its composition) or regolith (which would incorrectly tag it as basaltic fines), and I need to decide which misrepresentation is more acceptable for the model.
> 
> Looking at the reference compositions, industrial minerals have 13.5% calcium while pure calcium carbonate has 40%, so neither category is a perfect fit. I think splitting the materials makes the most sense—putting the calcium carbonate in industrial minerals since it has economic potential, and keeping the clays in regolith.
> 
> Now I'm reconsidering the percentages. Twenty-two percent for industrial minerals is higher than what we see on continental land, but ocean floors are fundamentally different because of all the biogenic calcium carbonate accumulation. So I'm settling on a distribution that reflects this: water at 28%, regolith at 35% for clays and terrigenous sediments, industrial minerals at 20% for the calcium carbonate oozes and phosphorites, stone at 8% for basalt and lithified rock, with organics, sulfur, and metal ores making up the remaining small fractions.
> 
> But now I'm checking the math—those percentages only add up to about 93.8%, which leaves a gap. I'm also realizing that the regolith definition itself includes calcium at 7%, silicon at 22%, and aluminum at 7%, which is actually pretty similar to deep sea clay compositions. The distinction between what counts as regolith versus industrial minerals is getting blurry, especially since calcareous ooze is fundamentally different from basaltic regolith.
> 
> Let me reorganize this more systematically: clay-rich sediments map best to regolith, calcium carbonate deposits belong in industrial minerals as an economic resource, silica-rich oozes could go either way, and basalt stays as stone. So I'm proposing a cleaner allocation where regolith captures the clays and siliceous material, industrial minerals gets the calcareous and phosphate deposits, stone accounts for the basalt, and organics-tholins stays at 2.5.
> 
> When I add these up—water at 28, regolith at 30, industrial minerals at 25, stone at 8, organics-tholins at 2.5, plus sulfur and trace iron—I'm getting around 93.8, which leaves about 6 percent unaccounted for. I'm wondering if my porosity assumption is pushing the water content too high, since surface sediments can be 70-80% porous but deeper layers compress to 40-50%, so my 50% average might be slightly inflated. I should also reconsider the organic matter—ocean sediments typically contain 0.1-2% organic carbon, so averaging around 0.5% carbon would give roughly 1% total organic matter by mass, which translates to about 0.72% when accounting for the solid fraction.
> 
> Let me adjust the composition: water down to 25 to reflect a more conservative porosity estimate, regolith at 33 for clays and siliceous material, industrial minerals at 25 for carbonate ooze and phosphorites, stone at 10 for basalt and hard substrate, organics-tholins at 1, and iron ores at 0.5 for the metallliferous sediment layer. Though I'm second-guessing whether iron ores should capture all iron-rich sediment or just the truly concentrated deposits—the resource definition suggests ores are specifically the enriched masses, not background iron throughout the sediment column.
> 
> Now I'm calculating what metallliferous sediment actually contributes: if it covers about 5% of the ocean floor at roughly 10 meters thick with a density around 1.5, that works out to about 0.42% of the total ocean floor mass. That's actually pretty significant, but I need to decide whether this qualifies as "iron ore" in the simulation's sense—if it's being extracted for its iron content, it probably does count. I'll settle on 0.3% for iron ores to be conservative.
> 
> Now I'm laying out the remaining resource categories with their percentages and checking that everything adds up properly. I'm getting close to 94.5% total, which means I'm still short of 100%. I'm finalizing the ocean floor composition with these percentages, but I'm second-guessing whether 24% industrial minerals is realistic for an ocean floor—that seems quite high.
> 
> Looking at this more carefully, calcareous ooze on the deep seafloor isn't actually mined or used industrially the way shallow-water limestone is, so it probably shouldn't count as an industrial mineral at all. It's really just sediment. I should reclassify it as regolith along with the other fine materials, and reserve industrial minerals for things that are actually economically valuable like phosphorites and zeolites. That would shift the composition significantly—regolith becomes the dominant component at around 60%, while industrial minerals drops to a much smaller percentage.
> 
> Let me adjust the numbers to make sure they add up properly. If I set regolith at 61% to account for all the unconsolidated sediment, then the total comes to roughly 100% when I include water, stone, industrial minerals, organics, and the trace elements. This actually mirrors the continental pattern better, just with the proportions flipped—on land, stone dominates and regolith fills in the gaps, but on the ocean floor, it's the reverse. Now I'm adding the trace atmospheric components and verifying the total comes to nearly 100%, which confirms the distribution is balanced. I've settled on the final resource breakdown for the ocean floor. Now I'm working through the porosity calculation for the ocean floor—checking that a bulk density of 1.8 with roughly 26% water by mass makes physical sense given typical sediment grain densities around 2.5. The math checks out, and since the code normalizes the composition anyway, this 26% water content is close enough.
> 
> I've calculated the total mass of the ocean floor at about 6.05×10^19 kg, but I'm realizing the revised density of 1.8 instead of 2.0 means I need to recalculate the inner radius for the EARTH_OCEAN_FLOOR layer to keep the mass consistent. 3.36e22 cm³? Let me verify:
> 1 km³ = (1000 m)³ = 1e9 m³ = (1e5 cm)³ = 1e15 cm³. ✓
> 3.36e7 km³ = 3.36e7 × 1e15 cm³ = 3.36e22 cm³
> mass = 3.36e22 × 1.8 = 6.05e22 g = 6.05e19 kg. ✓
> 
> Everything checks out. Now I need to prepare the output with clear derivations, the new strata.tsv and strata_resources.tsv rows, and notes on simplifications. But I'm reconsidering the dissolved nitrogen in ocean floor pore water—my initial estimate of 0.002% seems too high. Seawater has about 12 mL(STP)/L of dissolved N₂, which translates to roughly 15 mg/L or 0.0015% by mass. Since pore water comprises about 26% of the ocean floor, that gives roughly 0.0004% for dissolved N₂ in pore water, so I should revise down to around 0.0005%. The actual concentration in deep-sea pore water could vary depending on bacterial consumption and decomposition processes, but this is a more defensible estimate.
> 
> For ammonia, anoxic marine sediments can accumulate 1-10 mM of NH₃/NH₄⁺, and averaging over 100 meters gives roughly 1 mM or 17 mg/L. Accounting for pore water fraction, that's about 0.00043%, so I'll include ammonia as a trace component at 0.0005%.
> 
> At this point I'm getting into extremely fine details that will be normalized away anyway, so I should finalize the composition and move forward with formatting the output properly. I need to be careful about column widths in the TSV files.
> 
> Now I'm double-checking the ocean crust resource values to make sure they're reasonable — iron and aluminum are locked in silicate minerals rather than concentrated ores, so those stay at zero, while sulfur is present at 0.5% and the various metal ores have their trace concentrations. I'm reconsidering whether to count the carbonate veins as CO₂ or keep them grouped with industrial minerals—since they're solid carbonates in the crust rather than gaseous CO₂, I'll leave them out of the gas category and let the industrial minerals percentage capture them. Now I'm finalizing the composition breakdown with stone as the dominant component, followed by industrial minerals and water, with smaller amounts of sulfur, regolith, metal ores, and organics.

## Derivation

### EARTH_ATMOSPHERE

The atmosphere is defined as 65 km thick above the 6,371 km reference radius.

Outer radius 6,436 km. Full spherical shell (area_fraction = 1). Area at outer boundary: \(4\pi \times 6436^2 = 5.21 \times 10^8\) km². Volume: \(\frac{4}{3}\pi(6436^3 - 6371^3) = 3.35 \times 10^{10}\) km³. Total atmospheric mass is well established at 5.15e18 kg. Mean density = 5.15e18 kg / 3.35e25 cm³ = 1.54e−7 g/cm³ (expected to be tiny given the 65 km volume definition). Survey level 10 (calibration reference).

**Composition by mass:** N₂ 75.4%, O₂ 23.1%, Ar 1.29%, H₂O ~0.25% (variable global average; ~1.27e16 kg vapor), CO₂ 0.064% (~420 ppmv → ~640 ppmw, as of mid-2020s), He 7.2e−5% (5.24 ppmv × 4/28.97), CH₄ ~1e−4% (~1.9 ppmv × 16/28.97), heavy noble gases (Ne+Kr+Xe) 0.0016% (~16 ppm by mass per CRC Handbook), H₂ ~3.5e−6%, CO ~1e−5%, SO₂ ~1e−8% (variable, volcanic). All components well-mixed → dispersion = 0. Very low abundance CV for major species; moderate CV for variable species (H₂O, CO₂).

### EARTH_OCEAN

Defined as all ocean water including over continental shelves: ~3.61e8 km² (70.8% of surface), mean depth 3.69 km (yielding volume ~1.33e9 km³, consistent with the accepted ~1.335e9 km³). Mean seawater density 1.025 g/cm³. Mass: 1.36e21 kg (actual ~1.385e21 kg; 2% difference from simplified geometry). Survey level 8.0 — bulk chemistry is extremely well characterized from decades of oceanographic surveys (WOCE, GO-SHIP, Argo floats), though trace-element spatial resolution has gaps.

**Composition:** H₂O ~96.5% (seawater salinity ~35 g/kg → ~3.5% dissolved salts). Dissolved salts (NaCl, MgSO₄, CaCl₂, KCl, NaHCO₃, etc.) assigned to INDUSTRIAL_MINERALS at 3.5%. Dissolved gases from Henry's law equilibrium and deep-water measurements: O₂ avg ~6 mg/L → 6e−4 wt%; N₂ avg ~15 mg/L → 1.5e−3 wt%; total DIC ~2100 µmol/kg → ~0.009 wt% as CO₂ equivalent (includes bicarbonate/carbonate extractable by acidification); Ar ~5e−5 wt% (~13 µmol/kg × 40 g/mol); dissolved organic carbon ~5e−5 wt%; dissolved CH₄ ~1e−6 wt% (concentrated near vents/seeps → nonzero dispersion); NH₃/NH₄⁺ ~1e−5 wt%; He ~7e−10 wt% (1.8 nM). Ocean is well-mixed so dispersion ≈ 0 for major species, small nonzero for heterogeneously sourced traces (CH₄, organics).

### EARTH_OCEAN_FLOOR

Defined as the top 100 m below the deep-ocean seafloor, excluding continental shelf (area ~336 million km², area_fraction = 0.659). Average deep-ocean depth ~4.0 km → seafloor at r ≈ 6,367 km, inner boundary at 6,366.9 km. Volume: 3.36e8 × 0.1 = 3.36e7 km³.

**Density reasoning:** Deep-sea sediment porosity averages ~50% over the top 100 m (70–80% near surface, ~40% at 100 m depth). With grain density ~2.5 g/cm³ and seawater ~1.025: bulk density ≈ 0.5 × 1.025 + 0.5 × 2.5 ≈ 1.8 g/cm³. Areas with thin sediment expose basalt (~2.9 g/cm³), raising the average slightly. I use 1.8 g/cm³ as the area-weighted mean. Mass: 6.05e19 kg. Survey level 2.0 — comparable to lunar crust knowledge; DSDP/ODP/IODP have drilled hundreds of sites but the vast majority of the deep ocean floor is unsampled at depth.

**Composition:** The deep-ocean floor is predominantly unconsolidated pelagic sediment (calcareous ooze ~48%, red clay ~38%, siliceous ooze ~14% by area). Basalt is exposed at mid-ocean ridges and areas with thin sediment cover (~15–20% of area). I assign the calcareous ooze fraction to REGOLITH (it is "surface fines and debris" even though compositionally different from basaltic regolith — an acknowledged simulation simplification). Phosphorites, zeolites, and authigenic minerals go into INDUSTRIAL_MINERALS. Fe-Mn metalliferous sediment (enriched zones near ridges, covering ~5% of area with ~10 m thickness) → IRON_ORES at ~0.3%. Mn nodules (~7.5e13 kg total), polymetallic sulfides (~1e12 kg), cobalt crusts (~1e13 kg) → INDUSTRIAL_METAL_ORES at 0.01% with high dispersion. REE-bearing muds (Pacific concentrations, estimated ~1e15 kg of enriched material) → RARE_EARTH_ORES at 0.005%. Pore water ≈ 26% by mass. Organic matter ≈ 1.5%.

### EARTH_OCEAN_CRUST

100 m below deep-ocean seafloor to the Moho (~7 km below seafloor on average), same area as ocean floor. Thickness 6.9 km. Volume: 3.36e8 × 6.9 = 2.32e9 km³. Density: 2.9 g/cm³ (standard oceanic crust — upper basalt + lower gabbro, from seismic layer 2+3 velocities). Mass: 6.73e21 kg. Survey level 1.0 — we understand the general stratigraphy from ophiolites (Oman, Cyprus, Troodos) and ~20 deep DSDP/ODP/IODP holes, but direct sampling is extremely sparse relative to the area.

**Composition:** Oceanic crust is overwhelmingly basalt and gabbro → STONE ≈ 95.6%. Hydrothermal alteration introduces water (greenschist/amphibolite-facies minerals, ~1–2 wt% H₂O in altered basalt) → WATER 1.5%. Epidote, prehnite, zeolite, clay alteration products → INDUSTRIAL_MINERALS 2%. Sulfide minerals (pyrite, chalcopyrite, pyrrhotite) → SULFUR 0.5%. VMS (volcanogenic massive sulfide) deposits concentrated at ridges → INDUSTRIAL_METAL_ORES 0.05% with high dispersion (1.8). Brecciated fault zones → REGOLITH 0.3%. Trace Au-Ag in VMS systems → PRECIOUS_METAL_ORES 5e−4%.

### EARTH_MANTLE

Moho to core-mantle boundary (CMB) at 2,890 km depth (r = 3,481 km). The Moho depth varies: ~7 km below deep-ocean seafloor (over 65.9% of surface), ~25 km under continental shelf (4.9%), ~35 km under land (29.2%). Area-weighted mean Moho radius: 0.659 × 6360 + 0.049 × 6346 + 0.292 × 6336 ≈ 6,352 km. Thickness: 2,871 km. Volume: \(\frac{4}{3}\pi(6352^3 - 3481^3) = 8.97 \times 10^{11}\) km³. Mass: 4.01e24 kg (standard value from Dziewonski & Anderson 1981 PREM). Density: 4.47 g/cm³ (cross-check: ranges from ~3.3 at top to ~5.6 at base). Survey level 0.5 — seismic tomography gives velocity structure, xenoliths from kimberlite pipes sample the upper ~200 km, exposed ophiolite mantle sections constrain shallow composition, but the deep lower mantle is inferred only from models.

**Composition:** The mantle IS stone by definition (the "stone" reference is primitive mantle pyrolite). STONE ≈ 99.7%. Water: the transition zone (410–660 km, wadsleyite + ringwoodite) may hold 0.1–1 wt% H₂O in crystal structure (~1–3 ocean masses); upper and lower mantle contain ~0.01–0.05%; bulk average ~0.1%. Diamonds + garnet + spinel concentrations → INDUSTRIAL_MINERALS 0.08%. Sulfide melt pockets (~200–250 ppm S in bulk mantle, McDonough & Sun 1995) → SULFUR 0.025%. Chromite pods + metal concentrations → INDUSTRIAL_METAL_ORES 0.04%. Carbon (~100–300 ppm C as carbonate/diamond/dissolved; Dasgupta & Hirschmann 2010) → CARBON_DIOXIDE 0.03% equivalent. PGE in sulfide segregations → PRECIOUS_METAL_ORES 5e−4%. Dispersed REE → RARE_EARTH_ORES 0.001%. Primordial + radiogenic He → HELIUM 1e−6%.

### EARTH_CORE

Center to CMB, radius 3,481 km. Volume: \(\frac{4}{3}\pi \times 3481^3 = 1.77 \times 10^{11}\) km³. Mass: ~1.95e24 kg (residual from Earth total 5.97e24 minus mantle 4.01e24 minus crust ~2.6e22 minus ocean/atmosphere). Density: 11.0 g/cm³ (outer core ~10–12, inner core ~13; Dziewonski & Anderson 1981). Survey level 0.3 — seismic P and S wave velocities constrain density and state (liquid outer, solid inner), and cosmochemical models (CI chondrite depletion patterns) constrain bulk composition, but light element partitioning remains debated.

**Composition:** Fe ~85%, Ni ~5% → IRON_NICKEL 85% (the reference Fe-Ni alloy composition captures this well). Light elements: Si ~6%, O ~2–5%, S ~1.9%, C ~0.2%, H ~0.06% (Hirose et al. 2013; Badro et al. 2015). Si + O would form silicate/oxide slag during processing → STONE 7.5%. Sulfur → SULFUR 5%. Siderophile trace elements (Co ~0.25%, Cr, P, W) → INDUSTRIAL_METAL_ORES 2%. Dissolved H → HYDROGEN 0.5%.

---

## New strata.tsv rows

```tsv
EARTH_ATMOSPHERE	EARTH	6371		ATMOSPHERE	6371	65	1	5.21e8	3.35e10	1.54e-7	5.15e18	10	Near-perfect knowledge; routine global atmospheric monitoring
EARTH_OCEAN	EARTH	6371		OCEAN	6367.31	3.69	0.708	3.61e8	1.33e9	1.025	1.36e21	8.0	Well-characterized bulk chemistry; extensive oceanographic data
EARTH_OCEAN_FLOOR	EARTH	6371		OCEAN_FLOOR	6366.9	0.1	0.659	3.36e8	3.36e7	1.8	6.05e19	2.0	DSDP/ODP/IODP sampling; vast areas unsampled
EARTH_OCEAN_CRUST	EARTH	6371		OCEAN_CRUST	6360	6.9	0.659	3.36e8	2.32e9	2.9	6.73e21	1.0	Inferred from ophiolites and sparse deep drilling; seismic constraints
EARTH_MANTLE	EARTH	6371		MANTLE	3481	2871	1	5.07e8	8.97e11	4.47	4.01e24	0.5	Seismic tomography and gravity models; xenolith constraints only
EARTH_CORE	EARTH	6371		CORE	0	3481	1	1.52e8	1.77e11	11.0	1.95e24	0.3	Seismic + geochemical/cosmochemical models; no direct sampling
```

## New strata_resources.tsv rows

```tsv
EARTH_ATMOSPHERE	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.25,0.15,0,0	3.5e-6,0.2,0,0	23.1,0.001,0,0	75.4,0.001,0,0	0.064,0.03,0,0	1e-5,0.2,0,0	0,0,0,0	1e-4,0.05,0,0	0,0,0,0	1e-8,0.5,0,0	7.2e-5,0.02,0,0	1.29,0.002,0,0	0.0016,0.02,0,0
EARTH_OCEAN	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	3.5,0.02,0.05,0.02	5e-5,0.3,0.15,0.08	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	96.5,0.005,0,0	0,0,0,0	5e-4,0.1,0.15,0.08	1.5e-3,0.1,0.1,0.05	0.009,0.1,0.1,0.05	0,0,0,0	1e-5,0.3,0.2,0.1	1e-6,0.5,0.8,0.4	0,0,0,0	0,0,0,0	7e-10,0.15,0.05,0.02	5e-5,0.1,0.05,0.02	5e-8,0.15,0.05,0.02
EARTH_OCEAN_FLOOR	0,0,0,0	0.3,0.4,1.2,0.4	0.01,0.5,0.8,0.3	0.01,0.5,1.8,0.5	5e-5,0.7,2.5,0.7	0.005,0.6,2.0,0.6	1e-4,0.5,1.5,0.5	0.15,0.3,0.6,0.25	3,0.3,0.8,0.3	1.5,0.25,0.8,0.3	0,0,0,0	0,0,0,0	8,0.2,0.5,0.2	61,0.15,0.2,0.1	26,0.15,0.15,0.08	1e-5,0.5,1.0,0.5	0,0,0,0	5e-4,0.3,0.3,0.15	0.01,0.3,0.3,0.15	0,0,0,0	5e-4,0.4,0.5,0.25	0.005,0.5,2.0,0.6	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
EARTH_OCEAN_CRUST	0,0,0,0	0,0,0,0	0,0,0,0	0.05,0.5,1.8,0.6	5e-4,0.7,2.0,0.6	0.005,0.5,0.8,0.4	1e-4,0.6,0.8,0.4	0.5,0.3,0.5,0.2	2,0.3,0.3,0.15	0.01,0.5,1.0,0.5	0,0,0,0	0,0,0,0	95.6,0.1,0.1,0.05	0.3,0.3,0.5,0.2	1.5,0.3,0.3,0.15	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
EARTH_MANTLE	0,0,0,0	0,0,0,0	0,0,0,0	0.04,0.5,0.3,0.15	5e-4,0.6,0.3,0.15	0.001,0.5,0.15,0.08	0,0,0,0	0.025,0.3,0.2,0.1	0.08,0.5,0.3,0.15	0,0,0,0	0,0,0,0	0,0,0,0	99.7,0.03,0.05,0.02	0,0,0,0	0.1,0.5,0.3,0.15	0,0,0,0	0,0,0,0	0,0,0,0	0.03,0.5,0.3,0.15	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	1e-6,0.5,0.2,0.1	0,0,0,0	0,0,0,0
EARTH_CORE	85,0.1,0.05,0.02	0,0,0,0	0,0,0,0	2,0.4,0.1,0.05	0,0,0,0	0,0,0,0	0,0,0,0	5,0.3,0.1,0.05	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	7.5,0.3,0.1,0.05	0,0,0,0	0,0,0,0	0.5,0.5,0.1,0.05	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
```

## Key simplifications and omissions

**Simplifications in this batch:**

The ocean floor's calcareous and siliceous ooze is lumped into REGOLITH despite being compositionally quite different from the basaltic regolith reference. This is the most significant simplification here — calcareous ooze is ~48% of the deep-ocean floor by area and is essentially chalk (CaCO₃), while the REGOLITH reference is basaltic. The simulation's fungibility assumption absorbs this. A future refinement could split ocean-floor sediment types into REGOLITH (siliceous and terrigenous) vs. INDUSTRIAL_MINERALS (calcareous), but this would push INDUSTRIAL_MINERALS to ~20% for the ocean floor, which seemed inconsistent with the way continental strata use INDUSTRIAL_MINERALS for specifically economic mineral concentrations rather than bulk sediment.

The ocean crust boundary (inner_r = 6,360 km) uses the deep-ocean Moho depth (~7 km below seafloor at ~4 km water depth), ignoring the variable crustal thickness at fracture zones, plateaus, and ridge flanks. The adjacent mantle outer boundary (r = 6,352 km) uses an area-weighted Moho, so these don't match — as expected per the strata.md note 2.

The mantle is treated as a single homogeneous stratum, which merges the compositionally and rheologically distinct upper mantle, transition zone, and lower mantle. The transition zone's potentially enormous water budget (~0.1–1 wt% in wadsleyite/ringwoodite) is averaged over the full mantle depth, diluting it to 0.1%. Diamond is assigned to INDUSTRIAL_MINERALS rather than getting its own category.

Core light-element partitioning (Si, O, S, C, H) remains debated. My composition follows Hirose et al. (2013) and Badro et al. (2015), favoring Si+O as the main light elements with moderate S. Other models prefer S-rich or O-rich cores; uncertainties are captured in the CV terms.

**Feedback for prompt/context improvement:**

The classification of ocean-floor calcareous ooze exposes a tension in the resource taxonomy: REGOLITH (basaltic reference) vs. INDUSTRIAL_MINERALS (economic minerals reference) vs. STONE (silicate rock reference) don't offer a natural home for massive biogenic CaCO₃ deposits. You might consider whether a note in extractable_resources.md should explicitly address how to handle compositionally anomalous filler material (e.g., "assign to the closest resource category by extraction context; the normalization step and downstream refining model handle compositional differences").

The strata.md description of the ocean floor ("Manganese nodules, polymetallic sulfides, cobalt-rich crusts, REE-bearing muds") emphasizes the economically interesting minority of the mass. A sentence noting the dominant sedimentary composition would help calibrate resource entries.

There are still several Earth strata missing from the tables: Continental Ice Bodies, Continental Water Bodies, Continental Shelf, Continental Near-Surface, Continental Deep Subsurface, Continental Extreme Subsurface, Continental Ultra Subsurface, and Continental Lower Crust. These are needed to complete the Earth mass budget (the existing continental land + subsurface strata account for only ~7e20 kg of the ~2.6e22 kg total continental crust). I can generate these in a follow-up.
