*Claude-Opus-4.6 (output effort = high). Feb, 2026.*

*Note on aborted chat: Claude struggled when producing tables with empty cells, at least for the large strata_resources.tsv file. So keep "name" and "0,0,0,0" values. I should probably adjust instructions for generic strata too.*

*Claude is totally hung up on getting strata boundaries to line up when instructions acknowledge that this can't be done for discontinuous boundaries. I'll make this even more clear in next prompt.*

---
---

I need information for a simulation that focuses on resource streams: extraction, commodity trading, and downstream usage in industrial/economic activities. The simulation starts at present-day technology and operations on Earth, but runs forward in time to include highly speculative (yet physically plausible) future technologies and operations in the wider solar system. Abstractions and simplifications are necessary. However, please tell me if my instructions seem too oversimplified or have erroneous assumptions. Ask for clarification if it may help in subsequent queries. Summarize simplifications and important omissions in your analyses. State references and show your work.

# Context files include:
* strata.md — Descriptive list of simulation "strata".
* extractable_resources.md — Descriptive list of simulation "extractable resources".
* strata.tsv — Under construction data table defining strata (model input).
* strata_resources.tsv — Under construction data table defining resource distributions in the strata (model input).

# strata.tsv

Columns:

* name — See strata.md, note 8.
* body — See strata.md, note 8.
* reference_r — Reference radius for the body. Surface or sea level radius. Mean radius for irregular bodies/bulk strata.
* territory — See strata.md, note 8.
* strata_group — See strata.md, note 8.
* inner_r  — Inner boundary radius of the specific stratum. 0 for core or bulk strata.
* thickness — Thickness of the stratum. For bulk strata, this should be the same as reference_r.
* area_fraction — Fraction of whole body spherical area for strata that include regional boundaries (e.g., Earth territories, Moon terrae versus maria, Mars polar cap, etc.). Use 1 when not applicable.
* area — Surface area of the stratum (at outer radius) treating all bodies as spherical.
* volume — Volume of the stratum.
* density — Mean density of the stratum.
* mass — Total mass of the stratum.
* survey_level — A value from 0 to 10 (decimals allowed) that indicates our relative knowledge about the stratum composition and deposit locations in it. More homogeneous compositions get a bonus here because there are fewer or no deposits to be uncertain about. Construct the scale considering all strata listed in strata.md. For calibration:
   * 0. No knowledge.
   * 0.5 Theoretical models with moderate uncertainty.
   * 2.5. Lunar regolith (excluding polar).
   * 5. Earth near-surface areas that have been extensively explored for resource extraction.
   * 7. Earth surface that is fully surveyed and explored.
   * 10. Earth atmosphere — near perfect knowledge with no uncertainty about deposits (because there aren't any).
* survey_note — A very brief qualitative note that justifies the value provided in survey_level.

# strata_resources.tsv

Rows are strata names ("name" column in strata.tsv). Columns are extractable resource names (see extractable_resources.md, note 5). Each data cell contains 4 numbers separated by commas. The numbers are:

* `relative_abundance` — This is %wt of the resource in the stratum. It will be used to generate resource mass (`abundance` as described in extractable_resources, note 3). The sum of all values in a stratum should be close to 100 but doesn't need to be exact; these will be normalized by code before conversion to mass.
* `abundance_cv` — This is coefficient of variation of the total resource mass, representing epistemic uncertainty. It will be used to generate `abundance_error` (after conversion of `relative_abundance` to `abundance`).
* `dispersion` — See extractable_resources.md, note 3.
* `dispersion_error` — See extractable_resources.md, note 3.

# Additional notes

1. Strata shape and position don't matter for the simulation. What matters here is consistency within the stratum, such that mass = volume × density. Geometry values should be consistent within the stratum given the simplified geometries. Use thin-shell approximations where appropriate.
2. Use estimations or speculative values where needed.
3. Use existing table data if it helps for cross-calibration.
4. For "generic" strata, leave spatial and mass fields blank in strata.tsv, but do calculate all values for strata_resources.tsv. (Volumes and total mass will be generate at runtime for derived unique bodies.)
5. Don't worry too much about getting survey_level exactly right. These will be cross-calibrated after we have all strata.

# Instructions

Modify the tables as needed. You can provide either specific table rows or entire tables as needed (code format is best). These are the changes I need right now:

* The definition of "Moon Crust (maria)" was changed in strata.md to now include anorthositic substrate beneath maria basalt (I changed its lower boundary), but the tables don't reflect this. Fix this stratum in the tables and check whether mantle below also needs adjustment.
* Consider adding volatiles to all strata in accordance with extractable_resources.md note 1 ("...volatiles represent what might be separated in the extraction processes"). Note that this means separation from other abstract resources such as stone and concentrated ores, not total decomposition into molecules or elements. One thing clearly missing is helium from strata that are targets of oil/gas drilling.
* Check other values and make adjustments if needed. Provide justification for changes.

Also provide feedback so I can improve the prompt and/or context files for subsequent analyses.

---
---

*Thinking...*

> Let me carefully analyze the request and the existing data to make the necessary modifications.
> 
> ## Task 1: Fix Moon Crust (Maria) boundaries
> 
> Looking at strata.md:
> - **Crust (maria)** — ~1–25 km. Mixed basaltic-anorthositic composition.
> 
> But wait, the description says "~1–25 km" but the current table has:
> - MOON_CRUST_MARIA: inner_r=1732, thickness=4, area_fraction=0.17
> 
> With reference_r=1737, that means:
> - outer_r = 1732 + 4 = 1736 (which is 1737-1 = 1 km below surface, correct)
> 
> 
> - inner_r = 1732 (which is 1737-5 = 5 km below surface)
> 
> So the current table only has 4 km thickness for maria crust, but the description says it extends to ~25 km. The user says "I changed its lower boundary" to include anorthositic substrate beneath the basalt. Let me re-read the strata.md description more carefully to understand what the full 1-25 km range should represent.
> 
> Looking at the maria crust definition of ~1–25 km with mixed basaltic-anorthositic composition, and comparing it to the terrae crust which goes 1–60 km (or up to 80 km farside) with anorthositic composition, I need to recalculate the radii. If maria crust spans 1-25 km depth, that's 24 km thickness total, which means the outer radius should be 1736 km and inner radius 1712 km. But the description says "~1–25 km" which suggests some uncertainty in those boundaries.
> 
> Now I'm working through the area and volume calculations for both crustal layers using their respective radii and thicknesses. Now I'm calculating the volume of the maria crust shell by finding the difference between the outer and inner spheres, then applying the area fraction—let me work through the cubic calculations more carefully to get the right values. I'm finishing the calculation of 1712³ and then realizing I should use the difference of cubes formula instead. With a = 1736 and b = 1712 (difference of 24), I can apply a³ - b³ = (a-b)(a² + ab + b²) to get the volume more efficiently. Let me finish calculating the volume by multiplying through: 0.17 times 8.964 × 10^8 gives roughly 1.52 × 10^8 km³, which is substantially larger than the original estimate because the thickness increased from 4 to 24 kilometers. The composition here is mixed basaltic-anorthositic since it spans both the basalt layer and the anorthosite beneath it, extending down to about 25 km depth, so I need to reconsider the density for this combined material.
> 
> For the density calculation, I'm taking a volume-weighted average of the basaltic portion (upper 4 km at 3.1 g/cm³) and the anorthositic portion (lower 20 km at 2.8 g/cm³), which works out to about 2.85 g/cm³. But I'm realizing there's a structural distinction I need to clarify: the terrae crust extends 1-60 km across 83% of the surface, while the maria crust goes 1-25 km across 17% of the surface, and below the original basalt layer there's anorthositic material that fills the rest of that maria thickness.
> 
> Now I'm seeing the real problem—the mantle boundaries don't align properly. The terrae crust bottoms out at 60 km depth (inner radius 1677), but the maria crust only goes to 25 km (inner radius 1712), so the mantle should have different starting depths depending on which region you're under. Currently the mantle is defined as a single layer with area_fraction=1, which doesn't account for this variation. So the overlap between the maria and mantle layers is actually acknowledged as acceptable in the simulation design—the strata don't need to line up perfectly. The real question is whether the mantle's outer radius should be adjusted now that the maria crust is thicker, since previously the mantle extended all the way up to cover the gap beneath the thin maria layer. the updated maria crust going to 25 km:
> - Terrae crust: 1-60 km depth, inner_r = 1677
> - Maria crust: 1-25 km depth, inner_r = 1712
> - Mantle should go from below the deepest crust, which is terrae at 60 km
> 
> So the mantle outer_r should remain 1677. In the maria region, between 25 km and 60 km depth, this would be "under" the maria crust but "above" the mantle. Actually, the mantle extends to 1677 with area_fraction=1, so in the maria region, the mantle starts at 1677 (60 km depth). But the maria crust only goes to 25 km depth (inner_r=1712). So between 25-60 km in the maria region, there's a gap.
> 
> Looking at the terrae crust more carefully, it has area_fraction=0.83, meaning it only covers 83% of the surface. In the remaining 17% (maria region), between 1-60 km depth, I have maria crust from 1-25 km and then nothing from 25-60 km. The simplest solution is to keep the mantle as-is with outer_r = 1677 and area_fraction = 1, so the mantle volume implicitly fills that 25-60 km gap in the maria region. So there's a gap in the 17% maria region between 25-60 km depth where no stratum is defined, meaning that mass isn't accounted for. The old model had a similar gap just at different depths, so the question is whether this unmapped mass is significant enough to matter for the simulation.
> 
> With the updated definition of maria crust as mixed basaltic-anorthositic material extending to 25 km, the gap is now smaller and less problematic than before. For the revised approach, I'm thinking the terrae crust should represent 83% of the area down to 60 km, while the maria crust covers 17% down to 25 km, and then I need to figure out what fills the remaining depth in the maria regions.
> 
> The simplest solution is to keep the mantle at a uniform outer radius of 1677 km everywhere, which means it only reaches up to 60 km depth globally—this leaves a minor mass discrepancy in the maria region between 25-60 km, but it's manageable. I'm calculating the volume of this gap to see how significant the missing mass actually is.
> 
> Working through the math, the missing volume comes out to about 2.1 × 10^8 km³, which is roughly 1% of the total mantle volume—noticeable but not critical. Given the complexity of adding variable mantle geometry, I'll accept this small gap and move forward with updating the strata file to reflect the maria crust thickness change from 4 km to the new value.
> 
> For the moon crust maria layer, I need to adjust the inner radius from 1732 to 1712 km, recalculate the volume based on the new thickness, and update the density from 3.10 to around 2.85 to account for the mixed basaltic-anorthositic composition, then recalculate the mass accordingly. The mantle configuration stays as is since its outer boundary aligns with the deeper terrae crust, creating only that minor gap in the maria region. I should just accept the minor gap and move forward with the updates. Let me recalculate the maria crust parameters with the new thickness of 24 km and area fraction of 0.17 to verify the geometry is correct. I'm finishing the volume calculation by multiplying through the intermediate result, getting approximately 1.52 × 10⁸ km³. Now I'm working on the density profile for the maria crust, accounting for the distinct layers of basaltic and anorthositic material with their different densities to find a weighted average. Now I'm converting the units properly—density in g/cm³ converts to 10³ kg/m³, so with the volume of 1.52×10⁸ km³ and density of 2.85 g/cm³, I get a mass of approximately 4.33×10²⁰ kg. Let me verify this against the original calculation to make sure the conversion is consistent.
> 
> For the survey level, I'm reconsidering whether it should stay at 2.0 given that the mare surface itself has been well-sampled by multiple Apollo and Luna missions, while the deeper anorthositic substrate relies more on orbital spectroscopy and inferred composition. Since both the mare crust and terrae crust have similar levels of direct sampling uncertainty, I'll keep it at 2.0.
> 
> Now I'm updating the strata_resources.tsv file for the moon crust maria...
> 
> The composition needs to reflect a mixed layer now—roughly one-sixth basaltic mare material in the upper ~4 km and five-sixths anorthositic highland material in the lower ~20 km. I'm looking at the existing basaltic composition values to see how they'll need to adjust when blended with the anorthositic substrate composition. Now I'm calculating the composition for a mixed lunar surface by blending the maria and terrae data in a 1:5 volume ratio, then adjusting for their different densities to get the mass proportions. Now I'm calculating the resource concentrations for the mixed composition by blending the basalt and anorthosite contributions—iron ores come out to about 4.4, aluminum ores around 10.7, with smaller amounts of industrial metal ores at 0.53 and rare earth ores at 0.52, while uranium remains negligible at 0.01. The totals are coming to around 97.8, which is close enough since the original datasets don't sum to exactly 100 either—they'll get normalized anyway. I'll stick with my current mixed distribution.
> 
> For the uncertainty values, I need to account for the fact that the deeper anorthositic layer is less directly sampled than the original mare basalt. Since the new 24 km crust only has the top 4 km well-characterized, I should increase the error terms slightly across the board, keeping them closer to the terrae values rather than splitting the difference.
> 
> For spatial dispersion, a mixed basaltic-anorthositic crust actually introduces more heterogeneity than either pure end-member because of the compositional transition zone between layers. So I'll bump those values up as well.
> 
> Now I'm calculating the blended abundance values for each resource by mass fraction, then applying these adjusted uncertainty and dispersion parameters to reflect the increased complexity of the layered system. I'm adjusting the maria crust composition to account for the mixed basaltic-anorthositic layering, which creates higher variability in mineral distribution across different regions.
> 
> Now I need to tackle adding volatiles to the strata. The key is that volatiles should appear wherever they'd naturally be present and separable during extraction—so if rock contains water or CO2 that would release during processing, those should be listed in the composition. I'm noticing helium from certain strata is specifically missing and needs to be added.
> 
> Looking at Earth's continental subsurface strata, I see water and methane are already there, but helium is absent—which is a significant gap since natural gas deposits contain meaningful amounts of helium that get commercially extracted. I should add helium to the subsurface strata composition to reflect this reality.
> 
> Beyond helium, there are other volatiles worth considering: CO2 appears in some natural gas deposits and carbonate formations at potentially significant concentrations, nitrogen shows up in certain fields in smaller amounts, and hydrogen seeps are increasingly documented though quantities remain uncertain. For the Moon, the conditions are fundamentally different and would require a separate analysis. The polar shaded regolith has higher helium concentrations, which makes sense given the accumulation of volatiles in those protected areas. For deeper lunar strata like the crust and mantle, having zero volatiles is reasonable since they're shielded from solar wind exposure. Now I'm thinking about helium in Earth's continental subsurface—it concentrates in natural gas deposits at 0.01-7% by volume, and I need to estimate how much helium would be present in the crustal strata where it's generated.
> 
> Looking at the numbers: the US holds about 3.9 billion cubic meters of proven helium reserves out of roughly 50 billion cubic meters globally. Converting that to mass and comparing it against the total subsurface mass gives me a helium fraction of around 1.4 parts per billion—a tiny amount relative to the total crustal material, but significant enough to track in natural gas-bearing formations.
> 
> Now I'm checking the methane calculations against the same framework. The US natural gas reserves are roughly 14 trillion cubic meters, which translates to about 9.2 trillion kilograms. But that's just proven reserves—the actual methane distributed throughout the subsurface, including unconventional sources and dissolved gas, would be substantially larger. The 0.0005% figure I calculated seems off when I compare it to known reserve estimates, so I need to reconsider what's actually being measured here. proven natural gas reserves as of ~2023 were about 14 trillion cubic feet ≈ 4e11 m³. At STP density of methane ~0.656 kg/m³, that's 2.6e11 kg. Total technically recoverable resources (including shale gas, tight gas, coalbed methane) might be ~100 trillion m³ = ~6.6e13 kg.
> 
> But the model uses "total mass" not "recoverable" - the abundance represents total mass of the resource in the stratum, with dispersion determining how much is in economically extractable concentrations. So the total methane in the subsurface could be much higher than what's recoverable - it includes dispersed methane in rock, dissolved in water, etc.
> 
> Now I'm working through helium's abundance relative to methane. In natural gas deposits, helium typically makes up about 0.1-0.3% by volume, which translates to roughly 0.025% by mass given the molecular weight difference. If methane is 0.0005 in the US subsurface, that would put helium at around 1.25e-7, which seems vanishingly small. But helium isn't just trapped alongside methane—it's continuously generated by radioactive decay throughout the crust and migrates into gas accumulations, so the He-to-CH4 ratio varies wildly depending on the field and its geological history.
> 
> I think the right approach is to treat helium as a minor component that concentrates heavily in natural gas traps rather than being uniformly dispersed. I'll set a small abundance value but with high dispersion to reflect how tightly it clusters in economically viable deposits.
> 
> For the US subsurface, I'm working with identified helium resources around 6.5×10^8 kg, but the total subsurface amount is probably 10-100 times larger, so roughly 6.5×10^10 kg. That gives a mass fraction of about 1.4×10^-9 relative to the total crustal mass—incredibly tiny compared to methane at 0.0005%. I'm cross-checking this against US natural gas reserves to see if the numbers align reasonably. Mass: 2.83e12 × 0.656 = 1.86e12 kg
> 
> But 2.36e14 is 100x more than this. Hmm. But again, the abundance represents ALL methane in the stratum, not just in recoverable gas fields. Methane is also dissolved in water, trapped in sediments, in coal seams, etc. So a factor of 100x above proven+undiscovered conventional gas seems... maybe high but not crazy.
> 
> OK, for helium: if total helium in the US subsurface is maybe 100x the identified resources:
> 6.5e8 × 100 = 6.5e10 kg
> As fraction: 6.5e10 / 4.72e19 = 1.4e-9 ≈ 0.0000001%
> 
> That's too small to even register in the model at the precision shown. Let me reconsider my helium estimates by thinking about crustal abundance instead. The Earth's crust is about 2.6e22 kg with helium at roughly 3 parts per billion by mass, which gives me a total crustal helium mass around 7.8e13 kg. Scaling that down to just the US subsurface, I can estimate what fraction of that 4.72e19 kg would contain helium.
> 
> Now I'm realizing the model's "volatiles" might not mean total helium content but rather what gets co-produced during natural gas extraction. Helium concentrations in natural gas typically range from 0.01% to 7% by volume, averaging around 0.1-0.5%, and since helium is much lighter than methane, the mass fraction would be even smaller. to CH4 (MW 16), the mass fraction is about 0.025-0.125% of the gas mass.
> 
> If we take helium as ~0.05% by mass of total natural gas (which seems like a reasonable average), then:
> - If methane in US subsurface = 0.0005%
> - Ethane = 0.00005%  
> - Total gas ≈ 0.0006% (roughly)
> - Helium ≈ 0.0006% × 0.0005 = 3e-7%
> 
> Actually wait. Helium is 0.05% by mass of the gas, so:
> Helium ≈ 0.0006 × 0.0005 = 3e-7... that doesn't make sense.
> 
> Let me redo: if total gas in the stratum is ~0.06% by mass. I need to clarify what the model's 0.0005 value actually represents—it's 0.0005% by weight, which translates to 5 ppm. So methane is 5 ppm, ethane is 0.5 ppm, giving a total gas concentration around 6 ppm. If helium comprises about 0.1% by volume of natural gas (roughly 0.025% by mass accounting for the molecular weight difference), then helium in the subsurface would be approximately 6 ppm times 0.00025, which gives me 0.0015 ppm.
> 
> That's far too small to represent meaningfully in the model. I'm reconsidering the approach—helium in the subsurface isn't just trapped alongside hydrocarbons; it also accumulates independently in structural traps from radiogenic decay. So I should estimate from actual production data instead. Global helium production runs about 160 million cubic meters annually, roughly 27,000 tonnes per year, with the US historically accounting for about 40% of that. Total identified global helium resources are around 50 billion cubic meters, which converts to approximately 8.3 million tonnes of helium.
> 
> For the US alone, helium resources are estimated between 3.9 and 8.5 billion cubic meters—roughly 0.65 to 1.4 billion kilograms. When I calculate this as a fraction of the total US subsurface mass, I get something like 0.0000000021%, which is still vanishingly small. Even if the total in-place helium is a thousand times larger than identified resources, that only pushes it to 0.000002%. The fundamental issue is that helium, despite its enormous value, exists in such minuscule quantities relative to total rock mass that at the model's precision level, it might need to be expressed as 0.000001 or smaller.
> 
> The normalization should handle this fine—even if I input something like 0.000001 for helium, it's so negligible compared to the rest of the values that it barely affects the calculation. The code adjusts everything to get the correct total stratum mass anyway. Looking at the existing data, the Moon's polar regolith actually contains more helium than Earth's subsurface does. Apollo measurements showed lunar regolith concentrations around 4 to 40 ppm by mass for helium-4, averaging maybe 20 ppm or 0.002%, but the table shows 0.001%—closer to 10 ppm, which is on the lower end of that range.
> 
> For Earth's subsurface, helium concentrations in crustal rock average around 0.003 ppm, but the real issue is that helium is extremely dispersed and only really concentrated in gas traps. I should add a reasonable estimate—something like 0.00001 (about 0.1 ppm) to account for the gas-bearing depth range, though that might still be generous for an average across 0.5 to 2 kilometers. The user flagged helium from oil and gas drilling as clearly missing, so I'll include it with a practical estimate rather than overthinking the exact concentration.
> 
> For the parameters, a dispersion of 2.0 makes sense here—it represents about 100x variation across the stratum, which matches how helium is distributed: essentially absent in most places but concentrated in gas deposits. For the US, which has significant known helium resources, I'll set the abundance to 0.00002 with a dispersion of 0.4.
> 
> Now I'm adding Russia with similar parameters since it also has substantial helium in gas fields, then accounting for other regions with smaller abundances. But I need to sanity-check this against actual helium resources—the US subsurface calculation gives me 9.4e12 kg, which is way too high compared to identified resources of around 1.7e8 to 8.3e8 kg. The high dispersion means most of that abundance is spread across non-extractable concentrations, so I need to recalibrate the base abundance downward.
> 
> Let me reconsider with a more realistic helium concentration of 3 ppb in crustal rock, which gives me about 1.4e11 kg total helium in that subsurface layer. That's an incredibly small percentage—3e-7—but I'm concerned the model might struggle with such tiny numbers, so I need to check what the smallest existing values are before deciding how to format this.
> 
> Looking at the data, ethane in Japan's subsurface sits at 0.00001, which is the smallest non-zero value I can find. Given that helium is an important extractable resource despite its low abundance, I'll set a practical concentration of 0.000001% (about 10 ppb) for average Earth subsurface, then adjust upward slightly for regions with better helium resources like the USA and Russia.
> 
> I'm assigning region-specific values based on known gas field characteristics, but I'm realizing I might be overcomplicating this—the user probably just needs reasonable non-zero helium values for the subsurface layer. I should also consider what other volatile compounds might belong in this dataset.
> 
> Looking at what's already included versus what's actually present in Earth's strata, I'm noticing several gaps. The continental subsurface should probably account for CO2 from gas fields and carbonate dissolution, nitrogen in certain deposits, and hydrogen traces, though most of these are minor contributors by mass. I need to decide whether to add these or keep the model simpler.
> 
> For nitrogen specifically, it shows up in natural gas at around 3% on average—maybe 5% by mass compared to methane—but the total contribution to subsurface volatiles is tiny, probably around 0.00003% when accounting for methane's already-low concentration. While nitrogen does get captured during gas processing, it's really just an impurity rather than a primary extraction target, so including it as a volatile makes sense even if it's not economically significant.
> 
> Now looking at CO2, which is much more relevant—it typically ranges from 1-5% in natural gas by volume, sometimes higher. CO2 gets captured during processing and has actual uses like enhanced oil recovery, so it's a more meaningful volatile to track. The math suggests CO2 in the subsurface gas would be around 0.000024% by mass, which is still minor but more substantial than nitrogen. So the key volatiles to focus on are helium, CO2, and nitrogen, with helium being the most economically important.
> 
> For hydrogen sulfide, I realize it's already accounted for since H2S from sour gas gets converted to elemental sulfur during desulfurization—that's already listed as a sulfur resource in the subsurface.
> 
> Shifting to the Moon now, the polar shaded regolith already has a good volatile inventory, but the regular regolith is missing some trace components like CO and CO2 that would come from solar wind interactions.
> 
> Looking at action items: I need to add helium to Earth's subsurface strata, and possibly small amounts of CO2 and N2 as gas field components. For continental land strata, helium from natural gas seeps is negligible enough to leave at zero.
> 
> Now I'm spot-checking the existing data for consistency—the USA continental land strata add up to about 98.13 after accounting for trace resources, which is close enough given that these values get normalized anyway. Let me verify the lunar data is also internally consistent, particularly the polar shaded regolith composition.
> 
> Checking the polar shaded regolith total...
> 
> that comes to about 98.6%, which is a bit under but acceptable given the composition is dominated by regolith at 93% and industrial minerals at 3%. Now moving to the near-surface material, which totals 98% with water ice making up the bulk at 75% and volatiles at 15%. The composition totals all check out around 99%. Now I need to verify the mantle layer—it's currently set with an inner radius of 337 and thickness of 1340, which puts its outer boundary at 1677, right where the terrae crust begins.
> 
> But there's a problem: in the maria regions, the crust only extends to 25 km depth, creating a gap between 1677 and 1712 km radius that isn't assigned to any layer. I'm weighing whether to leave this gap documented, adjust the mantle to cover it with a fractional area assignment, or calculate the unaccounted volume to see if it's significant enough to worry about.
> 
> Working through the math, that missing volume comes to roughly 2.1 × 10^8 km³, which is less than 1% of the total mantle volume of 1.96 × 10^10 km³—small enough that I can note it as a minor discrepancy without restructuring the whole model. Looking back at the old model, the maria crust gap was actually larger—55 km compared to the new 35 km—so this revision is an improvement. I'll just note this in my response and leave the mantle unchanged.
> 
> Now I need to update the strata files with the new maria crust parameters and add helium to Earth's subsurface composition, then prepare the actual data changes. I need to account for the compositional heterogeneity created by mixing basalt and anorthosite at different depths—the dispersion should be slightly elevated since extraction would encounter varying rock types depending on depth. Now I'm finalizing the resource concentrations and their associated uncertainty parameters for each ore type. The total comes to about 97.8, which aligns with the other lunar strata values. Now I'm moving on to Earth's subsurface volatiles—I need to incorporate helium across the continental subsurface layers, and consider whether to include trace amounts of CO2 and N2 from natural gas extraction, though those quantities are typically negligible.
> 
> I'm focusing on helium since that's what's clearly missing. The challenge is determining how small the abundance value should be—helium is extremely trace, with high dispersion in gas traps and significant uncertainty in distribution. I need to think through whether the abundance percentage will actually matter in the simulation's calculations.
> 
> Helium appears in natural gas at roughly 0.1-0.3% by volume, and working backward from subsurface methane and ethane concentrations, I'm getting an abundance around 5.5e-7%, which seems vanishingly small. Yet helium is commercially extracted, so it deserves some representation. The real question is whether the model's normalization process will still allow localized deposits of extractable concentration to emerge even with such tiny baseline numbers.
> 
> I think the key insight is that even if helium represents 0.0000001% of the total stratum mass, the model should correctly represent that proportion as long as numerical precision doesn't fail. For practical purposes, I'll estimate helium content in gas-bearing depths at 30-300 ppb—roughly 10-100 times higher than the crustal average of 3 ppb—since helium accumulates in gas traps. I'll use 500 ppb for regions with good helium resources and 100 ppb for less favorable areas, bumping these up slightly to ensure the numbers register properly in the model.
> 
> Now I'm assigning helium concentrations by region based on their gas production and helium resources, with the USA and Russia getting the highest values since they're major helium producers, while regions like Antarctica and Japan get much lower concentrations. Let me verify these make sense by checking what total helium mass they'd imply for the USA. So 50 parts per billion for atmospheric helium seems reasonable—that gives the USA around 2.36 million tonnes in the subsurface, which is several times the identified reserves but plausible given how much helium is dispersed and unextractable. Now I'm setting up regional parameters for the model with concentration percentages and depth ranges for the major helium-holding regions.
> 
> Let me cross-check this against methane concentrations to see if my numbers are calibrated right. The USA methane estimate comes out to about 236 billion tonnes, which is roughly 24 times the identified natural gas reserves—that's high but seems defensible when you account for all the unconventional sources like tight gas and coalbed methane that aren't fully accounted for in reserve estimates.
> 
> For ethane, I'm getting around 23.6 billion tonnes in the model versus 1-2 billion in identified reserves, so that's a 12-24x multiplier. The methane-to-ethane ratio of 10:1 tracks well with natural gas composition, which is about 90% methane and 5% ethane. Now I need to figure out helium by using the same proportional approach—helium makes up about 0.01-0.5% by volume in natural gas, or roughly 0.0025-0.125% by mass, so I'll scale it relative to the methane abundance I've already established.
> 
> Working through the calculation, that gives me about 3 parts per billion for helium in hydrocarbon gas alone, but helium also accumulates in non-hydrocarbon gas deposits, formation water, and mineral inclusions, so the total subsurface helium is probably higher. I'll bump that estimate up by a factor of 10 to account for these other sources, landing on about 30 ppb for the USA.
> 
> Now I'm applying these concentration estimates to the total subsurface gas volumes for each region. The USA estimate yields around 1.42 million tonnes of in-place helium, which is roughly double the identified reserves—that seems reasonable. Russia's enormous gas fields push the estimate to 3.46 million tonnes, and the "Other" category covering major helium regions like Algeria, Qatar, and Australia comes to about 7.76 million tonnes. I'm still working through China's numbers.
> 
> For the smaller regions—EU, India, Japan, and Antarctica—I'm checking whether these will contribute meaningfully to the model despite their tiny concentration values. I'm also reconsidering whether to include argon separately, since while radiogenic argon-40 accumulates in subsurface strata from potassium decay, it's not commercially extracted from drilling operations and remains a trace component in natural gas, so I'll leave it out of the subsurface calculation.
> 
> Now I'm weighing whether to add helium, nitrogen, and CO2 to the subsurface strata. Helium escapes from shallow zones, so skipping it makes sense. For nitrogen and CO2—both present in natural gas at meaningful concentrations before processing—I need to decide if they're worth tracking given that most N2 comes from air separation rather than gas processing, and CO2 removal is standard industrial practice.
> 
> I'm leaning toward adding CO2 since carbon capture from gas processing is becoming commercially significant, even if the volumes are small relative to total stratum mass. For hydrogen, while geological hydrogen is an emerging field, the data is too speculative and uncertain right now, so I'll hold off on including it.
> 
> Looking at the nitrogen distribution across lunar strata, the pattern makes sense—solar wind implantation creates the trace amounts in the regolith layers, while deeper strata have none. Now I'm verifying the Moon mantle calculations to make sure the geometry checks out. Now I'm working through the volume calculation for the mantle layer. I need to compute the difference between the outer and inner radii cubed, then apply the volume formula. After multiplying out 1677³ and 337³, I get a difference of about 4.68 billion, which when plugged into the (4/3)π formula gives me 1.96e10 cubic units—matching what the table shows.
> 
> Then multiplying by the density of 3.40 × 10¹² gives a mass of 6.66e22, which also checks out perfectly.
> 
> Now I'm moving on to verify the lunar crust maria layer with its inner radius of 1732, thickness of 4, and area fraction of 0.17. I need to calculate the volume using the difference of cubes formula. Now I'm working through the volume calculation for the lunar maria crust layer, computing the difference between the outer and inner radii cubed, then applying the density and volume fraction to get the final mass estimate.
> 
> With the new maria crust mass coming in around 4.33×10²⁰ kg compared to the old estimate of 7.99×10¹⁹ kg, I'm noticing this represents a significant increase—about 3.53×10²⁰ kg more—so I should verify whether this affects the overall mass balance for the Moon.
> 
> The total lunar mass is roughly 7.346×10²² kg with the mantle accounting for 6.66×10²². The key insight is that the old model left a gap unaccounted for—that 17% area with 55 km thickness between the basalt layers—whereas my new model assigns this volume to the maria crust with a revised thickness of 35 km plus the deeper basalt layer now extending to 24 km. This isn't mass appearing from nowhere; it's simply that the old model had unaccounted mass in that gap region, and the new approach reduces that gap by properly assigning the strata. I should calculate exactly how much mass was previously unaccounted for to confirm this is an improvement. I'm calculating the volume and mass of the new gap between 1677 and 1712 radius, which gives me 2.147e8 km³ and a mass of 6.23e20 kg at mantle density—a significant reduction from the previous 9.56e20 kg. The increase in maria crust mass (3.53e20 kg) accounts for exactly what was missing before, so the model is internally consistent without needing mantle adjustments. Now I should check for other potential issues, starting with the terrae crust parameters. corresponds to 60 km depth. The strata.md says "~1–60 km (thinner nearside, up to ~80 km farside)." The table uses 59 km thickness (inner_r=1677, outer_r=1736), so effectively 1-60 km depth. That's fine.
> 
> 2. Lunar mantle outer boundary at 1677 matches terrae crust inner boundary. Good.
> 
> 3. The Near-Surface goes from 10m to 1 km, with inner_r=1736, thickness=0.99, outer_r=1736.99≈1737. The surface is at 1737, so the near-surface shell's outer boundary sits 10m below the surface, and the inner boundary is 1 km down. That checks out.
> 
> 4. For MOON_REGOLITH: inner_r=1736.99, thickness=0.01, outer_r=1737. That's the top 10m. Correct.
> 
> Now I'm ready to prepare the output, though I should verify whether argon needs to be added to any subsurface layers.
> 
> Radiogenic Ar-40 exists in Earth's subsurface in trace amounts, but it's not typically extracted and isn't worth modeling. Argon comes primarily from air separation. For noble gases like xenon and krypton from natural gas, the concentrations are negligible. The Moon's polar shaded regolith could theoretically have cold-trapped solar wind argon, but the amount is so minimal I'll leave it at zero.
> 
> I think I'm ready to finalize the response and organize the changes to strata.tsv.
> 
> Now I'm reviewing the specific updates needed for the Moon's crustal maria layer—adjusting the inner radius, thickness, volume, density, mass, and survey parameters—along with updating the resource composition to reflect a blended basaltic-anorthositic mix. I also need to add helium values across all eight Earth continental subsurface strata and check if there are any other adjustments worth making.
> 
> Looking at whether argon should be included in Earth's continental strata—it wouldn't make sense since atmospheric argon in soil is negligible. For the lunar regolith, carbon monoxide could theoretically form from solar wind implantation of carbon and oxygen, but in reality it's minimal compared to hydrogen, helium, and nitrogen, so I'll keep it at zero. Comparing the polar shaded regolith to regular regolith, the cold-trapped volatiles like carbon dioxide and carbon monoxide are higher in the shadowed regions, which makes sense given the temperature differences.
> 
> I'm noticing that regular moon regolith has organics at zero, but small amounts of organics have actually been detected from meteoritic infall—though we're talking ppb to ppm levels. The polar shaded regolith shows 0.2% organics representing cold-trapped organic volatiles, whereas any meteoritic organics in warmer regolith would be negligible.
> 
> Hydrogen distribution looks correct as is—solar wind hydrogen only implants into the surface regolith, not deeper layers, and the crust and mantle are essentially anhydrous.
> 
> Now I'm reconsidering the maria crust composition more carefully. The basaltic maria have very high iron ore content around 15% since mare basalts are iron-rich with ilmenite as a major phase, moderate aluminum around 5% from plagioclase, and high industrial metals overall.
> 
> For the highland terrae crust, the anorthositic composition means low iron ore at 2%, very high aluminum at 12% from anorthite-rich plagioclase, and minimal industrial metal ores. I'm also working through the mixed crust composition now.
> 
> The new mixed crust at 18.1% basaltic and 81.9% anorthositic by mass is heavily weighted toward anorthositic material, which makes sense given that the basalt layer in maria regions is relatively thin—typically 1-4 km—compared to the underlying anorthositic crust. According to GRAIL data, total maria crustal thickness ranges from 20-30 km, and with the user's 25 km depth setting, I need to account for roughly 4 km of dense basalt with high iron and titanium content, underlain by about 20 km of anorthositic material.
> 
> But I should also factor in the KREEP component. The Procellarum KREEP Terrane underlies much of Oceanus Procellarum and overlaps significantly with maria regions, meaning the sub-basalt layer in these areas would be enriched in rare earth elements compared to a simple compositional blend. This suggests I should adjust the rare earth ore content upward from what a straightforward mass-weighted average would give.
> 
> I'm also reconalculating the industrial metal ores contribution from the basaltic component's titanium-rich minerals, and considering whether the sub-basalt anorthositic crust in maria regions might contribute additional amounts.
> 
> Now I'm finalizing the MOON_CRUST_MARIA composition with adjusted values for rare earth and uranium ores to reflect KREEP enrichment, and setting industrial metal ores at around 0.55%. I'm looking at the rest of the resource composition data and calculating the total sum across all materials—it comes to about 97.908, which is acceptable for normalization purposes. Now I'm ready to move forward with the next step.
> 
> For the helium component in Earth's subsurface, I need to account for how it forms through radioactive decay of uranium and thorium, then migrates upward through fractures and gets trapped in the same geological structures that hold natural gas. Since helium is highly dispersed and only concentrated in specific gas fields, I'll need to set parameters that reflect very low overall abundance but with moderate uncertainty around crustal uranium and thorium content, plus high dispersion and dispersion error given how poorly we understand helium distribution. I'm going to assign helium values for each territory based on known regional helium endowments, starting with major producers like the USA.
> 
> Now I'm reviewing the major helium reserves globally—the USA has substantial historical resources around 3-4 billion cubic meters, Russia holds even larger amounts in Siberian gas fields at 6-10 billion cubic meters, Qatar's North Field contains significant helium, and newer discoveries in Tanzania are adding to the picture. The "Other" territory actually contains most of the world's helium resources since it encompasses the Middle East, Africa, and Australia, so I need to calibrate the subsurface mass values accordingly, with "Other" at 3.88e20 kg and Russia's portion being much smaller by comparison.
> 
> Now I'm working through the total global helium inventory—identified resources are around 50 billion cubic meters or roughly 8.3 billion kilograms, but accounting for undiscovered in-place resources at maybe 100 times that amount gives me about 830 billion kilograms total. I'm distributing this across regions by allocating roughly 15% to the USA and 25% to Russia, then converting these to fractional abundances relative to their subsurface masses to get the helium concentration values I need. These fractions are vanishingly small. I'm questioning whether my 100x multiplier assumption for total-in-place versus identified reserves is actually reasonable. Let me calibrate this against methane data—the US subsurface methane estimate of 2.36e14 kg seems way too high compared to proven natural gas reserves around 2.6e11 kg, with total estimated resources closer to 5.6e13 kg, so my model is significantly overestimating.
> 
> If I apply the same 4x multiplier to helium resources, I'd get around 2-5 billion kg, which translates to fractions like 0.000000004% of the total—essentially zero in any practical sense. The core issue is that helium is genuinely scarce in absolute terms.
> 
> Looking at the existing data, the smallest non-zero relative_abundance values are around 0.00001 for ethane in certain subsurfaces. I could use something like 0.000001 for helium-rich areas, which is 100x smaller but should still work numerically. I think the pragmatic move is to set helium at around 0.000001 for the most concentrated subsurface deposits.
> 
> Now I'm testing this against known reserves—if I use 0.000001 for the US with a dispersion of 2.2, that gives me roughly 472,000 tonnes total in-place, which is actually close to the identified reserves of around 650,000 tonnes. So maybe I should bump it up slightly to 0.000002 to better capture the full in-place helium including the dispersed component.
> 
> Let me set the parameters for each region: USA gets 0.000002 since it has well-documented reserves, Russia gets 0.000003 because its larger gas fields contain more helium, Other regions get 0.0000008 to account for major helium provinces spread across a large area, China gets 0.0000005, and the EU gets 0.0000004. All of them use the same dispersion factor of 2.2 and concentration of 0.5 to reflect how helium is typically distributed. I'm checking whether the helium contribution actually moves the needle on the total—with values this small, it's essentially rounding error. But I should also verify whether there are other factors I'm missing in this calculation.
> 
> Now I'm considering argon and other noble gases. Argon does accumulate in natural gas deposits through radioactive decay, and it's actually more abundant than helium in those environments, but since the atmosphere provides unlimited argon for commercial purposes, it doesn't make sense to include it. Same reasoning applies to xenon and krypton—they're valuable but present in such trace amounts that they won't meaningfully affect the calculation.
> 
> Before I finalize this, I want to double-check whether the continental land strata needs any adjustments for volatile compounds.
> 
> Looking at the existing values for EARTH_USA_CONTINENTAL_LAND, I see methane and water are already accounted for. CO2 could theoretically be included since it dissolves in groundwater and exists in soil gas, but the concentrations are so minimal relative to the total mass that including it would be false precision. Helium from radiogenic decay in shallow zones has mostly escaped by now, and natural hydrogen seeps are essentially nonexistent at the surface. So the current values stand.
> 
> Now I'm moving on to consider the Moon's near-surface composition. Now I'm reviewing the composition values across different strata to spot any inconsistencies. The lunar core's sulfur content aligns with models of a sulfur-rich iron core, and the mantle being nearly all stone makes sense for deep silicate material. I notice Japan's continental land has the highest sulfur among continental strata, which tracks with its volcanic geology on the Ring of Fire. Antarctica's uncertainty values are quite high, and the thought cuts off there.
> 
> Looking at the volatiles definition in the documentation, I'm clarifying how volatiles fit into the extraction model—they're the byproducts or associated materials released during extraction processes, separate from what's already captured in the water content. I'm ready to write up my response now. ible. Now I'm double-checking the water content values for the lunar strata. The polar shaded regolith at 5% aligns well with LCROSS measurements of 5.6 ± 2.9% water ice, and the 0.01% for general regolith matches observations of solar-wind-produced hydroxyl and water on illuminated surfaces—right around 100 ppm, which is consistent with Chandrayaan-1 data. I'm satisfied with these estimates and ready to finalize the analysis.
> 
> Now I'm organizing my response around the key changes: updates to MOON_CRUST_MARIA in both strata files, helium additions to Earth subsurface strata, and some minor adjustments. I'll need to provide the updated rows clearly for both files. I'm also considering whether to revisit the MOON_MANTLE row—the volume checks out and composition doesn't need adjustment since the mantle boundary hasn't shifted, though slightly less mass gets allocated there. I'm second-guessing the density choice for the sub-maria crust at 2.85 g/cm³. The GRAIL mission data suggests the upper anorthositic crust is around 2.5-2.6, while deeper crustal layers range from 2.8-3.0, so my estimate seems reasonable as a middle ground, though the sub-maria crust could be slightly less dense if it's more fractured from basin-forming impacts, or denser if intruded by basaltic material. My estimates assume the deeper crust (1-25 km under maria) has progressively higher density.
> 
> Actually, let me reconsider. Pure anorthosite is ~2.73 g/cm³, and crustal density includes porosity. GRAIL measured average crustal density as ~2.55 g/cm³, with porosity ~12% in upper crust declining with depth. At 10-25 km depth, porosity is much lower, so density approaches intact rock values (2.7-2.8 for anorthosite, 3.0-3.3 for basalt).
> 
> For a volume-weighted average accounting for basalt in the top 4 km and anorthosite below, I'm calculating different scenarios with varying porosity assumptions. The calculations give me a range from 2.73 to 2.85 g/cm³ depending on how much porosity I factor in at each depth. I'll use 2.8 as a reasonable middle ground, which gives me a total crustal mass around 4.256 × 10²⁴ kg.
> 
> Now I'm second-guessing whether to differentiate the maria crust density from the terrae value of 2.80 that I already set. The basaltic enrichment should make it denser, but the upper-crustal porosity counteracts that. Since 2.80 is already in my reasonable range and represents a good compromise between these competing effects, I'll stick with it rather than overthinking the distinction.
> 
> Actually, wait—using 2.80 for both would mask the basaltic influence entirely. The old pure basalt was 3.10, the terrae anorthosite is 2.80, so the mixed crust should fall between them. I'll go with 2.85 to show that basaltic component while keeping it close to the terrae value, since anorthosite still dominates. That gives me a recalculated mass of about 4.33e20.
> 
> Now I need to double-check my volume calculation for the shell between those radii with the 17% area fraction, working through the cubic terms carefully. Now I'm multiplying through to get the volume in cubic kilometers, which comes out to about 1.52 × 10^8 km³. Then I'm converting that to mass using the density of 2.85 × 10^12 kg/km³, giving me roughly 4.34 × 10^20 kg, though I want to double-check the precision on that calculation.
> 
> I'm planning to organize my response with the analysis, updated rows for both data files, and feedback, but I should first verify whether the user wants changes to Earth's continental land strata as well.
> 
> Looking back at the requirements, I concluded that Earth's land strata already have methane and water, so no changes are needed there. For subsurface strata, helium should be added. The Moon's strata are already appropriate. The only addition is helium to Earth's subsurface layer, though I'm reconsidering whether shallow natural gas deposits in the upper 100 meters might also warrant helium—though the concentrations there are much lower than in deeper deposits.
> 
> Now I'm doing a more thorough review of the values as requested. I'm checking the Moon's crustal terrae calculations: the area fraction of 0.83 with an inner radius of 1677 and thickness of 59 gives an outer radius of 1736, which yields an area around 3.14e7, matching the table's 3.15e7 closely. Let me verify the volume calculation as well.
> 
> Computing the volume using the difference between the outer and inner radii cubed, I get approximately 1.792e9, which matches the table's 1.79e9. The mass calculation of 5.01e21 also checks out. Now I'm moving on to verify the Moon's core parameters with an inner radius of 0 and thickness of 337.
> 
> The surface area comes to about 1.43e6, and the volume calculation gives 1.603e8, both matching the table values. The mass of 1.12e21 is consistent as well. Everything's checking out so far.
> 
> Let me spot-check some Earth strata to make sure the values are reasonable. Looking at the continental land mass for the USA, the calculation gives roughly 1.97e18.
> 
> Now I'm examining the continental subsurface layer, which has an inner radius of 6369 and thickness of 1.5, placing it between 0.5 and 2 km depth below the surface. The area fraction is 0.0242, which accounts for both the continental land and shelf regions of the USA. says "Continental strata below these encompass both land and shelf area." So the subsurface area_fraction should be land + shelf.
> 
> USA land area_fraction = 0.0193
> USA shelf area - I'd need to estimate. The US has a significant continental shelf (Gulf of Mexico, Atlantic, Pacific, Alaska). Let me check:
> 
> US exclusive economic zone is huge but continental shelf is smaller. US continental shelf area is roughly 2.5 million km² (one of the largest). As fraction of Earth's surface: 2.5e6 / 5.1e8 = 0.0049.
> 
> So USA continental subsurface area_fraction should be ~0.0193 + 0.0049 = 0.0242. That matches! Good.
> 
> Now I'm calculating the surface area using the combined area fraction, which gives me 1.23e7 km² — exactly what the table shows. For the volume calculation, I'm working through the difference between the outer and inner sphere radii, using the algebraic expansion to get the volume of that thin shell, which comes out to about 182,575 km³.
> 
> Multiplying through by the density and area fraction gives me a mass of 4.72e19 kg, which matches the table perfectly. All the numbers are checking out.
> 
> Let me think about whether I'm missing any volatiles here. The extraction notes say volatiles are what might be separated during processing, sometimes the target product itself. For lunar regolith, when you're extracting oxygen through chemical reduction, that oxygen is already accounted for in the composition—it's part of the metal oxides being processed. The oxygen release would show up downstream in the refining step, not as a separate volatile in the raw extraction.
> 
> But atmospheres work differently. When you extract from Earth's atmosphere, you're directly separating N2, O2, Ar, and CO2 as the extractable resources themselves. Same logic applies to gas giant atmospheres—you're pulling out H2, He, CH4 directly. For solid lunar strata though, the extractable resources are the physical materials you can mine: regolith, stone, and concentrated ores. Since the Moon has essentially no trapped volatiles in its rock, there's nothing additional to list here.
> 
> For Earth's subsurface, I need to reconsider what gases actually come out during drilling. Water and methane are already tracked, but helium gets co-produced with natural gas and isn't currently listed—that's a gap I should fill. CO2 and N2 are minor enough that I can mention them as optional additions, but helium is the main one worth adding.
> 
> Now I'm second-guessing whether helium should appear in the continental land strata at all. There's radiogenic helium in soil gas and groundwater, sometimes 10-100 times more concentrated than atmospheric levels, but when I work through the math—accounting for only 4% of land being water and helium at maybe 500 ppb in that water—it comes out to essentially 0.000002% of the total stratum. It's technically present but so negligible that including it might not be worth the complexity.
> 
> I'm ready to finalize the response now, but I want to flag something about the moon crust composition. The maria basalts had rare earth ores at 1.5%, which is high because of the KREEP component concentrated in the Procellarum KREEP Terrane on the nearside. But this enrichment isn't uniform across all maria—the farside basalts have much less KREEP. When I expand the model to include the deeper anorthositic substrate, the KREEP influence should actually intensify in the sub-basalt layer within the PKT region.
> 
> My blended value of 0.6% rare earth ores seems reasonable as an average between the basalt and terrae values, but I need higher dispersion to capture the variation between PKT and non-PKT regions. The existing data shows terrae at 0.6 dispersion and maria at 0.5, so the new mixed crust should be higher to reflect this compositional heterogeneity. I'll set it to 0.65.
> 
> Now I'm finalizing the rare earth ores parameters and also reconsidering the megaregolith layer—the 15% regolith combined with 75% stone accounts for most of the near-surface material, leaving about 10% for minerals and ores, which tracks well. I should verify whether water should be included in this layer.
> 
> Looking at the water question: trace water ice might exist in permanently shadowed polar regions, but those are handled separately by the polar shaded regolith stratum, so the general near-surface layer correctly stays at 0. Now I'm ready to compile the final output with the updated rows and explanations.
> 
> For the Earth subsurface helium updates, I'll provide the full rows to keep things clear rather than just the column values. And I should double-check the lunar mantle—it's essentially anhydrous, though recent research hints at trace water amounts that are still negligible for practical purposes.
> 
> Now I'm verifying whether the deeper maria crust affects the Moon's total mass budget, checking if the strata masses still align with the known lunar mass of about 7.346e22 kg. Now I'm summing up the major components—the mantle dominates at 6.66e22, followed by the terrestrial crust and core—which gives me roughly 7.29e22. Comparing this to the actual Moon mass of 7.346e22, there's a small discrepancy of about 5.6e21 that I need to account for.
> 
> When I adjust the maria crust estimate upward to 4.34e20, the total shifts slightly to 7.294e22, narrowing that gap to 5.2e21—a modest improvement that suggests my component estimates are converging toward the right answer.
> 
> That remaining 5.2e20 kg represents about 0.7% of the Moon's total mass, which feels reasonable for a simplified model accounting for the depth discrepancy in the maria regions and accumulated rounding errors. I should move forward with this.
> 
> But wait—the user also asked me to incorporate volatiles across all strata following the extractable_resources.md guidelines. I need to check which layers might be missing volatile assignments, starting with the continental crust and working through the other strata.
> 
> Looking at what's actually in the current dataset, it's limited to Earth's continental land and subsurface layers plus the Moon strata, so I should focus on what's there rather than worrying about incomplete sections. The main addition would be helium in Earth's subsurface, while surface strata already have their volatiles accounted for. I'm working through whether argon should be included in the polar shaded regolith. Solar wind does implant argon, but at PSR temperatures around 40K, argon's vapor pressure stays too high relative to the extremely low pressures there to allow efficient cold-trapping—it wouldn't condense like water and CO2 do. LCROSS detected some argon, though that finding was contested, so I'm leaving it out of the lunar volatiles for now. Let me check what the user specified for the updated strata definitions to make sure my assignments align with their current framework. I'm organizing my final deliverables: the updated lunar maria rows in both the strata and resources files, the helium additions to Earth subsurface entries, plus my rationale and feedback. I'm also double-checking whether the "Rich in Fe, Ti, Mg, O" description still holds given the 82% anorthositic composition—it's technically accurate since maria are still enriched relative to terrae, just more modestly than pure basalt would suggest. Now I'm reviewing the actual resource numbers to make sure everything's consistent. The industrial minerals estimate of 10% for old maria seems reasonable given that the Moon's regolith contains materials like ilmenite, plagioclase, and pyroxene useful for construction and oxygen extraction. Now I'm ready to structure my response, starting with the updated moon crust definition that reflects a 24 km mixed basaltic-anorthositic layer rather than just a thin basaltic veneer, with composition weighted by the density and thickness of each component.
> 
> There's a minor mass discrepancy in the maria regions—about 6×10²⁰ kg of unassigned material between the crust and mantle boundaries—but this is negligible relative to total lunar mass. I've also lowered the survey confidence level since the deeper anorthositic substrate hasn't been directly sampled. Next I need to provide the updated data rows and then move into the helium subsurface section, which involves understanding how helium concentrates in crustal structures through natural gas accumulation and radioactive decay of uranium and thorium. Now I'm deciding how to present the updated helium data—I could just list the new values for each territory or provide complete rows, but since only the helium column changes, I'll give the values with a note that everything else stays the same to keep it concise. I'm continuing through the remaining subsurface entries, updating each HELIUM column with specific concentration values while keeping all other data intact. The pattern shows varying helium concentrations across different continental regions, with the final entry for Antarctica still being specified. The remaining regions show much smaller helium concentrations—China and the EU each around 130-200k tonnes, with India, Japan, and Antarctica holding significantly less, ranging from just 3k to 31k tonnes.
> 
> Summing these up gives roughly 7.8 million tonnes of total in-place helium, which aligns closely with the 8.3 million tonnes of globally identified helium resources. The high spatial concentration factor means most of this total in-place helium wouldn't form economically viable deposits, but I'm noticing the numbers seem to suggest identified resources are nearly as large as the total in-place estimate—which feels off, since in-place should typically exceed what's been identified and classified as reserves.
> 
> Thinking through this more carefully, the relationship between total in-place and identified resources really depends on how thoroughly helium has been explored. Since helium prospecting has been fairly extensive across known gas fields worldwide, the identified resources already capture most of the tested accumulations. When I factor in that some subsurface helium exists dissolved in rock and water rather than trapped in gas fields, my current estimate of 7.8 million tonnes is probably just capturing the gas-trap portion—the actual total in-place could easily be 10 to 100 times higher when accounting for all dispersed and dissolved helium throughout the subsurface.
> 
> Scaling up by roughly 10x to account for this dispersed component gives me around 75 million tonnes total, which is about 9 times the identified resources. The high dispersion value of 2.2 means the model would spread this across many cells with varying concentrations, then apply an economic cutoff to determine what's actually extractable.
> 
> If I want the model to output about 8 million tonnes as the economically viable fraction from an 80 million tonne total, that's roughly the top 10% of the distribution exceeding the economic threshold. I'm checking whether this makes sense given the lognormal distribution with σ = 2.2 in log₁₀ space.
> 
> Working through the math: if the median concentration is m and I need P(X > c) = 0.10, then the economic cutoff is about 660 times the median. The mean of this lognormal is roughly 380,000 times the median, so the cutoff sits around mean/576—meaning only the richest deposits become economically viable.
> 
> Actually, I'm overcomplicating the statistical modeling here. The framework will handle the distribution internally, so I just need to anchor reasonable total helium inventories for each region. Let me set moderate parameters and calculate the implied in-place totals: USA around 2.4 million tonnes, Russia around 6.9 million tonnes, and other regions around 11.6 million tonnes, with China as a smaller contributor.
> 
> Finishing out the remaining regions—China at 400k tonnes, EU at 270k tonnes, India at 71k tonnes, Japan at 6k tonnes, and Antarctica at 61k tonnes—brings the global total to roughly 21.8 million tonnes, which feels reasonable as about 2.6 times the currently identified resources. Though I should double-check my Russia assumptions.
> 
> Russia's Kovykta and Chayanda fields in eastern Siberia are among the world's richest helium sources at 0.3-0.6%, with identified resources around 1-1.7 million tonnes, but given Russia's massive untested gas reserves, the total subsurface helium could be substantially larger. My initial 6.9 million tonne estimate was probably too aggressive—that's 4-7 times the identified amount. Adjusting down to 4.3 million tonnes feels more defensible, still 2.5-4 times identified resources but more conservative.
> 
> Now I'm working through the subsurface helium estimates for different regions by applying consistent parameters across the major players. Tallying these up gives me roughly 19.1 million tonnes of in-place helium in the shallow subsurface globally, compared to about 8.3 million tonnes of identified resources—so there's a meaningful gap between what we've found and what's likely down there.
> 
> Let me verify my calculations for Russia and the "Other" category to make sure the numbers hold up. Russia checks out at around 4.3 million tonnes, and the "Other" grouping is substantial because it covers such vast areas and includes major helium-producing regions like Qatar, Algeria, and Australia, so 11.6 million tonnes seems defensible. I'm comfortable with these estimates now.
> 
> For the uncertainty parameters, I've set the coefficient of variation at 0.5 for most regions, which reflects roughly ±50% confidence in our total helium estimates—a reasonable level of uncertainty given how sparse our direct measurements are.  Antarctica gets higher uncertainty at 0.8 since we have almost no data there. The dispersion error of 0.5 captures how uncertain we are about whether deposits are concentrated or spread out. These feel appropriately calibrated.
> 
> I'm also reconsidering whether to include argon from the subsurface. Radiogenic argon-40 from potassium decay does accumulate in crustal rocks, but there's no commercial extraction of it—air separation is far cheaper. Natural gas contains only trace argon anyway, so it's not a meaningful component when drilling.
> 
> Now I'm ready to write the actual response. I'll structure it around the key updates: the geometric and compositional changes to the lunar maria, adding a helium column for Earth's subsurface, some additional observations, and my feedback on the model.

## 1. Moon Crust (Maria) — Geometry and Composition Update

The strata.md definition now extends from ~1–25 km (24 km thickness), incorporating the anorthositic substrate beneath the mare basalt flows. The existing table had a 4 km pure-basalt layer. I recomputed the geometry and blended the composition by mass fraction.

**Density reasoning.** The upper ~4 km is mare basalt (ρ ≈ 3.0–3.1 g/cm³), the lower ~20 km is anorthositic (ρ ≈ 2.75–2.80 g/cm³). A volume-weighted average gives (4 × 3.05 + 20 × 2.78) / 24 ≈ 2.82. I round to **2.85** to reflect that the basalt-enriched upper crust and possibly denser lower-crust noritic/troctolitic lithologies nudge the mean slightly above pure anorthosite. The terrae crust is listed at 2.80, so 2.85 captures the basaltic influence.

**Composition reasoning.** Mass fractions are ~18.1% basaltic, ~81.9% anorthositic (ratio of 4 × 3.05 to 20 × 2.78). I blended each resource from the existing CRUST_MARIA and CRUST_TERRAE rows accordingly, then applied two adjustments: (a) slight KREEP enrichment (rare-earth and uranium ores) in the sub-maria layer of the Procellarum KREEP Terrane, and (b) slightly increased dispersion to reflect the compositional layering and lateral heterogeneity (PKT vs non-PKT maria).

**Mantle impact.** The mantle geometry (inner_r = 337, outer_r = 1677) is unchanged. The gap in the 17% maria area between r = 1677 and r = 1712 (a 35 km thick unassigned zone) represents ~6 × 10²⁰ kg of mass — about 0.8% of total lunar mass. This is reduced from the prior gap (55 km thick, ~9.6 × 10²⁰ kg) and is acceptable for the simplified model.

**Survey level.** Lowered from 2.2 to 1.8. The upper basalt is well-sampled (Apollo 11, 12, 15, 17; Luna 16, 24), but the anorthositic substrate below ~4 km is characterized only by orbital spectroscopy (Kaguya, LRO, Chandrayaan) and Apollo seismic models. Deeper composition is inferred, not directly measured.

**Volume check:**

$$
V = 0.17 \times \frac{4}{3}\pi\bigl(1736^3 - 1712^3\bigr) = 0.17 \times 4.189 \times 2.140 \times 10^8 = 1.52 \times 10^8 \text{ km}^3
$$

**Mass:** \(1.52 \times 10^8 \times 2.85 \times 10^{12} = 4.33 \times 10^{20}\) kg.

### Updated strata.tsv row

```
MOON_CRUST_MARIA	MOON	1737		CRUST_MARIA	1712	24	0.17	6.44e6	1.52e8	2.85	4.33e20	1.8	Apollo/Luna mare basalt samples; anorthositic substrate inferred from orbital + seismic data
```

### Updated strata_resources.tsv row

```
MOON_CRUST_MARIA	0.08,0.4,0.6,0.3	4.4,0.25,0.5,0.2	10.7,0.22,0.45,0.18	0.55,0.4,0.6,0.25	0,0,0,0	0.6,0.35,0.65,0.25	0.008,0.5,0.7,0.3	0.07,0.35,0.5,0.2	6,0.22,0.35,0.18	0,0,0,0	0,0,0,0	0,0,0,0	75.5,0.12,0.22,0.12	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0
```

Row sum ≈ 97.9 (normalizes to 100). Key changes from the old pure-basalt row: IRON_ORES 15 → 4.4, ALUMINUM_ORES 5 → 10.7 (now dominated by anorthositic feldspar), INDUSTRIAL_METAL_ORES 2 → 0.55, RARE_EARTH_ORES 1.5 → 0.6 (diluted but still slightly enriched relative to terrae by KREEP), STONE 65 → 75.5.

---

## 2. Helium in Earth Continental Subsurface

Helium is produced by α-decay of U and Th throughout the crust, migrates upward, and accumulates in the same structural traps as natural gas. It is the primary source of commercial helium worldwide and is clearly a co-product of oil/gas drilling. The existing subsurface rows had HELIUM at `0,0,0,0`.

**Abundance calibration.** Average crustal He is ~3 ppb by mass (Ozima & Podosek 2002). In the 0.5–2 km depth range where gas traps concentrate, I estimate 5–50 ppb (0.0000005–0.000005 %wt) depending on regional U/Th endowment and gas trap density. I calibrated against global identified helium resources (~8.3 Mt in ~50 Gm³; USGS 2023) aiming for total in-place to be roughly 2–3× identified resources, accounting for undiscovered accumulations and dispersed/dissolved helium.

**Dispersion.** Set at σ = 2.2 (log₁₀ units), comparable to oil and precious metals. Helium is only extractable where it has accumulated in gas traps; most crustal rock has sub-economic concentrations. The high dispersion ensures the model generates a few rich deposits against a vast low-concentration background.

**Implied masses (cross-check):**

| Territory | Abundance (%wt) | Stratum mass (kg) | He in-place (tonnes) | Notes |
|---|---|---|---|---|
| USA | 5 × 10⁻⁶ | 4.72 × 10¹⁹ | 2.4M | US reserves ~0.65 Mt; large Basin & Range, Hugoton, Wyoming |
| Russia | 5 × 10⁻⁶ | 8.64 × 10¹⁹ | 4.3M | Kovykta, Chayanda, Orenburg; world's largest gas reserves |
| Other | 3 × 10⁻⁶ | 3.88 × 10²⁰ | 11.6M | Qatar, Algeria, Tanzania, Australia, etc. |
| China | 1 × 10⁻⁶ | 4.03 × 10¹⁹ | 0.4M | Sichuan basin, expanding exploration |
| EU | 1 × 10⁻⁶ | 2.68 × 10¹⁹ | 0.27M | North Sea, Netherlands; modest resources |
| India | 5 × 10⁻⁷ | 1.41 × 10¹⁹ | 0.07M | Rajmahal, limited gas-associated He |
| Japan | 2 × 10⁻⁷ | 2.98 × 10¹⁸ | 0.006M | Very limited gas resources |
| Antarctica | 1 × 10⁻⁷ | 6.12 × 10¹⁹ | 0.06M | No data; minimal radiogenic accumulation expected |

Global total: ~19 Mt in-place vs ~8.3 Mt identified. The ≈2.3× ratio is consistent with the maturity of helium exploration (many gas fields tested, but significant undiscovered resources remain, plus dispersed/dissolved helium not counted in reserves).

### Updated strata_resources.tsv rows (Earth continental subsurface)

Only the HELIUM column changes; all other columns are identical to the existing table. I reproduce the full rows for unambiguous replacement:

```
EARTH_USA_CONTINENTAL_SUBSURFACE	0,0,0,0	0.002,0.25,1.5,0.25	0.0004,0.35,1.5,0.3	0.004,0.3,1.5,0.25	0.0001,0.4,2.0,0.3	0.0003,0.4,1.5,0.3	0.0002,0.4,1.8,0.3	0.005,0.3,1.2,0.25	8,0.15,0.7,0.12	0.5,0.3,1.3,0.25	0.003,0.3,1.8,0.25	0.0006,0.35,1.8,0.3	80,0.05,0.2,0.05	2,0.25,0.7,0.2	3,0.2,0.6,0.1	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.0005,0.3,1.5,0.25	0.00005,0.4,1.5,0.3	0,0,0,0	0.000005,0.5,2.2,0.5	0,0,0,0	0,0,0,0
EARTH_CHINA_CONTINENTAL_SUBSURFACE	0,0,0,0	0.003,0.3,1.5,0.3	0.0005,0.35,1.5,0.35	0.006,0.3,1.5,0.3	0.00008,0.45,2.0,0.35	0.0008,0.3,1.5,0.3	0.0002,0.45,1.8,0.35	0.005,0.35,1.2,0.3	8,0.18,0.7,0.15	0.4,0.35,1.3,0.3	0.004,0.3,1.8,0.3	0.0003,0.4,1.8,0.35	80,0.06,0.2,0.06	2,0.3,0.7,0.25	3,0.22,0.6,0.12	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.0003,0.35,1.5,0.3	0.00003,0.45,1.5,0.35	0,0,0,0	0.000001,0.55,2.2,0.5	0,0,0,0	0,0,0,0
EARTH_EU_CONTINENTAL_SUBSURFACE	0,0,0,0	0.0015,0.2,1.5,0.2	0.0003,0.3,1.5,0.25	0.004,0.25,1.5,0.2	0.00008,0.35,2.0,0.3	0.0003,0.35,1.5,0.3	0.0002,0.35,1.8,0.3	0.004,0.3,1.2,0.2	9,0.12,0.7,0.1	0.3,0.3,1.3,0.2	0.002,0.25,1.8,0.2	0.0003,0.3,1.8,0.3	79,0.04,0.2,0.04	2,0.2,0.7,0.18	3,0.18,0.6,0.1	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.0003,0.3,1.5,0.25	0.00003,0.4,1.5,0.3	0,0,0,0	0.000001,0.5,2.2,0.5	0,0,0,0	0,0,0,0
EARTH_RUSSIA_CONTINENTAL_SUBSURFACE	0,0,0,0	0.004,0.3,1.5,0.3	0.0005,0.35,1.5,0.3	0.006,0.3,1.5,0.3	0.00015,0.4,2.0,0.35	0.0005,0.4,1.5,0.35	0.0003,0.4,1.8,0.35	0.005,0.35,1.2,0.25	7,0.18,0.7,0.15	0.6,0.35,1.3,0.3	0.003,0.3,1.8,0.3	0.001,0.35,1.8,0.3	79,0.06,0.2,0.06	2,0.3,0.7,0.22	3,0.25,0.6,0.12	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.0008,0.35,1.5,0.3	0.00008,0.4,1.5,0.35	0,0,0,0	0.000005,0.5,2.2,0.5	0,0,0,0	0,0,0,0
EARTH_INDIA_CONTINENTAL_SUBSURFACE	0,0,0,0	0.003,0.3,1.5,0.3	0.0006,0.35,1.5,0.3	0.003,0.35,1.5,0.3	0.00005,0.45,2.0,0.4	0.0004,0.4,1.5,0.35	0.0001,0.5,1.8,0.4	0.004,0.35,1.2,0.3	8,0.2,0.7,0.15	0.3,0.4,1.3,0.3	0.003,0.35,1.8,0.3	0.0002,0.45,1.8,0.4	81,0.07,0.2,0.07	2,0.3,0.7,0.25	3,0.25,0.6,0.12	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.0002,0.4,1.5,0.35	0.00002,0.5,1.5,0.4	0,0,0,0	0.0000005,0.55,2.2,0.55	0,0,0,0	0,0,0,0
EARTH_JAPAN_CONTINENTAL_SUBSURFACE	0,0,0,0	0.0005,0.3,1.5,0.25	0.0001,0.4,1.5,0.35	0.001,0.35,1.5,0.3	0.00004,0.4,2.0,0.35	0.0002,0.4,1.5,0.35	0.0001,0.45,1.8,0.35	0.006,0.3,1.2,0.25	8,0.15,0.7,0.12	0.2,0.35,1.3,0.3	0.001,0.35,1.8,0.3	0.00005,0.45,1.8,0.4	82,0.05,0.2,0.05	2,0.25,0.7,0.2	3,0.2,0.6,0.1	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.0001,0.4,1.5,0.35	0.00001,0.5,1.5,0.4	0,0,0,0	0.0000002,0.6,2.2,0.55	0,0,0,0	0,0,0,0
EARTH_ANTARCTICA_CONTINENTAL_SUBSURFACE	0,0,0,0	0.002,0.7,1.5,0.6	0.0005,0.8,1.5,0.6	0.003,0.7,1.5,0.6	0.00008,0.8,2.0,0.7	0.0003,0.8,1.5,0.7	0.0002,0.8,1.8,0.7	0.004,0.6,1.2,0.5	7,0.5,0.7,0.4	0.3,0.7,1.3,0.5	0.002,0.6,1.8,0.5	0.0002,0.8,1.8,0.6	81,0.2,0.2,0.15	2,0.5,0.7,0.4	4,0.4,0.6,0.3	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.0002,0.7,1.5,0.6	0.00002,0.8,1.5,0.6	0,0,0,0	0.0000001,0.8,2.2,0.6	0,0,0,0	0,0,0,0
EARTH_OTHER_CONTINENTAL_SUBSURFACE	0,0,0,0	0.003,0.35,1.5,0.35	0.0006,0.4,1.5,0.35	0.005,0.35,1.5,0.3	0.00012,0.45,2.0,0.4	0.0005,0.45,1.5,0.4	0.0003,0.5,1.8,0.4	0.005,0.4,1.2,0.3	8,0.2,0.7,0.18	0.5,0.4,1.3,0.3	0.003,0.4,1.8,0.35	0.0005,0.4,1.8,0.35	79,0.07,0.2,0.06	2,0.3,0.7,0.25	3,0.25,0.6,0.15	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0,0,0,0	0.0004,0.4,1.5,0.35	0.00004,0.5,1.5,0.4	0,0,0,0	0.000003,0.5,2.2,0.5	0,0,0,0	0,0,0,0
```

---

## 3. Other Volatiles Considered but Not Added

**CO₂ and N₂ in Earth subsurface:** Natural gas contains ~2–5% CO₂ and ~1–5% N₂ by volume on average. These are separated during gas processing (CO₂ increasingly captured for CCS). However, their total mass relative to the stratum is extremely small — comparable to or smaller than the ethane entries already present. I omitted them for now; they can be added in a future pass if the model needs to track CO₂ from gas processing as a distinct resource stream.

**Argon in Earth subsurface:** Radiogenic ⁴⁰Ar from ⁴⁰K decay accumulates in crustal rock and gas fields at 0.001–0.02% of gas by volume. However, subsurface argon is not commercially exploited (atmospheric air separation is far cheaper and essentially unlimited). Omitted.

**Hydrogen in Earth subsurface:** Natural ("geological" or "gold") hydrogen is an emerging area of interest, with surface seeps mapped in Mali, Brazil, Oman, and elsewhere. However, the resource base is highly speculative as of 2025 — estimates span orders of magnitude. Omitted pending better constraints; could be added as a speculative entry with very high uncertainty.

**Lunar strata:** The existing volatile entries (He, H₂, N₂, CO₂, CO, NH₃, CH₄, SO₂ in PSR and partial set in regolith) are consistent with LCROSS/LAMP measurements and Apollo sample analyses. No changes needed. Deeper strata (near-surface, crust, mantle, core) are essentially anhydrous/volatile-free; zero entries are correct.

**Earth continental land strata (0–100 m):** Already have water and methane. Helium, CO₂, N₂ from soil gas and shallow groundwater are negligible by mass relative to the stratum. No changes.

---

## 4. Other Checks and Minor Observations

**Numerical verification of existing rows.** I spot-checked all Moon strata and several Earth strata for internal consistency (mass = volume × density, geometric calculations). Everything checks out to within rounding precision. Row sums in strata_resources.tsv range from ~97.6 to ~99.8 across Moon strata; all will normalize correctly.

**Moon total mass budget.** Sum of all Moon strata masses (with updated maria crust) ≈ 7.29 × 10²² kg vs actual Moon mass 7.346 × 10²² kg, a deficit of ~0.7%. The deficit is the unassigned 17%-area gap between maria crust bottom (r = 1712) and mantle top (r = 1677), plus rounding. This is improved from the prior model (where the gap was larger with the thin 4 km maria crust).

**strata.md description consistency.** The updated description says "Rich in Fe, Ti, Mg, O (ilmenite, pyroxene, olivine)." With the crust now ~82% anorthositic by mass, these enrichments are relative to the terrae average rather than absolute — the maria crust still has ~2× the iron resources and ~1.5× the titanium of terrae. The description is accurate but could be augmented to mention aluminum enrichment from the anorthositic substrate (Al₂O₃ resources are 10.7% vs 12% in terrae, a substantial fraction).

---

## 5. Feedback on Prompt and Context Files

**Things that work well:**

The four-parameter per-cell format (`relative_abundance, abundance_cv, dispersion, dispersion_error`) is compact and powerful. The separation of strata geometry from composition into two TSV files is clean. The extractable_resources.md document is thorough and the compositional definitions are well thought out. The survey_level calibration anchors (0 to 10 with specific reference points) are very helpful.

**Suggestions for improvement:**

1. **Adjacency rules or a stratum stacking diagram in strata.md.** The biggest source of confusion during this analysis was determining how crust strata with different area_fractions and depths tile together, and whether gaps between adjacent strata are intentional. A per-body "stratum stack" (even ASCII art) showing radial boundaries and area_fractions would make this immediately clear and help prevent errors when adjusting boundaries. For the Moon, something like:

   ```
   1737 ─── Regolith (0.01 km)
   1736 ─── Near-Surface (0.99 km)
            ┌─ Terrae 83% ──┬─ Maria 17% ─┐
   1736 ─── │  Crust Terrae  │ Crust Maria │
   1712 ─── │                │─────────────┘ ← gap 17% area
   1677 ─── └────────────────┘
            Mantle (100% area)
   337  ─── Core
   0
   ```

2. **Minimum representable abundance.** The helium values I added go down to 10⁻⁷ %wt. If the simulation code truncates or loses precision below a certain threshold, it would help to document that threshold in the column specification (e.g., "values below 10⁻⁸ are treated as zero"). This avoids wasted effort calibrating very trace resources.

3. **Volatile tracking convention clarification.** The note in extractable_resources.md ("volatiles represent what might be separated in the extraction processes") could be expanded with a few examples to remove ambiguity, e.g.: "For subsurface drilling strata, this includes natural gas components (CH₄, C₂H₆, He, CO₂), formation water, and H₂S (tracked as sulfur). For solid rock strata, it does NOT include oxygen or other elements that require chemical decomposition to liberate — those are tracked within the elemental composition of the abstract resource (e.g., stone, ores) and released during downstream refining."

4. **Territory area_fraction documentation.** The shelf area added to each territory for subsurface strata is implicit (I verified it by back-calculation). Documenting the land and shelf area_fractions separately (even in a comment) would help anyone cross-checking the data.

5. **Gap mass accounting.** Consider adding a brief note in strata.md (perhaps in note 2) explicitly stating that small mass gaps between adjacent fractional-area strata are expected and acceptable, and giving a rough tolerance (e.g., <2% of body mass). This would forestall questions from anyone reviewing the data.

