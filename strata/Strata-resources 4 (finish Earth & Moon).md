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

* Add missing Earth strata.

Also provide feedback so I can improve the prompt and/or context files for subsequent analyses.

---
---

*Thinking...*

> The user wants me to add missing Earth strata to both `strata.tsv` and `strata_resources.tsv`. Let me first identify what Earth strata are described in `strata.md` but not yet present in the tables.
> 
> From strata.md, Earth strata include:
> 1. Atmosphere ✓ (EARTH_ATMOSPHERE)
> 2. Continental Ice Bodies ✗
> 3. Continental Water Bodies ✗
> 4. Continental Land (8 territories) ✓ (all 8 present)
> 5. Continental Shelf ✗ (8 territories)
> 6. Continental Near-Surface ✗ (8 territories)
> 
> 
> 7. Continental Subsurface ✓ (all 8 present)
> 8. Continental Deep Subsurface ✗ (8 territories)
> 9. Continental Extreme Subsurface ✗ (8 territories)
> 10. Continental Ultra Subsurface ✗ (8 territories)
> 11. Continental Lower Crust ✗ (8 territories)
> 12. Ocean ✓ (EARTH_OCEAN)
> 13. Ocean Floor ✓ (EARTH_OCEAN_FLOOR)
> 14. Ocean Crust ✓ (EARTH_OCEAN_CRUST)
> 15. Mantle ✓ (EARTH_MANTLE)
> 16. Core ✓ (EARTH_CORE)
> 
> So I'm looking at what's missing: Continental Ice Bodies and Water Bodies as non-territorial single strata, plus six territorial strata with 8 territories each for the subsurface layers and lower crust. That comes to 50 new rows total. But I need to verify whether the ice and water bodies are actually territorial or not by checking the strata documentation. I'm wrestling with whether Ice Bodies and Water Bodies should follow the same 8-region subdivision as other continental strata, or if they're special cases that warrant different treatment. The note explicitly says each continental stratum gets subdivided, but ice is concentrated in Antarctica and Greenland while fresh water is more globally distributed, which makes me think they might not fit neatly into the territorial framework. are continental strata. The user's existing tables show territorial subdivisions for "continental land" and "continental subsurface". The description in strata.md says "Each Earth 'continental' stratum" - so if these are classified as continental, they should be subdivided. But Ice Bodies and Water Bodies are somewhat different in character - they're above-ground deposits rather than geological strata.
> 
> I think the practical approach is to follow the user's implicit convention. Looking at the existing tables, atmosphere and ocean are non-territorial. Ice bodies and water bodies seem similar in nature as above-ground deposits, so I'll treat them as non-territorial single strata.
> 
> For the deeper continental layers though, each one needs to be subdivided across the eight territories. So I'm looking at shelf, near-surface, deep subsurface, extreme subsurface, ultra subsurface, and lower crust - that's six layers times eight territories, plus the two non-territorial ice and water bodies. The total count comes to 50 strata. I'm realizing that ice and water bodies are surface features sitting atop the continental land, not subdivisions of it—so they should remain as single non-territorial strata while the deeper layers get divided into 8 territories. Now I'm working through the specific calculations for the ice bodies layer, starting with the area and thickness from the reference data. For the density, I need to account for the fact that ice sheets contain firn and dust in the upper layers, making them less dense than pure ice. The Antarctic ice sheet averages around 0.917 g/cm³ for solid ice, but considering the whole column with firn, something closer to 0.90 g/cm³ is more realistic. Now I'm converting the volume and density to calculate the total mass, being careful with the unit conversions from km³ and g/cm³. I'm reconsidering the survey level for ice sheets—since we know the composition is essentially pure water ice, I'd bump that up to around 6.0, though the detailed underground structure remains less certain. Now moving on to continental water bodies, which cover about 2.1 million km² at roughly 46 meters thick, so I'm calculating the volume from those dimensions.
> 
> The volume works out to about 96,600 km³, which aligns well with known freshwater lake volumes plus reservoir estimates. With a density of about 1.0 g/cm³ for fresh water, that gives a mass around 9.66×10¹⁶ kg, and since these are well-documented systems, I'm setting the survey level at 7.0.
> 
> Now I'm looking at the continental shelf data—about 25 million km² of seafloor extending to 100 meters below the surface, which is technically part of the continental system rather than the open ocean floor.
> 
> I need to distribute this shelf area across 8 territories, so I'm working through the math: the shelf represents roughly 4.9% of Earth's total surface. For the US specifically, the continental shelf extends to about 200 meters depth and covers roughly 1.4 million km², though separating this from the broader EEZ is tricky. Rather than trying to parse exact boundaries, I should estimate the shelf distribution across all territories more systematically. Now I'm converting these continental shelf areas into fractions of Earth's total surface area to get a sense of their relative significance globally.
> 
> For the actual mass calculations, I need to account for the sediment layer beneath the seafloor—roughly 100 meters thick with a density similar to continental crust around 2.0 g/cm³. I'm working through the volume and mass for each territory's continental shelf by multiplying area by thickness and then by density, converting units appropriately as I go. Now I'm compiling the continental shelf data for the remaining regions—Japan, Antarctica, and a catch-all category for other shelves—then moving into the near-surface continental layer that spans both land and shelf areas across about 174 million square kilometers with an average thickness of 0.4 kilometers.
> 
> I need to position this layer correctly in the radial structure: it sits between the surface land layer (ending at 100 meters) and the deeper subsurface (starting at 500 meters), so the inner radius is 6370.5 km and outer radius is 6370.9 km.
> 
> Now I'm calculating the area fractions for each territory by combining their land and continental shelf areas, then dividing by the total combined area of about 510 million km².
> 
> Wait, I'm noticing something odd—the existing subsurface entries already seem to include shelf area in their calculations, so the shelf component I'm deriving doesn't quite match up with what's already in the data.
> 
> Let me use those existing subsurface area fractions as my baseline since they already represent the combined land plus shelf areas. I'm looking at the breakdown across regions—USA, China, EU, Russia, India, Japan, Antarctica, and others—and realizing that Continental Near-Surface should mirror the same land plus shelf coverage as Continental Subsurface. cover land+shelf, just at different depths). So I should use the same area fractions.
> 
> Wait, but the Continental Shelf has its OWN entry as a 0-100m stratum. And Continental Land is also 0-100m. Then Continental Near-Surface is 100-500m covering both land AND shelf footprints. And Continental Subsurface is 500-2000m covering both.
> 
> So the area for Near-Surface and all deeper layers = land area + shelf area = same as existing subsurface entries.
> 
> Let me verify the numbers: the subsurface area fractions sum to 0.340, while the land fractions sum to 0.292. That means shelf area fractions total 0.048, which translates to about 2.45 million km² — matching the stated shelf area pretty closely. So I'll apply the same area_fraction values from Continental Subsurface to Continental Near-Surface, then calculate the individual shelf fractions by subtracting land from the combined totals for each territory.
> 
> Now I'm working through the shelf area breakdown by territory, subtracting each land fraction from its corresponding subsurface fraction to isolate just the shelf portion, then converting those to actual areas. The total checks out at 2.45 million km². With that confirmed, I can move forward with setting up the Continental Shelf entries, which will have a thickness of 0.1 km, use the same inner radius as continental land, and assume a density around 2.0 g/cm³ for seafloor sediments.
> 
> Now I'm calculating the volume and mass for each continental shelf region using the area fractions and consistent parameters across USA, China, EU, Russia, and India shelves.
> 
> Continuing with Japan, Antarctica, and the remaining shelf areas, then moving into the near-surface continental layer calculations with a 0.4 km thickness and updated density values.
> 
> Now I'm refining the density progression across different continental depths—starting with looser surface materials around 2.0 g/cm³ and gradually increasing to denser rock at greater depths, reaching up to 2.75 g/cm³ in the ultra-deep subsurface layers.
> 
> For the near-surface calculations, I'm using 2.45 g/cm³ as the density and applying the same area fractions from the subsurface data. I need to be careful with the unit conversions when calculating mass from volume and density—converting from km³ and g/cm³ into kilograms.
> 
> Now I'm working through the mass calculations for each territory using the formula: mass = volume × density × 10^12 kg. I've got USA at 1.21×10^19 kg, China at 1.03×10^19 kg, and EU at 6.86×10^18 kg, with Russia still to complete.
> 
> Continuing with Russia at 2.21×10^19 kg, India at 3.63×10^18 kg, Japan at 7.62×10^17 kg, Antarctica at 1.57×10^19 kg, and Other regions totaling 9.90×10^19 kg for the near-surface layer.
> 
> Now shifting to the continental deep subsurface between 2.0 and 5.0 km depth—I'm using a 3.0 km thickness with an inner radius of 6366 km and a density of about 2.65 g/cm³, applying the same area fractions as before. Now I'm calculating the mass distribution for the deeper continental subsurface layer, working through the volume and density conversions for each region down to 12 kilometers depth, then tallying up the remaining areas and Antarctica's contribution to get the full picture of subsurface mass. Looking at the spherical area calculations, the radius difference between the surface and extreme subsurface boundary creates only a 0.4% variation in area, so I can safely use the same area fractions throughout. Checking the existing layer definitions, the continental subsurface sits at 0.5-2.0 km depth with consistent boundaries, which aligns with my strata calculations.
> 
> Now I'm mapping out the complete depth structure from the surface down to 20 km, with each layer defined by its inner and outer radii corresponding to specific depth ranges. I'm verifying the area calculations work consistently across different radii—the difference is negligible, so I'll use the same values throughout. Now I need to systematically define the resource compositions for each stratum type, starting with the continental shelf sediments which contain a mix of sand, gravel, mud, phosphorite, placer deposits, sulfides, organic material, and groundwater.
> 
> The shelf is underwater seafloor sediment, so it should mirror continental land resources but with higher water saturation, more marine organics, less coal and oil, abundant sand and gravel, phosphorite nodules, and placer deposits. I'll model it as a marine-adjusted version of continental land. The near-surface layer (100-500m) represents accessible mining depths with slightly enriched weathering products, while the deep subsurface (2-5km) requires different assumptions about resource concentration and accessibility.
> 
> Now I'm working through the deeper continental layers—the extreme subsurface at 5-12km is mostly crystalline basement with some sedimentary basins, the ultra-deep zone at 12-20km becomes increasingly metamorphic, and the lower crust at 20-35km contains granulites that are mostly speculative. Ice bodies are essentially water ice with trapped air and dust, while water bodies contain dissolved minerals and organics. This is a lot of data to organize, so I'm going to systematically define each continental resource category starting with ice bodies.
> 
> For continental ice, I'm breaking down the composition: it's almost entirely water ice at 99.7% by mass, with trace amounts of stone and regolith from dust particles, plus trapped atmospheric gases like nitrogen, oxygen, CO₂, argon, and methane. The air content in ice cores is roughly 90 milliliters per kilogram at standard temperature and pressure, which translates to a very small fraction of the total mass—probably around 0.01% when accounting for compression at depth.
> 
> Now I'm calculating the mass percentages of each gas within that trapped air. Nitrogen makes up about 75.5% of the air by mass, oxygen around 23.1%, argon 1.3%, and CO₂ just 0.06%. When I multiply these proportions by the overall air content, nitrogen comes out to roughly 0.008% of the ice by mass, oxygen to about 0.0025%, and argon to an even tinier fraction.
> 
> For dust content, Antarctic ice typically contains around 0.01% dust and debris by mass when including basal material. So the final composition breaks down to water as the dominant component at nearly 99.97%, with regolith making up about 0.02%, the various atmospheric gases in trace amounts, and methane essentially negligible since it's only around 700 parts per billion in the trapped air.
> 
> Now I'm reconsidering these percentages in simpler terms—the numbers are quite small, so let me reframe them as actual mass percentages rather than scientific notation. Ice is fairly homogeneous overall, which means dispersion should be minimal, though dust content does vary between ice ages and interglacial periods. Water itself has essentially zero dispersion since it's the primary component.
> 
> Looking at what resources would actually be present in continental ice bodies—iron, nickel, aluminum ores, and other industrial materials would all be essentially zero since ice sheets don't contain these mineral deposits. Now I'm looking at the atmospheric composition data across different planetary bodies, with concentrations for various gases like nitrogen, carbon dioxide, and methane. But I'm realizing that trapped air in ice doesn't distribute the same way as a free atmosphere would.
> 
> For gases trapped in ice, they're actually pretty uniformly spread throughout the ice matrix, so dispersion should be zero—the variation with depth is really just different atmospheric compositions from different time periods, not spatial heterogeneity that would matter for extraction. Regolith is different though, since basal ice has significantly more debris mixed in, so some dispersion makes sense there.
> 
> Now I'm working through freshwater composition, where water makes up nearly all of it with just trace amounts of dissolved minerals and organics. Now I'm filling in the remaining dissolved gases—nitrogen, carbon dioxide, and argon at trace levels—and accounting for methane which can be significant in some freshwater systems. I'm noting that mineral and gas dispersion varies considerably across different lakes, with some being mineral-rich while others are quite pure, so I'm setting dispersion around 0.3-0.5 for minerals. For the continental water body resources, I'm confirming that iron, nickel, aluminum, and industrial metal ores are essentially absent from this stratum.
> 
> Continuing through the resource inventory, most ore deposits and fossil fuels show zero abundance here, but I'm including small amounts of industrial minerals, organic tholins, and regolith with moderate dispersion values. Water dominates at 99.95% abundance with minimal variation, while oxygen and nitrogen are present in trace amounts.
> 
> Now I'm looking at the atmospheric and noble gas composition for this region, with nitrogen and carbon dioxide as the primary atmospheric components, and trace amounts of ammonia and methane showing higher extraction potential. The continental shelf extends 100 meters below the seafloor and represents a distinct resource zone.
> 
> For the shelf itself, I'm mapping out a baseline composition with consolidated rock making up the majority, followed by unconsolidated sediment and pore water, then smaller percentages of industrial minerals like phosphorite and placer deposits of tin and titanium, along with trace iron and aluminum ores.
> 
> The precious metals and rare earths are minimal—mostly diamond and gold placers in heavy mineral sands—while organic-rich mud, shallow oil seeps, and dissolved gases like methane make up the remaining fractions. But I'm realizing the shelf extends only 0-100 meters below the seafloor, so I should recalibrate these proportions to better reflect marine sediment composition at that shallow depth rather than treating it like deep ocean floor.
> 
> Now I'm accounting for the high porosity of saturated marine sediment—around 30-50% for shallow deposits—which means seawater fills a significant portion of the pore space. Working through the mass fraction calculation, with typical sediment grain density and seawater density, I'm getting roughly 20% water by mass in the shallow seafloor material. So I need to revise my breakdown to allocate about 35-40% to consolidated stone, 35-40% to unconsolidated regolith, and around 20% to water content.
> 
> Comparing this to continental land composition where water makes up only about 4%, the shelf sediment should have considerably more water since it's fully saturated. For a typical continental shelf, I'm settling on roughly 50 for stone, 25 for marine regolith, and 18 for pore water, with smaller percentages for industrial minerals, organics, and trace dissolved gases. I'm looking at the remaining gas and dissolved element concentrations on continental shelves, then shifting to how these resource distributions vary significantly by region—the USA has diverse shelf geology with major Gulf of Mexico hydrocarbon deposits, while Russia's Arctic shelf holds enormous untapped potential, and each territory has distinct characteristics I need to account for.
> 
> Now I'm realizing the shelf sediment compositions should mirror the continental land data but adjusted for marine conditions, with the main variables being stone-to-regolith ratios at different depths, water content variations, ore concentration patterns that typically decrease deeper but can spike in certain zones, and declining organic matter with depth.
> 
> As I go deeper into the continental crust, the trends shift significantly: the near-surface zone sees weathering and active mining with good ore enrichment, the deep subsurface around 2-5 km contains oil and gas formations with less ore enrichment but more hydrothermal deposits, and the extreme subsurface beyond 5 km becomes increasingly crystalline with frontier drilling conditions.
> 
> Moving further down, oil becomes scarce at depth though some deep gas persists, and the ultra-subsurface transitions into the lower crust with granulite facies and much higher density, making resource content increasingly speculative. Now I'm organizing this systematically into data tables for strata and resources.
> 
> Generating strata data... I'm reconsidering the survey level for ice—since ice composition is straightforward (essentially pure water) and well-documented through ice cores and radar data, I should rate it higher at 6.5 rather than 5.0. Now I'm moving on to continental water bodies, calculating the area and thickness for lakes and rivers. Now I'm working through the continental shelf areas for each territory by subtracting land area from the subsurface measurements—so for the USA that's 1.23e7 minus 9.83e6 to get 2.47e6 km², and I'm doing the same calculation across China, the EU, Russia, India, Japan, and Antarctica.
> 
> Once I have all the shelf areas calculated, I'm summing them up to get a total of about 25 million km², then converting each territory's shelf area into a fraction of the total subsurface area of 5.10e8 km² to see what proportion each one represents. Now I'm compiling the continental shelf data for major regions—China, the EU, and Russia—tracking their surveyed areas, volumes, masses, and exploration activity levels across their respective offshore zones. Now I'm looking at the remaining continental shelf regions—Antarctica has a massive area but almost no survey data due to ice coverage, while the catch-all "other" category encompasses diverse offshore zones across Australia, Africa, and Southeast Asia with moderate exploration levels. Moving into the near-surface continental layer, I'm setting up the same territorial breakdown with a thinner depth range and adjusted density parameters. Now I'm looking at the geological survey data for the EU, Russia, and India regions, comparing their area fractions, calculated volumes and masses, along with their survey coverage levels and mining history notes. Looking at the continental deep subsurface data now, starting with the USA region and its calculated volume and mass figures, then moving through the other territories with their respective survey parameters and density measurements. Compiling regional subsurface data for Europe, Russia, and India—tracking their respective survey depths, drilling infrastructure, and estimated mass distributions across continental zones. Now I'm looking at the remaining continental deep subsurface regions and moving into the extreme subsurface layer, which extends much deeper at around 7 kilometers. The other continental areas show moderate survey coverage with some deep drilling in oil-rich zones, while the USA's extreme subsurface has limited survey data despite its significant volume and mass. Now I'm looking at the subsurface data for Russia, India, and Japan—Russia has the most extensive direct sampling from the Kola borehole at over 12 kilometers depth, while India and Japan have much sparser coverage relying primarily on seismic inference. Now I'm working through the continental ultra subsurface layer across eight territories, calculating volumes and masses based on an 8 km thickness and density of 2.85. The survey data is sparse for most regions, relying heavily on seismic and gravity models rather than direct sampling. Now I'm finishing the Antarctica calculation and moving through the Other category, then shifting to the continental lower crust layer with its greater thickness of 15 kilometers and slightly lower density, working through the same territorial breakdown. I'm finishing up the survey levels for the ultra subsurface and lower crust across all regions, with Antarctica and Other territories having lower confidence scores of 0.3 due to limited data. Now I'm moving into resource compositions and need to map out how materials vary with depth, starting with the continental shelf where marine sediments and shallow rock dominate with high water saturation, phosphorites, and some placer minerals, which differs significantly from continental land deposits.
> 
> The near-surface zone from 100-500 meters represents the accessible mining range where weathering and supergene enrichment create favorable ore concentrations, sitting between the shelf and deeper subsurface in terms of composition and rock stability. For the deep subsurface beyond 2 kilometers, I'm establishing the baseline for what exists in the drilling and deep mining zones.
> 
> Now I'm mapping out the extreme and ultra-deep subsurface layers, where crystalline rock dominates and thermal conditions destroy most hydrocarbons, leaving only dry gas and deep hydrothermal deposits as viable resources, with helium accumulating from radioactive decay at these depths.
> 
> I'm starting to construct the actual resource compositions by interpolating between the surface and subsurface data I've already defined, using the existing USA continental values as my baseline reference point. Now I'm looking at the subsurface composition data and noticing the pattern—deeper layers have significantly more stone, minimal regolith, and a shift in mineral distribution with higher organic content and trace helium appearing. I'm going to interpolate these values to create a near-surface layer that bridges between the surface and subsurface compositions. Looking at the rest of the resource concentrations across the deeper layers, I'm noticing the organics and tholins show a notably higher abundance compared to the heavier metals and rare earth elements, which is an interesting pattern to consider.
> 
> Now I need to think through how to handle the dispersion and error values systematically. The data suggests that land-based deposits tend to have higher variability for ores, while subsurface compositions show lower dispersion but with uncertainty that tends to increase slightly with depth. I'm planning to generate resource compositions for six distinct depth zones across each territory—from the continental shelf down through various subsurface layers to the lower crust—which gives me 48 territorial resource rows plus the non-territorial water and ice bodies, totaling 50 entries to work through.
> 
> Given the scale of this, I think presenting the full tables as structured data blocks will be the clearest approach. Let me work through the composition trends more carefully, starting with the deep subsurface zone where oil and gas deposits dominate, then adjusting the mineral concentrations accordingly.
> 
> Generating composition table... Now I'm working through the composition of the USA continental shelf—it's marine sediment that's water-saturated with some offshore oil and gas deposits, so I'm allocating most of the mass to stone and regolith with significant pore water, then adding smaller fractions for phosphorites and other industrial minerals found in marine environments, plus trace amounts of placer deposits like offshore gold and heavy mineral sands. The percentages add up to about 98.1, which is close enough since they'll be normalized anyway. But I should reconsider the shelf composition more carefully—the top 100 meters of seafloor is primarily sediment with different depositional patterns than the deeper ocean floor, so it shouldn't include things like manganese nodules. Let me compare this against the actual ocean floor composition to make sure I'm capturing the right distinction.
> 
> Looking at the ocean floor data, I notice it's heavily weighted toward regolith and water, which makes sense for deep-sea sediment. The shelf should sit somewhere between that and typical terrestrial sediment—more consolidated stone than the ocean floor since shelf sediment tends to be sandier, less water content since it's denser, and potentially more industrial minerals like phosphorites that concentrate in shelf environments. I'm working through what the actual proportions should be.
> 
> Shelf sediment porosity typically ranges from 40-60% near the surface down to 20-30% at depth, so I'm estimating around 35% average. Using that to calculate water content by mass—accounting for water density at 1.025 g/cm³ and sediment density at 2.65 g/cm³—I get roughly 17% water by mass. That's notably less than the ocean floor's 26%, which tracks with my expectations. So I'm revising the shelf composition to reflect more stone, less water, and a bit more industrial minerals than the deep ocean baseline.
> 
> Now I'm filling in the trace elements and compounds—precious metals, rare earths, uranium, sulfur, organics, hydrocarbons, and atmospheric gases—to complete the shelf sediment profile and verify the percentages sum correctly. The four major components add up to 97, leaving about 3% for everything else. I'm distributing the remaining fraction across ores, organics, and volatiles—iron and aluminum ores get the bulk, with organics and tholins taking a significant share since shelf sediments tend to have substantial marine organic matter, and the rest trailing off into trace amounts.
> 
> Now I'm checking whether this composition is internally consistent. The total comes to about 98.1%, which is close enough. I'm noticing that iron ores might need adjustment since shelf mineralogy differs from the open ocean floor—heavy mineral sand deposits with ilmenite and rutile are common on shelves, so I should probably bump iron ores up to 0.1-0.3% and account for placer deposits in the industrial metal ores category. But I'm going to stop second-guessing myself here and just lock in reasonable values so I can move forward.
> 
> For dispersion, I'm thinking the shelf should have moderate to high values for ores (1.0-1.8) since placer deposits are concentrated, while stone and water stay low (0.2-0.5). Deeper strata would generally have lower dispersion as things become more homogeneous, except for trapped fluids like oil and gas which stay concentrated. I'm also considering that shelf resources have moderate uncertainty in their abundance since they're less explored than some other areas.
> 
> Now I'm working through the uncertainty pattern across all depth zones—near-surface mirrors subsurface conditions, while deep subsurface gets progressively more uncertain, with the ultra-deep and lower crust having the highest uncertainty. I need to construct all 50 resource rows systematically, organized by stratum type. Given the size of this dataset with 27 columns per row, I'll present it as compact code blocks rather than trying to format it inline.
> 
> Generating resource data tables... will be 0,0,0,0 (no coal, no oil, no surface minerals at those depths). This simplifies things.
> 
> Let me think about which resources are present at each depth:
> 
> **Continental Shelf (0-100m below seafloor)**:
> Present: Most resources, similar to land but marine. Coal rare, oil rare at this depth. Some methane (shallow gas).
> 
> **Continental Near-Surface (100-500m)**:
> Present: Similar to subsurface. All major ores, some coal, some oil/gas, organics.
> 
> **Continental Deep Subsurface (2.0-5.0 km)**:
> Present: Ores (deep hydrothermal), significant oil/gas, deep coal rare, organics (source rocks), deep water, helium increases.
> 
> **Continental Extreme Subsurface (5.0-12.0 km)**:
> Present: Some ores (less concentrated), methane (deep gas), no oil (destroyed by heat), some organics (over-mature), water (brines), helium, CO₂, hydrogen. No coal.
> 
> **Continental Ultra Subsurface (12.0-20.0 km)**:
> Present: Very limited ores, water (metamorphic fluids), CO₂, some methane, helium. Very homogeneous rock. No fossil fuels.
> 
> **Continental Lower Crust (20.0-35.0 km)**:
> Present: Essentially just stone + minor components. Some industrial metal ores (transition toward mantle composition), water (fluid inclusions), CO₂, helium. Very homogeneous.
> 
> Now I'm constructing the baseline compositions for each territory type, starting with the USA as a reference point before adjusting for regional variations. I'm looking at the resource parameters for various gases and atmospheric compounds, continuing through the periodic table with entries for nitrogen, carbon dioxide, ammonia, methane, and other volatile substances, each with their associated numerical values for different properties. I'm continuing to map out the resource distribution parameters for the near-surface layer, filling in the remaining ore types, minerals, and volatile compounds with their respective abundance, accessibility, concentration, and purity values. Now I'm looking at the ore and mineral composition for the deeper subsurface layer, mapping out abundance levels and distribution parameters for metals like aluminum, industrial ores, precious metals, rare earths, and uranium, along with sulfur deposits and organic compounds.
> 
> Continuing with the volatile and regolith composition data for this same layer, tracking the abundance and distribution characteristics for coal, hydrocarbons, stone, water ice, and atmospheric gases. Now I'm looking at the distribution parameters for the deeper subsurface materials, continuing through the industrial metals, rare earth elements, and various mineral deposits down to the stone layer. Now I'm looking at the deeper subsurface layers, mapping out the rarer elements and ore deposits that become more prevalent at greater depths, with their respective abundance and distribution parameters. Looking at the resource abundance values and their associated parameters across different materials, I'm noticing the distribution patterns and now wondering whether I should factor in iron and aluminum as well.
> 
> At 12-20 km depth in the middle crust, we're dealing with metamorphic rock in the amphibolite to granulite facies—iron and aluminum are bound up in silicate minerals rather than existing as distinct ore deposits, so those should register as zeros. Industrial metal ores like sulfides could theoretically persist at these depths and temperatures, but we're so far beyond where any actual mining occurs that the distinction between "ore" and "stone" breaks down; it's all metamorphic silicate material now. Since the model works with mass fractions, I need to account for how these elements are actually distributed through the rock matrix rather than treating them as extractable resources. in highly metamorphic rock, there are still elements present that, if they were at the surface, would constitute ores. The key is that the dispersion should be very low (homogeneous distribution = no economic deposits).
> 
> Let me reconsider: I think at 12-20 km depth, the approach should be:
> - Most "ore" elements are incorporated into the bulk silicate (stone)
> - Very low concentrations of separate "ore minerals"
> - Very low dispersion (homogeneous)
> 
> So I'll set most ores to 0 at these depths, with the understanding that the elements are absorbed into "stone". Industrial metal ores and sulfur might still be present as minor phases.
> 
> For the lower crust below 20 km, I'm keeping aluminum ores absent while allowing trace amounts of industrial metals, precious metals, rare earths, and uranium to persist in the metamorphic assemblage. Sulfur remains as a minor phase, and industrial minerals stay relatively abundant since they're stable at depth, though their concentration decreases with pressure. Looking at how resource composition varies by territory and depth—at greater depths the crustal composition becomes more uniform across regions, so I can use nearly identical compositions for all territories in the extreme and lower crust layers with just minor adjustments for uncertainty differences. The shallower strata show more significant territorial variation due to different geological settings and mineral endowments, so I'll need to differentiate those more carefully. I'm thinking through a baseline approach where I define the USA composition and then adjust for each territory's specific characteristics, though I might simplify the deepest layers to be nearly identical across all regions except for Antarctica's higher uncertainty and possibly Russia's unique Kola data.
> 
> Now I'm ready to construct the actual data tables. I need to work with the strata.tsv format and lay out the columns properly, then populate the area fractions I've calculated for each territory's continental shelf and other depth zones.
> 
> Generating data tables... I'm verifying the subsurface area sum matches the ~174 million km² description, and it checks out at 173.87 million km². Now I need to confirm the volume and mass calculations are using the right unit conversions—multiplying volume in km³ by density in g/cm³ by 1e12 to get mass in kg—and spot-checking a few examples to make sure they're consistent. I'm also wondering whether the continental area should shrink for the deeper strata. with depth due to the continent-ocean boundary. At 35 km depth, the continental crust transitions to oceanic crust in some places. However, the model already handles this separately (ocean crust is a different stratum). The continental layers extend under both land and shelf, so the area stays the same from near-surface through lower crust.
> 
> Re-reading the strata documentation confirms all continental layers maintain that consistent ~174 million km² area across every depth range. Now I'm working through the detailed compositions for each territorial shelf region—the USA shelf has diverse resources including sand, gravel, and phosphorite deposits, while the China shelf shows active oil and gas exploration, and the North Sea represents a well-explored region with major hydrocarbon reserves.
> 
> The key variables that distinguish these shelf areas are oil and gas potential, mineral deposits like iron ores and heavy mineral sands, organic content, and how well each region has been surveyed. I'm trying to decide how granular to get with the resource data before building out the complete tables.
> 
> For the ice bodies, I need to nail down the composition more precisely—they're almost entirely water in frozen form, with trace amounts of trapped atmospheric gases and some regolith material, especially at the base where glaciers have scraped up sediment. Antarctica dominates the ice coverage at around 89%, with Greenland making up most of the rest.
> 
> For water bodies like freshwater lakes and reservoirs, the composition is similarly water-dominated but with dissolved minerals, organic matter, and suspended sediment. The dissolved gases—oxygen and nitrogen—are present in small amounts, and there's some carbon dioxide mixed in as well.
> 
> Now I'm ready to construct the resource table with all the composition data I've gathered. Each entry needs the four values for relative abundance, coefficient of variation, dispersion, and dispersion error. Let me organize this by stratum type and work through all 50 rows systematically. Before I do that, I should double-check that I'm capturing all the Earth strata the user asked for—let me review what I might have missed. I'm finishing the list of missing rows and noticing a naming inconsistency in the existing table format that I need to resolve before writing these out.
> 
> Generating strata table additions... Now I'm assigning survey levels for the deeper subsurface strata, with Antarctica and other regions having minimal coverage due to the extreme difficulty of exploration at those depths, while developed nations like the USA and EU have slightly better knowledge from research initiatives.
> 
> Looking back at the density values, I need to make sure they align properly across all the layers—the continental crust should show a logical progression from the lighter surface materials down through increasingly dense rock formations. Now I'm calculating the volume-weighted average across all these layers to verify the continental crust density checks out at around 2.83 g/cm³. The calculated value of 2.83 matches the standard really well. For the shelf sediments, I'm reconsidering whether 2.0 is the best choice—marine sediment with higher porosity and water content might be slightly less dense, so I'm working through the bulk density calculation accounting for porosity and seawater saturation, which gives me around 2.08, though 2.0 remains a reasonable approximation that keeps things consistent.
> 
> But now I'm noticing the land surface is also modeled at 2.0, and when I calculate that out accounting for soil porosity and groundwater, I'm getting closer to 2.3, which suggests the existing 2.0 value might be accounting for a shallower layer with more weathered material, alluvium, and loose sediment that brings the average down. Rather than overthinking this, I should just stick with what the existing data uses.
> 
> For ice I'll use 0.90 g/cm³ and for water 1.0 g/cm³. Now I'm ready to structure my response: I'll introduce what I'm adding, then provide the new rows for both data files, and finish with a discussion of the simplifications I'm making. For the resource compositions, I'm planning to keep the deeper strata fairly consistent across territories but vary the uncertainty levels, while the shallower strata will show more territorial variation to reflect real geological differences.
> 
> Now I'm mapping out the territorial patterns for each shallow stratum. The shelf zones differ significantly—the USA and EU have established oil and gas production, China has high potential in the South China Sea, Russia dominates Arctic resources, and Antarctica remains largely unknown with very high uncertainty. The near-surface and deep subsurface layers follow similar patterns to existing land data but adjusted for depth, with major basins like the Permian and Gulf deepwater for the USA and growing exploration in China.
> 
> I have enough detail now to construct the resource tables. Let me verify the column structure for the strata_resources table first—I need to make sure I'm using the correct resource type names in the right order. So that's 28 columns total—the name field plus 27 resource types. Now I'm moving on to construct the actual tables, starting with calculating volumes and masses for the strata. I should also verify my area calculations for the shelf against what's already defined.
> 
> Looking at the existing data, I can derive the shelf areas by taking the subsurface fraction minus the land fraction for each region. For the USA, that's 0.0049 with an area around 2.47-2.50 million square kilometers. I'll work through the same calculation for China, the EU, and the other regions to get consistent shelf area values.
> 
> Now I'm calculating the ice masses by multiplying each shelf area by the thickness and density parameters—with a thickness of 0.1 and density of 2.0, the USA shelf would contribute about 2.47×10^5 in mass. Now I'm calculating the near-surface ice mass for the remaining regions, working through each area's volume and converting to total mass using the density factor. Now I'm calculating the deep subsurface masses for the remaining regions, multiplying each area by the 3.0 kilometer thickness and 2.65 density factor to get the total mass contributions from that layer. Continuing through the ultra subsurface calculations with thickness 8.0 and density 2.85, finishing out the regional mass estimates for Japan, Antarctica, and the aggregate "Other" category, then moving into the final ultra-deep layer computations. Now I'm calculating the lower crust mass contributions for each region, multiplying their areas by the 15.0 km thickness and 2.95 density factor to get the total crustal mass for that layer.
> 
> With all the strata data compiled, I'm moving on to define the resource compositions for each layer type—starting with baseline compositions for shelf, near-surface, and deep subsurface environments, then applying territorial adjustments across the 50 rows and 27 resource columns. For the shelf I'm accounting for marine sediment characteristics like higher water saturation, marine organics, heavy mineral sands, and phosphorites rather than terrestrial deposits. The near-surface and deep subsurface will have their own distinct resource profiles reflecting their geological environments. Now I'm working through the shelf resource distributions by territory, starting with iron ore concentrations that reflect regional geology—heavier deposits in areas like India's heavy mineral sands and the Arctic, lighter in places like Japan. Moving into the oil concentrations across shallow shelf zones, accounting for known reserves like the North Sea and Bombay High.
> 
> Organizing the methane hydrate data across regions, with Japan showing notably higher concentrations due to its shallow shelf deposits, Russia's Arctic holdings, and more speculative values for Antarctica and other areas.
> 
> Now I'm ready to write out the full response with the strata additions and the complete resource table—this is going to be a substantial dataset with over a thousand cells to populate. Before I dive in, I want to sanity-check my resource values against known crustal composition data to make sure the proportions align with what we understand about Earth's continental crust. more mafic (~52% SiO₂). In this model, "stone" represents bulk silicate rock with other ores and minerals separated out, so the stone composition is what remains after extracting those components. The reference composition I'm using is pyrolite, which is actually mantle material rather than crustal rock—for the crust I'd expect something more felsic. But since stone functions as a generic filler rock with fixed composition defined in the resource file, what really matters is getting the mass fractions right between stone and other resources across different strata. Deeper layers have more competent stone and less loose regolith.
> 
> Now I'm thinking through the lower crust specifically—it's more mafic and approaches mantle composition, which actually makes the pyrolite reference more appropriate at depth even though the real crust is more evolved. I should increase industrial metal ores slightly in the lower crust to reflect higher chromium, nickel, and cobalt content, while reducing rare earth elements since they're incompatible and concentrate upward. With all these constraints in mind, I'm ready to finalize the composition values.
> 
> I need to present this efficiently as a single table rather than breaking it into pieces. I'm going to establish base compositions for each stratum type, then apply territorial modifiers to generate the eight variants. Looking at the existing data patterns, I can see clear regional differences—China has roughly double the iron ores compared to the USA, about 1.5 times the aluminum ores, twice the industrial metal ores, and notably five times the rare earth ores.
> 
> Generating the resource table... China dominates rare earths with similar coal and stone/regolith/water levels, but notably less oil and slightly higher organics, plus greater dispersion error for ores. These patterns should carry through to the shelf with marine environment adjustments.
> 
> For the three deepest strata, I'm taking a simpler approach—using nearly identical base compositions across all territories since these depths represent uniform continental crust with minimal direct sampling, then varying only the uncertainty coefficients to reflect survey quality differences, with Antarctica having the highest uncertainty and Russia slightly lower due to Kola data.
> 
> Now I need to check the exact format of the existing data file to make sure I'm structuring everything correctly.
> 
> Looking at the shelf stratum, I'm defining the base resource composition with specific values for iron ores, aluminum ores, industrial metals, precious metals, and rare earth elements, then I'll apply territorial modifications on top of that. Now I'm looking at the atmospheric composition data with abundance levels and various parameters for gases like hydrogen, oxygen, nitrogen, and methane across different conditions.
> 
> I need to adjust these baseline values for each continental shelf region—the USA shelf stays close to base with slightly elevated methane in the Gulf, China's shelf gets boosted iron ores and industrial metals with higher oil and rare earth concentrations, the EU shelf emphasizes North Sea oil with tighter uncertainty bounds from better exploration data, and Russia's Arctic shelf will need significant adjustments given its enormous reserves.
> 
> For the remaining regions, India's shelf reflects heavy mineral sands with elevated iron ores and rare earths from coastal deposits, Japan's shelf prioritizes methane hydrates while keeping most ore values lower due to limited resources, Antarctica gets very high uncertainty across all parameters since it's largely unexplored, and the catch-all "Other Shelf" category gets moderate-to-high ore content with elevated uncertainty to account for regional variability. Now I'm filling in the remaining regional shelves with their specific resource parameters, adjusting coefficients based on each region's geological characteristics and exploration maturity. Looking at the resource parameters for the remaining shelf regions—Japan has notably lower ore concentrations with limited iron deposits and some methane hydrates, while Antarctica's data carries much higher uncertainty across all measurements so I'll need to use generic composition values. The other shelves show moderate resource distributions with iron ores and trace oil presence.
> 
> Now I'm going to construct the complete resource table by interpolating between the land and subsurface values I already have for each territory. For near-surface estimates, I'll compute intermediate values that sit between the established land and subsurface data points—this gives me a systematic way to fill in the missing layer without having to manually specify every single parameter.
> 
> I'm realizing the full output is going to be quite substantial though. With 50 resource rows across 27 columns, each containing multiple values, I'm looking at tens of thousands of characters just for the tables alone, before adding any discussion or context. Let me think about how to present this efficiently.
> 
> I'll organize the data by stratum type and provide the new rows as appendable datasets. For the near-surface values, I'll calculate them as midpoints between land and subsurface estimates, and extend the pattern logically for deeper crustal layers. The extreme and ultra-lower crust strata will use consistent base compositions with minimal variation except for Antarctica-specific coefficients.
> 
> Generating the data tables now... I'm checking whether the percentages add up correctly—the subsurface composition totals around 93.5%, and the land composition comes to 98%, both falling short of 100%. Since the description mentions these will be normalized anyway, I should verify the math is consistent before moving forward. Looking at the subsurface data more carefully now—I'm seeing the ore and mineral entries with their respective values and multipliers. The iron ores show 0.002 as the base, aluminum ores at 4e-4, and so on through the rare earth and uranium ores, with industrial minerals contributing a significant 8 to the total. Now I'm finishing out the material composition table and calculating the total abundance—adding up all the individual material values, which comes to roughly 93.5 units when I sum the major components like stone, regolith, and water along with the trace gases and ores.
> 
> The sum being slightly under 100 is fine since the code normalizes everything anyway, so the exact percentages matter less than getting the relative proportions right. I should focus on keeping my new strata compositions internally consistent rather than obsessing over hitting exactly 100.
> 
> Let me just write out the response now instead of continuing to second-guess the numbers.
> 
> Generating the composition tables... Now I'm looking at the atmospheric composition data with abundance levels and various parameters for gases like hydrogen, oxygen, nitrogen, and other compounds found in the environment.
> 
> Continuing with the noble gases and then moving into mineral composition data for the China shelf region, cataloging ore types with their respective measurements and concentration values. Now I'm looking at the atmospheric composition data, continuing through gases like oxygen, nitrogen, and various compounds with their respective parameters across different scales.
> 
> Moving into mineral resources on the EU shelf, I'm cataloging ore deposits—iron, aluminum, industrial metals, precious metals, rare earth elements, and uranium—each with their own concentration and distribution metrics. Now I'm looking at the resource parameters for various gases and atmospheric compounds, continuing through nitrogen, carbon dioxide, methane, and other trace elements with their respective abundance and distribution metrics. Now I'm looking at the distribution parameters for the remaining resource types on the Russia shelf, including industrial minerals, organics, fossil fuels, and various gases—each with their own abundance, variance, and scale characteristics. Now I'm looking at the mineral composition data for the India shelf, cataloging the various ore deposits and their characteristics—iron ores with ilmenite concentrations, aluminum and industrial metal ores from heavy mineral sands, precious metals, rare earth elements including monazite which represents a significant global resource, uranium, and sulfur deposits. Continuing through the atmospheric and gas composition data for different resource types, tracking their abundance, extraction efficiency, and various other parameters across the remaining entries. Looking at the resource distribution parameters for the remaining materials—completing the industrial minerals entry and then moving through organics, fossil fuels, and various planetary materials with their respective spawn rates and distribution characteristics. Now I'm looking at the resource distribution across Antarctica's shelf, mapping out concentrations and availability metrics for various ore types and industrial minerals. Now I'm looking at the remaining atmospheric and mineral resources, including trace gases like ammonia, methane, and ethane, along with some inert gases, and then moving to the mineral shelf where iron-nickel isn't available but iron ores are present in small quantities.
> 
> Continuing through the ore and mineral inventory, I see aluminum, industrial metals, precious metals, rare earths, and uranium all have varying availability levels, with industrial minerals being notably abundant and organics showing moderate presence. Now I'm looking at the resource parameters for various atmospheric compounds, finishing up the shelf data before moving to near-surface territories. For the near-surface layer spanning 100-500 meters, I'm planning to interpolate between land and subsurface values, weighting them roughly 60% toward subsurface since that depth range shares more characteristics with deeper layers than surface conditions.
> 
> Working through the USA calculations, I'm interpolating ore concentrations between land and subsurface values to establish the near-surface estimates—iron ores dropping to around 0.005, aluminum ores to 0.002, and so on down the list. Now I'm working through the resource distributions for each territory, starting with USA near-surface deposits. I'm calculating the mean, dispersion, and coefficient of variation for each ore type by taking intermediate values between land and subsurface measurements.
> 
> Continuing with the rest of the USA near-surface resources... Now I'm calculating the interpolated values for China's near-surface composition, weighting the land and subsurface measurements at 40% and 60% respectively. I'll use these approximate figures rather than pursuing excessive precision for the mineral ore concentrations. Continuing through the resource composition table with the remaining materials like coal, oil, stone, regolith, and water, each with their own distribution parameters. I'm noticing the pattern holds consistently across territories, so I'll work through all fifty rows systematically. For the deepest strata, I can use the same composition across all territories and just adjust the coefficient of variation values higher for Antarctica and unexplored regions.
> 
> Now I'm mapping out the oil and gas distribution across the 2-5 km zone, accounting for each region's production capacity—major producers like the USA and Russia get higher concentrations, while regions like Japan and Antarctica get minimal amounts, with Antarctica's values being more speculative given the lack of exploration data.
> 
> Generating the data tables...

