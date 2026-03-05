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
* `territory` — See strata.md, note 8.
* `strata_group` — See strata.md, note 8.
* `depth`  — Reference radius (mean surface, sea level) minus inner boundary radius of the specific stratum. For bulk strata, mean radius of the body. 0 for generic strata.
* `thickness` — Thickness of the stratum. For bulk strata, this should be the same as `depth`. 0 for generic strata.
* `spherical_fraction` — Fraction of spherical area for strata that include regional boundaries (e.g., Earth territories, Moon terrae versus maria, Mars polar cap, etc.). Use 1 when not applicable (or generic strata).
* `area` — Surface area of the stratum at its outer radius boundary, treating all bodies as spherical. 0 for generic strata.
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

* `relative_abundance` — This is %wt of the resource in the stratum. It will be used to generate resource mass (`abundance` as described in extractable_resources, note 3). The sum of all values in a stratum should be 100 ± 1. It doesn't need to be exact because these will be normalized by code before conversion to mass. Note: the simulation can handle **very** small numbers here, so be sure to include volatiles in accordance with extractable_resources.md note 1.
* `abundance_cv` — This is coefficient of variation of the total resource mass, representing epistemic uncertainty. It will be used to generate `abundance_error` (after conversion of `relative_abundance` to `abundance`).
* `dispersion` — See extractable_resources.md, note 3.
* `dispersion_error` — See extractable_resources.md, note 3.

Keep significant digits of each value ≤ 2 with the following exceptions: 1) "Filler" stone and regolith can have 3 significant digits to help normalize the total abundance to ~100. 2) Relatively homogeneous strata (e.g., atmosphere, ocean) can have more significant digits where precise values are known and the higher precision is warranted.

# Instructions

Modify the tables as needed. Provide either specific table rows or entire tables as needed, formatted as code. These are the changes and additions I need right now:

* Keep Antarctica and Other Continental Ice Bodies (the latter includes Greenland), but remove Continental Ice Bodies for the other 6 territorial regions.
* Remove Continental Water Bodies for all 8 territorial regions.
* For the removals above, we want to treat the ice and/or water content as part of the respective Continental Land strata. Recalculate Continental Land values in strata_resources.tsv to reflect this change. Consider that dispersion should be high for water in this stratum because it now includes distinct bodies of nearly pure water.
* Provide text updates for strata.md that reflect changes above.
* Add Mars (all strata), Phobos, and Deimos.


Also provide feedback so I can improve the prompt and/or context files for subsequent analyses.

---
---

*Thinking...*