This is a large addition — 50 new rows across both tables. I'll work through the calculations systematically, then provide the complete row sets.

## Identification of missing Earth strata

From `strata.md`, the following are absent from the current tables:

**Non-territorial (2 strata):** Continental Ice Bodies, Continental Water Bodies

**Territorial ×8 (48 strata):** Continental Shelf, Continental Near-Surface, Continental Deep Subsurface, Continental Extreme Subsurface, Continental Ultra Subsurface, Continental Lower Crust

## Spatial calculations

**Shelf area derivation.** Since the existing Continental Subsurface entries encompass land + shelf combined, the per-territory shelf area is the difference between subsurface and land areas:

| Territory | Subsurface area | Land area | Shelf area | Shelf area_fraction |
|-----------|----------------|-----------|------------|-------------------|
| USA | 1.23e7 | 9.83e6 | 2.47e6 | 0.00484 |
| China | 1.05e7 | 9.60e6 | 9.00e5 | 0.00176 |
| EU | 7.00e6 | 5.00e6 | 2.00e6 | 0.00392 |
| Russia | 2.26e7 | 1.71e7 | 5.50e6 | 0.0108 |
| India | 3.69e6 | 3.29e6 | 4.00e5 | 7.84e-4 |
| Japan | 7.78e5 | 3.78e5 | 4.00e5 | 7.84e-4 |
| Antarctica | 1.60e7 | 1.40e7 | 2.00e6 | 0.00392 |
| Other | 1.01e8 | 8.98e7 | 1.12e7 | 0.0220 |
| **Total** | **1.74e8** | **1.49e8** | **2.45e7** | **0.0480** |

Total shelf ≈ 24.5 million km², consistent with the ~25 million km² in strata.md.

**Depth layer geometry.** All layers from Near-Surface downward share the land+shelf footprint (~174 million km²). Using the same per-territory area fractions as the existing Continental Subsurface entries:

| Layer | Depth range | Thickness | inner_r | Density |
|-------|-----------|-----------|---------|---------|
| Shelf | 0–0.1 km below seafloor | 0.1 | 6370.9 | 2.0 |
| Near-Surface | 0.1–0.5 km | 0.4 | 6370.5 | 2.45 |
| Deep Subsurface | 2.0–5.0 km | 3.0 | 6366.0 | 2.65 |
| Extreme Subsurface | 5.0–12.0 km | 7.0 | 6359.0 | 2.75 |
| Ultra Subsurface | 12.0–20.0 km | 8.0 | 6351.0 | 2.85 |
| Lower Crust | 20.0–35.0 km | 15.0 | 6336.0 | 2.95 |

**Density progression cross-check.** Volume-weighted mean continental crust density (0–35 km) using all layers including existing Land (ρ=2.0, Δz=0.1) and Subsurface (ρ=2.55, Δz=1.5): \(\bar{\rho} = \sum(f_i \cdot \rho_i) = 2.84\) g/cm³, consistent with standard continental crust estimate ~2.83 g/cm³ (Christensen & Mooney 1995).