> This is a complex request. Let me break it down:
> 
> 1. **Remove Continental Ice Bodies** for USA, China, EU, Russia, India, Japan (keep Antarctica and Other)
> 2. **Remove Continental Water Bodies** for all 8 territorial regions
> 3. **Recalculate Continental Land** strata_resources.tsv to absorb ice/water content from removed strata
> 4. **Recalculate strata.tsv** for Continental Land to reflect absorbed mass/volume
> 5. **Provide strata.md text updates**
> 6. **Add Mars (all strata), Phobos, and Deimos** to both tables
> 
> 
> 7. **Provide feedback** on the prompt/context files
> 
> Now I'm working through this systematically, starting with identifying which ice bodies need to be removed from the six territorial regions while preserving Antarctica, then mapping out the water bodies across all eight regions that also need to be eliminated.
> 
> For each territory, I need to recalculate the Continental Land layer by redistributing the mass from both the removed ice and water bodies. Let me start examining the current data for USA... I'm recalculating the total volume to around 1 million cubic kilometers. But I need to reconsider the approach here—the user wants ice and water merged into the Continental Land stratum, yet I'm realizing the strata are meant to be exclusive categories. The Continental Land already encompasses all land to 100 meters depth, so ice and water bodies sitting on that land shouldn't be double-counted by area, but their mass should be incorporated into the land's total mass. from land. When we merge them, the total mass increases but the area/volume geometry doesn't necessarily change much because the land area was already defined to include the area under these features. Looking at the spherical fractions for the USA, I can see that ice bodies, water bodies, and continental land are tracked as distinct area fractions that sum to about 0.01996. The design seems to treat these as exclusive categories—ice and water cover certain surface areas, while continental land is everything else not covered by ice or water.
> 
> When merging these layers, the land strata would need to expand its area to account for the regions currently covered by ice and water. But I'm getting confused about the depth encoding in the data—the Continental Land entry shows depth=0.1 and thickness=0.1, and I need to figure out whether this represents the 0-0.2 km range or something else based on how depth is actually defined in the strata documentation. of the inner boundary (0.1 km = 100 m), and thickness is 0.1 km = 100 m. This means it spans from surface (0) to 100 m depth.
> 
> The ice bodies are at depth=0 with some thickness (e.g., USA: thickness=0.10 km). Water bodies are at depth=0 with thickness=0.046 km.
> 
> So the ice/water bodies occupy the same depth range as the upper part of Continental Land but are different material.
> 
> When merging, the simplest approach is:
> 1. Add the mass of ice/water to the Continental Land mass
> 2. Add the volume of ice/water to the Continental Land volume
> 3. The area might increase slightly (since ice/water had distinct areas)
> 4. Recalculate density
> 
> Actually, let me reconsider. The user says "treat the ice and/or water content as part of the respective Continental Land strata." I think the intent is that instead of having separate ice/water strata, we just modify the composition of Continental Land to include some ice/water. The geometry of Continental Land stays roughly the same (or slightly increases in area to include those areas).
> 
> Looking at how the strata exclusive areas work: for a territory like USA, the surface is divided into ice area, water area, land area, and shelf area, while below those surface strata are near-surface and subsurface layers covering land and shelf. When merging ice and water into land, the land area should expand to include those ice and water areas, with corresponding increases in volume and mass. But there's a subtlety here—I need to think through whether I'm just absorbing the ice/water mass or if there's underlying rock that was previously excluded.
> 
> Now I'm looking at the actual area numbers to understand the scale. USA's total continental area breaks down into land (9.83 million km²), ice (87,000 km²), and water (250,000 km²), which sums to about 10.17 million km². But the USA total area is already listed as roughly 9.83 million km², which seems to already include inland water, so I need to clarify what's being double-counted here.
> 
> Let me check the spherical fraction values instead—they should give me a clearer picture of how these components relate to each other. For the USA, I'm seeing that Near-Surface (0.0242) equals Land (0.0193) plus Shelf (0.00484), which suggests these categories are meant to be additive. So the land area (0.0193) does NOT include ice or water body areas. The total surface = ice + water + land + shelf = 1.71e-4 + 4.90e-4 + 0.0193 + 0.00484 = 0.02480.
> 
> But Near-Surface (0.0242) doesn't include ice or water body areas either! 0.0242 ≈ 0.0193 + 0.00484. This means the near-surface strata only cover land + shelf, not the areas under ice or water.
> 
> Actually wait, that makes sense from an extraction perspective. The ice/water body strata cover their own area, and the near-surface etc. cover the remaining area. But that means when we merge ice and water into land, we need to expand Continental Land to include those areas, which increases its mass to include both the rock underneath plus the ice/water material itself. The problem is I'm not sure what was under the ice/water before—there seems to be a gap where the rock at 0-100m depth under ice/water bodies isn't accounted for in any stratum. The ice/water are only their own material, and the rock beneath them at 0-100m isn't in Continental Land (different area) or Near-Surface (which only covers land+shelf, not under ice/water).
> 
> Maybe the model just accepts this gap as negligible since ice/water volumes are tiny compared to continental rock. For the merge, I should expand Continental Land to include ice and water areas, calculate the new volume from the expanded area and thickness, then set the new mass to old land mass plus ice mass plus water mass, which implicitly accounts for the missing rock underneath. I'm realizing the rock beneath the ice and water needs to be accounted for—it would occupy part of that 100m depth, so I need to adjust the calculation to avoid double-counting. The simplest fix is to treat the entire combined area as land with a uniform 0.1 km thickness and 2.0 g/cm³ density, since ice and water masses are negligible compared to rock for the overall calculation. The user wants me to merge the ice and water body masses into the Continental Land strata composition in strata_resources.tsv. For the USA example, I need to add the water content from the removed ice and water bodies to the existing water abundance percentage in Continental Land, then recalculate the total water mass accordingly. Now I'm calculating the new water fraction after accounting for the removed water bodies and ice sheets, which brings the total to about 4.93% — a modest increase from the baseline 4%. I should check whether this pattern holds across other regions or if places like Antarctica show more dramatic shifts given their massive ice sheets. For the strata.tsv updates, I'm weighing whether to keep land mass constant and just adjust composition, or redistribute the ice and water mass into the land itself — and since ice and water typically represent less than 1% of land mass for most territories, the simpler approach of keeping land properties fixed seems reasonable, though Antarctica will need special consideration.
> 
> Looking at Antarctica's numbers, the ice sheet mass is roughly 8.5 times larger than the land mass itself, so I definitely can't ignore it there. But the user's instructions are clear: keep Antarctica's ice bodies intact and only remove the water bodies, which are negligible anyway. Same logic applies to the Other region (Greenland). So for both, I'm just removing the continental water bodies while preserving the ice.
> 
> For the remaining six territories—USA, China, EU, Russia, India, and Japan—I need to strip out both ice and water bodies. Checking the ice-to-land ratios, they're all pretty minor: USA's ice is less than 0.4% of its land mass, and the others are even smaller, so removing them won't significantly skew the results.
> 
> Now looking at water coverage, USA has about 0.58% water relative to land, EU around 0.46%, and the others range from 0.12% to 0.30%—all relatively small percentages that I should exclude for consistency.
> 
> For the strata.tsv updates, I need to recalculate each territory by combining the ice and water masses, volumes, and areas with the existing land data, then derive the new density from the combined mass and volume. The key insight is that ice and water bodies occupy the surface layer alongside land, so when merging them, I'm essentially treating the combined material as a single stratum. now part of the land stratum. The area of land should expand to include the ice/water areas. The volume increases by the area increase × thickness (100m) PLUS the ice/water volumes that extend above the "100m land surface layer" concept... 
> 
> Hmm, I think I'm overcomplicating this. Let me take a practical approach:
> 
> Since the ice/water mass is <1% of land mass for all territories being modified, I'll:
> 1. Add the ice/water mass to the land mass
> 2. Add ice/water area to land area
> 3. Recalculate volume from new area × thickness
> 4. Recalculate density from new mass / new volume
> 5. Update spherical_fraction proportionally
> 
> But since the changes are so small (< ~1%), maybe I should just keep strata.tsv land values unchanged and only change the composition (strata_resources.tsv). The user specifically wants me to recalculate Continental Land values in strata_resources.tsv, though they also mention modifying strata.tsv as needed. Given how minimal these changes are, I'll focus on the composition updates while keeping the strata.tsv land rows mostly intact—just updating the survey notes. I should also recalculate the spherical fractions since we're removing strata, and check whether near-surface areas need adjusting too. Looking at the numbers, if I add ice to the near-surface area for somewhere like the USA, that changes the calculation significantly.
> 
> Actually, I need to be more careful about the mass fractions. When I merge ice and water into land, the new composition for each resource is the weighted average of the old compositions, scaled by their respective masses.
> 
> For the USA, the water content increases from 4% to about 4.93% when I account for the ice and water bodies being absorbed. I should also check what other resources come from the ice bodies—looking at the composition data, industrial minerals are present at trace levels. Looking at the composition data, the non-water elements are negligible, so the main shift is water increasing by roughly a percentage point while everything else scales down proportionally. Since we're now mixing concentrated water bodies like lakes and glaciers with the dispersed groundwater that was already there, the water dispersion should jump significantly—I'm thinking from 0.6 up to around 1.5-2.0 to account for those localized areas that are nearly 100% water alongside the more typical continental regions.
> 
> Given that dispersion is measured in log₁₀ units, a value of 2.0 captures the roughly 100-fold variation between pure water bodies and dry rock areas. I should also bump up the abundance_error for water since we're now combining two distinct material types, which introduces more epistemic uncertainty into the estimate.
> 
> Now I'm working through each territory to recalculate the resource compositions. For the major populated regions—USA, China, EU, Russia, India, and Japan—I need to compute the new water fraction, adjust stone and regolith downward to maintain the total around 100%, and set water dispersion to approximately 2.0. For Antarctica and Other, the water contribution is negligible enough that I can simply remove those bodies without major adjustments.
> 
> Calculating new compositions for each territory... Now I'm checking the composition breakdown for the USA land resources—since the ice and water bodies are almost entirely water, the non-water portion is negligible, so I'll keep most resource percentages unchanged and just reduce the stone filler to maintain the total around 100%.
> 
> Looking at the sum of all values, I'm getting approximately 100.0195%, which is close enough. With the new water percentage at 4.9%, that's a 0.9% increase, so I'll trim stone by about 0.5% and regolith by about 0.4% to balance it out.
> 
> The trace amounts of regolith and industrial minerals in the ice and water bodies are negligible enough that I don't need to account for them separately. What matters is how the stone and regolith percentages shift when I add this extra mass from ice and water—since those bodies contain neither stone nor regolith, their contribution dilutes the overall percentages of those materials on the land. × 0.04 + 7.83e15 × 0 + 1.15e16 × 1) / 1.99e18
> = (7.88e16 + 1.15e16) / 1.99e18
> = 9.03e16 / 1.99e18 = 0.0454 → 4.5%
> 
> So the final percentages are STONE at 62.8%, REGOLITH at 27.3%, and WATER at 4.5%, which means the ice and water contributions barely moved the needle since they're so much smaller in mass. The adjustments are minimal, so I'll focus on recalculating the WATER percentage for each territory where ice and water are both removed, then proportionally adjust the other components. I'm working through the six affected territories systematically, starting with USA at 4.93% water, then moving to China and the others to ensure the composition shifts are consistent. Now I'm working through the EU calculations, finding that water makes up about 4.77% of the total after accounting for the ice and regolith fractions, with stone remaining at 61.8%. Moving on to Russia's composition with a much larger total mass of 3.43e18. Now I'm working through India's water fraction calculation, which comes to 3.24% after accounting for the different surface types, with stone at 63.8% and regolith at 27.8%. Moving on to Japan, where the total area is 7.58e16 and the water fraction is 0.30%.
> 
> For Japan's adjusted water content, I'm recalculating with the weighted contributions from each surface type, arriving at 4.29% water, 65.2% stone, and 25.4% regolith. Antarctica barely registers since its water bodies are negligible compared to the land mass, so the water percentage stays essentially flat at 6%. I still need to finish the calculation for the "Other" category. I'm seeing that adding concentrated water bodies creates a bimodal distribution—mostly rocky terrain with isolated pure water zones—so I'll set the dispersion around 2.0-2.5 and abundance_error to 0.25-0.3. For dispersion_error, the localized ice and glaciers give us certainty about where the water concentrates, but modeling the combined distribution adds uncertainty, so I'm settling on 0.2. This gives me the parameters for the new water entries: USA gets 4.9% abundance with 0.2 error and 2.0 dispersion, while China's at 4.3% with similar error and 2.0 dispersion.
> 
> Actually, I need to reconsider what dispersion really represents here—it's the standard deviation in log₁₀ units for a lognormal distribution, so 2.0 means concentrations span roughly two orders of magnitude. With pure water bodies embedded in rocky strata, that huge dynamic range justifies the higher dispersion value. Though thinking about it more, this parameter probably doesn't significantly affect extraction economics since surface water is already easily accessible regardless of how the concentration varies.
> 
> Let me just set dispersion to 2.0 with error 0.2 across all territories and move on. Looking at the existing USA Continental Land water entry, the current dispersion of 0.6 reflects groundwater heterogeneity, so bumping it to 2.0 for the merged dataset makes sense. The trickier part is the abundance_cv—the original 0.2 reflected uncertainty about total water content, but now I'm combining well-surveyed surface water with less certain underground reserves, so I need to figure out what the merged uncertainty should be.
> 
> Now I'm going to systematically update all the composition rows. Since the resource changes are minimal except for water, my approach is straightforward: bump water abundance, adjust stone and regolith to maintain the ~100 total, set water dispersion to 2.0, and leave everything else as-is. I should also consider whether the Near-Surface and Subsurface strata need area adjustments to account for the expanded land boundaries, but the area shifts are so small they're probably within acceptable margins.
> 
> For the strata.tsv updates, the mass changes are negligible enough that they fall within the model's error tolerance, though I'll make the adjustments anyway for consistency—recalculating the area, spherical fraction, and volume for regions like the USA based on the new land extent.
> 
> Now I'm working through the unit conversions carefully: volume in km³ times density in g/cm³ needs the 1e12 conversion factor to get mass in kg. The old mass checks out, and the new absorbed mass from the additional land area comes to roughly 1.99e18 kg.
> 
> The problem is that I'm double-counting rock—when I add the ice and water body footprints to the land area and apply the 100m depth, I'm getting more volume than the actual absorbed mass suggests, because the ice and water have lower densities than rock. Rather than trying to account for all these overlapping volumes and density mismatches, it might be cleaner to keep the land area, volume, and mass fixed but adjust the composition to include a water and ice fraction instead.
> 
> Actually, the better approach is to expand the land area to include the ice and water bodies, keep the rock density at 2.0, and accept that this gives us a slightly larger total mass—which represents the rock that was previously "missing" from the model beneath the ice and water. Then I can merge all the masses together and recalculate the composition by tracking how much water comes from the original land, how much from the ice, and so on. I think the cleanest approach is to calculate the total mass in the expanded region, then subtract what we already know (old land, ice, water) to get the rock mass underneath. Since this hidden rock is part of the same geological formation, I can assume it has the same composition as the existing land, which lets me combine those masses and weight everything proportionally in the final calculation.
> 
> Now I'm working through the USA numbers to see how this plays out—adding up the new total area including the ice and water coverage, converting to volume with the density factor, and computing the new mass. I'm finishing the composition calculations and verifying the totals work out, then realizing I need to take a step back and recalculate this more systematically for each territory individually rather than trying to adjust everything at once. I'm finishing the composition calculation by multiplying in the water and ice fractions, then applying the same approach to Antarctica and other regions that only have water bodies to account for. Now I'm working through the actual numbers for the USA, computing the new land area by adding ice and water contributions, then deriving the updated mass and composition fractions. Now I'm calculating the composition fractions for China—ice, water, and rock percentages based on the updated mass—then moving through the same process for the EU region with its own area and volume adjustments. Now I'm working through the same calculations for India, computing the updated area, surface fraction, volume, and mass after accounting for the additional regions, then determining the fractional compositions of ice and water in the total mass.
> 
> Moving on to Japan with similar calculations—the area is much smaller, so the ice fraction comes out negligible while water still comprises a small portion of the total mass.
> 
> For Antarctica, I'm checking what happens if we only remove water. The area and volume barely change since the water mass is trivial compared to the ice sheet, so there's no meaningful impact worth modeling separately.
> 
> Now looking at the other regions when water is removed: the area increases to about 9.13e7, surface fraction to 0.179, and the new mass calculation gives me a water fraction of about 0.37%. This shifts the composition slightly—water drops to around 4.35% while stone stays near 63.3%.
> 
> For the near-surface strata, I'm checking whether the land area changes should propagate there too. The adjustments are minimal across all regions—USA gains about 2.7%, China and EU less than 3%—so since the user didn't specifically ask about near-surface, I'll note these but leave them as is for now. Though I should verify whether the spherical fractions I used were actually based on the original near-surface areas.
> 
> Now I'm working through the new compositions for each territory, starting with USA where I need to recalculate each resource type using the updated fractions for rock, ice, and water. Now I'm calculating the weighted contributions for the remaining resource categories, accounting for the secondary pathways and their smaller fractional impacts on the final abundances. I'm working through the composition calculations, adding up the stone contribution of 62.7 with the other components. I can see water contributes about 4.91 based on the weighted average across the three materials, and now I need to finish calculating methane's contribution to get the final total. × 1e-7 + 0.00564 × 1e-5 = 0.00297 → 0.003
> 
> So 67.816 + 4.91 + 0.003 = 72.73. REGOLITH = 100 - 72.73 = 27.3
> 
> I also need to account for resources that were in water bodies but not on old land. OXYGEN from water contributes negligibly. NITROGEN adds a small amount around 3e-5. CO2 is also negligible. AMMONIA and HELIUM are too small to matter. METHANE is dominated by the land contribution, and ARGON is still being calculated.
> 
> These absorbed volatile amounts are so minimal—parts per million of the total land mass—that keeping them at zero in the land composition makes sense. The user's note about including volatiles is satisfied by the land composition already having them. I'll document this decision but maintain zeros for the absorbed gases.
> 
> Now I need to reconsider the dispersion parameters. For water in the new Continental Land, I'm looking at a huge range—from maybe 0.01% in dry rock to nearly 100% in lakes and glaciers. That's roughly four orders of magnitude, so a log-standard deviation around 2 seems appropriate to capture this spread. The other resources should keep their existing dispersions since we're mainly diluting with water.
> 
> For water's dispersion_error, I'm setting it to 0.3 because while we know where the major water bodies are, the combined distribution is tricky to model as a single lognormal. The abundance_cv for water gets 0.2—we have decent estimates for lakes and glaciers, though groundwater adds some uncertainty. I'm ready to write out the configuration now.
> 
> Shifting to Mars, Phobos, and Deimos, I need to pull the stratification data from the reference file. Mars has a thin CO₂-dominated atmosphere extending about 100 km up, polar caps at both ends (water ice in the north, CO₂ over water ice in the south), and then the subsurface layers starting with regolith in the top 5 meters.
> 
> Now I'm calculating Mars's basic parameters—its mean radius of 3,390 km gives a surface area around 145 million square kilometers and a volume of roughly 163 billion cubic kilometers. The atmosphere extends to an outer radius of 3,490 km, and I need to establish the boundaries for each layer down through the mantle and core.
> 
> For the atmospheric layer specifically, I'm working through the volume between the outer and inner radii, which comes to about 15.1 billion cubic kilometers. Given the surface pressure of roughly 600 Pa and the known atmospheric mass of about 2.5×10¹⁶ kg, I can derive a mean density of around 1.66×10⁻³ kg/m³. I'm now verifying this calculation by cross-checking the atmospheric mass using the surface gravity of 3.72 m/s². Now I'm converting the volume to standard units and recalculating the density, which comes out to about 1.67 × 10⁻⁶ g/cm³. For the survey level, Mars's atmosphere is pretty well characterized from multiple missions like Viking, Curiosity, and MAVEN, so I'd rate it around 7 out of 10 compared to Earth's perfect 10. Now I'm looking at the composition data for the Martian atmosphere. I need to refine these measurements—methane is actually much lower than initially stated, around 2 parts per billion with occasional spikes, and I should account for the noble gases like neon, krypton, and xenon that make up a small but measurable portion of the atmosphere. Let me recalculate the composition more carefully by volume to ensure accuracy.
> 
> Now I'm working through the mass fractions by multiplying each component's mole fraction by its molecular weight and dividing by the mean molecular weight of about 43.34 g/mol, which is dominated by CO₂'s 44.01 g/mol. I need to report mass fractions for the actual molecular species in the atmosphere rather than elemental compositions. Looking at the resource categories, they're defined as specific molecules like CO₂, N₂, Ar, and so on, so I should calculate the mass fraction for each of these molecular forms directly from the composition data. .3)
> H₂: ~1.5e-5 (MW 2.016) (some sources say 15-20 ppm by volume)
> 
> Total = 0.9987 (close enough to 1)
> 
> Now I'm calculating the mean molecular weight by multiplying each component's volume fraction by its molecular weight and summing them all up. Now I'm finishing the mass fraction calculations for the remaining trace gases, working through oxygen, carbon monoxide, water vapor, and the heavier noble gases to get their individual percentages of the total molar mass. I'm verifying the mass fraction calculation for heavy noble gases—1.98e-6 converts to about 0.0002% or roughly 2 parts per million, which checks out. Now I'm thinking through what other atmospheric components might not be explicitly categorized as resources.
> 
> Looking at the composition breakdown, I notice NO is negligible at around 0.01%, so it could either fall under a nitrogen category or be safely ignored given how trace it is. Let me compile the full atmospheric profile: carbon dioxide dominates at 96.62%, followed by nitrogen at 1.74%, argon at 1.47%, then progressively smaller amounts of oxygen, carbon monoxide, water vapor, hydrogen, and those trace gases—everything else sums to less than a tenth of a percent.
> 
> For the data table, I need to account for suspended dust in the Martian atmosphere. While it's typically just a few parts per million by mass, during dust storms it can spike dramatically, so I'll represent it as a small but variable regolith component rather than leaving it out entirely.
> 
> Actually, let me recalculate—even during major dust storms, the suspended dust is only around 10^-7 of the total atmospheric mass, so it's negligible enough to set to zero. Now I'm laying out the Mars atmosphere composition with carbon dioxide as the dominant component, followed by nitrogen, argon, and trace amounts of oxygen, carbon monoxide, and water vapor, each with their own variability estimates.
> 
> Water vapor is tricky since it varies dramatically by season and location on Mars, ranging from nearly zero to 0.1%, but I'll keep the dispersion at zero since the model assumes large-scale mixing. Moving on to hydrogen and methane—methane is particularly contentious because Curiosity detected seasonal fluctuations between a baseline of about 0.4 ppb and peaks around 7 ppb, though the data remains controversial.
> 
> For the trace gases, methane is so minimal that even accounting for its mass fraction it rounds to essentially zero, so I'm setting it to zero. Sulfur dioxide is similarly negligible at 0.1-1 ppb. Helium likely isn't present in significant amounts due to Mars's low gravity and solar wind stripping, so that's zero too. For the heavier noble gases, I'm working with 2e-4% abundance, but I need to figure out what uncertainty value to assign based on the lander mass spectrometer data.
> 
> I'm setting the coefficient of variation to about 0.1 for those heavier noble gases. Now let me verify the total composition adds up—running through the numbers, I get about 99.97%, which is close enough. Actually, I should factor in nitrogen monoxide at 0.007% by mass, so I'll adjust nitrogen slightly upward to 1.75% to absorb that and keep the total consistent. The composition is now balanced at roughly 99.98%.
> 
> Now I'm looking at what we know about Mars's atmosphere from various missions—the mass spectrometer data gives us pretty solid knowledge of the bulk composition, I'd rate that around 7-8 in terms of confidence.
> 
> For the north polar cap, I'm working through the characteristics: it's mostly water ice with some seasonal CO₂ frost on top, roughly 1,000 km in diameter with about 2 km thickness and a volume around 1.2-1.6 million cubic kilometers. Let me recalculate the area more carefully—if the diameter is 1,000 km, that's a radius of 500 km, giving me roughly 785,000 square kilometers.
> 
> I'm trying to reconcile the measurements now. The MARSIS radar data suggests about 1.14 million cubic kilometers, but when I multiply my area estimate by the average thickness, I'm getting closer to 1.57 million cubic kilometers. There's a discrepancy here, so I need to either adjust the area or thickness downward to match the measured volume better.
> 
> Let me settle on some compromise values: an area of about 800,000 square kilometers with an average thickness of 1.5 kilometers gives me roughly 1.2 million cubic kilometers, which is closer to the observed data. For the model parameters, that translates to a spherical fraction of about 0.0055, and I'll set the density to account for water ice mixed with dust impurities—something around 0.9 to 1.0 grams per cubic centimeter seems reasonable given the composition.
> 
> Working through the density calculation more carefully: if the cap is roughly 95% water ice at 0.917 g/cm³ and 5% dust at 2.5 g/cm³, that gives me about 1.0 g/cm³ overall. So the total mass comes out to around 1.2 × 10¹⁸ kilograms.
> 
> The composition is dominated by water ice with interbedded dust layers making up a few percent, while seasonal CO₂ ice on the surface and any trapped CO₂ in deeper layers are minor contributors to the overall structure.
> 
> Now I'm setting up the material parameters for the model: water ice gets low uncertainty since it's fairly uniform, regolith gets moderate uncertainty due to the interbedded layers, and the minor components like trapped CO₂, salts, and rock inclusions all get higher uncertainty since they're less well characterized.
> 
> For the south polar cap, I'm looking at the survey data—MARSIS and SHARAD radar have given us pretty good coverage, plus some ground truth from Phoenix, so I'd rate the confidence around 3-4. The cap itself is mostly a thin residual CO₂ layer sitting on top of much thicker water ice deposits that extend across a wider area than the north cap in some estimates.
> 
> I'm settling on simplified parameters: roughly 1 million square kilometers for the total area, averaging about 1.5 kilometers thick, which gives me a volume around 1.5 million cubic kilometers. That translates to a spherical fraction of about 0.0069 relative to Mars's radius.
> 
> For the density calculation, I need to account for the composition—the south polar deposits contain more CO₂ than the north, so I'm estimating around 80% water ice, 10% CO₂ ice, and 10% dust, which works out to a density of roughly 1.1 g/cm³. That gives me a total mass of about 1.65 million cubic kilometers times that density.
> 
> Now I'm breaking down the material composition by percentage—water dominates at 80%, with significant CO₂ at 10%, plus smaller amounts of regolith, industrial minerals like salts, and stone. The south cap is less well-characterized than the north because the thick CO₂ atmosphere interferes with radar observations, so there's more uncertainty in these estimates.
> 
> For the survey characterization level, I'm settling on 2.5—MARSIS has done extensive profiling and found those buried CO₂ deposits, so it's reasonably well-studied, but the atmospheric complications mean there's still more uncertainty than what we have for the north cap.
> 
> Now shifting to the broader Mars regolith layer in the top 5 meters—it's primarily basaltic dust and fines mixed with iron oxides, perchlorates, and sulfates, with composition varying significantly across the planet. I have measurements from multiple rovers and landers across different sites, and I'm calculating the regolith area by taking the total Mars surface and subtracting both polar caps.
> 
> Working through the volume calculation: roughly 1.43×10⁸ km² of regolith at 5 meters thick gives me about 7.15×10⁵ km³. Using a density of 1.5 g/cm³ for loose soil, that translates to a total mass around 1.07×10¹⁸ kg. The composition from rover data shows silica as the dominant oxide at roughly 45%, with the rest being iron oxides, aluminum silicates, and various other minerals. Looking at the trace elements and additional components—chromium, manganese, potassium oxides, nickel, and zinc make up less than 2% combined, while perchlorates and water of hydration add another 1-4% to the composition. Now I'm thinking through how regolith fits into the resource model—it's essentially the bulk filler material, mostly fine basaltic particles dominated by iron oxides, but I need to clarify whether the model treats regolith as a distinct resource category or if it's just the matrix holding everything else. of the mass IS regolith with smaller fractions of other things. Let me look at MOON_REGOLITH for calibration:
> 
> MOON_REGOLITH: IRON_NICKEL 0.5, RARE_EARTH 0.1, SULFUR 0.05, INDUSTRIAL_MINERALS 3, STONE 2, REGOLITH 94.3, WATER 0.01, HYDROGEN 0.005, NITROGEN 0.002, HELIUM 3e-4
> 
> So for Moon, regolith is 94.3% REGOLITH. The remaining 5.7% includes minerals, metals, and trace volatiles.
> 
> For Mars regolith, the composition is different. There's much more sulfur from perchlorates and sulfates, more water locked in hydrated minerals, and iron ores like hematite and goethite, especially concentrated at Meridiani. Now I'm working out the Mars regolith composition with specific percentages for iron ores, industrial metal ores, rare earth elements, and sulfur across different regions. I'm laying out the remaining resource categories with their abundance and variability metrics—water as hydrated minerals and ground ice, carbon dioxide as adsorbed compounds, nitrogen from nitrates, and oxygen bound in minerals rather than free. But I'm realizing I need to reconsider how regolith should be represented, since it's fundamentally the surface fines with a specific elemental composition rather than a discrete resource type.
> 
> The key distinction is that Mars regolith is basaltic by nature, but the model treats specialized categories like iron ores and sulfur as concentrated deviations from that baseline. So I'm working through whether uniformly distributed sulfur should still fall under the sulfur category or remain part of the regolith composition, depending on whether it meets the threshold for being considered an extractable resource rather than just background material. potentially extractable resource distinct from the regolith.
> 
> On the other hand, the model says REGOLITH is "ref. generic basalt (Moon/Mars/MORB average)." So Mars regolith IS the reference material for the REGOLITH category. The Mars regolith's higher S content compared to generic basalt would be reflected in the SULFUR resource being higher for Mars than for the Moon.
> 
> I think the approach should be:
> - REGOLITH: bulk basaltic fines (~85-90%)
> - SULFUR: the excess sulfur beyond what's in generic basalt (~2% on Mars vs ~0.1% in generic basalt) → ~1.5-2%
> - IRON_ORES: concentrated iron oxide deposits (hematite blueberries, etc.) beyond what's in generic basalt → maybe 1-2%
> - INDUSTRIAL_MINERALS: perchlorates, phosphates, salts → ~5-8%
> - WATER: hydrated minerals and ground ice → 2-5% (highly variable)
> - STONE: some coherent rock fragments → minimal for surface regolith
> 
> Now I'm looking at the actual model distributions more carefully. The Moon regolith breaks down with regolith as the dominant component at 94.3%, a small fraction of coherent stone fragments around 2%, and industrial minerals making up about 3%, with trace amounts of other resources. For Mars, the main divergences are significantly more water content from ground ice and hydrated minerals, elevated sulfur compounds, and a different mix of industrial minerals.
> 
> I'm setting up the Mars regolith composition with specific resource categories and their variability ranges. Iron ores like hematite and goethite represent concentrated deposits, industrial metal ores capture trace chromium, manganese, nickel, and zinc concentrations, rare earth ores are poorly characterized, sulfur includes both sulfates and elemental forms, industrial minerals encompass perchlorates and phosphates, organics and tholins are extremely sparse, the bulk regolith is basaltic fines, and water exists as ground ice and hydrated minerals with high regional variability.
> 
> Finishing out the composition with carbon dioxide as adsorbed gas, nitrogen as nitrates, and coherent stone fragments, which totals to 100%. Native iron from Mars meteorite evidence is too rare to meaningfully include in the regolith model, so I'm leaving that at zero.
> 
> For the ore deposits—aluminum, precious metals, and uranium—there's no concentrated surface evidence on Mars, so those are all zero as well. Given that we have soil analyses from six landing sites across the planet plus orbital spectroscopy data, but coverage is still limited to just a handful of locations on such a vast world, I'm setting the survey confidence level at 2.0.
> 
> Now moving into the near-surface layer, which extends from 5 meters down to half a kilometer and contains weathered bedrock, cemented regolith, and ground ice concentrated at mid-to-high latitudes.
> 
> I'm calculating the volume of this layer at about 7.15×10^7 cubic kilometers, with a spherical fraction of 0.987. The density here is around 2.2 grams per cubic centimeter—intermediate between loose regolith and solid rock—which gives me a total mass of roughly 1.57×10^20 kilograms. The composition is similar to the surface regolith but more consolidated, with more weathered stone, less loose material, and notably more ground ice at higher latitudes where it can comprise 10 to 30 percent of the volume in some areas.
> 
> Ground ice becomes a major factor at high latitudes but essentially disappears near the equator, creating significant regional variation in water content. I'm now setting concentration parameters for various ore types—iron ores, aluminum ores, industrial metals, rare earths, uranium, and sulfur—each with different abundance levels, concentration factors, and distribution patterns to reflect how these resources vary across the planet. I'm finishing out the composition table with methane and nitrogen, then verifying the percentages sum to 100. The survey data is sparse—we've got some radar and seismic readings from past missions, but no actual subsurface drilling, so I'm relying on thermal and neutron spectroscopy to constrain these estimates.
> 
> Now moving to the deeper subsurface layer between half and five kilometers down. I'm calculating the volume of intact basaltic rock at this depth—about 6.4 × 10^8 cubic kilometers—and using a density of 2.7 grams per cubic centimeter to get a total mass around 1.74 × 10^21 kilograms. At this depth, the rock is less weathered and might contain sulfide deposits or deep brines, which changes the composition profile significantly from the near-surface material.
> 
> Now I'm setting up the ore and mineral abundance parameters for this subsurface layer, assigning concentration factors and distribution weights across different resource types like iron, aluminum, rare earths, and sulfur. I'm finishing out the atmospheric composition with nitrogen and hydrogen, then verifying the total adds up to roughly 100 percent. The survey confidence is pretty low since we're mostly working from radar and seismic data plus theoretical models, so I'm rating that around 0.5 to 1.0. Now I'm moving into the Mars deep crust layer, which extends from about 5 to 50 kilometers down.
> 
> For the deep crust volume calculation, I'm treating it as a spherical shell between two radii and working through the geometry—the outer boundary sits at 3385 km from Mars's center and the inner at 3340 km, giving me a volume around 6.16 billion cubic kilometers. The density here is roughly 2.9 grams per cubic centimeter.
> 
> Now I'm calculating the total mass of this layer at about 1.79 × 10²² kilograms, accounting for the compression at depth. The composition is likely dominated by basaltic material similar to Earth's oceanic crust, with possible ultramafic rock deeper down, though there's significant uncertainty here. I'm setting ore concentration parameters for iron, aluminum, industrial metals, and precious metals to reflect the varying enrichment levels and extraction feasibility at these depths.
> 
> I'm now assigning concentrations for the remaining resource types—rare earth elements, uranium, sulfur, and industrial minerals all have modest abundances, while stone makes up the vast majority at 92% as intact basaltic and gabbroic bedrock, with regolith comprising the fractured zones. Water presence is minimal at this depth. I'm summing up all the composition values and getting close to 100, so I'll adjust the STONE percentage to 92.1 to hit the target more precisely—that gives me a total of 99.99, which works well.
> 
> Now I'm calculating the Mars mantle layer, which extends from 50 to 1,700 km depth. The survey data is sparse—mostly just some layering information from InSight seismic readings, so I'm rating that confidence at 0.5. I'm working through the volume calculation for this silicate-rich region and determining the surface area at the outer boundary, with a density around 3.5 g/cm³.
> 
> Now I'm cross-checking my mantle mass estimate against Mars's total mass to make sure everything adds up. I'm breaking down the core and mantle contributions separately—the core at about 1,690 km radius with higher density around 6.5 g/cm³, and the mantle mass coming in around 4.75 × 10²³ kg.
> 
> Adding up all the layers—core, mantle, crust, and negligible contributions from the atmosphere and polar caps—I get roughly 6.24 × 10²³ kg, which is only about 3% off from Mars's known total mass of 6.42 × 10²³ kg. That's close enough, so I'm going to fine-tune the mantle density slightly to get the numbers to align perfectly.
> 
> Working backward from the target total mass, I need the mantle to contribute about 4.93 × 10²³ kg. Dividing that by the mantle's volume gives me a density of around 3.6 g/cm³, which is reasonable for a composition similar to Earth's mantle but enriched in iron oxide. With that adjustment, my total comes to 6.38 × 10²³ kg—essentially matching the actual value.
> 
> Now I'm thinking through the mantle's composition, which would be dominated by silicates but with a higher iron content than Earth's mantle, along with some industrial metal ore concentrations. Now I'm verifying the composition adds up correctly, and the survey confidence is quite low given the limited seismic data we have. For the core itself, I'm calculating the volume of a liquid iron-sulfur layer with a radius around 1,690 kilometers. Now I'm refining the core composition breakdown—reconsidering what carbon species would actually exist in a reduced environment, then settling on a more realistic model where iron-nickel dominates around 80%, sulfur comprises 15%, with smaller amounts of silicates and trace metal ores making up the remainder. Now I'm calculating Phobos's physical properties—working through its volume using the mean radius of about 11 km, accounting for its irregular shape with dimensions around 27 × 22 × 18 km, and then deriving surface area and mass based on its known density of roughly 1.87 g/cm³, which reflects its porous composition.
> 
> For composition, the leading theories center on whether Phobos is a captured carbonaceous asteroid with C/D-type spectral characteristics, material co-accreted with Mars during formation, or impact debris from a collision—each scenario suggesting different mineralogy and volatile content, though the evidence currently points more toward a primitive captured body.
> 
> Now I'm setting up the material composition to reflect that C/D spectral type, balancing minor metallic components with iron ores, sulfur, industrial minerals, and a significant organic-rich surface layer of tholins, with the bulk being silicate stone.
> 
> Rounding out the composition with hydrated minerals, regolith, and trace amounts of volatile compounds like water ice, carbon dioxide, and various frozen gases to capture the characteristics of a carbonaceous asteroid.
> 
> Now I'm calculating the bulk properties for Deimos, starting with its mean radius of about 6.2 kilometers and working through the volume calculation. Now I'm setting up the resource composition for Deimos with abundance percentages and distribution parameters across different material types, including metals, minerals, sulfur, organics, stone, regolith, water, and other volatile compounds. Now I'm verifying the composition totals sum to 100%, which they do. Looking at the survey data level, it seems comparable to Phobos with roughly similar coverage. But I need to reconsider how the polar cap areas factor in—the north and south caps are geographically distinct from the regolith layer, so I should account for their separate spatial footprints when calculating spherical fractions.
> 
> For the south polar region, I'm distinguishing between the permanent CO₂ cap and the broader polar layered deposits. The strata data describes the south cap as having CO₂ over H₂O ice at 1–1.5 km thick, which encompasses the full deposit rather than just the thin CO₂ surface. I'll estimate the south polar area at around 5.0e5 km², slightly smaller than the north, giving a spherical fraction of about 0.0034 with a thickness of 1.2 km.
> 
> Now I'm cross-checking against observational data—Planum Boreum covers roughly 1e6 km² with an ice volume around 1.14e6 km³ from MARSIS measurements, while Planum Australe is more extensive at 1.2e6 km² but with a more irregular shape and about 1.6e6 km³ of ice volume, though this includes significant buried CO₂. Recent studies indicate the south contains roughly 9,500 km³ of CO₂ ice within the layered deposits plus the residual cap.
> 
> Let me settle on cleaner estimates: the north polar cap at 8.0e5 km² with an average thickness of 1.5 km gives me 1.2e6 km³, which aligns better with the MARSIS data than my initial calculation. For the south, I'm working with 5.0e5 km² as the baseline area.
> 
> Actually, I need to reconsider the south polar cap dimensions too—the reported volume is around 1.6e6 km³, which suggests either a larger area or greater thickness than I initially estimated. If I use 1.2e6 km² for the south with a mean thickness of about 1.3 km, that gets me closer to the observed volume.
> 
> Now I'm working through the north polar cap parameters systematically: with an area of 8.0e5 km² and thickness of 1.4 km, I get a volume of about 1.12e6 km³, which aligns well with the literature estimates. For the density, I'm accounting for the mixture of water ice and dust—pure ice is around 0.92 g/cm³ while dust is denser, so blending them gives roughly 1.0 g/cm³ as a reasonable estimate.
> 
> For the south polar cap, I'm using 1.3 km thickness which fits the strata.md range of 1–1.5 km, giving a volume of 1.56e6 km³. The density here is slightly higher at 1.05 because the south cap contains a notable fraction of CO₂ ice mixed with the water ice, making it denser overall. I'm reconsidering the exact composition breakdown—roughly 85% water ice with some CO₂ ice component—to get the density right.
> 
> Now I need to calculate the regolith area by subtracting both polar caps from Mars's total surface area, which leaves about 1.43e8 km² for the non-polar regions.
> 
> For the south polar cap, I'm estimating its composition based on observations showing it has more CO₂ ice than the north—roughly 10% of the total volume—along with water ice as the dominant component, some dust, and trace minerals.
> 
> Now I'm adding uncertainty parameters for each material in both caps, accounting for measurement variability and the seasonal CO₂ variations that are much more pronounced in the north.
> 
> I've got the complete composition breakdown for both regions totaling 100%, so now I need to think through how these strata map spatially—specifically whether the subsurface layers extend under the polar caps or if they're confined to the non-polar regions like in the Earth model. I'm realizing the polar caps create a depth overlap issue—they extend down to about 1.3-1.4 km, which overlaps with the Near-Surface layer (5m to 0.5km). Following Earth's model where ice bodies are treated as separate surface features, I should exclude polar cap areas from the Near-Surface and deeper strata to avoid double-counting that overlapping depth range. I'm settling on a cleaner model: polar caps form their own exclusive surface layer, while regolith and near-surface rock both exclude the polar cap areas since ice occupies that space down to about 1.5 km depth. The subsurface layer below 0.5 km would include the rock beneath the caps, but since the caps themselves sit on top, there's no rock to count in those regions until you get past the ice thickness. I'm realizing I need to make the strata consistent across all layers. The cleanest approach is to exclude polar caps from the shallow strata (regolith and near-surface) but include the full area for subsurface and deeper layers, treating it like Earth's model where surface features have distinct areas but deeper layers span the entire region. separate ice/water areas)
> - This means there's a "gap" for the shallow rock under ice/water bodies
> 
> For Mars, the equivalent would be:
> - Surface strata (polar caps, regolith) have distinct areas summing to ~full Mars
> - Below: Near-Surface and deeper cover the area under the regolith (excluding polar caps)
> 
> So Near-Surface area should exclude polar caps, matching the Earth model pattern.
> 
> Let me proceed with:
> - Regolith area: 1.45e8 × 0.986 = 1.43e8 km²
> - Near-Surface area: 1.43e8 km² (same as regolith)
> - Subsurface area: 1.45e8 km² (full, since at 0.5-5 km depth the caps don't extend)
> 
> The polar caps are only 1.3-1.5 km thick, so at 0.5 km depth they're still present, but by 1.5 km the ice ends and rock takes over. Since the subsurface layer spans 0.5-5 km, most of it sits below the caps anyway. I'll keep it simple: near-surface excludes polar cap areas like Earth's model does, while subsurface covers the full planet since the depth range mostly extends past the cap thickness. Setting regolith to sf = 0.986 as the baseline.
> 
> Now for the near-surface strata entry, I need to calculate the volume using the spherical shell formula rather than just multiplying area by thickness. The outer radius at the near-surface top is approximately 3390 km, and the inner radius is 3389.5 km after subtracting the 0.5 km thickness.
> 
> Since this layer is so thin relative to Mars's radius, the thin-shell approximation works perfectly—volume equals area times thickness. Using the spherical fraction of 0.986 and rounding the thickness to 0.5 km gives me a volume of about 7.15e7 cubic kilometers.
> 
> Now I'm ready to finalize all the Mars strata rows for the TSV file.
> 
> I need to clarify the depth column definition: it represents the reference radius minus the inner boundary radius of each stratum. So I'm working through each layer—regolith from 0-5m has a depth of 0.005 km, the near-surface zone from 5m to 0.5 km has depth 0.5 km, subsurface goes to 5 km depth, deep crust to 50 km, and the mantle extends down to 1700 km.
> 
> For the core at 1700-3390 km, the depth is 3390 km with a thickness of 1690 km. The atmosphere sits at depth 0 but extends 100 km above the surface on Mars. The polar caps are trickier—they sit on the surface with the north cap reaching about 1.4 km depth and the south cap about 1.3 km depth, though I'm still figuring out whether they extend above the surface or just downward from it.
> 
> Looking at how Earth ice bodies are modeled, they use depth = 0 with thickness representing the total vertical extent, so I'll apply the same approach to Mars's polar caps: both north and south caps get depth = 0 with their respective thicknesses of 1.4 and 1.3 km.
> 
> Before finalizing everything, I need to verify the Mars core dimensions—the literature shows some variation, with InSight data suggesting around 1830 km, though more recent analyses put it between 1650-1800 km, and the reference material I'm using lists it as approximately 1700 km.
> 
> Now I'm working through the core calculations using a 1690 km radius, computing its volume and surface area, then cross-checking against Mars's total mass by summing up the contributions from each layer—atmosphere, polar caps, and the other strata. I'm recalculating the subsurface volume using the thin-shell approximation to keep things consistent, which gives me a mass of about 1.75e21 kg for that layer. Let me apply the same approach to the near-surface region as well to make sure all my estimates are using the same method.
> 
> For the deep crust, I need to switch to spherical geometry since it's a thicker shell—calculating the volume between radii of 3385 and 3340 km, which gives me roughly 6.2e9 km³ and a mass around 1.80e22 kg. Now I'm moving on to the mantle, which extends from 3340 km down to 1690 km radius.
> 
> Continuing with the mantle calculation, the volume works out to about 1.358e11 km³, giving a mass of 4.89e23 kg. The core has a volume of 2.02e10 km³ but I still need to determine its density. Now I'm summing up all the masses excluding the core, which is dominated by the mantle contribution at 4.89e23 kg, with the deep crust and other layers adding progressively smaller amounts.
> 
> Working backward from the total Mars mass of 6.42e23 kg, I can subtract the non-core layers to get the core mass at 1.33e23 kg. Dividing by the core volume gives a density of 6.6 g/cm³, which matches well with literature values for Mars's core. Now I'm compiling the complete stratum data with surface areas calculated from the outer boundary radius. I'm working through the atmospheric composition values now, trying to balance precision with the constraint of keeping non-filler resources to 2 significant digits. Since CO₂ is the dominant atmospheric component rather than a filler, I'm reconsidering whether the 3 significant figure allowance applies here, but given that Mars atmospheric data is well-established, I'll stick with the more precise values and continue filling in the remaining gases. I'm adjusting the significant figures for the atmospheric composition values—keeping nitrogen and argon at 2 sig figs to match the constraint, while verifying that oxygen, carbon monoxide, and the trace gases are all properly formatted at 2 sig figs as well. I see that Argon in the Earth data also uses 4 sig figs, which aligns with the guidance about homogeneous strata warranting higher precision. So I'll apply 3 sig figs consistently across Mars's major atmospheric components, setting up the composition data with appropriate uncertainty ranges for each gas.
> 
> Now I'm realizing the percentages I entered don't add up to 100 - I'm getting 100.08, which means I need to recalibrate. Let me pull the more precise volume fractions from the Viking and Curiosity measurements and recalculate to make sure everything balances correctly. Now I'm converting these mole fractions to mass fractions by calculating the mean molecular weight of the Martian atmosphere and then determining what fraction each component contributes by mass rather than by volume. Now I'm verifying these mole fractions sum to approximately 1, then converting them to percentages for the atmospheric composition table—CO₂ dominates at 96.64%, followed by nitrogen at 1.74% and argon at 1.47%, with the remaining gases making up less than 0.2%. Now I'm working through the trace gas calculations—converting volume fractions to mass fractions for neon, krypton, and xenon, then summing them to get the total heavy noble gases contribution. After adding hydrogen and these trace elements, the composition still rounds to about 99.97%, which checks out. I'm now considering methane, which is tricky on Mars since the measurements are contentious—the Trace Gas Orbiter shows almost nothing while Curiosity's readings are questioned by the scientific community.
> 
> Given the uncertainty, I'm treating methane as negligible for the model, essentially zero. Similarly, sulfur dioxide measurements put it at fractions of a ppb, so that's also below the model's resolution. This leaves me with the final atmospheric composition: carbon dioxide dominates at 96.6%, nitrogen at 1.74%, and argon at 1.47%, with everything else too small to matter.
> 
> Now I'm filling in the trace gases—oxygen, carbon monoxide, water vapor, hydrogen, and the heavy noble gases all get their measured values. For helium, there's no direct detection, but solar wind escape balance suggests around 10 ppm by volume, which converts to roughly 1e-4% by mass.
> 
> The atmospheric composition adds up to about 99.97%, which checks out. Since this response is going to be lengthy, I should organize everything as efficiently as possible while staying thorough. Now I need to move on to Phobos and finalize its composition breakdown.
> 
> Looking at the Phobos numbers I've set, I'm checking whether they're physically reasonable. Given Phobos's density of about 1.87 and accounting for ~30% porosity, the solid material would have a density around 2.67, which aligns well with primitive carbonaceous chondrite compositions. This suggests Phobos is likely a captured C-type or D-type asteroid, so the composition I've outlined—dominated by stone and regolith with modest amounts of water ice, organics, and trace volatiles—seems consistent with that origin. Water content: CI chondrites have ~10-15% water by mass. But D-type would be less hydrated. I set 5%, which is moderate.
> - Organics: CI have ~3-4% C in organic form. With other elements (H, N, O, S), total organics might be 5-7%. I set 3%, reasonable for D-type.
> - Total volatiles: ~8-10% for D-type.
> - Sulfur: CI have ~6% S (as troilite, etc.). Some goes to INDUSTRIAL_METAL_ORES (as sulfides). I'll set SULFUR at 2% (elemental/extractable).
> 
> If Phobos is Mars-related debris:
> - Much lower volatile content
> - Basaltic composition
> - Very little water or organics
> 
> Given the C/D-type spectral affinity, I'm leaning toward a primitive composition but probably less hydrated than CI meteorites, so my current values feel reasonable. Now I'm adding rare earth and precious metal ores—for a C/D-type body, REEs would be at chondritic levels without significant concentration, so I'm setting rare earth ores at 0.02% and precious metals at zero since there's no mechanism for enrichment. I should also account for helium from solar wind implantation on this small body, though without an atmosphere to retain it, the contribution is minimal. I need to refine the radius to get Phobos's mass right—using 11.1 km instead of 11 km gives me a volume of 5.73e3 km³ and a calculated mass of 1.07e16 kg, which matches the actual value of 1.066e16 kg much better. Now I'm moving on to set up the Deimos entry.
> 
> For Deimos, I'm working with a radius of 6.2 km, which gives me a volume around 1.00e3 km³ and a surface area of 4.83e2 km². Using a density of 1.47, I get a mass of 1.47e15 kg, which is very close to the actual 1.48e15 kg.
> 
> Now I need to update the strata.md file to reorganize the Earth section—consolidating the ice bodies into just Antarctica and Greenland, removing the separate continental water bodies category, and rolling that surface ice and water into the continental land section.
> 
> Generating updated data tables... it but not change the near-surface and deeper strata. Now I'm reconsidering whether the Continental Land values in strata.tsv should also be updated—the area, volume, and mass all increase proportionally when accounting for the former ice and water areas, even though density remains constant. The user specifically asked for strata_resources.tsv changes, but updating strata.tsv too makes sense for consistency. I'll provide updated Continental Land rows for strata.tsv as well, though Antarctica's water body is too small to warrant changes, and the "Other" category does have a meaningful shift worth capturing.
> 
> Now I'm ready to compile the actual output. I'm taking the approach where new land incorporates extra rock volume to fill previously ice/water-covered areas, with that rock having the same composition as existing land, plus the absorbed ice and water mass. This gives me a weighted average composition for each modified Continental Land region. The key shifts are that WATER content increases significantly—its dispersion jumping from around 0.6 to 2.0—while STONE and REGOLITH decrease slightly to compensate. Let me generate the full resource rows.
> 
> For the water parameters specifically, I'm setting dispersion to 2.0 across all modified land strata, bumping dispersion_error up to 0.3 to account for the higher uncertainty in modeling the combined ice/water distribution, and keeping abundance_cv at 0.2 since we have good data on the volumes we're adding. Now I'll write out the complete table rows.
> 
> I also need to update the strata.md descriptions—removing the Continental Water Bodies section entirely, clarifying that Continental Ice Bodies only covers Antarctica and Greenland, and noting that Continental Land now includes the surface ice and water components. I'm calculating the combined volume and area for the remaining ice bodies—Antarctica and Greenland together cover about 14.5 million km² at roughly 2 km average thickness, which aligns well with the original since these two regions dominate the ice mass. Now I'm compiling the Continental Land data for all eight regions, starting with recalculating the USA's combined area and volume across its land, glacier, and water components. Now I'm calculating the remaining stone by subtracting all the other mineral resources from the 62.7 total, accounting for how the ice and water contributions scaled down the ore concentrations while leaving industrial minerals relatively stable at around 5.0. Now I'm applying the old ratio to distribute the remaining mass between stone and regolith, which gives me 62.6 and 27.3 respectively. The total comes to 99.93, which is close enough given the rounding involved.
> 
> I'll go with STONE = 62.7 and REGOLITH = 27.3 to get closer to 100, since the code normalizes anyway. Now moving to China's calculations: I'm computing the new area by adding the incremental changes, then working through the density and mass to determine the ice and water fractions. Now I'm working through the remaining resource values and their contributions to the total composition. Adding up all the non-filler, non-water resources gives me about 5.167, which combined with water brings the total to 9.44, leaving roughly 90.56 for stone and regolith distributed according to their density ratio of about 2.3 to 1.
> 
> For the EU calculations, I'm determining the new surface area by adding the base area to the additional coverage, then computing the resulting volume and mass, and finally calculating the ice fraction based on these updated parameters.
> 
> Now I'm working through the water content by combining contributions from the existing water, ice, and other sources, which gives me about 4.76 units. The remaining resource fractions stay relatively stable since the rock component dominates at 0.992, so I'm just noting the minor ore and mineral concentrations that carry forward essentially unchanged. Now I'm calculating the composition for Russia by combining the new area measurements and computing the mass-based fractions for ice, water, and rock, which gives me a water content of about 4.985.
> 
> Adding the remaining components from the calculation brings the total to 5.33, and I'm noting that the other resource concentrations—iron ores, aluminum, rare earth elements, sulfur, industrial minerals, and the rest—remain essentially unchanged from the baseline estimates.
> 
> Now I'm working through the final sums: the non-filler, non-water resources total 5.162, water comes to 5.33, giving me an overall sum of 10.49. That means stone and regolith together make up 89.51 percent. Using the ratio of 63.3 to 26.5, I'm splitting this into stone at 63.1 percent and regolith at 26.4 percent.
> 
> For India, I'm calculating the new area by adding the base area to the additional contributions, which gives me approximately 3.33 million square kilometers.
> 
> Now I'm computing the composition for India's resources: the new surface fraction is 0.00653, volume reaches 3.33 times 10 to the fifth, and total mass comes to 6.66 times 10 to the 17. Breaking down the fractions—ice, water, and rock—I get water content at about 3.25 percent. The other ore resources like iron and aluminum remain essentially unchanged.
> 
> Tallying up all the non-filler, non-water resources gives me roughly 5.117, so combined with water at 3.25, the total sits around 8.37 percent, leaving stone and regolith to make up the remaining 91.63 percent. Using the ratio of 2.294 between stone and regolith, I'm splitting that remainder into stone at 63.8 percent and regolith at 27.9 percent.
> 
> Now I'm working through Japan's composition—calculating its surface area, specific formation, volume, and mass, then determining the water fraction at about 0.3 percent with rock making up the rest. From there I'm computing the water content by blending the rock-based composition with the water-based one.
> 
> For the remaining resources, I'm listing out all the minor constituents like iron ores, aluminum, rare earths, uranium, sulfur, and organics, then summing up everything that isn't water or filler material to see what else contributes to the total.
> 
> Now I'm calculating the final composition: water comes to 4.29, all other resources sum to about 5.07, leaving 90.64 for stone and regolith combined. Using the ratio of 65.4 to 25.5 between them, I get stone at 65.3 and regolith at 25.4, which checks out to 100.06 total.
> 
> For Antarctica specifically, the water body mass is so small compared to the land mass that I'm leaving the strata values unchanged. But for the other regions, removing that water body shifts the calculations—the new area and volume change slightly, which adjusts the water fraction to about 0.00369 and rock fraction to 0.996, giving me a water contribution of roughly 4.35.
> 
> Now I'm working through the resource distribution for the remaining materials. After summing up all the ore types, industrial minerals, and organic resources, I get about 5.15 total, which combined with the water value of 4.35 leaves stone and regolith to make up the remaining 90.50 of the total.
> 
> I'm calculating how to split that 90.50 between stone and regolith using a ratio that accounts for the different material compositions, which gives me stone at 63.0 and regolith at 27.5. The numbers check out to exactly 100.0 when I add everything together.
> 
> For the water distribution parameters across the modified continental terrain, I'm increasing the dispersion value from 0.6 to 2.0 to better represent how water concentrates in certain areas while other regions remain dry, and adjusting the dispersion error accordingly to 0.3.
> 
> Antarctica's water parameters look reasonable as they were, but for the other regions I'm bumping the water abundance up to 4.4 with the new dispersion settings of 2.0 and error of 0.3. Now I need to compile the complete resource rows with all 27 columns for each modified continental land type.
> 
> Given the scope of changes, I'll organize the output as: rows to remove, the full modified continental land rows, and then the new Mars/Phobos/Deimos entries for both strata and resource files. Let me format these as TSV blocks.
> 
> For Mars subsurface composition, I'm setting stone at 85% for intact basaltic rock at depth, which aligns well with Earth's continental subsurface baseline of around 86%.
> 
> Now I'm refining the Mars near-surface layer—the weathered bedrock and cemented regolith between 5m and 500m depth should sit between loose surface material and solid rock. My current split of 65% stone and 16% regolith feels right, with the remaining percentage accounting for water and minerals.
> 
> Let me verify the math on all the trace components: iron ores, aluminum, rare earths, sulfur, industrial minerals, water, and various gases add up to about 19%, which means stone and regolith together fill the remaining 81%—and my 65/16 split lands exactly there.
> 
> Now I'm ready to write out the full response, though I'm also wondering how to handle the subsurface and deep crust layers for Mars, particularly around density considerations.
> 
> Looking at the crustal density estimates across different depths—surface regolith at around 1.5, near-surface porous rock closer to 2.2, upper crust ranging from 2.5 to 2.8, and lower crust between 2.8 and 3.0—my choices of 2.7 for subsurface and 2.9 for deep crust fit well within those ranges.
> 
> I should also include sulfur dioxide in the south polar cap composition since that's been detected there.
> 
> Now I'm noticing several issues to flag in the feedback: the strata descriptions need to clarify what happens at boundaries between layers, the area columns should specify whether they're measuring outer or inner boundaries, changes to land areas should propagate through the near-surface and deeper strata, and some precision conventions could be more explicit. Before I write the response, let me double-check how I'm handling the Mars strata for the composition calculations, particularly verifying the atmospheric composition adds up correctly. I'm mapping out all 27 resource columns in the correct order and preparing to fill in the complete data rows, even where values are zero.
> 
> Now I need to decide whether to include full rows for unchanged strata or just the modified ones—since the user wants complete code when making changes, I should provide the entire rows for all modified Mars strata to be thorough.
> 
> Generating resource table data... Now I'm looking at performance metrics for different Mars regions, with the north polar cap showing varying parameter values across different conditions. Now I'm looking at the spectral data for different Martian regions, examining the reflectance values and absorption characteristics across various wavelength bands for the south polar cap and regolith compositions. I'm verifying the distribution adds up correctly to 100%, and now looking at the near-surface Mars composition data with its own set of elemental parameters. Looking at the parameter values and verifying they sum to approximately 100, which checks out. Now I'm setting up the subsurface composition data for Mars with its own set of parameters. I'm looking at parameter values and adjusting them to get the sum closer to 100—tweaking STONE down to 85.0 brings it to about 99.97, which works. Now I'm compiling the deep crust parameters for Mars, working through the various compositional and structural measurements to calculate aggregate values. Continuing to work through the MARS_MANTLE configuration values and verifying the distribution sums to the expected total. similar to Earth's mantle. Now I'm setting up the Mars core composition with iron as the dominant element at 80%, then layering in smaller amounts of nickel, sulfur, and oxygen to reflect the planet's smaller, cooler core compared to Earth's.
> 
> Verifying the Mars core percentages sum to 100, then moving on to define Phobos's bulk composition with its own elemental distribution. I'm verifying the composition totals and they check out around 100. For Phobos specifically, I should reconsider whether methane and carbon monoxide make sense given its airless, small body nature—the organic content is better captured through tholins, and any volatile ices would be primarily water ice with trace amounts of CO2 and ammonia rather than those gases. I'm verifying the composition totals sum to 100%, which checks out. Now I'm reconsidering whether 5% water content makes sense for Phobos—if it's a captured D-type asteroid, water could reasonably range from 0-10%, but if it's debris from Mars, it'd be closer to 1-3%. The high coefficient of variation at 0.5 seems appropriate given this uncertainty. Moving on to Deimos with similar but slightly adjusted parameters. Now I'm verifying the composition values sum to approximately 100%, which checks out. Deimos appears less dense than Phobos, so I've adjusted its composition to reflect higher porosity with slightly more water content and less metal. I'm ready to start building out the strata.tsv file for Mars.
> 
> For the atmosphere layer, I need to calculate the surface area at the outer boundary rather than at Mars's surface—checking against Earth's data confirms this approach, where the atmosphere area is measured at the top of the atmospheric layer rather than at the planet's surface.
> 
> Now I'm computing the volume of the atmospheric shell by taking the difference between the outer and inner spheres, then converting to cubic meters to get the density: with a mass of 2.5e16 kg and a volume of 1.51e19 m³, that gives me a density of about 1.66e-3 kg/m³.
> 
> Converting to g/cm³ requires dividing by 1000, which puts Earth's atmosphere at roughly 1.54e-4 g/cm³—I can verify this checks out using Earth's known atmospheric mass and volume. Now I'm working through Mars's density using the same approach.
> 
> The Mars calculation gives 1.66e-6 g/cm³, which seems surprisingly low compared to surface pressure, but that's because I'm averaging the density across the entire 100 km atmospheric shell where most of the mass concentrates near the surface. Let me verify my Earth numbers using the pressure-based approach: surface pressure times area divided by gravity gives about 5.27e18 kg, which matches the known atmospheric mass pretty closely. Now I need to check Mars the same way. So that gives me 1.66e-6 g/cm³ for the Martian atmosphere density. Now I'm working through the parameters for the north polar cap—calculating its area from the spherical fraction, then determining volume and density based on a depth of 1.4 km.
> 
> For the south polar cap, I'm using similar calculations with slightly different dimensions: 1.3 km depth and a larger spherical fraction, which yields a higher mass of about 1.72e18 grams. Now I'm moving into the regolith layer with much shallower depth of 0.005 km and nearly complete surface coverage.
> 
> For the near-surface layer, I'm calculating the volume with a 0.5 km thickness, accounting for the overlap with the regolith layer below it—the actual thickness is 0.495 km, but rounding to 0.5 km keeps the calculation simple since the difference is negligible. Now I'm calculating the subsurface layer with a depth of 5 km and thickness of 4.5 km, giving a volume of about 6.53e8 km³. The outer boundary sits at 3389.5 km radius where the near-surface layer begins, so the surface area there is roughly 1.44e8 km², accounting for the full spherical coverage. I'll use the thin-shell approximation to simplify this—the subsurface layer is thin enough that I can just multiply the surface area by the thickness to get volume, which gives me about 6.48×10^8 km³. With a density of 2.7, that translates to a mass around 1.75×10^21 kg. Now I'm moving on to the deep crust layer, which extends from 5 km to 50 km depth, so it's 45 km thick, and I need to calculate its surface area at the outer boundary.
> 
> For the deep crust, I'm using the full spherical shell formula since the thickness is more significant here—subtracting the volume of the inner sphere from the outer sphere gives me about 6.41×10^9 km³, and with a density of 2.9, the mass comes out to roughly 1.86×10^22 kg. Next I'm looking at the mantle layer. Now I'm calculating the core's volume and mass using the same approach—with an outer radius of 1690 and density of 6.6, I get a mass of about 1.33e23. Let me verify this against the total Mars mass by summing up all the layers from atmosphere through core. Now I'm calculating the bulk properties for Phobos and Deimos, working through their dimensions, volumes, and masses based on their mean radii and assumed densities.
> 
> With all the calculations complete, I'm ready to compile the final response, including confidence levels for different survey categories across Mars—from the well-characterized atmosphere down through the poorly understood deep crust.
> 
> Actually, let me reconsider the dispersion parameters for the modified Earth Continental Land resource settings. I'm checking whether the non-water resources should have their dispersion values adjusted to match the water configuration, or if the existing iron ore parameters should stay as they are.
> 
> Since the ice and water areas I'm adding contain minimal non-water resources, and the underlying rock composition matches the rest of the continental land, the spatial distribution characteristics shouldn't shift meaningfully. I'll keep those dispersions unchanged.
> 
> For water itself, I'm setting the abundance based on my earlier calculations, keeping the coefficient of variation at 0.2 since we have good survey data on these water bodies, and adjusting the dispersion to 2.0 to reflect the bimodal distribution of concentrated water bodies within rocky terrain. The dispersion error goes to 0.3 because modeling this combined distribution as lognormal introduces some uncertainty. Now I'm wondering whether the dispersion error should actually vary depending on how much ice coverage each territory has.
> 
> Territories with larger ice-to-land ratios would have more concentrated water bodies and potentially higher dispersion, but the differences between them are fairly minimal. I'll apply 2.0 and 0.3 uniformly across all territories. For India specifically, the ice contribution is negligible and water bodies only add about 0.21% to the total, but even small amounts of concentrated water create high dispersion values since some locations reach nearly 100% water. Antarctica's dispersion stays at 0.6 since I'm not changing its composition, and I need to figure out what happens with the "Other" category now that it's absorbing significant water mass.
> 
> Looking at the Other Continental Land strata more carefully, the original water parameters were 4, 0.25, 0.6, 0.15, and after absorbing the water bodies they become 4.4, 0.2, 2.0, 0.3. This feels consistent. Before I finalize everything, I should check whether the survey notes in strata.tsv need updating for the modified land strata—though since the original survey levels already accounted for these surface features, adding the ice and water mass probably doesn't meaningfully change our confidence levels.
> 
> Now I'm ready to organize the response into sections covering the markdown updates, the rows to remove and modify in both TSV files, the new planetary rows, and some notes on simplifications. Given how much data there is, I'll present the TSV changes as formatted blocks and keep the explanatory text tight. I should also double-check my near-surface resource values for Mars before finalizing. The composition shifts for Japan are negligible—ice contributes essentially nothing and water adds only 0.30%, so the percentage changes round away. For India, the ice is similarly trivial at 0.049%, though I should check if the water contribution is significant enough to warrant updating those regions.
> 
> Actually, I need to reconsider—the user specifically asked for these updates, and the water dispersion change from 0.6 to 2.0 is meaningful regardless of mass impact. I'm also recalculating the area and scale factor updates for both territories, which involve small but measurable adjustments to their geographic parameters.
> 
> For the documentation, I'm adding clarifying notes about surface ice and water bodies to the modified strata entries, then reviewing what needs updating in the strata.md file. The Earth section description requires modifications to reflect the ice body changes, particularly for Antarctica and Other regions, while the Continental Water Bodies entry should be removed. The regional subdivision note doesn't need changes since it doesn't specifically address ice or water features.
> 
> Now I'm organizing the response to present these updates clearly. structure, I'll start with a brief summary, then walk through the changes to strata.md and the TSV files, note the simplifications around removing ice/water strata, and provide feedback.
> 
> I should also mention that the Continental Land survey notes now include surface ice/water in the strata, and I need to revisit note 4 in strata.md since it describes how continental strata relate—removing water bodies and some ice means I'll need to simplify that description.
> 
> Now I'm verifying the resource values and making sure every cell is populated, even with zeros. Let me double-check the Mars near-surface composition totals to ensure everything adds up correctly before finalizing. I'm finishing up the resource distribution table and verifying the totals sum to 100. The numbers add up to 99.95, which is within acceptable range, but I could fine-tune it by adjusting Stone slightly to hit exactly 100.
> 
> 05 if needed—though 99.95 is close enough. Now I'm ready to write out the response.
> 
> Before I do, I need to clarify something important about the Continental Land modifications: when I merge the ice and water strata into the land stratum, I'm updating the area, surface area, volume, and mass values in both the strata and resource files. The question is whether thickness and density should change too. The original land stratum has a thickness of 0.1 km and density of 2.0, and since the ice and water masses are negligible compared to the rock underneath, I can keep the density at 2.0 and just recalculate the mass based on the expanded volume.
> 
> I'll present the changes as just the modified rows for both files to keep things concise. I should also note that the near-surface strata might need adjusting since its spherical fraction was based on land plus shelf, but the user didn't ask for that and the impact is minimal—I'll mention it as a side note. Let me quickly check how much the land surface area changes for the USA as a reference point.

This is a substantial set of changes. I'll work through it systematically: (1) Earth modifications (remove ice/water strata, merge into land), (2) Mars system additions, (3) strata.md text updates, and (4) feedback.

---

## Work and Assumptions

### Earth Continental Land — absorbing ice and water

For each territory, the new land stratum absorbs the mass of removed ice/water bodies. I also account for previously unmodeled rock under ice/water by expanding the land area to cover those footprints. The new land composition is a weighted average of the enlarged rock mass (at old land composition) and the absorbed ice/water mass.

**Mass fraction calculations** (showing USA as worked example):

| Territory | Old land mass | Ice mass | Water mass | New total mass | f_ice | f_water | f_rock | New WATER% |
|---|---|---|---|---|---|---|---|---|
| USA | 1.97e18 | 7.83e15 | 1.15e16 | 2.04e18 | 0.0038 | 0.0056 | 0.991 | 4.9 |
| China | 1.92e18 | 3.19e15 | 2.30e15 | 1.94e18 | 0.0016 | 0.0012 | 0.997 | 4.3 |
| EU | 1.00e18 | 3.60e15 | 4.60e15 | 1.03e18 | 0.0035 | 0.0045 | 0.992 | 4.8 |
| Russia | 3.42e18 | 2.75e15 | 9.20e15 | 3.48e18 | 0.0008 | 0.0026 | 0.997 | 5.3 |
| India | 6.58e17 | 3.24e14 | 1.38e15 | 6.66e17 | 0.0005 | 0.0021 | 0.997 | 3.2 |
| Japan | 7.56e16 | 2.70e10 | 2.30e14 | 7.66e16 | ≈0 | 0.0030 | 0.997 | 4.3 |
| Antarctica* | 2.80e18 | — | 4.60e12 | 2.80e18 | — | ≈0 | ≈1 | 6.0 (no change) |
| Other** | 1.80e19 | — | 6.76e16 | 1.83e19 | — | 0.0037 | 0.996 | 4.4 |

\* Antarctica only removes water bodies (negligible). \*\* Other only removes water bodies (ice bodies kept).

New WATER% formula: \(f_{rock} \times W_{old} + f_{ice} \times 99.8 + f_{water} \times 99.95\)

STONE and REGOLITH are adjusted downward proportionally to maintain ~100% total. Water dispersion increases from ~0.6 to 2.0 (in log₁₀ units) because the stratum now spans from dry rock (~1–4% groundwater) to pure-water bodies (lakes, glaciers), covering ~2 orders of magnitude in concentration.

### Mars strata — geometry calculations

Mars mean radius: 3,390 km. Surface area: \(4\pi \times 3390^2 = 1.45 \times 10^8\) km².

**Mass budget cross-check** (target: Mars total mass 6.42e23 kg):

| Stratum | Outer R (km) | Inner R (km) | Volume (km³) | Density (g/cm³) | Mass (kg) |
|---|---|---|---|---|---|
| Atmosphere | 3490 | 3390 | 1.51e10 | 1.7e-6 | 2.5e16 |
| N. Polar Cap | surface | — | 1.12e6 | 1.0 | 1.12e18 |
| S. Polar Cap | surface | — | 1.56e6 | 1.1 | 1.72e18 |
| Regolith | 3390 | 3389.995 | 7.15e5 | 1.5 | 1.07e18 |
| Near-Surface | 3389.995 | 3389.5 | 7.15e7 | 2.2 | 1.57e20 |
| Subsurface | 3389.5 | 3385 | 6.53e8 | 2.7 | 1.76e21 |
| Deep Crust | 3385 | 3340 | 6.41e9 | 2.9 | 1.86e22 |
| Mantle | 3340 | 1690 | 1.36e11 | 3.6 | 4.89e23 |
| Core | 1690 | 0 | 2.02e10 | 6.6 | 1.33e23 |
| **Total** | | | | | **6.42e23** ✓ |

Mantle density (3.6 g/cm³) and core density (6.6 g/cm³) are solved to match the total mass, consistent with InSight-era estimates of a lighter, sulfur-rich core.

**Mars compositions** are based on: Viking/Curiosity mass spectrometry (atmosphere), MARSIS/SHARAD radar + Phoenix (polar caps), APXS/CheMin from 6 landing sites + orbital spectroscopy (regolith), InSight seismology + SNC meteorite chemistry (deeper layers). The high sulfur content of Mars regolith (~5% SO₃) and the high ground-ice variability with latitude are the most distinctive features relative to Earth.

**Phobos/Deimos** compositions reflect C/D-type spectral affinity with high porosity (density 1.87/1.47 g/cm³). I used a carbonaceous chondrite-like template with moderate water of hydration (~5–6%) and organic content (~3%), consistent with a captured primitive body interpretation. High abundance_cv values (0.5) reflect the fundamental ambiguity between captured-body and co-accretion/impact-debris formation hypotheses.

---

## strata.md text updates

Replace the Continental Ice Bodies, Continental Water Bodies, and Continental Land bullets in the Earth section with the following:

> - **Continental Ice Bodies** — Major ice sheets and glaciers for Antarctica and Other (Greenland) regions only; simplified geometry is ~14.5 million km² area (~2.8% of Earth surface) at ~2.0 km average thickness. For all other territorial regions, glacier and ice mass is absorbed into Continental Land.
> - **Continental Land** — All land area including areas covered by glaciers and surface water bodies, to 100 m depth (~152 million km²). Composition includes groundwater, permafrost, surface water bodies (lakes, rivers, reservoirs), and glaciers (for non-Antarctic/Greenland territories). Quarrying, strip mining, placer deposits, regolith extraction, shallow wells, freshwater extraction.

(Remove the Continental Water Bodies bullet entirely.)

Update note 4 — replace:

> 4. Earth "continental land" and "continental shelf" encompass land area and shelf area, respectively. Continental strata below these encompass both land and shelf area (this should capture all offshore drilling operations). "Ocean" as a volume includes water over the continental shelf. Therefore, in area construction: total surface = continental land + continental shelf + ocean floor = continental near-surface (or deeper "continental") + ocean crust; ocean = continental shelf + ocean floor; ocean ≠ ocean floor.

with:

> 4. Earth "continental land" and "continental shelf" encompass land area (including glacier and surface water body footprints, except for Antarctica and Other ice bodies which remain separate strata) and shelf area, respectively. Continental strata below these encompass both land and shelf area (this should capture all offshore drilling operations). "Ocean" as a volume includes water over the continental shelf. Therefore, in area construction: total surface ≈ continental ice bodies (Antarctica + Other) + continental land + continental shelf + ocean floor; continental near-surface (or deeper "continental") ≈ continental land + continental shelf; ocean = continental shelf + ocean floor; ocean ≠ ocean floor.

---

## strata.tsv changes

### Rows to remove

```
EARTH_USA_CONTINENTAL_ICE_BODIES
EARTH_CHINA_CONTINENTAL_ICE_BODIES
EARTH_EU_CONTINENTAL_ICE_BODIES
EARTH_RUSSIA_CONTINENTAL_ICE_BODIES
EARTH_INDIA_CONTINENTAL_ICE_BODIES
EARTH_JAPAN_CONTINENTAL_ICE_BODIES
EARTH_USA_CONTINENTAL_WATER_BODIES
EARTH_CHINA_CONTINENTAL_WATER_BODIES
EARTH_EU_CONTINENTAL_WATER_BODIES
EARTH_RUSSIA_CONTINENTAL_WATER_BODIES
EARTH_INDIA_CONTINENTAL_WATER_BODIES
EARTH_JAPAN_CONTINENTAL_WATER_BODIES
EARTH_ANTARCTICA_CONTINENTAL_WATER_BODIES
EARTH_OTHER_CONTINENTAL_WATER_BODIES
```

### Modified and new rows

```tsv
name	body	territory	strata_group	depth	thickness	spherical_fraction	area	volume	density	mass	survey_level	survey_note
EARTH_USA_CONTINENTAL_LAND	EARTH	USA	CONTINENTAL_LAND	0.1	0.1	0.0200	1.02e7	1.02e6	2.0	2.04e18	5.5	Extensive USGS surveys; includes Great Lakes and Alaska glaciers
EARTH_CHINA_CONTINENTAL_LAND	EARTH	CHINA	CONTINENTAL_LAND	0.1	0.1	0.0190	9.71e6	9.71e5	2.0	1.94e18	5.0	Broad exploration; includes Tibetan Plateau glaciers and major lakes
EARTH_EU_CONTINENTAL_LAND	EARTH	EU	CONTINENTAL_LAND	0.1	0.1	0.0101	5.15e6	5.15e5	2.0	1.03e18	6.0	Longest mining history; includes Alpine glaciers and extensive lake systems
EARTH_RUSSIA_CONTINENTAL_LAND	EARTH	RUSSIA	CONTINENTAL_LAND	0.1	0.1	0.0340	1.74e7	1.74e6	2.0	3.48e18	4.5	Soviet-era surveys extensive; includes Lake Baikal and Arctic island glaciers
EARTH_INDIA_CONTINENTAL_LAND	EARTH	INDIA	CONTINENTAL_LAND	0.1	0.1	0.00653	3.33e6	3.33e5	2.0	6.66e17	4.5	Major mining districts well explored; includes Himalayan glaciers and reservoirs
EARTH_JAPAN_CONTINENTAL_LAND	EARTH	JAPAN	CONTINENTAL_LAND	0.1	0.1	7.51e-4	3.83e5	3.83e4	2.0	7.66e16	5.5	Small territory; well-surveyed; includes Lake Biwa and other water bodies
EARTH_ANTARCTICA_CONTINENTAL_LAND	EARTH	ANTARCTICA	CONTINENTAL_LAND	0.1	0.1	0.0275	1.40e7	1.40e6	2.0	2.80e18	1.5	Ice-covered; minimal surface geological exploration
EARTH_OTHER_CONTINENTAL_LAND	EARTH	OTHER	CONTINENTAL_LAND	0.1	0.1	0.179	9.13e7	9.13e6	2.0	1.83e19	3.5	Highly variable by country; includes Canadian lakes and major mining nations
MARS_ATMOSPHERE	MARS		ATMOSPHERE	0	100	1	1.53e8	1.51e10	1.7e-6	2.5e16	7.0	Well-characterized by Viking Curiosity Perseverance MAVEN; trace species less certain
MARS_NORTH_POLAR_CAP	MARS		NORTH_POLAR_CAP	1.4	1.4	5.5e-3	8.0e5	1.12e6	1.0	1.12e18	3.0	MARSIS/SHARAD radar profiling; Phoenix nearby; dust-layer stratigraphy resolved
MARS_SOUTH_POLAR_CAP	MARS		SOUTH_POLAR_CAP	1.3	1.3	8.3e-3	1.20e6	1.56e6	1.1	1.72e18	2.5	MARSIS radar; buried CO2 discovered but volume uncertain; less accessible than north
MARS_REGOLITH	MARS		REGOLITH	0.005	0.005	0.986	1.43e8	7.15e5	1.5	1.07e18	2.0	Six landing sites with APXS/CheMin analysis; orbital spectroscopy; vast unsampled areas
MARS_NEAR_SURFACE	MARS		NEAR_SURFACE	0.5	0.50	0.986	1.43e8	7.15e7	2.2	1.57e20	1.2	InSight seismic; neutron/radar ice mapping; no direct subsurface sampling
MARS_SUBSURFACE	MARS		SUBSURFACE	5	4.5	1	1.45e8	6.53e8	2.7	1.76e21	0.8	Radar hints of structure; theoretical models; no drilling data
MARS_DEEP_CRUST	MARS		DEEP_CRUST	50	45	1	1.44e8	6.41e9	2.9	1.86e22	0.5	InSight seismic layering; gravity models; SNC meteorite constraints
MARS_MANTLE	MARS		MANTLE	1700	1650	1	1.40e8	1.36e11	3.6	4.89e23	0.5	InSight seismic; SNC meteorite chemistry; mineral physics models
MARS_CORE	MARS		CORE	3390	1690	1	3.59e7	2.02e10	6.6	1.33e23	0.3	InSight seismic core shadow; moment of inertia; sulfur content highly uncertain
PHOBOS_BULK	PHOBOS		BULK	11.1	11.1	1	1.55e3	5.73e3	1.87	1.07e16	1.5	Mars Express flybys; spectral and density data; no surface measurements
DEIMOS_BULK	DEIMOS		BULK	6.2	6.2	1	4.83e2	1.00e3	1.47	1.47e15	1.0	Limited flyby imaging; density measurement; spectral class inferred
```

---

## strata_resources.tsv changes

### Rows to remove

Same 14 names as listed above for strata.tsv.

### Modified Earth Continental Land rows

```tsv
name	IRON_NICKEL	IRON_ORES	ALUMINUM_ORES	INDUSTRIAL_METAL_ORES	PRECIOUS_METAL_ORES	RARE_EARTH_ORES	URANIUM_ORES	SULFUR	INDUSTRIAL_MINERALS	ORGANICS_THOLINS	COAL	OIL	STONE	REGOLITH	WATER	HYDROGEN	OXYGEN	NITROGEN	CARBON_DIOXIDE	CARBON_MONOXIDE	AMMONIA	METHANE	ETHANE	SULFUR_DIOXIDE	HELIUM	ARGON	HEAVY_NOBLE_GASES
EARTH_USA_CONTINENTAL_LAND	0,0,0,0	0.008,0.25,1.5,0.25	0.004,0.35,1.5,0.35	0.015,0.3,1.5,0.25	5e-4,0.4,2.0,0.35	0.002,0.4,1.5,0.35	0.001,0.45,1.8,0.35	0.02,0.35,1.2,0.25	5,0.15,0.8,0.15	0.05,0.35,1.3,0.25	0.015,0.3,1.8,0.25	0.001,0.5,2.0,0.4	62.7,0.06,0.25,0.06	27.3,0.08,0.3,0.08	4.9,0.2,2.0,0.3	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.003,0.5,1.5,0.4	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
EARTH_CHINA_CONTINENTAL_LAND	0,0,0,0	0.015,0.3,1.5,0.3	0.006,0.35,1.5,0.35	0.03,0.3,1.5,0.3	4e-4,0.45,2.0,0.4	0.01,0.3,1.5,0.3	0.001,0.5,1.8,0.4	0.02,0.35,1.2,0.3	5,0.18,0.8,0.18	0.06,0.35,1.3,0.3	0.02,0.3,1.8,0.3	5e-4,0.5,2.0,0.45	63.1,0.07,0.25,0.07	27.4,0.1,0.3,0.1	4.3,0.2,2.0,0.3	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.004,0.5,1.5,0.4	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
EARTH_EU_CONTINENTAL_LAND	0,0,0,0	0.006,0.2,1.5,0.2	0.003,0.3,1.5,0.3	0.015,0.25,1.5,0.25	3e-4,0.35,2.0,0.3	0.002,0.35,1.5,0.3	8e-4,0.4,1.8,0.35	0.015,0.3,1.2,0.25	6,0.12,0.8,0.12	0.04,0.3,1.3,0.25	0.008,0.25,1.8,0.25	3e-4,0.45,2.0,0.4	61.8,0.05,0.25,0.05	27.4,0.08,0.3,0.08	4.8,0.2,2.0,0.3	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.002,0.45,1.5,0.4	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
EARTH_RUSSIA_CONTINENTAL_LAND	0,0,0,0	0.02,0.3,1.5,0.3	0.005,0.35,1.5,0.35	0.03,0.3,1.5,0.3	8e-4,0.4,2.0,0.35	0.003,0.45,1.5,0.35	0.0015,0.45,1.8,0.35	0.02,0.35,1.2,0.3	5,0.18,0.8,0.18	0.06,0.35,1.3,0.3	0.015,0.35,1.8,0.3	0.001,0.45,2.0,0.4	63.1,0.07,0.25,0.07	26.4,0.1,0.3,0.1	5.3,0.2,2.0,0.3	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.005,0.45,1.5,0.35	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
EARTH_INDIA_CONTINENTAL_LAND	0,0,0,0	0.02,0.3,1.5,0.3	0.008,0.35,1.5,0.3	0.015,0.35,1.5,0.3	3e-4,0.45,2.0,0.4	0.003,0.4,1.5,0.35	5e-4,0.5,1.8,0.4	0.015,0.35,1.2,0.3	5,0.2,0.8,0.18	0.04,0.4,1.3,0.3	0.012,0.35,1.8,0.3	3e-4,0.5,2.0,0.45	63.8,0.08,0.25,0.08	27.9,0.1,0.3,0.1	3.2,0.2,2.0,0.3	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.003,0.5,1.5,0.4	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
EARTH_JAPAN_CONTINENTAL_LAND	0,0,0,0	0.002,0.3,1.5,0.3	0.001,0.4,1.5,0.35	0.005,0.35,1.5,0.3	2e-4,0.4,2.0,0.35	0.001,0.4,1.5,0.35	3e-4,0.5,1.8,0.4	0.04,0.3,1.2,0.25	5,0.15,0.8,0.15	0.02,0.35,1.3,0.3	0.002,0.35,1.8,0.3	1e-4,0.5,2.0,0.45	65.3,0.06,0.25,0.06	25.4,0.08,0.3,0.08	4.3,0.2,2.0,0.3	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.001,0.5,1.5,0.4	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
EARTH_ANTARCTICA_CONTINENTAL_LAND	0,0,0,0	0.01,0.7,1.5,0.6	0.005,0.8,1.5,0.7	0.02,0.7,1.5,0.6	5e-4,0.8,2.0,0.7	0.002,0.8,1.5,0.7	0.001,0.8,1.8,0.7	0.02,0.7,1.2,0.6	4,0.5,0.8,0.4	0.03,0.7,1.3,0.6	0.005,0.6,1.8,0.5	3e-4,0.8,2.0,0.7	64.4,0.2,0.25,0.15	25.5,0.25,0.3,0.2	6,0.4,0.6,0.3	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.003,0.8,1.5,0.6	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
EARTH_OTHER_CONTINENTAL_LAND	0,0,0,0	0.015,0.35,1.5,0.35	0.008,0.4,1.5,0.35	0.025,0.35,1.5,0.35	0.001,0.45,2.0,0.4	0.003,0.45,1.5,0.4	0.0015,0.5,1.8,0.4	0.02,0.4,1.2,0.3	5,0.2,0.8,0.2	0.06,0.4,1.3,0.3	0.01,0.4,1.8,0.35	0.003,0.5,2.0,0.45	63.0,0.08,0.25,0.08	27.5,0.12,0.3,0.12	4.4,0.2,2.0,0.3	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.003,0.5,1.5,0.4	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
```

### New Mars, Phobos, Deimos rows

```tsv
name	IRON_NICKEL	IRON_ORES	ALUMINUM_ORES	INDUSTRIAL_METAL_ORES	PRECIOUS_METAL_ORES	RARE_EARTH_ORES	URANIUM_ORES	SULFUR	INDUSTRIAL_MINERALS	ORGANICS_THOLINS	COAL	OIL	STONE	REGOLITH	WATER	HYDROGEN	OXYGEN	NITROGEN	CARBON_DIOXIDE	CARBON_MONOXIDE	AMMONIA	METHANE	ETHANE	SULFUR_DIOXIDE	HELIUM	ARGON	HEAVY_NOBLE_GASES
MARS_ATMOSPHERE	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.013,0.3,0,0	7e-5,0.3,0,0	0.096,0.03,0,0	1.74,0.01,0,0	96.6,0.002,0,0	0.052,0.05,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	1e-4,0.5,0,0	1.47,0.01,0,0	2e-4,0.1,0,0
MARS_NORTH_POLAR_CAP	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	1,0.3,0.5,0.2	0,0,0,0	0,0,0,0	0,0,0,0	0.5,0.3,0.5,0.2	3,0.2,0.5,0.2	95,0.05,0.1,0.05	0,0,0,0	0,0,0,0	0,0,0,0	0.5,0.4,0.5,0.2	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
MARS_SOUTH_POLAR_CAP	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	3,0.3,0.5,0.2	0,0,0,0	0,0,0,0	0,0,0,0	2,0.3,0.5,0.2	5,0.2,0.5,0.2	82,0.1,0.15,0.08	0,0,0,0	0,0,0,0	0,0,0,0	8,0.3,0.3,0.15	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
MARS_REGOLITH	0,0,0,0	2,0.2,0.8,0.3	0,0,0,0	0.3,0.3,1.0,0.4	0,0,0,0	0.05,0.5,0.8,0.4	0,0,0,0	2,0.15,0.6,0.2	6,0.15,0.5,0.2	0.01,0.8,1.5,0.5	0,0,0,0	0,0,0,0	3,0.15,0.3,0.1	83.6,0.08,0.2,0.08	3,0.3,1.5,0.5	0,0,0,0	0,0,0,0	0.01,0.5,0.8,0.4	0.03,0.3,0.5,0.2	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
MARS_NEAR_SURFACE	0,0,0,0	1.5,0.25,0.8,0.3	0.5,0.4,1.0,0.4	0.3,0.3,1.0,0.4	0,0,0,0	0.05,0.5,0.8,0.4	0.01,0.6,1.0,0.5	1.5,0.2,0.6,0.25	5,0.2,0.5,0.2	0.005,0.8,1.5,0.5	0,0,0,0	0,0,0,0	65,0.08,0.2,0.08	16,0.15,0.3,0.1	10,0.4,2.0,0.5	0,0,0,0	0,0,0,0	0.005,0.5,0.8,0.4	0.08,0.3,0.5,0.2	0,0,0,0	0,0,0,0	5e-4,0.8,2.0,0.8	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
MARS_SUBSURFACE	0,0,0,0	1,0.3,1.0,0.4	0.3,0.4,1.0,0.4	0.5,0.35,1.2,0.4	5e-4,0.6,1.5,0.6	0.05,0.5,0.8,0.4	0.01,0.6,1.0,0.5	1,0.25,0.8,0.3	3,0.25,0.5,0.2	0.002,0.8,1.5,0.6	0,0,0,0	0,0,0,0	85.0,0.06,0.15,0.06	5,0.2,0.5,0.2	4,0.5,1.5,0.5	5e-5,0.6,1.5,0.6	0,0,0,0	0.003,0.5,0.8,0.4	0.1,0.4,0.8,0.3	0,0,0,0	0,0,0,0	0.005,0.8,2.0,0.8	0,0,0,0	0,0,0,0	1e-6,0.5,2.0,0.5	0,0,0,0	0,0,0,0
MARS_DEEP_CRUST	0,0,0,0	0.5,0.4,0.8,0.4	0.3,0.4,0.8,0.4	1,0.4,1.0,0.4	1e-3,0.6,1.5,0.6	0.02,0.5,0.6,0.3	5e-3,0.5,0.8,0.4	0.5,0.3,0.5,0.25	2,0.3,0.3,0.15	1e-3,0.8,1.0,0.5	0,0,0,0	0,0,0,0	92.1,0.1,0.1,0.05	2,0.3,0.5,0.2	1.5,0.5,0.8,0.4	1e-4,0.6,1.0,0.5	0,0,0,0	0.01,0.5,0.5,0.3	0.05,0.5,0.5,0.3	0,0,0,0	0,0,0,0	3e-3,0.8,1.5,0.7	0,0,0,0	0.01,0.5,0.5,0.3	5e-6,0.5,1.5,0.5	0,0,0,0	0,0,0,0
MARS_MANTLE	0,0,0,0	0,0,0,0	0,0,0,0	0.06,0.5,0.3,0.15	5e-4,0.6,0.3,0.15	0.001,0.5,0.15,0.08	0,0,0,0	0.05,0.3,0.2,0.1	0.1,0.5,0.3,0.15	0,0,0,0	0,0,0,0	0,0,0,0	99.6,0.03,0.05,0.02	0,0,0,0	0.15,0.5,0.3,0.15	0,0,0,0	0,0,0,0	0,0,0,0	0.03,0.5,0.3,0.15	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	1e-6,0.5,0.2,0.1	0,0,0,0	0,0,0,0
MARS_CORE	80,0.15,0.05,0.02	0,0,0,0	0,0,0,0	1.5,0.5,0.1,0.05	0,0,0,0	0,0,0,0	0,0,0,0	15,0.3,0.1,0.05	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	3,0.4,0.1,0.05	0,0,0,0	0,0,0,0	0.5,0.5,0.1,0.05	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
PHOBOS_BULK	0.5,0.5,0.5,0.25	1,0.4,0.8,0.3	0,0,0,0	0.3,0.4,0.8,0.3	0,0,0,0	0.02,0.5,0.5,0.3	0,0,0,0	2,0.3,0.5,0.2	3,0.3,0.5,0.2	3,0.5,0.5,0.3	0,0,0,0	0,0,0,0	55,0.15,0.3,0.1	30,0.15,0.2,0.1	5,0.5,0.5,0.3	0,0,0,0	0,0,0,0	0.05,0.6,0.5,0.3	0.1,0.5,0.5,0.3	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
DEIMOS_BULK	0.3,0.5,0.5,0.25	0.8,0.4,0.8,0.3	0,0,0,0	0.2,0.4,0.8,0.3	0,0,0,0	0.02,0.5,0.5,0.3	0,0,0,0	2,0.3,0.5,0.2	3,0.3,0.5,0.2	3,0.5,0.5,0.3	0,0,0,0	0,0,0,0	54.5,0.15,0.3,0.1	30,0.15,0.2,0.1	6,0.5,0.5,0.3	0,0,0,0	0,0,0,0	0.05,0.6,0.5,0.3	0.1,0.5,0.5,0.3	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
```

---

## Key simplifications and omissions

**Earth changes:**

- The absorbed ice/water mass is <1% of land mass for all territories, so non-water resource abundances and dispersions are kept unchanged (the difference is below model resolution).
- Antarctica Continental Land is effectively unchanged because the removed water body (4.60e12 kg) is negligible relative to land mass (2.80e18 kg).
- Expanding land area should propagate to Near-Surface and all deeper continental strata (their spherical_fraction = land + shelf). The effect is small (~1–3% area increase for the largest cases: USA and EU) and I have not modified those rows. You may wish to propagate these changes for full consistency.
- Dissolved gases and trace minerals from the absorbed ice/water bodies (O₂, N₂, CO₂, Ar at ppm levels relative to land mass) are dropped as negligible.
- The WATER dispersion of 2.0 is a modeling compromise: the true merged distribution is bimodal (dry rock vs. pure water bodies), which a single lognormal doesn't capture well. The elevated dispersion_error (0.3) flags this.

**Mars:**

- Mars atmospheric composition uses commonly cited values (95.3% CO₂ by volume → 96.6% by mass). Curiosity SAM measurements give slightly different ratios; the abundance_cv values cover this spread.
- Controversial Mars methane is excluded from the atmosphere (TGO upper limit ~0.05 ppb globally). A speculative methane source is included in subsurface strata with very high abundance_cv and dispersion.
- North/south polar cap areas and volumes are simplified from MARSIS radar estimates; the actual deposits have complex layered geometry with seasonal caps overlying permanent deposits.
- Mars regolith composition (6% industrial minerals, 2% sulfur) reflects the uniquely high sulfate and perchlorate content measured at landing sites. Real variation across Mars is substantial (Meridiani hematite vs. Gale sulfates vs. Jezero olivine-bearing deposits), captured by the dispersion parameters.
- Mars near-surface WATER at 10% (dispersion 2.0) reflects the extreme latitude dependence: ~60% water-equivalent hydrogen at polar latitudes vs. <2% at the equator (Mars Odyssey neutron spectrometer). The lognormal dispersion is a rough fit to this gradient.
- Mars core sulfur content (15%) reflects InSight-era estimates of a lighter-than-expected core; literature values range from 10–20%.
- Mars polar cap strata exclude the area from regolith and near-surface, but subsurface and deeper strata include the full area (the rock under polar caps at >0.5 km depth). This creates a minor gap similar to the Earth model's treatment of ice bodies.

**Phobos/Deimos:**

- Bulk compositions assume captured primitive body (C/D-type analog). If they formed from Mars impact debris, water and organics would be much lower (~1% and <0.5% respectively). The high abundance_cv (0.5) on water and organics captures this hypothesis ambiguity.
- No internal layering is modeled; the "bulk" treatment assumes homogeneous or nearly homogeneous composition, appropriate given their small size and likely rubble-pile structure.

---

## Feedback for improving prompt and context files

**Things that work well:**

The resource category system and 4-parameter cell format are well-designed for balancing tractability with physical realism. The explicit mass-budget cross-checks (composition sums to ~100%, volume × density = mass) are invaluable for catching errors. The strata.md descriptions provide excellent physical context for composition estimates.

**Suggestions:**

1. **Boundary convention clarity.** The strata.tsv `area` column description says "Surface area of the stratum at its outer radius boundary," but for surface strata like regolith and land (which are the outermost solid layer), the "outer boundary" is the body surface. For atmospheres, the outer boundary is above the surface. This works fine in practice but stating it explicitly would reduce ambiguity, especially for strata like polar caps that sit on top of the surface.

2. **Area propagation rules.** When surface strata are added/removed/merged, the deeper strata areas should track the changes. Consider adding a note in strata.md (or a separate construction guide) that specifies the propagation rule: e.g., "Near-Surface area = sum of all surface-level continental strata areas in the same territory." This would make it easier to maintain consistency when making structural changes like the ones in this request.

3. **Dispersion model limitations.** The single-lognormal dispersion model works well for ore deposits in rock, but breaks down for bimodal distributions (like the merged land+water case, or latitude-dependent ground ice on Mars). Consider whether you want a flag or note field in strata_resources.tsv for cells where the lognormal approximation is known to be poor. Alternatively, the simulation code could optionally handle mixture distributions.

4. **Coal and Oil on Mars.** The resource list includes Coal and Oil as "Earth only," which is appropriate. However, the strata_resources.tsv format requires all 27 columns for every stratum. It might be worth adding a note in extractable_resources.md confirming that Coal and Oil should always be `0,0,0,0` for non-Earth bodies, to prevent accidental assignment.

5. **Generic strata cross-reference.** Phobos and Deimos compositions are quite uncertain. When you add the Generic small-body strata (Class-C, Class-D, etc.), consider explicitly noting which generic class each specific body is closest to (e.g., "Phobos: closest to Class-D, use as fallback if specific data unavailable"). This would help the simulation fall back gracefully when specific data is revised.

6. **survey_level calibration across bodies.** The 0–10 scale is well-anchored for Earth (atmosphere=10, near-surface mining zone=5, deep subsurface seismic=0.5). For other bodies, the mapping is less clear. Mars atmosphere at 7.0 and Moon regolith at 2.5 seem right, but it might help to add 1–2 more calibration anchors in the strata.tsv column description, e.g., "3.0: Mars polar caps (radar profiling, no in situ composition)" and "1.5: small body with flyby spectral+density data."

7. **Volatile tracing.** Several Mars and outer solar system strata have CO₂ and H₂O as major bulk components (not just trace volatiles). The current model handles this fine mechanically, but the extraction semantics differ: extracting CO₂ from Mars atmosphere is "air separation," but CO₂ in the south polar cap is "ice mining." If the downstream simulation distinguishes extraction methods, this may need to be encoded somewhere (perhaps in the strata_group).