**Ice Bodies.** 14.8 million km², 1.95 km thick, density 0.90 g/cm³ (column-average including firn). Mass = 2.89e7 km³ × 0.90 × 1e12 = 2.60e19 kg. Cross-check: Antarctic ice sheet ~2.4e19 kg + Greenland ~2.7e18 kg + glaciers ~2e17 kg ≈ 2.7e19 kg. Consistent.

**Water Bodies.** 2.10 million km², 0.046 km thick, density 1.0 g/cm³. Mass = 9.66e4 km³ × 1.0 × 1e12 = 9.66e16 kg. Cross-check: global freshwater lake volume ~91,000 km³ + reservoirs ~8,000 km³ ≈ 99,000 km³. Consistent.

## Composition approach

**Shelf (0–100 m below seafloor):** Marine saturated sediment. Average porosity ~35% filled with seawater gives ~17% water by mass. More regolith (unconsolidated sediment) relative to stone than land. Marine organic matter higher. Heavy mineral sands (ilmenite, monazite, rutile) on some shelves. Some shallow gas. Territorial variation follows known offshore geology.

**Near-Surface (100–500 m):** Interpolated between existing Land (surface) and Subsurface (0.5–2 km) at roughly 40:60 weighting toward the deeper end — this is the open-pit / shallow underground mining zone.

**Deep Subsurface (2–5 km):** Prime oil/gas production depth. Mostly competent rock (stone ~86%). Deep hydrothermal ore deposits. Significant organics (source rocks). Increasing helium from radioactive decay. Less coal (too deep for most seams).

**Extreme Subsurface (5–12 km):** Frontier drilling zone. Crystalline/metamorphic basement in many areas. No oil (thermal cracking). Deep dry gas (methane) persists. Significant helium. Reduced ore diversity. Kola Superdeep Borehole (12.3 km) provides unique direct data for Russia.

**Ultra Subsurface (12–20 km):** Beyond all current extraction. Amphibolite-to-granulite facies. Very homogeneous (low dispersion). Deep metamorphic fluids (H₂O, CO₂). Territorial composition differences minimal — only uncertainty (cv) varies.

**Lower Crust (20–35 km):** Mafic granulite, approaching mantle composition. Known only from xenoliths, seismic velocity models, and exposed deep-crustal terranes. Nearly uniform across territories; highest uncertainty.

## strata.tsv additions

```tsv
name	body	reference_r	territory	strata_group	inner_r	thickness	area_fraction	area	volume	density	mass	survey_level	survey_note
EARTH_CONTINENTAL_ICE_BODIES	EARTH	6371		CONTINENTAL_ICE_BODIES	6371	1.95	0.029	1.48e7	2.89e7	0.90	2.60e19	6.5	Ice cores and radar surveys; composition nearly pure water ice
EARTH_CONTINENTAL_WATER_BODIES	EARTH	6371		CONTINENTAL_WATER_BODIES	6371	0.046	0.0041	2.10e6	9.66e4	1.0	9.66e16	7.5	Extensive limnological monitoring; composition is freshwater with minor dissolved load
EARTH_USA_CONTINENTAL_SHELF	EARTH	6371	USA	CONTINENTAL_SHELF	6370.9	0.1	0.00484	2.47e6	2.47e5	2.0	4.94e17	4.5	Extensive OCS surveys; Gulf of Mexico and Atlantic sand/gravel well explored
EARTH_CHINA_CONTINENTAL_SHELF	EARTH	6371	CHINA	CONTINENTAL_SHELF	6370.9	0.1	0.00176	9.00e5	9.00e4	2.0	1.80e17	3.5	Active offshore exploration; South and East China Sea sediment characterized
EARTH_EU_CONTINENTAL_SHELF	EARTH	6371	EU	CONTINENTAL_SHELF	6370.9	0.1	0.00392	2.00e6	2.00e5	2.0	4.00e17	5.0	North Sea thoroughly explored; extensive marine aggregate and geological mapping
EARTH_RUSSIA_CONTINENTAL_SHELF	EARTH	6371	RUSSIA	CONTINENTAL_SHELF	6370.9	0.1	0.0108	5.50e6	5.50e5	2.0	1.10e18	3.0	Vast Arctic shelf; significant potential but much remains unexplored
EARTH_INDIA_CONTINENTAL_SHELF	EARTH	6371	INDIA	CONTINENTAL_SHELF	6370.9	0.1	7.84e-4	4.00e5	4.00e4	2.0	8.00e16	3.5	Bombay High and western shelf surveyed; east coast heavy mineral sands characterized
EARTH_JAPAN_CONTINENTAL_SHELF	EARTH	6371	JAPAN	CONTINENTAL_SHELF	6370.9	0.1	7.84e-4	4.00e5	4.00e4	2.0	8.00e16	4.0	Limited shelf but well-surveyed; methane hydrate exploration active
EARTH_ANTARCTICA_CONTINENTAL_SHELF	EARTH	6371	ANTARCTICA	CONTINENTAL_SHELF	6370.9	0.1	0.00392	2.00e6	2.00e5	2.0	4.00e17	1.0	Ice-covered shelf; very sparse geological data from limited coring
EARTH_OTHER_CONTINENTAL_SHELF	EARTH	6371	OTHER	CONTINENTAL_SHELF	6370.9	0.1	0.0220	1.12e7	1.12e6	2.0	2.24e18	3.0	Highly variable; includes major offshore mining provinces (Australia SE Asia W Africa)
EARTH_USA_CONTINENTAL_NEAR_SURFACE	EARTH	6371	USA	CONTINENTAL_NEAR_SURFACE	6370.5	0.4	0.0242	1.23e7	4.92e6	2.45	1.21e19	5.0	Well-explored mining zone; extensive open-pit and shallow underground operations
EARTH_CHINA_CONTINENTAL_NEAR_SURFACE	EARTH	6371	CHINA	CONTINENTAL_NEAR_SURFACE	6370.5	0.4	0.0206	1.05e7	4.20e6	2.45	1.03e19	4.5	Active mining exploration; many major open-pit operations
EARTH_EU_CONTINENTAL_NEAR_SURFACE	EARTH	6371	EU	CONTINENTAL_NEAR_SURFACE	6370.5	0.4	0.0137	7.00e6	2.80e6	2.45	6.86e18	5.5	Long mining tradition; thorough geological mapping to this depth
EARTH_RUSSIA_CONTINENTAL_NEAR_SURFACE	EARTH	6371	RUSSIA	CONTINENTAL_NEAR_SURFACE	6370.5	0.4	0.0443	2.26e7	9.04e6	2.45	2.21e19	4.0	Soviet-era mining surveys; vast remote areas less explored
EARTH_INDIA_CONTINENTAL_NEAR_SURFACE	EARTH	6371	INDIA	CONTINENTAL_NEAR_SURFACE	6370.5	0.4	0.00723	3.69e6	1.48e6	2.45	3.63e18	4.0	Major mining districts explored at depth; many areas with limited data
EARTH_JAPAN_CONTINENTAL_NEAR_SURFACE	EARTH	6371	JAPAN	CONTINENTAL_NEAR_SURFACE	6370.5	0.4	0.00153	7.78e5	3.11e5	2.45	7.62e17	4.5	Small territory well-characterized; limited mineral resources at depth
EARTH_ANTARCTICA_CONTINENTAL_NEAR_SURFACE	EARTH	6371	ANTARCTICA	CONTINENTAL_NEAR_SURFACE	6370.5	0.4	0.0314	1.60e7	6.40e6	2.45	1.57e19	0.8	Ice-covered; virtually no direct geological data at this depth
EARTH_OTHER_CONTINENTAL_NEAR_SURFACE	EARTH	6371	OTHER	CONTINENTAL_NEAR_SURFACE	6370.5	0.4	0.198	1.01e8	4.04e7	2.45	9.90e19	3.0	Highly variable; includes major mining regions and unexplored areas
EARTH_USA_CONTINENTAL_DEEP_SUBSURFACE	EARTH	6371	USA	CONTINENTAL_DEEP_SUBSURFACE	6366	3.0	0.0242	1.23e7	3.69e7	2.65	9.78e19	3.5	Extensive deep drilling for oil/gas; deep mine data in select districts
EARTH_CHINA_CONTINENTAL_DEEP_SUBSURFACE	EARTH	6371	CHINA	CONTINENTAL_DEEP_SUBSURFACE	6366	3.0	0.0206	1.05e7	3.15e7	2.65	8.35e19	3.0	Growing deep drilling program; Tarim and Sichuan basin characterization
EARTH_EU_CONTINENTAL_DEEP_SUBSURFACE	EARTH	6371	EU	CONTINENTAL_DEEP_SUBSURFACE	6366	3.0	0.0137	7.00e6	2.10e7	2.65	5.57e19	4.0	Deep mining tradition; North Sea deep wells; KTB borehole data
EARTH_RUSSIA_CONTINENTAL_DEEP_SUBSURFACE	EARTH	6371	RUSSIA	CONTINENTAL_DEEP_SUBSURFACE	6366	3.0	0.0443	2.26e7	6.78e7	2.65	1.80e20	3.0	Extensive West Siberia deep drilling; some unique deep mine data
EARTH_INDIA_CONTINENTAL_DEEP_SUBSURFACE	EARTH	6371	INDIA	CONTINENTAL_DEEP_SUBSURFACE	6366	3.0	0.00723	3.69e6	1.11e7	2.65	2.94e19	2.5	Some deep oil/gas wells; limited deep mining experience
EARTH_JAPAN_CONTINENTAL_DEEP_SUBSURFACE	EARTH	6371	JAPAN	CONTINENTAL_DEEP_SUBSURFACE	6366	3.0	0.00153	7.78e5	2.33e6	2.65	6.17e18	3.0	Limited deep drilling; geothermal exploration provides some data
EARTH_ANTARCTICA_CONTINENTAL_DEEP_SUBSURFACE	EARTH	6371	ANTARCTICA	CONTINENTAL_DEEP_SUBSURFACE	6366	3.0	0.0314	1.60e7	4.80e7	2.65	1.27e20	0.3	No deep drilling; inferred from seismic and gravity models only
EARTH_OTHER_CONTINENTAL_DEEP_SUBSURFACE	EARTH	6371	OTHER	CONTINENTAL_DEEP_SUBSURFACE	6366	3.0	0.198	1.01e8	3.03e8	2.65	8.03e20	2.0	Variable; deep drilling in major oil provinces; mostly unexplored at depth
EARTH_USA_CONTINENTAL_EXTREME_SUBSURFACE	EARTH	6371	USA	CONTINENTAL_EXTREME_SUBSURFACE	6359	7.0	0.0242	1.23e7	8.61e7	2.75	2.37e20	1.5	Sparse ultra-deep drilling; seismic and gravity model constraints
EARTH_CHINA_CONTINENTAL_EXTREME_SUBSURFACE	EARTH	6371	CHINA	CONTINENTAL_EXTREME_SUBSURFACE	6359	7.0	0.0206	1.05e7	7.35e7	2.75	2.02e20	1.0	Limited ultra-deep drilling; seismic inference primarily
EARTH_EU_CONTINENTAL_EXTREME_SUBSURFACE	EARTH	6371	EU	CONTINENTAL_EXTREME_SUBSURFACE	6359	7.0	0.0137	7.00e6	4.90e7	2.75	1.35e20	1.5	KTB borehole (9.1 km) provides direct sampling; sparse elsewhere
EARTH_RUSSIA_CONTINENTAL_EXTREME_SUBSURFACE	EARTH	6371	RUSSIA	CONTINENTAL_EXTREME_SUBSURFACE	6359	7.0	0.0443	2.26e7	1.58e8	2.75	4.35e20	1.5	Kola Superdeep Borehole (12.3 km); unique direct data at this depth
EARTH_INDIA_CONTINENTAL_EXTREME_SUBSURFACE	EARTH	6371	INDIA	CONTINENTAL_EXTREME_SUBSURFACE	6359	7.0	0.00723	3.69e6	2.58e7	2.75	7.10e19	0.8	Very limited deep data; seismic constraints only
EARTH_JAPAN_CONTINENTAL_EXTREME_SUBSURFACE	EARTH	6371	JAPAN	CONTINENTAL_EXTREME_SUBSURFACE	6359	7.0	0.00153	7.78e5	5.45e6	2.75	1.50e19	1.0	Deep geothermal wells provide some data; seismic characterization
EARTH_ANTARCTICA_CONTINENTAL_EXTREME_SUBSURFACE	EARTH	6371	ANTARCTICA	CONTINENTAL_EXTREME_SUBSURFACE	6359	7.0	0.0314	1.60e7	1.12e8	2.75	3.08e20	0.3	No direct data; seismic and gravity models only
EARTH_OTHER_CONTINENTAL_EXTREME_SUBSURFACE	EARTH	6371	OTHER	CONTINENTAL_EXTREME_SUBSURFACE	6359	7.0	0.198	1.01e8	7.07e8	2.75	1.94e21	0.8	Very sparse ultra-deep drilling globally; mostly seismic inference
EARTH_USA_CONTINENTAL_ULTRA_SUBSURFACE	EARTH	6371	USA	CONTINENTAL_ULTRA_SUBSURFACE	6351	8.0	0.0242	1.23e7	9.84e7	2.85	2.80e20	0.5	No direct sampling; seismic velocity and gravity models only
EARTH_CHINA_CONTINENTAL_ULTRA_SUBSURFACE	EARTH	6371	CHINA	CONTINENTAL_ULTRA_SUBSURFACE	6351	8.0	0.0206	1.05e7	8.40e7	2.85	2.39e20	0.5	No direct sampling; seismic and gravity models only
EARTH_EU_CONTINENTAL_ULTRA_SUBSURFACE	EARTH	6371	EU	CONTINENTAL_ULTRA_SUBSURFACE	6351	8.0	0.0137	7.00e6	5.60e7	2.85	1.60e20	0.5	No direct sampling; seismic models and deep crustal xenolith constraints
EARTH_RUSSIA_CONTINENTAL_ULTRA_SUBSURFACE	EARTH	6371	RUSSIA	CONTINENTAL_ULTRA_SUBSURFACE	6351	8.0	0.0443	2.26e7	1.81e8	2.85	5.16e20	0.5	No direct sampling; Kola data extrapolated; seismic models
EARTH_INDIA_CONTINENTAL_ULTRA_SUBSURFACE	EARTH	6371	INDIA	CONTINENTAL_ULTRA_SUBSURFACE	6351	8.0	0.00723	3.69e6	2.95e7	2.85	8.41e19	0.5	No direct sampling; seismic models only
EARTH_JAPAN_CONTINENTAL_ULTRA_SUBSURFACE	EARTH	6371	JAPAN	CONTINENTAL_ULTRA_SUBSURFACE	6351	8.0	0.00153	7.78e5	6.22e6	2.85	1.77e19	0.5	No direct sampling; seismic models only
EARTH_ANTARCTICA_CONTINENTAL_ULTRA_SUBSURFACE	EARTH	6371	ANTARCTICA	CONTINENTAL_ULTRA_SUBSURFACE	6351	8.0	0.0314	1.60e7	1.28e8	2.85	3.65e20	0.3	No direct data; poorly constrained seismic and gravity models
EARTH_OTHER_CONTINENTAL_ULTRA_SUBSURFACE	EARTH	6371	OTHER	CONTINENTAL_ULTRA_SUBSURFACE	6351	8.0	0.198	1.01e8	8.08e8	2.85	2.30e21	0.5	No direct sampling; seismic models only
EARTH_USA_CONTINENTAL_LOWER_CRUST	EARTH	6371	USA	CONTINENTAL_LOWER_CRUST	6336	15.0	0.0242	1.23e7	1.85e8	2.95	5.46e20	0.5	Seismic velocity and xenolith constraints; no direct sampling
EARTH_CHINA_CONTINENTAL_LOWER_CRUST	EARTH	6371	CHINA	CONTINENTAL_LOWER_CRUST	6336	15.0	0.0206	1.05e7	1.58e8	2.95	4.66e20	0.5	Seismic models; North China Craton xenolith data
EARTH_EU_CONTINENTAL_LOWER_CRUST	EARTH	6371	EU	CONTINENTAL_LOWER_CRUST	6336	15.0	0.0137	7.00e6	1.05e8	2.95	3.10e20	0.5	Seismic models; Variscan and Caledonian xenolith constraints
EARTH_RUSSIA_CONTINENTAL_LOWER_CRUST	EARTH	6371	RUSSIA	CONTINENTAL_LOWER_CRUST	6336	15.0	0.0443	2.26e7	3.39e8	2.95	1.00e21	0.5	Seismic models; Siberian craton xenolith data
EARTH_INDIA_CONTINENTAL_LOWER_CRUST	EARTH	6371	INDIA	CONTINENTAL_LOWER_CRUST	6336	15.0	0.00723	3.69e6	5.54e7	2.95	1.63e20	0.5	Seismic models; Dharwar craton xenolith constraints
EARTH_JAPAN_CONTINENTAL_LOWER_CRUST	EARTH	6371	JAPAN	CONTINENTAL_LOWER_CRUST	6336	15.0	0.00153	7.78e5	1.17e7	2.95	3.45e19	0.5	Seismic models; active volcanic arc complicates structure
EARTH_ANTARCTICA_CONTINENTAL_LOWER_CRUST	EARTH	6371	ANTARCTICA	CONTINENTAL_LOWER_CRUST	6336	15.0	0.0314	1.60e7	2.40e8	2.95	7.08e20	0.3	Poorly constrained seismic and gravity models only
EARTH_OTHER_CONTINENTAL_LOWER_CRUST	EARTH	6371	OTHER	CONTINENTAL_LOWER_CRUST	6336	15.0	0.198	1.01e8	1.52e9	2.95	4.48e21	0.5	Seismic models; xenolith data from select cratons globally
```

## strata_resources.tsv additions

```tsv
name	IRON_NICKEL	IRON_ORES	ALUMINUM_ORES	INDUSTRIAL_METAL_ORES	PRECIOUS_METAL_ORES	RARE_EARTH_ORES	URANIUM_ORES	SULFUR	INDUSTRIAL_MINERALS	ORGANICS_THOLINS	COAL	OIL	STONE	REGOLITH	WATER	HYDROGEN	OXYGEN	NITROGEN	CARBON_DIOXIDE	CARBON_MONOXIDE	AMMONIA	METHANE	ETHANE	SULFUR_DIOXIDE	HELIUM	ARGON	HEAVY_NOBLE_GASES
EARTH_CONTINENTAL_ICE_BODIES	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.02,0.3,0.5,0.2	99.97,0.001,0,0	0,0,0,0	0,0,0,0	0.008,0.1,0,0	7e-5,0.15,0,0	0,0,0,0	0,0,0,0	1e-7,0.3,0,0	0,0,0,0	0,0,0,0	0,0,0,0	1.4e-5,0.1,0,0	0,0,0,0
EARTH_CONTINENTAL_WATER_BODIES	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.02,0.3,0.5,0.2	0.005,0.4,0.5,0.2	0,0,0,0	0,0,0,0	0,0,0,0	0.02,0.4,0.5,0.2	99.95,0.005,0,0	0,0,0,0	8e-4,0.15,0.1,0.05	1.5e-3,0.1,0.1,0.05	5e-4,0.2,0.15,0.08	0,0,0,0	1e-6,0.5,0.3,0.15	1e-5,0.5,0.8,0.4	0,0,0,0	0,0,0,0	1e-8,0.3,0.1,0.05	3e-5,0.1,0.05,0.02	0,0,0,0
EARTH_USA_CONTINENTAL_SHELF	0,0,0,0	0.1,0.35,1.3,0.3	0.02,0.4,1.3,0.3	0.05,0.35,1.5,0.35	2e-4,0.5,2.0,0.45	0.02,0.45,1.5,0.35	3e-4,0.5,1.5,0.35	0.05,0.35,1.0,0.25	5,0.18,0.8,0.15	0.8,0.3,1.0,0.25	0.003,0.55,2.0,0.45	4e-4,0.5,2.0,0.45	40,0.08,0.3,0.08	35,0.1,0.3,0.08	17,0.1,0.15,0.05	5e-6,0.5,1.0,0.4	3e-4,0.15,0.15,0.08	0.002,0.2,0.2,0.1	0.003,0.25,0.3,0.12	0,0,0,0	5e-5,0.5,0.3,0.15	0.005,0.45,1.5,0.4	3e-4,0.5,1.5,0.4	0,0,0,0	1e-6,0.5,1.5,0.5	3e-5,0.15,0.1,0.05	0,0,0,0
EARTH_CHINA_CONTINENTAL_SHELF	0,0,0,0	0.15,0.4,1.3,0.35	0.03,0.45,1.3,0.35	0.08,0.4,1.5,0.35	1.5e-4,0.55,2.0,0.5	0.05,0.4,1.5,0.35	3e-4,0.55,1.5,0.4	0.04,0.4,1.0,0.3	5,0.2,0.8,0.18	0.9,0.35,1.0,0.3	0.002,0.6,2.0,0.5	5e-4,0.55,2.0,0.5	39,0.1,0.3,0.1	36,0.12,0.3,0.1	17,0.12,0.15,0.06	5e-6,0.5,1.0,0.4	3e-4,0.15,0.15,0.08	0.002,0.2,0.2,0.1	0.003,0.3,0.3,0.15	0,0,0,0	5e-5,0.5,0.3,0.15	0.008,0.5,1.5,0.45	5e-4,0.55,1.5,0.45	0,0,0,0	1e-6,0.55,1.5,0.5	3e-5,0.15,0.1,0.05	0,0,0,0
EARTH_EU_CONTINENTAL_SHELF	0,0,0,0	0.06,0.3,1.3,0.25	0.015,0.35,1.3,0.3	0.03,0.3,1.5,0.3	1e-4,0.45,2.0,0.4	0.015,0.4,1.5,0.35	2e-4,0.45,1.5,0.35	0.04,0.3,1.0,0.25	5.5,0.15,0.8,0.15	0.7,0.25,1.0,0.25	0.005,0.4,2.0,0.4	5e-4,0.4,2.0,0.4	41,0.06,0.3,0.06	34,0.08,0.3,0.08	17,0.08,0.15,0.05	5e-6,0.5,1.0,0.4	3e-4,0.12,0.15,0.08	0.002,0.15,0.2,0.1	0.003,0.25,0.3,0.12	0,0,0,0	5e-5,0.5,0.3,0.15	0.004,0.4,1.5,0.35	2e-4,0.45,1.5,0.4	0,0,0,0	1e-6,0.5,1.5,0.5	3e-5,0.12,0.1,0.05	0,0,0,0
EARTH_RUSSIA_CONTINENTAL_SHELF	0,0,0,0	0.12,0.45,1.3,0.4	0.02,0.5,1.3,0.4	0.06,0.45,1.5,0.4	3e-4,0.55,2.0,0.5	0.02,0.5,1.5,0.4	4e-4,0.5,1.5,0.4	0.05,0.4,1.0,0.3	4.5,0.22,0.8,0.2	1.2,0.35,1.0,0.3	0.002,0.6,2.0,0.5	3e-4,0.55,2.0,0.5	38,0.1,0.3,0.1	37,0.12,0.3,0.1	17,0.12,0.15,0.06	1e-5,0.5,1.0,0.4	3e-4,0.2,0.15,0.08	0.003,0.25,0.2,0.12	0.004,0.3,0.3,0.15	0,0,0,0	1e-4,0.5,0.3,0.15	0.01,0.45,1.5,0.45	8e-4,0.5,1.5,0.45	0,0,0,0	2e-6,0.5,1.5,0.5	5e-5,0.2,0.1,0.05	0,0,0,0
EARTH_INDIA_CONTINENTAL_SHELF	0,0,0,0	0.2,0.35,1.3,0.3	0.03,0.4,1.3,0.35	0.08,0.4,1.5,0.35	1e-4,0.55,2.0,0.5	0.05,0.35,1.5,0.3	2e-4,0.55,1.5,0.4	0.03,0.4,1.0,0.3	4.5,0.22,0.8,0.18	0.6,0.35,1.0,0.3	0.001,0.6,2.0,0.5	3e-4,0.55,2.0,0.5	40,0.1,0.3,0.1	36,0.12,0.3,0.1	17,0.12,0.15,0.06	5e-6,0.5,1.0,0.4	3e-4,0.15,0.15,0.08	0.002,0.2,0.2,0.1	0.003,0.3,0.3,0.15	0,0,0,0	5e-5,0.5,0.3,0.15	0.004,0.5,1.5,0.4	2e-4,0.55,1.5,0.45	0,0,0,0	5e-7,0.55,1.5,0.5	3e-5,0.15,0.1,0.05	0,0,0,0
EARTH_JAPAN_CONTINENTAL_SHELF	0,0,0,0	0.03,0.35,1.3,0.3	0.01,0.45,1.3,0.35	0.02,0.4,1.5,0.35	5e-5,0.5,2.0,0.45	0.01,0.45,1.5,0.4	1e-4,0.5,1.5,0.4	0.05,0.35,1.0,0.3	4.5,0.18,0.8,0.15	0.5,0.35,1.0,0.3	0.001,0.6,2.0,0.5	1e-4,0.55,2.0,0.5	42,0.07,0.3,0.07	34,0.1,0.3,0.08	17,0.1,0.15,0.05	5e-6,0.5,1.0,0.4	3e-4,0.15,0.15,0.08	0.002,0.2,0.2,0.1	0.003,0.3,0.3,0.15	0,0,0,0	5e-5,0.5,0.3,0.15	0.02,0.4,1.5,0.35	5e-4,0.5,1.5,0.4	0,0,0,0	3e-7,0.6,1.5,0.5	3e-5,0.15,0.1,0.05	0,0,0,0
EARTH_ANTARCTICA_CONTINENTAL_SHELF	0,0,0,0	0.1,0.7,1.3,0.6	0.02,0.8,1.3,0.6	0.05,0.7,1.5,0.6	2e-4,0.8,2.0,0.7	0.02,0.8,1.5,0.6	3e-4,0.8,1.5,0.6	0.04,0.7,1.0,0.5	4.5,0.5,0.8,0.4	0.7,0.6,1.0,0.5	0.001,0.8,2.0,0.7	1e-4,0.8,2.0,0.7	40,0.2,0.3,0.15	36,0.2,0.3,0.15	17,0.2,0.15,0.1	5e-6,0.7,1.0,0.5	3e-4,0.3,0.15,0.1	0.002,0.3,0.2,0.15	0.003,0.5,0.3,0.2	0,0,0,0	5e-5,0.7,0.3,0.2	0.003,0.7,1.5,0.6	2e-4,0.7,1.5,0.6	0,0,0,0	5e-7,0.8,1.5,0.6	3e-5,0.3,0.1,0.08	0,0,0,0
EARTH_OTHER_CONTINENTAL_SHELF	0,0,0,0	0.15,0.4,1.3,0.35	0.03,0.45,1.3,0.35	0.08,0.4,1.5,0.4	3e-4,0.55,2.0,0.5	0.03,0.45,1.5,0.4	3e-4,0.55,1.5,0.4	0.05,0.4,1.0,0.3	5,0.22,0.8,0.2	1.0,0.35,1.0,0.3	0.002,0.6,2.0,0.5	5e-4,0.5,2.0,0.45	39,0.1,0.3,0.1	36,0.12,0.3,0.1	17,0.12,0.15,0.06	1e-5,0.5,1.0,0.4	3e-4,0.2,0.15,0.08	0.002,0.2,0.2,0.1	0.003,0.3,0.3,0.15	0,0,0,0	5e-5,0.5,0.3,0.15	0.006,0.5,1.5,0.45	4e-4,0.5,1.5,0.45	0,0,0,0	2e-6,0.5,1.5,0.5	3e-5,0.15,0.1,0.05	0,0,0,0
EARTH_USA_CONTINENTAL_NEAR_SURFACE	0,0,0,0	0.005,0.25,1.5,0.25	0.002,0.35,1.5,0.3	0.008,0.3,1.5,0.25	2.5e-4,0.4,2.0,0.3	0.001,0.4,1.5,0.3	5e-4,0.4,1.8,0.3	0.01,0.3,1.2,0.25	7,0.15,0.75,0.12	0.3,0.3,1.3,0.25	0.008,0.3,1.8,0.25	8e-4,0.4,1.9,0.35	73,0.05,0.22,0.05	10,0.12,0.4,0.1	3.5,0.2,0.55,0.1	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.001,0.4,1.5,0.3	2e-5,0.45,1.5,0.3	0,0,0,0	2e-6,0.5,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_CHINA_CONTINENTAL_NEAR_SURFACE	0,0,0,0	0.008,0.3,1.5,0.3	0.003,0.35,1.5,0.35	0.015,0.3,1.5,0.3	2e-4,0.45,2.0,0.35	0.004,0.3,1.5,0.3	5e-4,0.45,1.8,0.35	0.01,0.35,1.2,0.3	7,0.18,0.75,0.15	0.25,0.35,1.3,0.3	0.01,0.3,1.8,0.3	3e-4,0.45,1.9,0.4	73,0.06,0.22,0.06	10,0.15,0.4,0.12	3.5,0.22,0.55,0.12	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	8e-4,0.45,1.5,0.35	1.5e-5,0.5,1.5,0.35	0,0,0,0	1e-6,0.55,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_EU_CONTINENTAL_NEAR_SURFACE	0,0,0,0	0.003,0.2,1.5,0.2	0.0015,0.3,1.5,0.25	0.008,0.25,1.5,0.2	1.5e-4,0.35,2.0,0.3	8e-4,0.35,1.5,0.3	3e-4,0.35,1.8,0.3	0.008,0.3,1.2,0.2	8,0.12,0.75,0.1	0.2,0.3,1.3,0.2	0.004,0.25,1.8,0.2	3e-4,0.35,1.9,0.35	73,0.04,0.22,0.04	10,0.1,0.4,0.08	3.5,0.18,0.55,0.1	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	8e-4,0.35,1.5,0.25	1.5e-5,0.4,1.5,0.3	0,0,0,0	1e-6,0.5,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_RUSSIA_CONTINENTAL_NEAR_SURFACE	0,0,0,0	0.01,0.3,1.5,0.3	0.002,0.35,1.5,0.3	0.015,0.3,1.5,0.3	3.5e-4,0.4,2.0,0.35	0.0015,0.4,1.5,0.35	8e-4,0.4,1.8,0.35	0.01,0.35,1.2,0.25	6.5,0.18,0.75,0.15	0.4,0.35,1.3,0.3	0.008,0.3,1.8,0.3	8e-4,0.4,1.9,0.35	73,0.06,0.22,0.06	10,0.15,0.4,0.12	3.5,0.25,0.55,0.12	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.002,0.4,1.5,0.3	1e-4,0.45,1.5,0.35	0,0,0,0	3e-6,0.5,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_INDIA_CONTINENTAL_NEAR_SURFACE	0,0,0,0	0.01,0.3,1.5,0.3	0.003,0.35,1.5,0.3	0.008,0.35,1.5,0.3	1.5e-4,0.45,2.0,0.4	0.0015,0.4,1.5,0.35	2.5e-4,0.5,1.8,0.4	0.008,0.35,1.2,0.3	7,0.2,0.75,0.15	0.2,0.4,1.3,0.3	0.006,0.35,1.8,0.3	2.5e-4,0.45,1.9,0.4	73,0.07,0.22,0.07	10,0.15,0.4,0.12	3.5,0.25,0.55,0.12	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	6e-4,0.45,1.5,0.35	1e-5,0.5,1.5,0.4	0,0,0,0	5e-7,0.55,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_JAPAN_CONTINENTAL_NEAR_SURFACE	0,0,0,0	0.001,0.3,1.5,0.25	5e-4,0.4,1.5,0.35	0.002,0.35,1.5,0.3	1e-4,0.4,2.0,0.35	5e-4,0.4,1.5,0.35	2e-4,0.45,1.8,0.35	0.008,0.3,1.2,0.25	7,0.15,0.75,0.12	0.15,0.35,1.3,0.3	0.0015,0.35,1.8,0.3	8e-5,0.45,1.9,0.4	74,0.05,0.22,0.05	10,0.1,0.4,0.08	3.5,0.2,0.55,0.1	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	3e-4,0.45,1.5,0.35	5e-6,0.5,1.5,0.4	0,0,0,0	2e-7,0.6,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_ANTARCTICA_CONTINENTAL_NEAR_SURFACE	0,0,0,0	0.005,0.7,1.5,0.6	0.002,0.8,1.5,0.6	0.008,0.7,1.5,0.6	2e-4,0.8,2.0,0.7	8e-4,0.8,1.5,0.7	3e-4,0.8,1.8,0.7	0.008,0.6,1.2,0.5	6.5,0.5,0.75,0.4	0.2,0.7,1.3,0.5	0.003,0.6,1.8,0.5	2e-4,0.8,1.9,0.6	73,0.2,0.22,0.15	10,0.25,0.4,0.2	3.5,0.4,0.55,0.3	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	5e-4,0.7,1.5,0.6	3e-5,0.8,1.5,0.6	0,0,0,0	1e-7,0.8,2.0,0.6	0,0,0,0	0,0,0,0
EARTH_OTHER_CONTINENTAL_NEAR_SURFACE	0,0,0,0	0.008,0.35,1.5,0.35	0.003,0.4,1.5,0.35	0.012,0.35,1.5,0.3	2.5e-4,0.45,2.0,0.4	0.0015,0.45,1.5,0.4	5e-4,0.5,1.8,0.4	0.01,0.4,1.2,0.3	7,0.2,0.75,0.18	0.35,0.4,1.3,0.3	0.006,0.4,1.8,0.35	5e-4,0.45,1.9,0.4	73,0.07,0.22,0.06	10,0.15,0.4,0.12	3.5,0.25,0.55,0.15	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.001,0.45,1.5,0.35	5e-5,0.5,1.5,0.4	0,0,0,0	2e-6,0.5,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_USA_CONTINENTAL_DEEP_SUBSURFACE	0,0,0,0	0.001,0.3,1.5,0.3	2e-4,0.4,1.5,0.35	0.003,0.35,1.5,0.3	5e-5,0.5,2.0,0.4	1.5e-4,0.5,1.5,0.4	1e-4,0.5,1.8,0.4	0.003,0.3,1.2,0.25	6,0.2,0.6,0.15	1.0,0.3,1.2,0.25	5e-4,0.4,1.8,0.35	0.003,0.35,2.0,0.35	86,0.04,0.15,0.04	0.5,0.4,0.7,0.3	2,0.25,0.5,0.12	5e-5,0.5,1.5,0.5	0,0,0,0	5e-4,0.3,0.5,0.2	0.01,0.35,1.0,0.35	0,0,0,0	0,0,0,0	0.002,0.35,1.5,0.35	2e-4,0.4,1.5,0.35	0,0,0,0	3e-5,0.4,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_CHINA_CONTINENTAL_DEEP_SUBSURFACE	0,0,0,0	0.0015,0.35,1.5,0.35	3e-4,0.4,1.5,0.4	0.004,0.35,1.5,0.35	4e-5,0.55,2.0,0.45	3e-4,0.4,1.5,0.4	1e-4,0.55,1.8,0.4	0.003,0.35,1.2,0.3	6,0.22,0.6,0.18	0.8,0.35,1.2,0.3	8e-4,0.4,1.8,0.35	0.001,0.4,2.0,0.4	86,0.05,0.15,0.05	0.5,0.45,0.7,0.35	2,0.28,0.5,0.15	5e-5,0.5,1.5,0.5	0,0,0,0	3e-4,0.35,0.5,0.25	0.008,0.4,1.0,0.4	0,0,0,0	0,0,0,0	0.001,0.4,1.5,0.4	1e-4,0.45,1.5,0.4	0,0,0,0	1e-5,0.45,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_EU_CONTINENTAL_DEEP_SUBSURFACE	0,0,0,0	8e-4,0.25,1.5,0.25	1.5e-4,0.35,1.5,0.3	0.003,0.3,1.5,0.25	4e-5,0.4,2.0,0.35	1e-4,0.4,1.5,0.35	1e-4,0.4,1.8,0.35	0.003,0.3,1.2,0.25	7,0.15,0.6,0.12	0.8,0.3,1.2,0.25	3e-4,0.35,1.8,0.3	0.002,0.3,2.0,0.3	85,0.04,0.15,0.04	0.5,0.35,0.7,0.25	2,0.2,0.5,0.1	5e-5,0.5,1.5,0.5	0,0,0,0	5e-4,0.3,0.5,0.2	0.008,0.3,1.0,0.3	0,0,0,0	0,0,0,0	0.0015,0.3,1.5,0.3	1.5e-4,0.4,1.5,0.35	0,0,0,0	2e-5,0.4,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_RUSSIA_CONTINENTAL_DEEP_SUBSURFACE	0,0,0,0	0.002,0.35,1.5,0.35	3e-4,0.4,1.5,0.35	0.004,0.35,1.5,0.3	8e-5,0.5,2.0,0.4	2.5e-4,0.45,1.5,0.4	1.5e-4,0.45,1.8,0.4	0.003,0.35,1.2,0.3	6,0.2,0.6,0.18	1.2,0.35,1.2,0.3	5e-4,0.4,1.8,0.35	0.003,0.35,2.0,0.35	85,0.05,0.15,0.05	0.5,0.45,0.7,0.35	2,0.3,0.5,0.15	8e-5,0.5,1.5,0.5	0,0,0,0	5e-4,0.35,0.5,0.25	0.012,0.35,1.0,0.35	0,0,0,0	0,0,0,0	0.003,0.35,1.5,0.35	3e-4,0.4,1.5,0.35	0,0,0,0	5e-5,0.4,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_INDIA_CONTINENTAL_DEEP_SUBSURFACE	0,0,0,0	0.0015,0.35,1.5,0.35	3e-4,0.4,1.5,0.35	0.002,0.4,1.5,0.35	3e-5,0.55,2.0,0.45	2e-4,0.5,1.5,0.4	5e-5,0.55,1.8,0.45	0.003,0.4,1.2,0.3	6,0.25,0.6,0.2	0.6,0.4,1.2,0.35	5e-4,0.45,1.8,0.4	8e-4,0.45,2.0,0.4	87,0.06,0.15,0.06	0.5,0.45,0.7,0.35	2,0.3,0.5,0.15	3e-5,0.5,1.5,0.5	0,0,0,0	3e-4,0.4,0.5,0.25	0.006,0.4,1.0,0.4	0,0,0,0	0,0,0,0	8e-4,0.45,1.5,0.4	8e-5,0.5,1.5,0.4	0,0,0,0	5e-6,0.45,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_JAPAN_CONTINENTAL_DEEP_SUBSURFACE	0,0,0,0	3e-4,0.35,1.5,0.3	5e-5,0.45,1.5,0.4	8e-4,0.4,1.5,0.35	2e-5,0.5,2.0,0.4	1e-4,0.5,1.5,0.4	5e-5,0.5,1.8,0.4	0.004,0.3,1.2,0.25	6,0.2,0.6,0.15	0.4,0.35,1.2,0.3	2e-4,0.45,1.8,0.4	1e-4,0.5,2.0,0.45	87,0.05,0.15,0.05	0.5,0.35,0.7,0.25	2,0.25,0.5,0.12	3e-5,0.5,1.5,0.5	0,0,0,0	3e-4,0.35,0.5,0.2	0.005,0.35,1.0,0.35	0,0,0,0	0,0,0,0	5e-4,0.4,1.5,0.35	5e-5,0.5,1.5,0.4	0,0,0,0	2e-6,0.5,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_ANTARCTICA_CONTINENTAL_DEEP_SUBSURFACE	0,0,0,0	0.001,0.7,1.5,0.6	2e-4,0.8,1.5,0.6	0.003,0.7,1.5,0.6	5e-5,0.8,2.0,0.7	1.5e-4,0.8,1.5,0.7	1e-4,0.8,1.8,0.7	0.003,0.6,1.2,0.5	6,0.5,0.6,0.4	0.8,0.7,1.2,0.5	3e-4,0.7,1.8,0.6	1e-4,0.8,2.0,0.7	86,0.2,0.15,0.1	0.5,0.5,0.7,0.4	2,0.45,0.5,0.3	5e-5,0.7,1.5,0.5	0,0,0,0	3e-4,0.5,0.5,0.3	0.008,0.6,1.0,0.5	0,0,0,0	0,0,0,0	8e-4,0.7,1.5,0.6	8e-5,0.8,1.5,0.6	0,0,0,0	1e-5,0.7,2.0,0.6	0,0,0,0	0,0,0,0
EARTH_OTHER_CONTINENTAL_DEEP_SUBSURFACE	0,0,0,0	0.0015,0.4,1.5,0.4	3e-4,0.45,1.5,0.4	0.003,0.4,1.5,0.35	6e-5,0.5,2.0,0.45	2e-4,0.5,1.5,0.4	1.5e-4,0.55,1.8,0.45	0.003,0.4,1.2,0.3	6,0.25,0.6,0.2	1.0,0.4,1.2,0.3	5e-4,0.45,1.8,0.4	0.005,0.4,2.0,0.4	85,0.06,0.15,0.06	0.5,0.45,0.7,0.35	2,0.3,0.5,0.15	8e-5,0.5,1.5,0.5	0,0,0,0	5e-4,0.35,0.5,0.25	0.01,0.4,1.0,0.4	0,0,0,0	0,0,0,0	0.003,0.4,1.5,0.4	3e-4,0.5,1.5,0.4	0,0,0,0	3e-5,0.45,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_USA_CONTINENTAL_EXTREME_SUBSURFACE	0,0,0,0	5e-4,0.4,1.2,0.4	1e-4,0.5,1.2,0.4	0.002,0.4,1.2,0.4	2e-5,0.6,1.8,0.5	1e-4,0.6,1.2,0.5	5e-5,0.6,1.5,0.5	0.002,0.4,1.0,0.35	4,0.3,0.5,0.2	0.5,0.4,1.0,0.4	0,0,0,0	0,0,0,0	90,0.03,0.1,0.03	0.1,0.5,0.5,0.3	1.5,0.3,0.5,0.2	1e-4,0.5,1.5,0.5	0,0,0,0	3e-4,0.4,0.5,0.25	0.02,0.4,1.0,0.4	0,0,0,0	0,0,0,0	0.003,0.4,1.5,0.5	3e-4,0.5,1.5,0.5	0,0,0,0	8e-5,0.4,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_CHINA_CONTINENTAL_EXTREME_SUBSURFACE	0,0,0,0	5e-4,0.45,1.2,0.45	1e-4,0.55,1.2,0.45	0.002,0.45,1.2,0.45	2e-5,0.65,1.8,0.55	1e-4,0.6,1.2,0.5	5e-5,0.65,1.5,0.55	0.002,0.45,1.0,0.4	4,0.35,0.5,0.25	0.5,0.45,1.0,0.4	0,0,0,0	0,0,0,0	90,0.04,0.1,0.04	0.1,0.55,0.5,0.35	1.5,0.35,0.5,0.25	1e-4,0.5,1.5,0.5	0,0,0,0	3e-4,0.45,0.5,0.3	0.02,0.45,1.0,0.45	0,0,0,0	0,0,0,0	0.003,0.45,1.5,0.5	3e-4,0.5,1.5,0.5	0,0,0,0	8e-5,0.45,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_EU_CONTINENTAL_EXTREME_SUBSURFACE	0,0,0,0	5e-4,0.35,1.2,0.35	1e-4,0.45,1.2,0.4	0.002,0.35,1.2,0.35	2e-5,0.55,1.8,0.45	1e-4,0.5,1.2,0.45	5e-5,0.5,1.5,0.45	0.002,0.35,1.0,0.3	4,0.25,0.5,0.2	0.5,0.35,1.0,0.35	0,0,0,0	0,0,0,0	90,0.03,0.1,0.03	0.1,0.45,0.5,0.3	1.5,0.25,0.5,0.18	1e-4,0.5,1.5,0.5	0,0,0,0	3e-4,0.35,0.5,0.2	0.02,0.35,1.0,0.35	0,0,0,0	0,0,0,0	0.003,0.35,1.5,0.45	3e-4,0.45,1.5,0.45	0,0,0,0	8e-5,0.35,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_RUSSIA_CONTINENTAL_EXTREME_SUBSURFACE	0,0,0,0	5e-4,0.4,1.2,0.4	1e-4,0.5,1.2,0.4	0.002,0.4,1.2,0.4	2e-5,0.55,1.8,0.5	1e-4,0.55,1.2,0.5	5e-5,0.55,1.5,0.5	0.002,0.4,1.0,0.35	4,0.3,0.5,0.2	0.6,0.4,1.0,0.35	0,0,0,0	0,0,0,0	90,0.03,0.1,0.03	0.1,0.5,0.5,0.3	1.5,0.3,0.5,0.2	1e-4,0.5,1.5,0.5	0,0,0,0	3e-4,0.4,0.5,0.25	0.02,0.4,1.0,0.4	0,0,0,0	0,0,0,0	0.004,0.4,1.5,0.5	4e-4,0.45,1.5,0.45	0,0,0,0	1e-4,0.35,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_INDIA_CONTINENTAL_EXTREME_SUBSURFACE	0,0,0,0	5e-4,0.5,1.2,0.5	1e-4,0.6,1.2,0.5	0.002,0.5,1.2,0.5	2e-5,0.65,1.8,0.55	1e-4,0.6,1.2,0.5	5e-5,0.65,1.5,0.55	0.002,0.5,1.0,0.4	4,0.35,0.5,0.25	0.5,0.5,1.0,0.45	0,0,0,0	0,0,0,0	90,0.05,0.1,0.04	0.1,0.55,0.5,0.35	1.5,0.35,0.5,0.25	1e-4,0.5,1.5,0.5	0,0,0,0	3e-4,0.5,0.5,0.3	0.02,0.5,1.0,0.45	0,0,0,0	0,0,0,0	0.003,0.5,1.5,0.5	3e-4,0.55,1.5,0.5	0,0,0,0	5e-5,0.5,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_JAPAN_CONTINENTAL_EXTREME_SUBSURFACE	0,0,0,0	5e-4,0.45,1.2,0.4	1e-4,0.55,1.2,0.45	0.002,0.45,1.2,0.4	2e-5,0.6,1.8,0.5	1e-4,0.55,1.2,0.5	5e-5,0.6,1.5,0.5	0.002,0.4,1.0,0.35	4,0.3,0.5,0.2	0.4,0.45,1.0,0.4	0,0,0,0	0,0,0,0	90,0.04,0.1,0.03	0.1,0.5,0.5,0.3	1.5,0.3,0.5,0.2	1e-4,0.5,1.5,0.5	0,0,0,0	3e-4,0.45,0.5,0.25	0.02,0.4,1.0,0.4	0,0,0,0	0,0,0,0	0.003,0.45,1.5,0.5	3e-4,0.5,1.5,0.5	0,0,0,0	5e-5,0.5,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_ANTARCTICA_CONTINENTAL_EXTREME_SUBSURFACE	0,0,0,0	5e-4,0.7,1.2,0.6	1e-4,0.8,1.2,0.6	0.002,0.7,1.2,0.6	2e-5,0.8,1.8,0.7	1e-4,0.8,1.2,0.7	5e-5,0.8,1.5,0.7	0.002,0.7,1.0,0.5	4,0.5,0.5,0.4	0.5,0.7,1.0,0.6	0,0,0,0	0,0,0,0	90,0.15,0.1,0.08	0.1,0.7,0.5,0.5	1.5,0.5,0.5,0.35	1e-4,0.7,1.5,0.5	0,0,0,0	3e-4,0.6,0.5,0.4	0.02,0.6,1.0,0.5	0,0,0,0	0,0,0,0	0.003,0.7,1.5,0.6	3e-4,0.8,1.5,0.6	0,0,0,0	8e-5,0.7,2.0,0.6	0,0,0,0	0,0,0,0
EARTH_OTHER_CONTINENTAL_EXTREME_SUBSURFACE	0,0,0,0	5e-4,0.5,1.2,0.45	1e-4,0.55,1.2,0.45	0.002,0.45,1.2,0.45	2e-5,0.6,1.8,0.5	1e-4,0.6,1.2,0.5	5e-5,0.6,1.5,0.5	0.002,0.45,1.0,0.4	4,0.35,0.5,0.25	0.5,0.45,1.0,0.4	0,0,0,0	0,0,0,0	90,0.05,0.1,0.04	0.1,0.55,0.5,0.35	1.5,0.35,0.5,0.25	1e-4,0.5,1.5,0.5	0,0,0,0	3e-4,0.45,0.5,0.3	0.02,0.45,1.0,0.45	0,0,0,0	0,0,0,0	0.003,0.45,1.5,0.5	3e-4,0.5,1.5,0.5	0,0,0,0	8e-5,0.45,2.0,0.5	0,0,0,0	0,0,0,0
EARTH_USA_CONTINENTAL_ULTRA_SUBSURFACE	0,0,0,0	0,0,0,0	0,0,0,0	0.002,0.5,0.8,0.4	1e-5,0.7,1.5,0.6	5e-5,0.6,0.8,0.5	3e-5,0.6,1.0,0.5	0.001,0.5,0.8,0.4	2,0.4,0.4,0.2	0.05,0.6,0.5,0.3	0,0,0,0	0,0,0,0	95,0.02,0.08,0.02	0,0,0,0	0.8,0.4,0.4,0.2	2e-4,0.5,1.2,0.5	0,0,0,0	1e-4,0.5,0.5,0.3	0.03,0.5,0.8,0.4	0,0,0,0	0,0,0,0	0.005,0.5,1.2,0.5	0,0,0,0	0,0,0,0	1.5e-4,0.4,1.8,0.5	0,0,0,0	0,0,0,0
EARTH_CHINA_CONTINENTAL_ULTRA_SUBSURFACE	0,0,0,0	0,0,0,0	0,0,0,0	0.002,0.5,0.8,0.4	1e-5,0.7,1.5,0.6	5e-5,0.6,0.8,0.5	3e-5,0.65,1.0,0.5	0.001,0.5,0.8,0.4	2,0.4,0.4,0.2	0.05,0.6,0.5,0.3	0,0,0,0	0,0,0,0	95,0.03,0.08,0.03	0,0,0,0	0.8,0.4,0.4,0.2	2e-4,0.5,1.2,0.5	0,0,0,0	1e-4,0.5,0.5,0.3	0.03,0.5,0.8,0.4	0,0,0,0	0,0,0,0	0.005,0.5,1.2,0.5	0,0,0,0	0,0,0,0	1.5e-4,0.45,1.8,0.5	0,0,0,0	0,0,0,0
EARTH_EU_CONTINENTAL_ULTRA_SUBSURFACE	0,0,0,0	0,0,0,0	0,0,0,0	0.002,0.5,0.8,0.4	1e-5,0.7,1.5,0.6	5e-5,0.6,0.8,0.5	3e-5,0.6,1.0,0.5	0.001,0.5,0.8,0.4	2,0.4,0.4,0.2	0.05,0.55,0.5,0.3	0,0,0,0	0,0,0,0	95,0.02,0.08,0.02	0,0,0,0	0.8,0.35,0.4,0.2	2e-4,0.5,1.2,0.5	0,0,0,0	1e-4,0.5,0.5,0.3	0.03,0.5,0.8,0.4	0,0,0,0	0,0,0,0	0.005,0.45,1.2,0.5	0,0,0,0	0,0,0,0	1.5e-4,0.4,1.8,0.5	0,0,0,0	0,0,0,0
EARTH_RUSSIA_CONTINENTAL_ULTRA_SUBSURFACE	0,0,0,0	0,0,0,0	0,0,0,0	0.002,0.5,0.8,0.4	1e-5,0.7,1.5,0.6	5e-5,0.6,0.8,0.5	3e-5,0.6,1.0,0.5	0.001,0.5,0.8,0.4	2,0.4,0.4,0.2	0.05,0.6,0.5,0.3	0,0,0,0	0,0,0,0	95,0.02,0.08,0.02	0,0,0,0	0.8,0.4,0.4,0.2	2e-4,0.5,1.2,0.5	0,0,0,0	1e-4,0.5,0.5,0.3	0.03,0.5,0.8,0.4	0,0,0,0	0,0,0,0	0.005,0.5,1.2,0.5	0,0,0,0	0,0,0,0	2e-4,0.4,1.8,0.5	0,0,0,0	0,0,0,0
EARTH_INDIA_CONTINENTAL_ULTRA_SUBSURFACE	0,0,0,0	0,0,0,0	0,0,0,0	0.002,0.55,0.8,0.4	1e-5,0.7,1.5,0.6	5e-5,0.65,0.8,0.5	3e-5,0.65,1.0,0.5	0.001,0.55,0.8,0.4	2,0.45,0.4,0.2	0.05,0.6,0.5,0.3	0,0,0,0	0,0,0,0	95,0.03,0.08,0.03	0,0,0,0	0.8,0.45,0.4,0.2	2e-4,0.5,1.2,0.5	0,0,0,0	1e-4,0.5,0.5,0.3	0.03,0.55,0.8,0.4	0,0,0,0	0,0,0,0	0.005,0.55,1.2,0.5	0,0,0,0	0,0,0,0	1.5e-4,0.5,1.8,0.5	0,0,0,0	0,0,0,0
EARTH_JAPAN_CONTINENTAL_ULTRA_SUBSURFACE	0,0,0,0	0,0,0,0	0,0,0,0	0.002,0.5,0.8,0.4	1e-5,0.7,1.5,0.6	5e-5,0.6,0.8,0.5	3e-5,0.65,1.0,0.5	0.001,0.5,0.8,0.4	2,0.4,0.4,0.2	0.05,0.6,0.5,0.3	0,0,0,0	0,0,0,0	95,0.03,0.08,0.03	0,0,0,0	0.8,0.4,0.4,0.2	2e-4,0.5,1.2,0.5	0,0,0,0	1e-4,0.5,0.5,0.3	0.03,0.5,0.8,0.4	0,0,0,0	0,0,0,0	0.005,0.5,1.2,0.5	0,0,0,0	0,0,0,0	1e-4,0.5,1.8,0.5	0,0,0,0	0,0,0,0
EARTH_ANTARCTICA_CONTINENTAL_ULTRA_SUBSURFACE	0,0,0,0	0,0,0,0	0,0,0,0	0.002,0.7,0.8,0.6	1e-5,0.8,1.5,0.7	5e-5,0.8,0.8,0.6	3e-5,0.8,1.0,0.6	0.001,0.7,0.8,0.5	2,0.5,0.4,0.3	0.05,0.8,0.5,0.4	0,0,0,0	0,0,0,0	95,0.1,0.08,0.06	0,0,0,0	0.8,0.5,0.4,0.3	2e-4,0.7,1.2,0.5	0,0,0,0	1e-4,0.6,0.5,0.4	0.03,0.6,0.8,0.5	0,0,0,0	0,0,0,0	0.005,0.7,1.2,0.6	0,0,0,0	0,0,0,0	1.5e-4,0.7,1.8,0.6	0,0,0,0	0,0,0,0
EARTH_OTHER_CONTINENTAL_ULTRA_SUBSURFACE	0,0,0,0	0,0,0,0	0,0,0,0	0.002,0.5,0.8,0.4	1e-5,0.7,1.5,0.6	5e-5,0.6,0.8,0.5	3e-5,0.65,1.0,0.5	0.001,0.5,0.8,0.4	2,0.4,0.4,0.2	0.05,0.6,0.5,0.3	0,0,0,0	0,0,0,0	95,0.03,0.08,0.03	0,0,0,0	0.8,0.4,0.4,0.2	2e-4,0.5,1.2,0.5	0,0,0,0	1e-4,0.5,0.5,0.3	0.03,0.5,0.8,0.4	0,0,0,0	0,0,0,0	0.005,0.5,1.2,0.5	0,0,0,0	0,0,0,0	1.5e-4,0.45,1.8,0.5	0,0,0,0	0,0,0,0
EARTH_USA_CONTINENTAL_LOWER_CRUST	0,0,0,0	0,0,0,0	0,0,0,0	0.01,0.5,0.5,0.3	5e-6,0.7,1.0,0.6	3e-5,0.6,0.5,0.3	1e-5,0.7,0.5,0.3	0.01,0.5,0.5,0.3	1,0.5,0.3,0.15	0,0,0,0	0,0,0,0	0,0,0,0	97.5,0.02,0.05,0.02	0,0,0,0	0.3,0.5,0.3,0.15	1e-4,0.5,1.0,0.5	0,0,0,0	5e-5,0.5,0.4,0.2	0.05,0.5,0.5,0.3	0,0,0,0	0,0,0,0	0.003,0.5,1.0,0.5	0,0,0,0	0,0,0,0	2e-4,0.4,1.5,0.5	0,0,0,0	0,0,0,0
EARTH_CHINA_CONTINENTAL_LOWER_CRUST	0,0,0,0	0,0,0,0	0,0,0,0	0.01,0.5,0.5,0.3	5e-6,0.7,1.0,0.6	3e-5,0.6,0.5,0.3	1e-5,0.7,0.5,0.3	0.01,0.5,0.5,0.3	1,0.5,0.3,0.15	0,0,0,0	0,0,0,0	0,0,0,0	97.5,0.02,0.05,0.02	0,0,0,0	0.3,0.5,0.3,0.15	1e-4,0.5,1.0,0.5	0,0,0,0	5e-5,0.5,0.4,0.2	0.05,0.5,0.5,0.3	0,0,0,0	0,0,0,0	0.003,0.5,1.0,0.5	0,0,0,0	0,0,0,0	2e-4,0.4,1.5,0.5	0,0,0,0	0,0,0,0
EARTH_EU_CONTINENTAL_LOWER_CRUST	0,0,0,0	0,0,0,0	0,0,0,0	0.01,0.5,0.5,0.3	5e-6,0.7,1.0,0.6	3e-5,0.6,0.5,0.3	1e-5,0.7,0.5,0.3	0.01,0.5,0.5,0.3	1,0.5,0.3,0.15	0,0,0,0	0,0,0,0	0,0,0,0	97.5,0.02,0.05,0.02	0,0,0,0	0.3,0.5,0.3,0.15	1e-4,0.5,1.0,0.5	0,0,0,0	5e-5,0.5,0.4,0.2	0.05,0.5,0.5,0.3	0,0,0,0	0,0,0,0	0.003,0.5,1.0,0.5	0,0,0,0	0,0,0,0	2e-4,0.4,1.5,0.5	0,0,0,0	0,0,0,0
EARTH_RUSSIA_CONTINENTAL_LOWER_CRUST	0,0,0,0	0,0,0,0	0,0,0,0	0.01,0.5,0.5,0.3	5e-6,0.7,1.0,0.6	3e-5,0.6,0.5,0.3	1e-5,0.7,0.5,0.3	0.01,0.5,0.5,0.3	1,0.5,0.3,0.15	0,0,0,0	0,0,0,0	0,0,0,0	97.5,0.02,0.05,0.02	0,0,0,0	0.3,0.5,0.3,0.15	1e-4,0.5,1.0,0.5	0,0,0,0	5e-5,0.5,0.4,0.2	0.05,0.5,0.5,0.3	0,0,0,0	0,0,0,0	0.003,0.5,1.0,0.5	0,0,0,0	0,0,0,0	2e-4,0.4,1.5,0.5	0,0,0,0	0,0,0,0
EARTH_INDIA_CONTINENTAL_LOWER_CRUST	0,0,0,0	0,0,0,0	0,0,0,0	0.01,0.55,0.5,0.3	5e-6,0.7,1.0,0.6	3e-5,0.65,0.5,0.3	1e-5,0.7,0.5,0.3	0.01,0.55,0.5,0.3	1,0.5,0.3,0.15	0,0,0,0	0,0,0,0	0,0,0,0	97.5,0.03,0.05,0.02	0,0,0,0	0.3,0.5,0.3,0.15	1e-4,0.5,1.0,0.5	0,0,0,0	5e-5,0.5,0.4,0.2	0.05,0.55,0.5,0.3	0,0,0,0	0,0,0,0	0.003,0.55,1.0,0.5	0,0,0,0	0,0,0,0	2e-4,0.45,1.5,0.5	0,0,0,0	0,0,0,0
EARTH_JAPAN_CONTINENTAL_LOWER_CRUST	0,0,0,0	0,0,0,0	0,0,0,0	0.01,0.5,0.5,0.3	5e-6,0.7,1.0,0.6	3e-5,0.6,0.5,0.3	1e-5,0.7,0.5,0.3	0.01,0.5,0.5,0.3	1,0.5,0.3,0.15	0,0,0,0	0,0,0,0	0,0,0,0	97.5,0.03,0.05,0.02	0,0,0,0	0.3,0.5,0.3,0.15	1e-4,0.5,1.0,0.5	0,0,0,0	5e-5,0.5,0.4,0.2	0.05,0.5,0.5,0.3	0,0,0,0	0,0,0,0	0.003,0.5,1.0,0.5	0,0,0,0	0,0,0,0	1.5e-4,0.5,1.5,0.5	0,0,0,0	0,0,0,0
EARTH_ANTARCTICA_CONTINENTAL_LOWER_CRUST	0,0,0,0	0,0,0,0	0,0,0,0	0.01,0.7,0.5,0.4	5e-6,0.8,1.0,0.7	3e-5,0.8,0.5,0.4	1e-5,0.8,0.5,0.4	0.01,0.7,0.5,0.4	1,0.6,0.3,0.2	0,0,0,0	0,0,0,0	0,0,0,0	97.5,0.08,0.05,0.04	0,0,0,0	0.3,0.6,0.3,0.2	1e-4,0.7,1.0,0.5	0,0,0,0	5e-5,0.6,0.4,0.3	0.05,0.6,0.5,0.4	0,0,0,0	0,0,0,0	0.003,0.7,1.0,0.6	0,0,0,0	0,0,0,0	2e-4,0.7,1.5,0.6	0,0,0,0	0,0,0,0
EARTH_OTHER_CONTINENTAL_LOWER_CRUST	0,0,0,0	0,0,0,0	0,0,0,0	0.01,0.5,0.5,0.3	5e-6,0.7,1.0,0.6	3e-5,0.6,0.5,0.3	1e-5,0.7,0.5,0.3	0.01,0.5,0.5,0.3	1,0.5,0.3,0.15	0,0,0,0	0,0,0,0	0,0,0,0	97.5,0.03,0.05,0.02	0,0,0,0	0.3,0.5,0.3,0.15	1e-4,0.5,1.0,0.5	0,0,0,0	5e-5,0.5,0.4,0.2	0.05,0.5,0.5,0.3	0,0,0,0	0,0,0,0	0.003,0.5,1.0,0.5	0,0,0,0	0,0,0,0	2e-4,0.45,1.5,0.5	0,0,0,0	0,0,0,0
```

## Key simplifications and assumptions

**Geometric.** All layers from Near-Surface through Lower Crust share the same per-territory area fractions as the existing Continental Subsurface (land + shelf footprint). The slight area reduction with depth (~0.4% over 35 km) from spherical geometry is neglected.

**Shelf composition.** Modeled as saturated marine sediment with ~17% pore water by mass (consistent with ~35% average porosity at grain density ~2.65 and seawater filling). India's shelf has elevated heavy mineral sand content (ilmenite, monazite) reflecting the world-class deposits along Kerala and Tamil Nadu coasts. Japan's shelf has unusually high methane (0.02%) reflecting the Nankai Trough methane hydrate program. Russia's shelf has high organics and gas reflecting the Arctic gas-rich sediments.

**Depth trends.** Stone fraction increases from ~62% (surface) → ~73% (near-surface) → ~80% (subsurface) → ~86% (deep) → ~90% (extreme) → ~95% (ultra) → ~97.5% (lower crust), reflecting increasing rock competence. Regolith drops to zero below extreme subsurface (no loose material at those pressures). Water decreases from ~4% to ~0.3% (metamorphic fluid inclusions only in lower crust). Coal disappears below deep subsurface (~5 km); oil disappears below extreme subsurface (~5 km); methane persists deeper as dry gas. Helium increases with depth due to radioactive decay accumulation (reaching ~2e-4 in lower crust). Individual ore categories disappear in ultra subsurface and below because those elements are fully incorporated into the bulk silicate structure at granulite-facies conditions; only industrial metal ores persist as trace sulfide phases.

**Territorial convergence at depth.** Extreme, ultra, and lower crust compositions are nearly identical across territories — only the abundance_cv values differ, with Antarctica systematically having the highest uncertainty (0.7–0.8 versus 0.4–0.5 for others). This is geologically justified: mid-to-lower continental crust composition is relatively uniform globally (Rudnick & Gao 2003), and territorial differences in resource endowment are predominantly a shallow crustal phenomenon driven by plate tectonics, weathering, and sedimentary processes.

**Deep subsurface oil/gas.** The 2–5 km layer is modeled as the prime hydrocarbon production zone. Oil abundance is highest for USA (0.003), Russia (0.003), and Other (0.005, reflecting Middle East and other major producers). It drops sharply to zero in the extreme subsurface because thermal cracking converts oil to gas above ~150–180 °C (typically at ~4–6 km depth).

**Existing composition sums.** The existing subsurface rows sum to ~93–94% rather than 100%. My new rows follow a similar convention (sums are close to but not exactly 100%), since the code normalizes. The relative ratios are what matter.

## Feedback on context files

Here are suggestions for improving the prompt and context files for subsequent work:

**strata.md — area treatment.** Note 4 defines the relationship between shelf, ocean, and ocean floor areas, but the territorial area allocation for shelf versus land is implicit (derived from the difference between subsurface and land areas in strata.tsv). It would help to explicitly state per-territory shelf areas in strata.md, or at least note that they are derived from the subsurface–land difference, since this is a non-obvious construction detail that future additions will need to follow.

**strata.md — density guidance.** The strata descriptions don't include reference densities. Adding expected density ranges for each layer (even approximate) would help ensure consistency when different people construct or modify the tables. The density progression through the continental crust (2.0 → 2.95 g/cm³) is physically important and currently only derivable by working backward from the ~2.83 g/cm³ average.

**extractable_resources.md — depth applicability.** The resource descriptions don't address which resources are expected to be absent at depth. For instance, coal and oil are inherently shallow-crustal; at granulite facies, "iron ores" and "aluminum ores" as separable phases don't exist. A note about depth-dependent resource applicability would help constructors know when to use `0,0,0,0`.

**strata_resources.tsv — normalization note.** A comment at the top of the file (or in the prompt) confirming that relative_abundance sums will be normalized to 100% by code would prevent constructors from spending excessive time trying to balance rows to exactly 100. The existing rows range from ~93% to ~100% in raw sum.

**Ice Bodies and Water Bodies — territorial subdivision.** I treated these as non-territorial single strata because they are above-ground deposits rather than geological layers, and ice bodies are overwhelmingly Antarctic. If the simulation needs territorial water resource accounting (e.g., Russia's Lake Baikal versus USA's Great Lakes), these could be subdivided later. The current non-territorial treatment is simpler and the total resource mass is very small compared to other strata.

**Deep methane and hydrogen.** There is growing interest in geological ("gold") hydrogen and deep abiogenic methane as potential future energy resources. The model currently captures deep methane at modest levels (0.003–0.005% in extreme and ultra subsurface) and trace hydrogen (1e-4 to 2e-4%). If deep hydrogen becomes a simulation-significant resource, you may want to revisit these values — some recent estimates suggest crustal H₂ flux could be substantial, but the topic remains highly speculative.