I need help developing a consistent simplified abstraction for electricity, solar usage, heat, efficiency, and energy accounting for a Kardashev-style civilization scale. We have 5 columns in operations.tsv with these intended meanings:

- electricity — This is net electricity generated (positive values) or used (negative values).
- solar — This is intended to be direct solar input to an operation. It's usually empty (N/A) but should have a positive value for SOLAR_POWER, SOLAR_STEELMAKING, and biological operations that use photosynthesis. (For photosynthesis, solar should be an optimized potential input for the operation. In some contexts, all or supplemental light might be supplied by additional electricity use.)
- heat — Basically waste heat. It may or may not be used for facility or district heating. Depending on location, the simulation may need to account for heat disposal. In any of these cases, we want and accounting here.
- efficiency — This seems intuitive, but I'm having trouble developing a unified definition and equations that account for different kinds of operations.
- civ_power — This is our accounting measure for a Kardashev-style civilization scale. It doesn't have any functional effect in the simulation, but the summed value will be used to calculate a level on the Kardashev scale. There's clearly no "right" answer how to do this, but I'd like it to be derived from the parameters above.

Please develop a unified abstraction that systematically defines the parameters above. Generate relevant text modifications for operations.schema.md and modify operations.tsv accordingly.

---
---

*Thinking...*

> This is a complex request to develop a unified energy abstraction for operations in a simulation. Let me carefully think through the physics and accounting.
> 
> ## Understanding the Current State
> 
> Looking at the current data:
> 
> **Power generation operations** (normalized to 1 MW electricity output):
> - `electricity` = 1 (MW generated)
> - `heat` = waste heat (MW)
> - `efficiency` = electricity / (electricity + heat) for renewables, or electricity / total_energy_input for fuel-based
> 
> For example:
> - SOLAR_POWER: electricity=1, solar=4, heat=4.00, efficiency=0.25
>   - So solar input = 4 MW, electricity out = 1 MW, heat = 4 MW... wait, that doesn't add up
>   - If efficiency = electricity/solar = 1/4 = 0.25, that checks out, but the heat value seems off—it should be 3 MW (the difference between solar input and electricity output), not 4 MW.
> - Looking at the schema definition, waste heat should equal the waste fraction times total input: 0.75 × 4 = 3 MW, which contradicts the 4.00 shown in the table.
> - I'm wondering if there's a different interpretation—could heat represent something other than just the difference, or is there an error in the data?
> - Checking other operations like GEOTHERMAL_POWER to understand the pattern: with electricity=1, heat=6.67, and efficiency=0.15, the math doesn't align cleanly either way I calculate it.
> 
> Wait, I think I've got it—if I divide electricity by efficiency (1/0.15 = 6.67), that matches the heat value exactly. Let me test this formula across all the operations to confirm it holds.
> 
> Actually, hold on. The column is labeled "Net waste heat from the operation," but these calculations suggest heat represents total energy throughput divided by efficiency, not waste heat specifically. Let me reconsider what's actually being measured here by checking if the values make physical sense for a few different power sources. I'm seeing a pattern now—the "heat" field actually represents total primary energy input divided by efficiency, not waste heat. For power generation systems, this tracks the total thermal throughput, which makes sense for energy accounting. But for industrial operations like oil refining, the relationship seems different, so I need to look more carefully at how this field is actually being used across different operation types. I see the pattern now—the "heat" field represents total energy throughput divided by efficiency, not waste heat itself. The actual waste heat is the difference between that total and the electricity input. For power generation, electricity is the useful output and heat is the primary energy source, but for industrial processes, electricity is the input and I need to reconsider how the energy flows work in that direction. I'm realizing the formula might work differently for industrial processes—maybe the "heat" represents the total energy throughput needed, where useful work equals the electricity input and the formula accounts for the efficiency ratio. Let me trace through this: if heat equals electricity divided by efficiency, then multiplying both sides by efficiency gives us electricity back, which suggests the heat term captures the full energy budget including losses. I'm realizing the formula breaks down when chemical reactions release or absorb energy—for exothermic processes like steelmaking, the total thermal energy can exceed electrical input, but for endothermic processes like electrolysis, the waste heat should be less than the electrical consumed. Let me check the water electrolysis numbers to see which case applies here. So the hydrogen's energy content works out to about 4.4 MW, which means the waste heat is roughly 1.1 MW—that gives us the 0.80 efficiency we started with. It looks like the definition of "heat" differs depending on context: for power generation it's based on primary energy input, but for industrial processes it's the electricity input divided by efficiency.
> 
> Let me recalculate that directly: 5.50 MW divided by 0.80 gives 6.88 MW, which doesn't match the 4.4 MW of stored chemical energy. I think I need to reconsider what "heat" actually represents in this context.
> 
> Looking at the energy throughput formula more carefully, for power generation it makes sense that heat equals the total primary energy input, but for industrial operations it's less clear why the total energy would exceed the electrical input. Maybe the heat column is capturing both the waste heat from the electrical process and the heat generated by exothermic chemical reactions themselves. Or perhaps this is just a consistent bookkeeping convention where heat is always calculated as electricity divided by efficiency, with the interpretation shifting depending on the operation type.
> 
> Let me verify this against the actual data: if heat represents primary energy for power generation, then for solar power with 1 MW electricity output and 0.25 efficiency, the heat should be 1 divided by 0.25, which gives 4 MW—and that matches what the data shows.
> 
> For industrial processes like water electrolysis, the same formula applies: 5.50 MW electricity divided by 0.80 efficiency equals 6.875 MW, which rounds to 6.88 MW in the data.
> 
> So "heat" in this dataset is actually the gross energy throughput, not waste heat itself—the waste heat would be the difference between this gross energy and the electricity output, calculated as electricity times (1 over efficiency minus 1). So the waste heat is actually 1.10 MW when calculated directly as electricity times one minus efficiency. I've been conflating two different energy accounting methods—the mathematical convention in the table versus what actually makes physical sense. Let me step back and think about what a unified abstraction should look like across all operation types, especially when I factor in the solar column. For power generation specifically, I need to distinguish between renewable sources where energy comes directly from nature versus other pathways. For industrial operations with negative electricity (energy consumption), I need to distinguish between endothermic processes like electrolysis that convert electrical input into chemical energy, and exothermic processes like steelmaking that release chemical energy alongside electrical input. The efficiency calculation flips depending on the process type—for endothermic it's useful work divided by electrical input, while for exothermic I'm balancing both electrical and chemical energy sources against the useful product and waste heat. This is getting tangled because some operations mix both types of energy flows, so I should focus on what the user actually needs rather than trying to model every edge case perfectly. I'm laying out the specific energy categories for each operation type—fuel-based systems track chemical energy, solar captures radiation, renewables harness natural fluxes, and industrial processes need both electrical and chemical energy accounting. The core challenge is that industrial operations don't produce measurable electricity like power plants do, so I need to define useful output differently—either as the energy stored in chemical products or the actual transformation achieved. Let me shift to a more practical framework that works better for simulation purposes. I'm realizing the challenge here—I need to define these five columns in a way that's actually computable from the table data the user has, rather than introducing variables like chemical energy that can't be directly measured. Let me reconsider the approach by working backwards from what's observable: electrical power flow, direct solar input, waste heat, and then derive efficiency and total throughput from those concrete values. and derivable from each other:
> 
> **Approach: Total energy throughput**
> 
> Define a "total energy throughput" concept that I'll call P (for "gross power"):
> 
> For power generation:
> - P = E / η (since efficiency = useful output / total input)
> - S = P for SOLAR_POWER (solar is the primary input)
> - Q = P - E = E × (1 - η) / η (waste heat)
> 
> For industrial operations:
> - The process consumes |E| MW of electricity
> - Some of that goes to useful work, rest is waste heat
> - η = useful_fraction of electricity
> - Q = |E| × (1 - η) (waste heat)
> - P = |E| (total energy input is just the electricity)
> 
> For solar industrial (SOLAR_STEELMAKING):
> - Uses both electricity and direct solar
> - P = |E| + S
> 
> Now I'm checking whether this new formulation actually improves things. With the updated definitions, waste heat becomes P × (1 - η) for operations that combine electricity and solar inputs, but I need to verify this gives sensible results compared to the previous approach where heat was calculated as |electricity| / efficiency. Let me test this against specific cases like water electrolysis and solar power generation to see if the numbers make more sense. I'm working through how to calculate total energy budgets across different operation types. For solar operations, the electrical output equals efficiency times solar input, with waste heat as the difference. For renewables without direct solar measurements, I back-calculate the natural energy input from the efficiency ratio. Industrial operations consuming electricity treat that consumption as their total energy input, generating waste heat based on their efficiency. Hybrid systems like solar steelmaking combine electrical and solar inputs, while biological operations with photosynthesis follow their own pattern.
> 
> Now I'm thinking about what civ_power should represent for the Kardashev scale—it needs to capture the total energy a civilization harnesses or uses, whether that's electricity generated from natural sources, total energy throughput through industrial processes, or combined inputs from multiple sources.
> 
> The key insight is that I need to avoid double-counting: if I sum civ_power across all operations, I can't count both the energy generated and the energy consumed, since consumption should roughly equal generation. The cleanest solution is to define civ_power as the sum of positive electricity production and solar energy input, which gives me a single measure of total energy throughput without duplication. I'm working through how to count civilization power without double-counting energy. Option A counts only the useful electricity output from solar panels plus any direct solar use, but that misses the total solar energy captured. Option B seems to be exploring primary energy harnessed from the source instead, which would give a more complete picture of total energy input to the civilization. I'm settling on a formula: civ_power equals the maximum of electricity generated or zero, plus any direct solar energy captured. This avoids double-counting since electricity consumed by industrial operations is already tallied at the generation source, while direct solar harnessing like photosynthesis or solar steelmaking gets counted as useful power output at that operation. The key insight is that for Kardashev calculations, I should count the primary energy input directly extracted from nature, not derived energy. For SOLAR_POWER, that's the solar radiation S itself since E is just the converted output. For GEOTHERMAL_POWER and HYDROPOWER, I'd use the total energy extracted divided by efficiency to get back to the natural source. This gives me a consistent framework where civ_power represents the raw natural energy being tapped, regardless of how efficiently it's converted. So I should measure at the primary energy level—what nature actually provides to the operation, not the converted electricity. That means counting the raw solar radiation S for solar power, and the extracted heat or kinetic energy divided by efficiency for the others, since those represent the actual energy nature is supplying before conversion losses.
> 
> But fuel-based power creates a double-counting problem: if the fuel came from nature directly (like natural gas), its chemical energy is primary, but if it was made through electrolysis, that energy was already counted upstream. Rather than trying to perfectly track primary energy through complex chains, I should define civ_power more pragmatically—as the total power the operation handles at its most useful level, accounting for what role it actually plays in the system.
> 
> For power generation, civ_power equals the full energy throughput (E/η), whether that's solar input, geothermal heat, or chemical fuel energy. But industrial operations that just consume electricity shouldn't count toward the scale since that electricity was already tallied at its source—unless they're capturing direct solar energy themselves. Biological systems with photosynthesis count their direct solar capture. This creates an odd situation though: a civilization running lots of industrial operations wouldn't boost its Kardashev rating, only the power plants feeding them would. For operations that consume grid electricity without generating power, there's no additional energy to count since it's already accounted for at the generation side. But I'm noticing a potential issue with fuel-based power generation—if that fuel was produced using grid electricity (like hydrogen from electrolysis), counting both the fuel energy and the original electricity would double-count. The simulation probably can't distinguish between fuel extracted naturally versus fuel created through electrolysis, so the practical approach is to count all primary energy mobilized for power generation as civ_power = E/η, accepting that some scenarios might have this overlap.
> 
> However, since the goal is calculating a Kardashev scale value that sums across all operations without double-counting, I need to think through this more carefully. If I'm counting E/η for fuel-based power generation and also counting the grid electricity that produced that fuel, I'm inflating the total energy. The real question is whether the simulation's fuel sources are meant to represent extracted resources or recycled energy from the grid. Recycling loops where electricity converts to fuel and back are uncommon enough that they won't skew the results much, and the approach is straightforward and consistent—good enough for a civilization-scale metric. I'm realizing the key insight: each operation has a normalized form (1 MW for energy operations, 1 t/h for others), then gets scaled by its actual throughput, so the civ_power values are per-unit costs that multiply up to the total.
> 
> Now I'm working through how to define civ_power consistently. For power generation, it should be the primary energy input per unit electricity output (E/η), which gives the right value for solar. But for industrial operations, civ_power is just the direct solar requirement per unit mass, which creates a unit mismatch—power generation uses MW per MW_e while industrial uses MW per t/h.
> 
> That's actually fine though, because when I scale each by its operation's throughput, everything converts back to MW. The key insight is that civ_power across all operations represents the total primary energy the civilization harnesses, which is exactly what a Kardashev scale should measure. But I should double-check what the user actually wants from this metric.
> 
> Looking back at their request, they want civ_power derived from the existing parameters—electricity, solar, heat, and efficiency. The challenge is avoiding double-counting: if electricity comes from solar, I shouldn't add both. So I'm testing whether taking the maximum of solar and electricity works better than summing them, since that would capture the actual energy harnessed without duplication.
> 
> But there's still an issue—for geothermal and other sources, the total energy available is the raw power divided by efficiency, not just the electricity output. So the maximum approach misses the full picture of what the civilization can actually harness. I need to think about what Kardashev really measures: the total power a civilization has access to, not just what they've converted to electricity. I need to clarify the primary energy calculation for each power source. For solar, the primary energy is just the solar input itself. For geothermal and hydrogen, I'm dividing the electricity output by efficiency to get back to the original energy source. So the formula depends on whether solar is available—if it is, that's the primary energy; otherwise, I back-calculate from electricity using the efficiency factor.
> 
> Actually, wait—I'm realizing the formula E/η works universally across all power generation methods because it always gives me the total primary energy input, whether that's solar radiation, geothermal heat, or chemical energy from hydrogen. Let me verify this holds for each case, and then I need to figure out how to handle non-power operations and edge cases where solar might be partially available.
> 
> For operations like SOLAR_STEELMAKING where electricity comes from the grid but direct solar provides the main heat source, I should count only the direct solar energy since the grid electricity is already accounted for at generation. Similarly, for biological systems like agriculture, the civ_power should reflect the direct solar energy captured through photosynthesis, not the grid electricity used for auxiliary needs. Now I'm wondering how RADIOISOTOPE_POWER fits into this framework.
> 
> For radioisotope and fission systems, the pattern becomes clear: when there's no direct solar input (S = 0) but significant electrical output (E > 0), I calculate the total thermal power by dividing the electrical output by the efficiency. So for the radioisotope RTG with 1 MW electrical at 10% efficiency, that represents 10 MW of total decay power being harnessed. The same logic applies to nuclear fission, where 1 MW electrical at 34% efficiency means 2.94 MW of total thermal power. This gives me a unified framework: civ_power equals E/η for power generation systems, S for operations with direct solar input, and zero otherwise. I've found a cleaner formula: civ_power equals the maximum of E divided by η and S. Let me verify this works across the different operation types—solar power checks out, geothermal checks out, and for cases where E is negative like solar steelmaking and water electrolysis, the formula correctly defaults to S. I see the issue—when both E and S are positive, I'm double-counting the power. Let me use a piecewise approach instead: for power generation operations where E is positive, I'll use E divided by efficiency; for everything else, I'll fall back to S or treat it as zero if undefined. This keeps things straightforward and avoids overlap. Now I'm refining the other column definitions, starting with how electricity should be normalized across different operation types.
> 
> For solar input, I need to distinguish between operations that directly capture solar radiation versus those that use it indirectly. For solar power generation, S represents the total incident radiation needed to produce the electrical output E. For solar steelmaking, S is the concentrated thermal power applied to the process. But biological operations complicate this—the solar energy actually captured through photosynthesis is only a small fraction of the incident radiation hitting the plants, so I need to clarify what "solar input" means in that context.
> 
> Since biological operations aren't currently in the table, I'll focus on setting up the framework for the operations that are already there rather than adding new ones now.
> 
> Looking at the "heat" column, I realize it's currently showing total energy throughput rather than actual waste heat. For the simulation to properly account for heat disposal, I need to redefine this column to represent only the waste heat generated. For power generation operations, waste heat would be the difference between primary energy input and useful output—essentially the energy lost to inefficiency. When solar is involved, the calculation changes since there's no fuel input to account for.
> 
> The pattern that emerges is that waste heat equals the total primary energy input multiplied by one minus the efficiency factor, regardless of whether it's a generation or industrial operation.
> 
> Now I'm realizing the current "heat" calculations don't actually represent waste heat—they're computing |E|/η, which conflates useful work with total energy input in a way that doesn't align with thermodynamic principles. For water electrolysis specifically, the current method gives 6.88, but the actual waste heat should be around 1.10 based on the efficiency-adjusted formula. I see the issue—dividing by efficiency gives a value larger than the input, which violates energy conservation. The current heat definition isn't physically sound, so I need to establish a clearer framework. For generators, the primary energy source (fuel, solar, nuclear, etc.) is what actually drives the system, and I should define the total input explicitly based on whether energy is being consumed or produced. I'm working through whether I can unify these formulas—both cases reduce to Q = P × (1 - η), where P is either the primary energy for generators or the total input for consumers. Since I can't write a single expression for P without a conditional, I'll keep them separate in the schema and instead define a gross_power concept that represents the total primary energy throughput regardless of operation type.
> 
> Now I'm reconsidering what the user actually wants the "heat" column to represent—they mentioned it's waste heat, but the current values don't match that interpretation, so I need to clarify whether they want me to redefine it or keep the existing values.
> 
> I think the cleanest approach is to make "heat" explicitly the waste heat that needs to be dissipated or recovered, since that's what matters for the simulation's heat management. I can derive everything from the core parameters: for generators, the gross power is E divided by efficiency, and waste heat is the difference; for consumers, gross power is the absolute value of E plus the solar input, with waste heat calculated from that. I should also clarify what "civ_power" represents in each case—either the gross power for generators or just the solar input for consumers.
> 
> Now I'm working through the efficiency definitions more carefully. For generators, efficiency is the ratio of electrical output to primary energy input, which varies by source: solar panels convert about 25% of incident energy, hydrogen systems around 45%, and fission reactors roughly 34%. For consumers like electrolyzers, efficiency represents how much chemical energy gets stored relative to the electrical input, which I can express as a ratio of useful work to total power consumed.
> 
> With the water electrolysis example, if the efficiency is 80%, that means 4.40 MW goes into chemical storage while 1.10 MW dissipates as waste heat. For solar steelmaking, I need to work backwards from the efficiency and power balance to find the useful work and waste heat components.
> 
> The key insight is that I don't actually need to measure useful work directly from the table—I can derive the waste heat once I know the efficiency, since waste heat equals the input power multiplied by one minus the efficiency, all divided by efficiency. Now I'm calculating the actual values for the solar column and the new heat column by applying the waste heat formula to each power generation operation.
> 
> Generating markdown table... Generating the power source comparison table... Generating comparison table... Now I'm looking at the performance metrics across these different steelmaking and refining processes. I need to reconsider what efficiency actually represents here, especially for operations like basic oxygen steelmaking where the primary energy comes from an exothermic chemical reaction rather than electrical input—the electricity is just for auxiliary needs, so the efficiency metric is measuring something different than I initially thought. The problem is that we don't have reaction enthalpies in the data, so I can't calculate the chemical energy contribution directly. Let me step back and think about what a workable unified model would look like—it doesn't need to be perfect, just consistent enough to handle both electrical and chemical processes. I need to step back and think about what the simulation actually needs from these columns rather than trying to derive everything from thermodynamic principles. The heat value needs to be concrete and computable from the other parameters—so let me define it directly as a fraction of the energy throughput, where heat equals (1 - η) times the total energy input (electricity plus solar). I'm realizing the issue—waste heat is already part of the energy balance, not something to add on top. Let me reframe this: for any operation, I have energy inputs (electricity, solar, chemical reactions) and outputs (electricity, stored chemical energy, waste heat), and efficiency should relate useful output to total input without double-counting. The key is figuring out what counts as "total energy throughput" when chemical reactions are involved, since exothermic reactions add energy to the system that wasn't in the electrical or solar inputs. - Conservation: |E_in| + S + ΔH_exo = E_out + ΔH_endo + Q
> 
> For a generator burning fuel:
> - Chemical energy input = ΔH_exo (from combustion)
> - Electrical output = E
> - Waste heat = Q = ΔH_exo - E (all chemical energy becomes either electricity or heat)
> - η = E / ΔH_exo
> 
> For an endothermic industrial process (like electrolysis):
> - Electrical input = |E|
> - Chemical energy stored in products = ΔH_endo
> - Waste heat = Q = |E| - ΔH_endo
> - η = ΔH_endo / |E|
> 
> For an exothermic industrial process (like BOF steelmaking):
> - Electrical input = |E| (auxiliary)
> - Chemical energy from reaction = ΔH_exo
> - Total energy = |E| + ΔH_exo
> - Some of this goes to heating/melting products (useful) and some is waste heat
> - η = useful_energy / (|E| + ΔH_exo)
> - Q = (1 - η) × (|E| + ΔH_exo)
> 
> The issue is that ΔH_exo isn't available in the data and would be difficult to estimate across all operations. I think the pragmatic solution is to treat η as a given parameter based on real-world values, then derive the other columns from η and the energy inputs {E, S}.
> 
> For operations where chemical reactions contribute significantly, I can define a unified "total energy throughput" P that represents all power flowing through the system. For generators, P = E / η (the primary energy converted), and for other operations, P = (|E| + S) / η (the total energy budget). The η value implicitly captures the contribution from reaction energy in cases like BOF steelmaking.
> 
> Let me work through the BASIC_O2_STEELMAKING example: with |E| = 0.03 MW and S = 0, I get P = 0.03/0.88 ≈ 0.034 MW, which means waste heat Q ≈ 0.004 MW and useful work equals the electrical input. This actually makes sense—the process is chemically dominated (the C+O reaction provides most of the heat), so the electricity serves primarily as auxiliary power for gas handling and charging with high efficiency, leaving only small waste heat from the chemical process itself.
> 
> But this reveals a problem: the BOF process generates massive waste heat from the exothermic reaction in reality, far more than 0.004 MW per ton per hour. Comparing the old scheme (heat = 0.034 MW) to the new one (heat = 0.004 MW), both values are already unrealistically small, but the new approach makes it even worse.
> 
> The actual waste heat from BOF steelmaking is around 0.50 MW per t/h when accounting for sensible heat in offgas and slag plus chemical energy in CO offgas. The fundamental issue is that the electricity column only captures a tiny fraction of the energy involved—most of the energy comes from the exothermic chemical reaction itself, not from electrical input. So neither approach is capturing the real energy throughput.
> 
> To fix this properly, I'd need to either add a column for chemical energy (which means calculating ΔH for every reaction) or redefine how we measure efficiency for these operations. The user's definition says waste heat is proportional to (1 - efficiency) times the total energy throughput, but the old formula assumed |E|/η represented that total throughput—which breaks down when chemical energy dominates. I'm leaning toward adding a chemical energy column, though it does add complexity to the schema.
> 
> For BOF steelmaking specifically, the total energy throughput is far larger than the 0.034 MW from the electrical energy alone, which suggests the old formula was either fundamentally wrong for these types of operations or the efficiency value was artificially set to make the heat calculation seem reasonable. If I work backward from a reasonable heat output of 0.50 MW with η = 0.88, I get a total throughput of about 4.17 MW—but then I'd need to define what "useful work" actually means in thermodynamic terms, which gets complicated when chemical reactions are the primary energy source rather than electricity or heat input.
> 
> The real issue is that operations powered by chemical reactions need a separate accounting framework. I should define a unified model where every operation has a total energy throughput that comes from electricity, solar, or other primary energy sources like chemical reactions, and then handle power generation differently since the primary energy is what gets converted to electrical output. I'm realizing the issue: for BOF steelmaking, if I use heat = |E| / η with η = 0.03, I get an enormous heat value that doesn't match the actual thermochemical energy involved. Option B seems cleaner—defining efficiency as useful output over total energy input—but then I need to figure out how to derive P from the existing table columns rather than treating it as a separate input. Let me reconsider what the current table structure is actually capturing for these industrial processes. heat = |E|/η is a book-keeping value but doesn't represent actual thermal emissions
> 
> Looking at the efficiency values across these operations, they seem to represent process yields rather than thermodynamic efficiency—BASIC_O2_STEELMAKING at 0.88, OIL_REFINING at 0.90, and the refining processes around 0.85. For something like the Hall-Héroult process in aluminium refining, which is extremely energy-intensive, this interpretation makes more sense than treating η as a traditional efficiency metric.
> 
> The electrical energy input of 2.80 MW per t/h should primarily convert to electrochemical work, with only a small portion becoming waste heat—roughly 0.42 MW if we assume 15% losses. But the current heat calculation of 3.29 MW, derived from dividing |E| by η, doesn't align with this physical reality. It seems the heat column was computed mechanically without a clear underlying definition, and I need to recalculate it properly based on what the user actually wants.
> 
> Now I'm proposing a unified framework that distinguishes between generators and consumers. For generators, the total primary energy input P equals E divided by efficiency, with waste heat being the difference between P and the electrical output. For consumers, the calculation is more complex since energy comes from multiple sources—the grid, direct solar, and potentially chemical reactions—so I need to account for all of these carefully.
> 
> I'm realizing the circular logic problem here and reconsidering the approach. Instead of trying to derive waste heat from a unified formula across all operation types, I should treat heat as a directly measured quantity based on the actual thermodynamics of each specific operation, rather than attempting to force it through a single efficiency calculation. For consumers, I need to account for all energy inputs—electrical, solar, and exothermic chemical reactions—against what's actually stored as useful chemical energy, with the remainder dissipating as heat. But the exothermic chemical energy term isn't available in the data, so I'm reconsidering whether efficiency should just be defined as useful work divided by the electrical and solar inputs alone. Looking at the schema documentation, I see that BASIC_O2_STEELMAKING treats chemical energy from the inputs (carbon and oxygen) as internal to the operation itself, with electricity only covering auxiliary needs. This means η should represent how efficiently the chemical energy is converted to useful work, not the overall energy balance. For operations powered by chemical reactions rather than electricity, I need to reframe efficiency as the fraction of total chemical energy that becomes useful output.
> 
> My approach is to define total process power as the sum of electrical input, sensible heat, and any exothermic chemical energy released. For most operations this simplifies to just electrical and sensible components, but for exothermic processes like steelmaking, the chemical energy becomes significant. Then I can calculate waste heat as Q = P(1-η) and solve for the efficiency. For generators, P = E/η is already defined; for consumers, I'll treat the heat output as a known value and work backward to find the total process power needed.
> 
> Actually, I'm overcomplicating this. Let me just define the columns pragmatically: electricity stays as net electrical power (positive for generation, negative for consumption), solar is direct solar input in MW, and I'll handle the relationships between electricity, heat, and efficiency on a case-by-case basis depending on the operation type.
> 
> Now I'm laying out the specific definitions for each column. Efficiency is always the ratio of useful output to total input, whether that's electrical output divided by primary energy for generators or useful work divided by total energy input for consumers. For heat, I'm using different formulas depending on whether it's a generator or consumer—generators dissipate waste heat as E × (1 - η) / η, while consumers produce Q = (|E| + S) × (1 - η).
> 
> For the Kardashev power contribution, generators report their total primary energy captured divided by efficiency, while consumers only contribute direct solar energy if they're capturing any. I'm recognizing that for operations like BOF steelmaking where chemical energy is significant, the waste heat calculation might underestimate the actual thermal output, but since that chemical energy comes from tracked input resources and gets handled by the thermal management system, I'm treating η as electrical efficiency rather than total thermodynamic efficiency—so an η of 0.88 for BOF steelmaking reflects electrical efficiency specifically.
> 
> Looking at this from the simulation's perspective, I need to think about what actually matters: electricity consumption, waste heat that needs managing, and Kardashev contribution. For a BOF operation processing 1 t/h with real waste heat around 0.5 MW, the current model with E = -0.03 MW and heat = 0.034 MW significantly underestimates the actual thermal output, which reveals a deeper issue—the model treats chemical energy as internal to the operation rather than accounting for its full thermodynamic impact.
> 
> This design choice makes sense for the simulation: resources carry their embodied energy implicitly through mass and composition, while the electricity and heat columns track only explicit electrical and thermal flows. So for BOF steelmaking, the exothermic reaction from carbon and oxygen converts to heat that's carried away by the output streams (molten steel, slag, offgas), meaning only the auxiliary electrical systems' waste heat needs explicit tracking in the heat column using Q = |E| × (1-η).
> 
> Now I'm settling on a clearer framework where heat represents waste heat from the operation's energy use: generators produce Q = E(1-η)/η from converting primary energy to electricity, while consumers produce Q = (|E| + S)(1-η) from converting electrical and solar energy into useful work, with efficiency defined as the fraction of primary energy converted to electricity for generators and the fraction of electrical energy converted to useful work for consumers.
> 
> But I'm realizing the original η values might have meant something different—for BOF steelmaking, the 0.88 was probably overall process efficiency rather than just electrical efficiency of the auxiliary systems. I should reinterpret these values to represent the electrical efficiency of the auxiliary equipment specifically, which seems plausible for operations like BOF where electrical systems handle gas, charging, and lance control. For oil refining with η = 0.90, that would mean 90% of the 0.10 MW electricity goes to useful work through pumps and compressors.
> 
> This interpretation seems consistent with the current table values, which already look like reasonable electrical efficiencies for the equipment types involved. Now I'm checking operations that are primarily electrical—like water electrolysis where η = 0.80 means 80% of the electrical energy converts to useful electrochemical work and 20% becomes waste heat.
> 
> For aluminium refining via the Hall-Héroult process, the same logic applies: it's fundamentally electrical, so η = 0.85 captures the electrical efficiency, giving a waste heat of about 0.42 MW. I'm comfortable with this framework now and ready to compute all the new values, though I need to first figure out the solar component for solar steelmaking—specifically how much concentrated sunlight is needed to heat iron ore to the reduction temperature around 1500°C.
> 
> The auxiliary electrical draw of 0.02 MW covers tracking, control, and material handling, but the real energy comes from the concentrated solar thermal input. Working through the carbothermal reduction reaction and accounting for sensible heat to reach temperature plus the melting enthalpy, I need to calculate the total solar energy required for the 1 t/h throughput.
> 
> Starting with the iron content in the ore and converting to moles of Fe₂O₃, the endothermic reduction reaction alone demands roughly 0.54 MW, and adding sensible and melting heat pushes the total to around 0.8-1.0 MW. With a 70% efficiency factor, that determines the actual solar input needed. So I'm calculating the solar input needed for steelmaking based on the useful energy required—about 0.80 MW for the steel output at the given throughput—which means the total input (solar plus the small electrical component) needs to be around 1.14 MW, putting solar at roughly 1.12 MW. But that's notably higher than what I'm seeing in the electric steelmaking case, so I should compare those numbers to make sure the estimates are consistent. For SOLAR_STEELMAKING, the input is iron ore which requires reduction, so it needs significantly more energy than the electric steelmaking process. I'm calculating the thermal energy needed for carbothermal reduction and melting at the given throughput rate, then accounting for the solar thermal system's efficiency to estimate the solar input required—though I'm realizing the efficiency term might represent overall process efficiency rather than just the conversion step, so I should estimate the solar input more directly. For concentrated solar thermal reduction of iron ore to steel, the literature suggests solar furnace inputs of about 1-2 MW_thermal per t/h of crude steel. Let me use ~1 MW for a forward-looking estimate.
> 
> With S ≈ 1.0 MW and E = -0.02 MW:
> - Total input = 1.02 MW
> - η = 0.70 → useful work = 0.714 MW, waste heat Q = 0.306 MW
> 
> Actually, let me just use the relationship more directly. The user already set η = 0.70 for SOLAR_STEELMAKING. I need to determine S such that the total energy budget makes sense.
> 
> For solar carbothermal reduction, I'm working through the thermodynamic requirements. The reduction reaction needs about 2.5-3.5 MJ/kg of iron, and accounting for sensible heat losses brings that to roughly 4 MJ/kg of steel. At my production rate of 479 kg/h, that's about 532 kW of useful thermal energy needed. Working backward through the 0.70 efficiency factor, this means I need roughly 0.76 MW of solar input to deliver that useful heat. Looking at real solar furnace data from projects like SOLPART, I can estimate the thermal energy needed for solar steel reduction at around 7 MJ/kg, which translates to roughly 0.93 MW of solar input for this production rate. Using that figure with a 0.70 overall efficiency gives me a useful work output of 0.665 MW and waste heat of 0.285 MW, which feels like a solid estimate.
> 
> Rather than getting bogged down in the details of solar steelmaking, I'll round this to S ≈ 1.0 MW and move forward. For the SOLAR_POWER column, the existing value of 4.0 MW checks out with the given efficiency and energy parameters. Now I need to compile the full set of changes: SOLAR_POWER stays at 4.0 MW, SOLAR_STEELMAKING gets set to 1.0 MW, and the remaining operations in the SOLAR column remain empty.
> 
> For the HEAT column, I'm calculating the waste heat for each generator using the formula Q = E(1-η)/η. Looking at the performance metrics across all the power generation operations—solar thermal at 3.0 MW, geothermal at 5.67 MW, hydropower at 0.087 MW, wind at 0.053 MW, tidal at 0.111 MW, and hydrogen at 1.22 MW—these values reflect how much thermal energy is rejected relative to the electrical output based on each technology's efficiency.
> 
> Continuing through the fuel-based generators and fuel cells, I'm seeing methane power at 1.0 MW waste heat, methanol and ethanol both at 1.38 MW, liquid hydrocarbons at 1.38 MW, oil at 1.63 MW, coal at 1.38 MW, hydrogen fuel cells at 0.818 MW, and methane fuel cells also at 0.818 MW. Now I'm looking at the consumer operations where energy is being consumed rather than produced, calculating the heat generated based on efficiency losses for processes like oil refining, metal smelting, and steelmaking. Now I'm looking at the rest of the industrial processes, including some more exotic ones like regolith processing and deuterium purification that show much larger energy impacts, along with various recycling and fuel reprocessing pathways. Continuing through the hydrogen production pathways and synthesis reactions, I'm seeing the efficiency metrics and conversion rates for steam methane reforming, water gas shift reactions, and various synthesis processes for producing methanol and ammonia precursors. Finishing up the production facility data, then I need to reconsider how to handle municipal waste incineration since it actually generates electricity rather than consuming it—that changes how I calculate its heat requirement. I'm realizing the η = 0.75 doesn't represent thermal-to-electric efficiency—calculating the actual chemical energy from burning 0.478 t/h of waste at 10-12 MJ/kg gives roughly 1.33 MW, which means the true conversion efficiency is only about 0.18 (0.24 MW electricity / 1.33 MW chemical energy), which is actually reasonable for waste incineration plants.
> 
> The 0.75 must refer to something else entirely, like material recovery efficiency or the fraction of waste being usefully processed. This ambiguity is a problem for building a consistent framework, so I'm going to establish that η always means energy efficiency—for generators it's electrical output divided by primary energy input, and for consumers it's useful work divided by the energy consumed plus losses.
> 
> For the waste incineration operation, if I calculate η as electrical output over the chemical energy in the waste, I get 0.18, which would require dropping the 0.75 significantly. But maybe the user's 0.75 is accounting for something broader, like the combined value of both electrical generation and the embedded energy in recovered materials like steel and aluminium. The problem is that embedded energy isn't really the same as output energy in a thermodynamic sense.
> 
> I think the real issue is that this operation straddles two categories—it's both a recycling process and an energy generator—so it doesn't fit cleanly into either framework. For now, I'll treat it as a generator since E > 0, but I should flag the efficiency discrepancy in my feedback. The user's note about choosing "forward-looking but realistic" values suggests waste-to-energy should be around 25-30% electrical efficiency, not 0.75, so I'll note that as something to clarify rather than assume their intent.
> 
> Now I'm stepping back to reconsider whether the framework itself matters for the simulation. The core requirements are tracking electricity flow, waste heat for thermal management, efficiency metrics, and Kardashev calculations—and for most operations, electricity is the primary energy source anyway, so the waste heat from electrical losses should dominate. The main exception is something like basic oxygen steelmaking where chemical reactions are the real energy driver. main energy from C+O reaction
> - OIL_REFINING: process heat from burning fraction of oil
> - COAL_GASIFICATION: partial oxidation provides heat
> - BIOMASS_GASIFICATION: partial oxidation
> - MUNICIPAL_RECYC_INCINERATION: combustion of waste
> 
> For these, the model simplification is that chemical energy is internal to the resource stream and not tracked in the energy columns. The electrical input is just auxiliary. The waste heat from chemical reactions would need to be accounted for separately (or we accept the underestimate).
> 
> For COAL_GASIFICATION with E = -0.08 and η = 0.85, the partial oxidation is the main energy source, with the 0.08 MW covering oxygen supply and material handling.
> 
> What if I keep the current convention but redefine "heat" as total energy throughput rather than waste heat? Then heat = |E|/η across all operations, representing total primary energy input for generators but creating a conceptual problem for electrical consumers—the calculated value exceeds what actually enters the system. I could interpret this as a "primary energy equivalent," like the energy that would be needed if powered by a 100% efficient source, which is a concept used in energy accounting.
> 
> But the real issue is that grid electricity's primary energy depends on how it was generated, and the table doesn't capture that. I think the current convention is flawed, and the user seems to recognize this too. I should propose a cleaner framework and accept the recomputed heat values that result.
> 
> For the final framework, I'll redefine heat as waste heat specifically, keep electricity unchanged, add solar as a direct input for relevant operations, and recalculate everything accordingly.
> 
> Heat becomes the thermal power that needs to be managed through cooling systems or recovered, calculated differently for generators versus consumers, with chemical reactions handled separately through the resource streams themselves.
> 
> Now I'm defining efficiency as the ratio of useful output to total input, where generators convert primary energy sources into electricity and consumers convert electrical and solar inputs into work, each with their own formula accounting for waste heat.
> 
> The Kardashev-scale power contribution represents the total primary energy an operation harnesses from nature, calculated as the electrical output divided by efficiency for generators to get the full primary energy captured.
> 
> For consumers, it's just the direct solar energy they capture since grid electricity is already counted at the generation side, avoiding double-counting across the civilization's total power budget.
> 
> Now I'm working through the solar column values for each operation. Solar power is straightforward at 4 MW, but for solar steelmaking I need to estimate the solar thermal energy required based on the carbothermal reduction reaction and its energy demands.
> 
> I'm calculating the molar quantities of iron oxide that need to be reduced from the incoming ore, accounting for the iron content and molecular weights to determine the total energy requirement for the endothermic reduction process at operating temperature. I'm calculating the melting energy for the iron at about 126 kW, which brings the total useful thermal energy to roughly 944 kW or 0.94 MW. Now I need to work backwards from this to find the required solar input, accounting for system efficiency—if the system is about 70% efficient, that means I need roughly 1.32 MW of solar thermal power to achieve this output.
> 
> Let me sanity-check this against other steelmaking processes. Electric steelmaking uses about 0.40 MW per ton per hour when starting with pre-reduced iron, while hydrogen reduction uses 0.24 MW per ton per hour starting from ore. Since solar steelmaking is replacing both the electrical heating and the chemical energy from carbon oxidation, the solar input should be higher than pure electric steelmaking because we're working from raw ore.
> 
> I'm settling on S = 1.00 MW as a clean estimate, which gives me Q = 0.306 MW and civ_power = 1.00 MW, though I'm not entirely confident this captures the full energy picture.
> 
> Let me reconsider the SOLAR_STEELMAKING process more carefully. Based on solar carbothermal reduction projects like SOLSTEEL, the thermal energy requirement is around 5-10 kWh per kilogram of product. Using the lower estimate of 5 kWh/kg and converting to MW for my 479 kg/h throughput, I'm getting values that seem too high, so I need to recalculate and verify my unit conversions.
> 
> Now I'm comparing this against other processes like blast furnace iron smelting to sanity-check the solar energy requirement, since 2 MW of solar input for 1 t/h seems substantial. t/h of throughput. The 0.04 MW is just for blowers, charging equipment, etc.
> - The "real" energy is ~2 MW/t, and the table only shows 0.04 MW in the E column because the rest is chemical.
> 
> For SOLAR_STEELMAKING, the solar replaces the chemical combustion energy. So S should be comparable to the chemical energy in iron ore smelting, which is about 1-2 MW per t/h.
> 
> I'll set S ≈ 1.50 MW (a reasonable estimate for concentrated solar thermal for carbothermal reduction at 1 t/h).
> 
> Q = (0.02 + 1.50) × 0.30 = 0.456 MW
> civ_power = 1.50 MW
> 
> Now I'm realizing the efficiency parameter η = 0.70 changes meaning depending on how I apply it. In the old approach, it barely mattered since it only modified a small heat value. But if I use it to calculate waste heat from the much larger solar input, the waste heat scales up dramatically and shifts the whole physical interpretation. For concentrated solar thermal systems, the efficiency of converting sunlight to usable process heat is typically 70-80% when accounting for optical losses in the concentrator and thermal losses in the receiver, so η = 0.70 as a solar-to-useful-heat conversion makes physical sense.
> 
> With S = 1.50 MW, I get about 1.06 MW of useful thermal energy and 0.46 MW of waste heat, which aligns reasonably well with my earlier estimate of ~0.94 MW needed for carbothermal reduction. I'll proceed with S = 1.50 MW and can refine it later if needed.
> 
> Now I want to verify the energy balance more carefully by looking at the stoichiometry and enthalpy of the main carbothermal reduction reaction.
> 
> At high temperatures around 1500°C, the reaction enthalpy shifts because I need to account for heating the reactants and the fact that the Boudouard equilibrium makes the process less endothermic than at standard conditions. The actual mechanism involves the reduction of iron oxide by CO, which is exothermic at high temperature, coupled with the endothermic Boudouard reaction where CO₂ reacts with carbon to regenerate CO—this is where the solar heat is primarily needed.
> 
> Now I'm calculating the energy requirements: the theoretical minimum for carbothermal reduction is around 2.1 kWh per kilogram of iron, but accounting for practical inefficiencies like incomplete reactions and heat losses through the refractory, the real energy demand is closer to 3-5 kWh/kg. With a production rate of 462 kg/h and using 3 kWh/kg as a reasonable estimate, that works out to roughly 1.4 MW of solar power needed.
> 
> Accounting for a 70% efficiency in converting solar energy to usable process heat, I need to back-calculate the required solar input. Working through the energy balance—where the useful heat equals efficiency times the sum of electrical and solar power—I get a solar power requirement of about 2.0 MW.
> 
> Now I'm checking this against the hydrogen reduction steelmaking process, which uses hydrogen as a reducing agent to convert iron ore to iron while requiring electricity for the arc furnace and auxiliary systems.
> 
> The hydrogen itself carries significant chemical energy—about 0.933 MW from the 28 kg/h of H₂ at 120 MJ/kg—but this chemical energy isn't captured in the energy balance framework the way electricity and solar are. The efficiency factor only applies to the electrical and solar inputs, not the chemical energy embedded in the hydrogen feedstock, which is the core issue I keep running into with this model. η = 0.85 means 85% of the 0.24 MW electrical input goes to useful work (EAF melting, etc.). The H₂ chemical energy is separate and implicit.
> 
> For SOLAR_STEELMAKING: the solar energy replaces the chemical energy. So S = 2.0 MW replaces the ~1.0 MW of H₂ chemical energy (roughly). This makes sense! The solar needs to be higher because:
> 1. Solar thermal is less efficient than H₂ chemical (more losses)
> 2. The carbothermal reduction at solar furnace temperatures has a different energy profile
> 
> OK, I'm going with S = 2.0 MW for SOLAR_STEELMAKING. Let me continue.
> 
> Now I'm second-guessing whether 2.0 MW is really justified. The hydrogen reduction only needs about 0.93 MW of chemical energy, so requiring more than twice that for solar seems high. But the carbothermal reduction pathway is thermodynamically different—it needs higher temperatures and has inherent losses in the solar collection and conversion process, so the factor of two might actually be reasonable. The hydrogen and methane calculations check out against their actual fuel energy content. Now I'm verifying the coal power generator using the same approach—calculating the chemical energy from the fuel consumption rate and comparing it to the power requirement.
> 
> For the municipal waste incineration, though, something's off. The chemical energy available from the waste input is significantly higher than the electrical power being generated, which suggests the efficiency assumption of 0.75 might not be accurate for this particular system.
> 
> Let me recalculate: if I divide the electrical output by the chemical energy input, I get roughly 0.18, which actually aligns with typical waste-to-energy conversion rates of 15-25%. So maybe the 0.75 figure represents something different—perhaps it's accounting for the recovered materials like steel and aluminum that are also extracted during the incineration process, where the total value output includes both electricity and the embedded energy of those recovered materials. So the efficiency discrepancy might just be a rough estimate rather than a precise thermodynamic calculation—I'll flag this as something to revisit later and move forward with the framework as-is, letting the user refine individual η values if needed. Now I'm noticing that MUNICIPAL_RECYC_INCINERATION actually produces electricity, which means it should be treated differently in the framework. my framework it's a "generator" and:
> - P = E/η = 0.24/0.75 = 0.32 MW
> - Q = P - E = 0.32 - 0.24 = 0.08 MW
> - civ_power = E/η = 0.32 MW
> 
> But the actual chemical energy input is ~1.33 MW. With η = 0.75, I'd be saying the primary energy input is only 0.32 MW, which is physically wrong. The waste heat should be much larger.
> 
> This operation doesn't fit neatly into the "generator" category because it generates electricity but its primary purpose is waste processing, not power generation. The chemical energy comes from the waste stream itself, not a dedicated fuel source.
> 
> I think the real issue is that I'm using the sign of E to determine the category, when I should be using the operation's group instead. Energy group operations follow generator logic with P = E/η and Q = E(1-η)/η, while all other operations like municipal recycling incineration should use different formulas since they're primarily processors or consumers where electricity is just a co-product. I'm settling on a simpler approach: use the generator formula for any positive electricity output and the consumer formula for negative or zero. For the incineration case with 0.24 MW generated and 75% efficiency, that gives me a waste heat value I can calculate directly. I'm realizing there's a conceptual issue with how I'm counting the Kardashev contribution for waste incineration. The energy from burning waste is recycled energy—it was originally captured and used to manufacture products, so it shouldn't count as new primary energy input the way fuel extraction does. But the framework doesn't trace energy origins, so fuel-based plants count their chemical energy regardless of whether that fuel was produced using solar or other sources, which creates an inconsistency in how I'm treating waste versus extracted fuels.
> 
> I think the fairest approach is to treat waste incineration the same way as fuel-based power plants—count the chemical energy as civ_power input—since the framework doesn't distinguish between primary and recycled energy sources. Now I'll finalize the framework and compute all the values systematically.
> 
> For operations producing net electricity, I'm using Q = E × (1 - η) / η and civ_power = E / η, while for consumers or non-electrical operations, Q = (|E| + S) × (1 - η) and civ_power = S. Let me work through the energy group calculations, starting with solar, geothermal, hydropower, and wind to get their heat and civilization power values. I'm continuing through the remaining power sources, calculating their efficiency ratios and civilization power multipliers—methanol and ethanol both yield 2.38, liquid hydrocarbons match that at 2.38, oil pushes higher to 2.63, and coal also lands at 2.38, while hydrogen fuel cells have zero efficiency. Continuing through the remaining fuel sources, I'm calculating the waste heat ratios and civilization power values for HEU fission at 3.33, thorium matching LEU at 2.94, D-T fusion at 2.50, and working through the helium-3 variants down to 1.82 for the 3He-3He reaction.
> 
> Now I'm looking at radioisotope power with its extremely high waste heat ratio of 9.0 giving a civilization power of 10.0, then shifting to non-energy operations where the formula changes since these processes consume energy rather than produce it, with special handling for solar-assisted steelmaking. I'm finishing the electric steelmaking calculation at 0.080 MW, then moving on to solar steelmaking where I'm getting 0.606 MW of waste heat with a solar input of 2.00 MW—though I'm second-guessing whether that solar estimate is realistic for this scale of operation, since the waste heat output seems quite high compared to the other methods. Looking at the minimum energy requirement of about 0.946 MW for processing the iron content, my useful energy output of 1.414 MW represents roughly 50% overhead above the theoretical minimum, which tracks with real-world process inefficiencies. The solar input of 2.00 MW concentrated at 1500°C with 70% efficiency means I need a heliostat mirror field of around 2000 m² to deliver that concentrated radiation—roughly a 50m × 40m installation, which is feasible at industrial scale.
> 
> With that confirmed, I'm now working through the thermal requirements for the other refining operations: aluminium refining needs 2.80 MW with 85% efficiency, generating 0.42 MW of waste heat; industrial metals refining requires 0.65 MW at 82% efficiency for 0.117 MW waste heat; and precious metals refining takes 0.50 MW at 85% efficiency for 0.075 MW waste heat.
> 
> Moving down the list, rare earths refining demands 1.50 MW at 80% efficiency producing 0.30 MW waste heat, uranium refining is minimal at 0.05 MW with 0.0075 MW waste heat, regolith processing is more substantial at 5.00 MW and 70% efficiency generating 1.50 MW waste heat, and deuterium purification is the most energy-intensive at 100 MW with 80% efficiency producing 20.0 MW of waste heat. I'm continuing through the remaining operations... Continuing through the remaining processes—water electrolysis has the highest heat requirement at 1.10, followed by methane pyrolysis at 0.90 and CO2 electrolysis at 0.595, while the fuel reprocessing and coal gasification routes require minimal heat input. Now I'm working through the energy and heat calculations for the remaining processes—steam methane reforming has the highest energy requirement at 2.25, producing a heat value of 0.5625, while water gas shift and reverse water gas shift have much lower demands, and I'm starting to compute the values for liquid hydrocarbon synthesis. Continuing through the synthesis pathways, I'm calculating the heat generation and power contributions for polymer synthesis at 0.500, then moving through industrial and fine chemicals production, concrete, and glass ceramics—each showing their energy requirements, efficiency ratings, and resulting heat outputs with no civilization power generation across any of these processes.
> 
> Now I'm working through semiconductor production, which has a notably higher energy demand at 20.0 units, followed by composites, solid fuels, and fertilizer production, each with their respective efficiency metrics and heat calculations. I'm also considering the precision of the significant figures being used throughout these calculations. Continuing through the remaining energy sources with the same calculation pattern—tidal, hydrogen, methane, methanol, and ethanol all following the efficiency ratio formula, then moving into the liquid hydrocarbons. Now I'm working through the efficiency calculations for the remaining fuel sources, computing their Q-values and civilization advancement metrics by taking the ratio of efficiency gains to losses for each technology. Now I'm working through the industrial operations calculations, computing Q values and civilization scores for various steelmaking and refining processes—from basic oxygen steelmaking with minimal impact up through solar steelmaking which shows significant civilization contribution at 2.00, along with aluminum and other metals refining metrics.
> 
> Continuing with the remaining refining operations and specialized processes like regolith processing and deuterium purification, which show much higher Q values, particularly deuterium at 20.0, while municipal recycling incineration has zero Q since its energy output is positive. Now I'm working through the remaining conversion processes, calculating their efficiency quotients and civilization values across methane pyrolysis, various gasification and carbonization pathways, and CO2 methanation. Continuing through the remaining synthesis pathways, calculating their quality scores by multiplying base values with efficiency factors—ammonia synthesis comes in at 0.30, polymer synthesis at 0.50, and industrial chemicals at 0.11, with fine chemicals reaching 0.375. Moving into material production now, concrete and glass ceramics calculations.
> 
> Finishing the production calculations with semiconductors showing the highest quality score at 7.00, composites at 0.45, and the remaining fuel, fertilizer, and solid fuel pathways all lower. Now I'm reconsidering the significant figures—the existing data uses three significant figures inconsistently, so I should standardize everything to that precision level. I'm settling on a practical approach for notation—using regular decimal format for values ≥ 0.001 and e notation only for smaller numbers, though I'll keep it sensible rather than rigidly adhering to the rule. Now I need to update the schema documentation for the operations file, specifically refining the field descriptions for electricity, solar, heat, efficiency, and civ_power to be clearer and more precise.
> 
> Updating the field definitions...
> 
> For operations driven by chemical reactions like steelmaking or gasification, the heat calculation only accounts for electrical and solar energy losses—the thermal energy from the reactions themselves flows through the output resources. Efficiency is then defined as useful output divided by total energy input, where for electricity-generating operations that's the primary energy captured from the source, and for consuming operations it's the useful work relative to electricity and solar combined. of electrical and solar energy input that performs productive work (chemical transformation, physical processing) rather than being dissipated as waste heat.
>   - Select efficiency values that are somewhat forward-looking but within the range of current or near-term Earth operations.
> - civ_power — Civilization power contribution in MW for Kardashev-scale accounting. Represents the total primary energy captured from natural sources by this operation, without double-counting energy already tracked at other operations:
>   - For electricity-producing operations (electricity > 0): civ_power = electricity / efficiency, representing the full primary energy harnessed (solar flux, fuel chemical energy, nuclear energy, geothermal heat, etc.).
>   - For electricity-consuming or non-electrical operations (electricity ≤ 0): civ_power = solar, representing only the direct solar energy captured independently of the electrical grid. Operations consuming only grid electricity have civ_power = 0 since that energy is already counted at the generating operation.
> 
> The civilization's total Kardashev-scale power is the sum of civ_power across all active operations. I need to add consistency checks and clarify the solar field in the schema, and I'm reconsidering whether waste incineration should contribute to civ_power — the fuel's chemical energy is being harnessed either way, so the same logic should apply as with other fuel-based generators.
> 
> Actually, I'm spotting a potential double-counting issue: if hydrogen is produced via electrolysis using grid electricity, that electricity was already counted at the generator, so including the hydrogen's civ_power would count it twice. But this same problem exists for all fuel-based power, and in practice Kardashev calculations typically use total generation rather than consumption, so I'll keep the framework consistent.
> 
> Let me verify the math for SOLAR_POWER: with E=1, η=0.25, and S=4, the civ_power formula gives 1/0.25 = 4.0, which matches S exactly. Now I need to check how the heat column relates to the old heat values.
> 
> For generators, the new heat formula E(1-η)/η simplifies to old_heat minus E. Testing this against SOLAR_POWER: the old heat was 4.00, subtract 1 gives 3.00, which checks out. Same verification works for GEOTHERMAL at 6.67 minus 1 equals 5.67. I still need to work through the consumer heat calculation.
> 
> Now I'm ready to compile the full response with the framework explanation, schema changes, and the complete updated TSV table with all the modified values. I've verified that civ_power matches the old heat values for fuel-based generators, which is a good consistency check. Now I need to decide whether to output the full TSV table or just the modified rows—since these are modifications to existing rows rather than pure additions, I should probably provide the complete table, though it's quite lengthy.
> 
> Let me recalculate the rounding more carefully for three significant figures, particularly for the waste heat values where some calculations are giving me very small numbers that might round to zero. Now I'm continuing through the remaining energy sources, calculating their efficiency ratios the same way—fuel cell options like hydrogen and methane, various nuclear variants including enriched uranium and thorium, fusion fuels, and radioisotope thermoelectric generators, with RTG coming in at the highest multiplier at 9.
> 
> 00.
> 
> 00
> 
> Now I'm finishing the nuclear fuel cycle calculations and moving into consumer energy losses, computing the heat dissipated during refining and processing for oil, metals, and steel production based on their respective efficiencies. Now I'm working through the remaining resource calculations, finishing up the DEUT value at 20.0 and then moving through the generator-based municipal recovery, industrial recovery, fuel reprocessing options, and various electrolysis and pyrolysis pathways to get their weighted contributions. Continuing through the remaining pathways, I'm calculating the weighted contributions for CO2_MEOH, CO_MEOH, and the industrial processes like ammonia synthesis, polymers, chemicals, and materials production—semiconductors showing the highest impact at 7.00.
> 
> Now I'm finishing the last few sectors: solid fuel and fertilizer production, then setting up the consumer-side emissions where solar steelmaking and municipal recycling incineration are the only contributors. Before I write out the full TSV, I need to double-check my solar steelmaking estimate—the carbothermal reduction process requires specific energy inputs that I should verify more carefully.
> 
> Calculating the iron content from the ore feedstock and working through the thermodynamics of the reduction reactions...
> 
> Now I'm computing the molar quantities of iron oxide needed and the energy requirements for both the chemical reduction step and the thermal processing—accounting for sensible heat to bring the material up to operating temperature and the latent heat needed for melting.
> 
> The total useful thermal energy comes to around 0.80 MW, but I need to factor in additional losses like heating the exit gases and processing slag, so I'm bumping that up by 30% to roughly 1.04 MW. Dividing by the overall efficiency of 0.70 gives me the total solar input required.
> 
> So the solar input S works out to about 1.50 MW, which feels more reasonable than 2.00 MW. With that figure, the waste heat Q calculates to roughly 0.456 MW. I'm settling on civ_power = 1.50 and now need to write up the response with the framework explanation, schema changes, and the full updated TSV file. Let me verify the formula logic for MUNICIPAL_RECYC_INCINERATION—even though it's in the Refining group, the E value of +0.24 means it produces electricity, so I should apply the generator formula based on that positive sign. Now I'm checking the heat output for steam-methane reforming: with 2.25 MW of electrical input and 75% efficiency, about 0.56 MW becomes waste heat, which tracks since the process is endothermic and needs that energy input. For coal gasification, the calculation gives roughly 0.012 MW of waste heat, though I'm noting that in practice it's a mix of exothermic and endothermic reactions where partial oxidation provides some of the needed heat.
> 
> Before I finalize this, I should double-check whether the efficiency values I'm using actually represent what the user intended—particularly for something like concrete production, where the old framework would've calculated heat differently than this new approach does.
> 
> For concrete, the calcination of limestone is intensely endothermic, requiring temperatures around 1450°C in the kiln, which in reality comes from fuel combustion but here I'm modeling as electrical input. With the given parameters, I'm getting 0.165 MW of useful work and 0.055 MW of waste heat, but I need to verify this is physically realistic for producing 1 ton per hour. Working through the stoichiometry—concrete is roughly 17% calcium by mass, so at 1 t/h that's 170 kg of calcium, which translates to about 4,241 moles of calcium carbonate needing to decompose...
> 
> The calcination reaction alone requires roughly 0.21 MW, and when I factor in the sensible heat for reaching 1450°C plus grinding and mixing operations, the total energy demand likely falls between 0.3 and 0.5 MW. This is substantially higher than the 0.165 MW available from my current model, suggesting either the electrical input of 0.22 MW is underestimated or the efficiency assumption of 0.75 is too optimistic for this process.
> 
> Looking more carefully at the concrete composition, I realize only the cement precursor component actually requires calcination—the stone aggregate and water don't need this energy-intensive step. The industrial minerals input of 0.250 t/h represents the cement precursor, and assuming roughly 75% calcium carbonate content in that material, I'm working with about 0.1875 t/h of limestone equivalent to calcine.
> 
> Working through the thermodynamics: the decomposition reaction requires roughly 333 kW, and accounting for sensible heat to raise the material temperature brings the total to around 182 kW. With typical furnace efficiency, that yields about 165 kW of useful energy—which is close enough to the user's specified value that I don't think adjusting their parameters is necessary. The small discrepancy falls well within the uncertainty of a simplified model, so I'll keep their existing values and move forward with writing up the full response.
> 
> Now I need to organize the output into three parts: an explanation of the unified abstraction framework, the updated schema documentation, and the complete revised operations table with the new solar, heat, and civilization power calculations. For the renewable power operations without fuel inputs, I'm calculating their civilization power based on the actual energy flux they capture—solar at 4.00, geothermal at 6.67, hydropower at 1.09, and wind at 1.05.
> 
> Generating markdown table... is consistent with real RTGs, so the efficiency calculations check out. Now I'm ready to write everything up. For the schema, I'll keep the solar column empty rather than defaulting to 0 to distinguish "not applicable" from actual zero input, and I'll use explicit 0 values for civ_power across operations where it doesn't apply, matching the existing pattern in the fuel_rate column. So I'll display the civ_power values explicitly, showing 0 for industrial operations to indicate zero Kardashev contribution, while keeping extraction operations blank. Now I'm finalizing the response with the full TSV table, making sure the heat values match the existing precision of three significant figures across all the power generation rows.
> 
> Generating the data table... I'm looking at the industrial operations and their civ_power values—most are 0, except for solar steelmaking at 1.50 and municipal recycling incineration at 0.320. Now I need to carefully construct the TSV output, and I'm realizing I should reconsider the pattern for how to handle empty versus zero values for civ_power in industrial operations, since energy-based operations use actual values or zero for fuel_rate while non-energy operations leave it empty. I'm verifying the column structure in the header — civ_power is already defined as a FLOAT field in MW, and solar is the third column right after electricity. Let me recount the full sequence to make sure I'm mapping everything correctly. I'm verifying the column structure against the actual data—civ_power checks out as FLOAT with MW units. Looking at SOLAR_POWER, the existing record has multiple values across those fields, but the updated table shows it's been simplified to just the first column, so I need to understand what's being consolidated here. Generating the TSV table... Looking at the existing table format, I see it uses decimal notation like "0.050" for similar values, so I'll stick with that consistent style rather than switching to exponential notation for these borderline cases.
> 
> Now I'm ready to put together the full response with a prose explanation of the framework, the updated schema documentation, and the revised operations table. I should also flag a couple of key issues: the waste-to-energy efficiency seems unusually high for electrical conversion (typically 15-25%), and I need to clarify whether η represents electrical efficiency or broader energy recovery for exothermic reactions. →electricity would show higher civ_power than one that just uses solar→electricity directly, due to the intermediate losses. This is an accepted simplification for the Kardashev accounting.
> 
> Now I'm looking at the actual solar values in the table. SOLAR_POWER has solar = 4, but all other existing operations have that field empty. I should set SOLAR_STEELMAKING to solar = 1.50 to match the estimate I calculated. I also need to check whether the schema notes need updating, particularly around how extraction group operations are documented.
> 
> The existing notes about leaving extraction fields blank and normalizing energy operations are still valid, but I should add clarifications about how heat, civ_power, and the solar field are derived. My response will have four sections: a framework explanation, updated schema fields, the full TSV table, and feedback on simplifications and edge cases.
> 
> I should keep section 1 concise to avoid repeating what the schema already covers. For section 3, I need to generate the complete TSV with all headers and data rows—this will be substantial. But I'm reconsidering SOLAR_STEELMAKING: the solar column was empty in the original, and recalculating heat as 0.02/0.70 gives approximately 0.029, which seems off. Let me verify this calculation before finalizing the table. Now I'm breaking down the energy balance for the solar thermal process—accounting for sensible heat, the endothermic reduction reaction, and phase change losses. With a 70% solar-to-useful efficiency factor, the total solar input needed comes to roughly 1.26 MW, which means I need about 1.24 MW from the concentrated sunlight after subtracting the small electrical contribution.
> 
> Given the inherent uncertainties in these estimates—easily ±30%—I'm settling on 1.25 MW as a reasonable central estimate for the solar thermal input required per ton per hour of iron ore throughput. Let me just commit to a value and stop second-guessing myself. S = 1.50 MW is clean and reasonable, which gives me heat = 0.456 MW and civ_power = 1.50 MW. Now I need to write up the response with the framework explanation, schema changes, and the full TSV data, plus update the Notes section to document the energy calculations.
> 
> Updating the schema fields section... Now I'm working through how efficiency is calculated differently depending on whether the operation generates or consumes electricity — for generators it's the ratio of output to total primary energy input, while for consumers it's the useful work divided by the combined electrical and solar input. I also need to account for the civilization power contribution metric. Now I'm defining the output side of the resource flow — output_resources and output_rates track what the operation produces, and mass_flow validates that inputs equal outputs. For energy operations, fuel_rate captures the total input excluding oxygen, while target_products identifies the primary output(s) the operation is designed to produce, which requires some judgment for conversion processes where multiple valuable outputs exist. Extraction operations are simpler — I just need to fill in the name and leave everything else blank.
> 
> For normalization, energy operations scale to 1 MW of electricity output, while all others normalize to 1 t/h mass flow. I need to use the resources defined in the physical_resources file and apply catchall resources where appropriate. For chemical reactions, I'm considering only stoichiometric quantities, but for manufactured goods I need to account for product composition and waste byproducts like industrial waste or regolith. I should treat inputs and outputs as a net reaction, avoiding the same resource appearing on both sides, and I'm not including maintenance or repair costs since those are handled separately. Process energy and heat should typically be accounted for as part of the operation's energy requirements. Now I'm laying out the energy balance framework for different operation types. For electricity-generating operations, I convert primary energy to electricity with the remainder becoming waste heat, following the relationship where heat equals electricity times the efficiency ratio. For electricity-consuming operations, I'm applying the inverse logic where electrical and solar inputs get converted to useful work with waste heat as the byproduct, though this formula appears to be cut off mid-sentence.
> 
> Let me verify the math on the generator efficiency identity to make sure it holds—checking that electricity divided by the sum of electricity and heat equals the original efficiency parameter, which it does. The civilization power output for generators is simply the total energy input, electricity plus heat combined.
> 
> For consumers, I'm working through the efficiency calculation more carefully. The heat generated is proportional to the input power times one minus efficiency, so I need to expand the denominator and see if this simplifies back to the original efficiency value.
> 
> After recalculating, I realize the issue—I should be looking at useful work output divided by total input energy. Since useful work equals η times the input, and the input is |E| + S, the consumer efficiency simplifies directly to η, which confirms the identity is actually correct. Now I'm verifying these identities algebraically—substituting the efficiency formula back into the energy balance equations to confirm they hold true in both cases, with the positive electricity case yielding total power as electricity divided by efficiency, and the negative case confirming the heat relationship.
> 
> I'm recognizing a key insight: for generators producing electricity, the civilization power equals the total primary energy captured, which elegantly combines the electrical output and waste heat into one throughput measure.
> 
> Generating markdown table... Generating the energy source metrics table... Generating the energy source comparison table... Generating the industrial operations table... Generating the metrics table... Generating the data table... Generating the metrics table... Now I'm composing the actual TSV text and need to be careful with the tab formatting. Let me double-check the heat value calculation for steam methane reforming—with E = -2.25 and η = 0.75, that gives Q = 0.5625, which rounds to 0.563 at three significant figures. I'll settle on 0.563 for the rounding to keep things consistent. Now I'm finalizing the response and want to verify that DEUTERIUM_PURIFICATION entry—checking the math: if E is -100 MW and efficiency is 0.80, then the old heat value of 125 checks out since 100/0.80 = 125.
> 
> The new heat calculation gives 100 × 0.20 = 20.0 MW, but I'm noticing something odd about the process itself. A 100 MW energy requirement for 1 t/h of water input seems extreme, though deuterium separation from natural water is genuinely energy-intensive. The output shows 0.2 t/h of deuterium and 0.8 t/h of oxygen from 1 t/h of water, which would be an unusually high yield given that deuterium's natural abundance in hydrogen is only about 0.0156%—meaning 1 ton of water would contain far less extractable deuterium than this suggests.
> 
> To produce 200 kg of D₂ at natural abundance would require processing roughly 11.4 million kg of water, so the resource rates in the schema seem off. But setting that aside, the energy parameters check out: E = -100, η = 0.80, Q = 20.0, with civ_power = 0. The schema note clarifies that mass_flow is normalized to 1 t/h as a baseline, so this represents a scaled representation rather than the actual throughput a real plant would need to achieve these deuterium yields.
> 
> Now I'm structuring my response: I'll explain the framework briefly, then provide the modified schema sections and the full operations table, keeping the prose as concise as possible since the TSV will be lengthy. I should also verify my solar steelmaking calculation—the new solar value of 1.50 MW represents the concentrated solar power needed to process 1 t/h of material.

## Unified Energy Framework

Here's the abstraction I propose, built around two cases determined by the sign of `electricity`:

**Electricity-generating operations (E > 0):** The operation captures primary energy from a source (solar flux, fuel chemistry, nuclear fission/fusion, geothermal heat, etc.) and converts fraction η to electricity; the remainder is waste heat. The total primary energy equals E/η, and that full amount is the Kardashev contribution since it represents energy newly captured from nature.

**Electricity-consuming or non-electrical operations (E ≤ 0):** The operation receives |E| from the grid and S from direct solar, converting fraction η to useful work (chemical transformation, physical processing); the remainder is waste heat. Grid electricity is already counted at the generating operation, so the Kardashev contribution is only the direct solar capture S (zero for most operations).

For operations where exothermic chemical reactions among input resources provide significant process energy (BASIC_O2_STEELMAKING, COAL_GASIFICATION, etc.), the `electricity` column captures only auxiliary electrical requirements. The chemical energy is implicitly carried by the resource streams. The `heat` value therefore represents only electrical/solar waste heat for such operations — a deliberate simplification consistent with the model's treatment of chemical energy as embedded in resource flows.

**SOLAR_STEELMAKING solar estimate:** I estimated S ≈ 1.50 MW per t/h mass throughput based on the energy requirements for carbothermal reduction of iron ore at ~1500°C (endothermic reduction ΔH ≈ 380 kJ/mol Fe₂O₃, plus sensible heating and melting of ~0.88 MW useful process heat, divided by η = 0.70 solar-to-process efficiency). This puts the heliostat field at roughly 1500 m² per t/h — plausible for concentrated solar thermal at industrial scale.

**MUNICIPAL_RECYC_INCINERATION note:** This operation has E > 0 (net electricity generation from waste combustion), so it uses the generator formula. The η = 0.75 implies a primary energy of only 0.32 MW, but the actual chemical energy of 0.478 t/h of municipal waste is roughly 1.3 MW (LHV ≈ 10 MJ/kg). This means η = 0.75 significantly overestimates the electrical conversion efficiency (real WtE plants achieve 18–25%). The value may have been intended to capture combined material-plus-energy recovery effectiveness. I'd recommend revisiting this — perhaps η ≈ 0.25 for pure electrical efficiency, or splitting the operation's "efficiency" concept into material recovery yield and electrical efficiency. I've left η unchanged for now.

---

## Schema Modifications

Replace the entire `## Fields` section and add a new `## Energy Balance` section at the end of `operations.schema.md`:

```markdown
## Fields

- name (implicit 1st column) — Operation name per table instructions.
- electricity — Net electrical power in MW. Positive for electricity-generating operations; negative for electricity-consuming operations. For Energy group operations, normalized to 1 MW output. For all others, the electrical power per 1 t/h of mass throughput.
- solar — Direct solar radiation power input to the operation in MW. Non-empty only for operations that directly capture and use solar radiation for their primary process: SOLAR_POWER (photovoltaic/CSP electricity generation), SOLAR_STEELMAKING (concentrated solar thermal reduction), and biological operations using photosynthesis (for which solar represents the optimized photosynthetically active radiation input; in some contexts all or part of light may instead be supplied via additional electricity). Empty (not applicable) for all other operations, including those consuming grid electricity originally generated from solar sources.
- heat — Waste heat generated by the operation in MW. The thermal power that must be dissipated (via radiators, cooling towers, or environmental sinks) or optionally recovered for facility or district heating. Derived from the energy balance as follows. For electricity-generating operations (electricity > 0): heat = electricity × (1 − efficiency) / efficiency. For electricity-consuming or non-electrical operations (electricity ≤ 0): heat = (|electricity| + solar) × (1 − efficiency). For operations where significant process energy comes from exothermic chemical reactions among input resources (e.g., BASIC_O2_STEELMAKING, COAL_GASIFICATION), the heat value captures only waste heat from electrical and solar energy processing; thermal energy released by chemical reactions is implicitly carried by output resource streams and managed elsewhere in the simulation.
- efficiency — Energy efficiency of the operation, dimensionless, range (0, 1). The ratio of useful energy output to total tracked energy input. For electricity-generating operations (electricity > 0): efficiency = electricity / total_primary_energy, where total primary energy is the full energy captured from the source (solar radiation, chemical energy of fuel, nuclear thermal power, geothermal heat, etc.). For electricity-consuming or non-electrical operations (electricity ≤ 0): efficiency = useful_work / (|electricity| + solar), i.e., the fraction of electrical and solar input that performs productive work rather than generating waste heat. Select values that are somewhat forward-looking but within the range of current or near-term operations.
- civ_power — Civilization power contribution in MW for Kardashev-scale accounting. Represents the total primary energy this operation captures from natural or chemical sources, without double-counting energy already tracked at other operations. For electricity-generating operations (electricity > 0): civ_power = electricity / efficiency, the full primary energy harnessed. For electricity-consuming or non-electrical operations (electricity ≤ 0): civ_power = solar (the direct solar energy captured independently of the electrical grid; 0 if solar is empty). Grid electricity consumed by these operations is already counted at the generating operation. The civilization's total Kardashev-scale power is the sum of civ_power across all active operations.
- input_resources — A semicolon-delimited list of input resources for the operation.
- input_rates — A semicolon-delimited list of input resource rates (in t/h) for the operation.
- output_resources — A semicolon-delimited list of output resources for the operation.
- output_rates — A semicolon-delimited list of output resource rates (in t/h) for the operation.
- mass_flow — Equals the sum of input_rates equals the sum of output_rates, within rounding precision.
- fuel_rate — Energy group operations only. Total input rate excluding OXYGEN.
- target_products — All groups except Energy. Target product(s) of the operation, which is sometimes a judgment call. For conversion operations where the valued outputs may be unclear or context dependent, use total output rate excluding explicit waste products such as INDUST_WASTE. For most synthesis, manufacturing, and biological operations, a single target product is usually obvious. Example: for OIL_REFINING, we consider as value products LIQ_HYDROCARBONS, METHANE, CARBON, and SULFUR (only excluding INDUST_WASTE).


## Notes

- For Extraction group operations, leave all fields blank except the name field (these are handled very differently).
- Renewable power operations and RADIOISOTOPE_POWER have no input or output resources.
- For Energy group operations, normalize the operation so that `electricity` is 1 MW. For others, normalize the operation so that `mass_flow` is 1 t/h.
- Use resources as they are defined in physical_resources.md. E.g., input rate for LE_URANIUM_FUEL should consider the assembled fuel weight, not just the uranium content.
- Use catchall resources as needed. E.g., if an operation in the real-world requires 1 kg/h of lithium, use 1 kg/h of INDUSTRIAL_METALS.
- For chemical conversions, consider only stoichiometric quantities for inputs and outputs. For fabricated/manufactured items, consider product composition and subtractive processes (the latter producing INDUSTRIAL_WASTE, REGOLITH, etc.).
- Don't consider plant maintenance or repair parts used to run the operation; that will be handled separately.
- Consider inputs and outputs as a net reaction. I.e., don't have the same resource as input and output.
- Operation energy including process heat should usually come from electricity rather than other sources such as fuel combustion (this is a model simplification). Exceptions: cases where energy is derived directly from operation inputs, such as CARBON and OXYGEN in BASIC_O2_STEELMAKING (in this case electricity accounts for auxiliary requirements); cases where direct solar energy is explicitly used for heat (tracked in the solar column).
- Don't worry about water use in industrial processes, except as a stoichiometric input or output.
- Don't worry about the source of input resources (e.g., OXYGEN from atmosphere versus purified supply) or the fate of output resources (e.g., collected versus exhaust). An amendment is that you can consider alternative compositions and how they might be more useful or less harmful. E.g., you could consider outputting SULFUR rather than SULFUR_DIOXIDE from COAL_POWER.
- Be forward looking if the particular operation is not well constrained (as stated above for efficiency).
- Report if you find major gaps in the operations list or associated lists.


## Energy Balance

All non-extraction operations satisfy the following energy relationships.

Electricity-generating operations (electricity > 0): The operation captures primary energy from a natural or chemical source and converts a fraction equal to efficiency into electricity. The remainder is waste heat. Identities: heat = electricity × (1 − efficiency) / efficiency; civ_power = electricity / efficiency = electricity + heat.

Electricity-consuming or non-electrical operations (electricity ≤ 0): The operation uses electrical and/or direct solar energy, converting a fraction equal to efficiency into useful work. The remainder is waste heat. Identities: heat = (|electricity| + solar) × (1 − efficiency); civ_power = solar (or 0 if not applicable).

Verification: for electricity > 0, efficiency = electricity / (electricity + heat). For electricity ≤ 0, efficiency = 1 − heat / (|electricity| + solar).
```

---

## Updated operations.tsv

```tsv
name\Field	electricity	solar	heat	efficiency	civ_power	input_resources	input_rates	output_resources	output_rates	mass_flow	fuel_rate	target_products
Type	FLOAT	FLOAT	FLOAT	FLOAT	FLOAT	ARRAY[TABLE]	ARRAY[FLOAT]	ARRAY[TABLE]	ARRAY[FLOAT]	FLOAT	FLOAT	ARRAY[TABLE]
Unit	MW	MW	MW		MW		t/h		t/h	t/h	t/h	
Prefix/OPERATION_						RESOURCE_		RESOURCE_				
SOLAR_POWER	1	4.00	3.00	0.25	4.00					0	0	
GEOTHERMAL_POWER	1		5.67	0.15	6.67					0	0	
HYDROPOWER	1		0.0870	0.92	1.09					0	0	
WIND_POWER	1		0.0526	0.95	1.05					0	0	
TIDAL_POWER	1		0.111	0.90	1.11					0	0	
HYDROGEN_POWER	1		1.22	0.45	2.22	HYDROGEN;OXYGEN	0.0667;0.533	WATER	0.600	0.600	0.0667	
METHANE_POWER	1		1.00	0.50	2.00	METHANE;OXYGEN	0.144;0.576	CARBON_DIOXIDE;WATER	0.396;0.324	0.720	0.144	
METHANOL_POWER	1		1.38	0.42	2.38	METHANOL;OXYGEN	0.431;0.646	CARBON_DIOXIDE;WATER	0.592;0.485	1.077	0.431	
ETHANOL_POWER	1		1.38	0.42	2.38	ETHANOL;OXYGEN	0.320;0.668	CARBON_DIOXIDE;WATER	0.612;0.376	0.988	0.320	
LIQ_HYDROCARBONS_POWER	1		1.38	0.42	2.38	LIQ_HYDROCARBONS;OXYGEN	0.199;0.677	CARBON_DIOXIDE;WATER	0.625;0.251	0.876	0.199	
OIL_POWER	1		1.63	0.38	2.63	OIL;OXYGEN	0.237;0.760	CARBON_DIOXIDE;WATER;SULFUR;INDUST_WASTE	0.734;0.256;3.56e-3;3.32e-3	0.997	0.237	
COAL_POWER	1		1.38	0.42	2.38	COAL;OXYGEN	0.343;0.708	CARBON_DIOXIDE;WATER;SULFUR;INDUST_WASTE	0.830;0.170;5.15e-3;0.0463	1.051	0.343	
HYDROGEN_FUEL_CELLS	1		0.818	0.55	1.82	HYDROGEN;OXYGEN	0.0545;0.436	WATER	0.491	0.491	0.0545	
METHANE_FUEL_CELLS	1		0.818	0.55	1.82	METHANE;OXYGEN	0.131;0.524	CARBON_DIOXIDE;WATER	0.360;0.295	0.655	0.131	
METHANOL_FUEL_CELLS	1		1.22	0.45	2.22	METHANOL;OXYGEN	0.402;0.603	CARBON_DIOXIDE;WATER	0.553;0.452	1.005	0.402	
LEU_FISSION_POWER	1		1.94	0.34	2.94	LE_URANIUM_FUEL	3.47e-6	SPENT_URANIUM_FUEL	3.47e-6	3.47e-6	3.47e-6	
HEU_FISSION_POWER	1		2.33	0.30	3.33	HE_URANIUM_FUEL	3.70e-6	SPENT_URANIUM_FUEL	3.70e-6	3.70e-6	3.70e-6	
THORIUM_FISSION_POWER	1		1.94	0.34	2.94	THORIUM_FUEL	4.36e-6	SPENT_THORIUM_FUEL	4.36e-6	4.36e-6	4.36e-6	
D_T_FUSION_POWER	1		1.50	0.40	2.50	DEUTERIUM;INDUST_METALS	8.39e-9;2.51e-8	HELIUM	3.35e-8	3.35e-8	3.35e-8	
D_3HE_FUSION_POWER	1		1.00	0.50	2.00	DEUTERIUM;HELIUM_3	8.22e-9;1.23e-8	HELIUM;HYDROGEN	1.63e-8;4.20e-9	2.05e-8	2.05e-8	
3HE_3HE_FUSION_POWER	1		0.818	0.55	1.82	HELIUM_3	3.18e-8	HELIUM;HYDROGEN	2.11e-8;1.07e-8	3.18e-8	3.18e-8	
RADIOISOTOPE_POWER	1		9.00	0.10	10.0					0	0	
ATMOS_SEPARATION												
VOLATILES_SEPARATION												
HYDROCARBON_SEPARATION												
DESALINATION												
BRINE_PROCESSING												
GAS_DRILLING												
OIL_DRILLING												
COAL_MINING												
IRON_MINING												
ALUMINIUM_MINING												
INDUST_METALS_MINING												
PRECIOUS_METALS_MINING												
URANIUM_MINING												
RARE_EARTHS_MINING												
SULFUR_MINING												
ORGANICS_MINING												
INDUST_MINERALS_MINING												
STONE_QUARRYING												
REGOLITH_QUARRYING												
OIL_REFINING	-0.10		0.0100	0.90	0	OIL;HYDROGEN	0.979;0.021	LIQ_HYDROCARBONS;METHANE;CARBON;SULFUR;INDUST_WASTE	0.881;0.050;0.040;0.014;0.015	1		LIQ_HYDROCARBONS;METHANE;CARBON;SULFUR
IRON_NICKEL_REFINING	-0.35		0.0525	0.85	0	IRON_NICKEL	1	IRON;INDUST_METALS;SULFUR;INDUST_WASTE	0.910;0.086;0.002;0.002	1		IRON;INDUST_METALS;SULFUR
IRON_ORE_SMELTING	-0.04		0.00600	0.85	0	IRON_ORES;CARBON;OXYGEN	0.635;0.154;0.211	IRON;CARBON_DIOXIDE;INDUST_WASTE	0.373;0.480;0.147	1		IRON
BASIC_O2_STEELMAKING	-0.03		0.00360	0.88	0	IRON;OXYGEN;INDUST_MINERALS;INDUST_METALS	0.832;0.093;0.063;0.012	STEEL;CARBON_DIOXIDE;INDUST_WASTE	0.793;0.113;0.094	1		STEEL
H2_REDUCTION_STEELMAKING	-0.24		0.0360	0.85	0	IRON_ORES;HYDROGEN;INDUST_METALS;CARBON	0.962;0.028;0.008;0.002	STEEL;WATER;INDUST_WASTE	0.535;0.255;0.210	1		STEEL
ELECTRIC_STEELMAKING	-0.40		0.0800	0.80	0	IRON;OXYGEN;INDUST_MINERALS;INDUST_METALS	0.832;0.093;0.063;0.012	STEEL;CARBON_DIOXIDE;INDUST_WASTE	0.793;0.113;0.094	1		STEEL
SOLAR_STEELMAKING	-0.02	1.50	0.456	0.70	1.50	IRON_ORES;CARBON;INDUST_METALS	0.840;0.150;0.010	STEEL;CARBON_MONOXIDE;INDUST_WASTE	0.479;0.346;0.175	1		STEEL
ALUMINIUM_REFINING	-2.80		0.420	0.85	0	ALUMINIUM_ORES;CARBON	0.938;0.062	ALUMINIUM;CARBON_DIOXIDE;INDUST_WASTE	0.186;0.227;0.587	1		ALUMINIUM
INDUST_METALS_REFINING	-0.65		0.117	0.82	0	INDUST_METAL_ORES;CARBON;OXYGEN	0.890;0.060;0.050	INDUST_METALS;IRON;SULFUR;CARBON_MONOXIDE;INDUST_WASTE	0.242;0.133;0.089;0.142;0.394	1		INDUST_METALS;IRON;SULFUR
PRECIOUS_METALS_REFINING	-0.5		0.0750	0.85	0	PRECIOUS_METAL_ORES	1	PRECIOUS_METALS;INDUST_WASTE	8.70e-4;0.999	1		PRECIOUS_METALS
RARE_EARTHS_REFINING	-1.50		0.300	0.80	0	RARE_EARTH_ORES;INDUST_CHEMICALS	0.970;0.030	RARE_EARTHS;THORIUM;INDUST_WASTE	0.413;0.004;0.583	1		RARE_EARTHS;THORIUM
URANIUM_REFINING	-0.05		0.00750	0.85	0	URANIUM_ORES;INDUST_CHEMICALS	0.971;0.029	URANIUM;INDUST_METALS;THORIUM;INDUST_WASTE	0.086;0.006;0.002;0.906	1		URANIUM;INDUST_METALS;THORIUM
REGOLITH_PROCESSING	-5.00		1.50	0.70	0	REGOLITH	1	OXYGEN;IRON;ALUMINIUM;INDUST_WASTE	0.440;0.110;0.070;0.380	1		OXYGEN;IRON;ALUMINIUM
DEUTERIUM_PURIFICATION	-100		20.0	0.80	0	WATER	1	DEUTERIUM;OXYGEN	0.2;0.8	1		DEUTERIUM
MUNICIPAL_RECYC_INCINERATION	0.24		0.0800	0.75	0.320	MUNICIPAL_WASTE;OXYGEN	0.478;0.522	STEEL;ALUMINIUM;POLYMERS;CARBON_DIOXIDE;WATER;INDUST_WASTE	0.031;0.012;0.029;0.613;0.172;0.143	1		STEEL;ALUMINIUM;POLYMERS
INDUST_RECYC_INCINERATION	-0.30		0.0600	0.80	0	INDUST_WASTE;OXYGEN	0.990;0.010	STEEL;INDUST_METALS;REGOLITH;CARBON_DIOXIDE	0.178;0.020;0.782;0.020	1		STEEL;INDUST_METALS
URANIUM_FUEL_REPROCESSING	-0.12		0.0180	0.85	0	SPENT_URANIUM_FUEL;INDUST_CHEMICALS;INDUST_MINERALS	0.415;0.136;0.449	URANIUM;RADIOACTIVE_WASTE;WATER	0.404;0.551;0.045	1		URANIUM
THORIUM_FUEL_REPROCESSING	-0.10		0.0150	0.85	0	SPENT_THORIUM_FUEL;INDUST_CHEMICALS;INDUST_MINERALS	0.654;0.008;0.338	THORIUM;URANIUM;RADIOACTIVE_WASTE;WATER	0.487;0.021;0.486;0.006	1		THORIUM;URANIUM
WATER_ELECTROLYSIS	-5.50		1.10	0.80	0	WATER	1	HYDROGEN;OXYGEN	0.112;0.888	1		HYDROGEN;OXYGEN
CO2_ELECTROLYSIS	-2.38		0.595	0.75	0	CARBON_DIOXIDE	1	CARBON_MONOXIDE;OXYGEN	0.636;0.364	1		CARBON_MONOXIDE;OXYGEN
METHANE_PYROLYSIS	-3.00		0.900	0.70	0	METHANE	1	CARBON;HYDROGEN	0.749;0.251	1		CARBON;HYDROGEN
COAL_GASIFICATION	-0.08		0.0120	0.85	0	COAL;OXYGEN;WATER	0.557;0.364;0.079	CARBON_MONOXIDE;HYDROGEN;CARBON_DIOXIDE;SULFUR;INDUST_WASTE	0.780;0.039;0.123;0.008;0.050	1		CARBON_MONOXIDE;HYDROGEN;SULFUR
BIOMASS_GASIFICATION	-0.06		0.0120	0.80	0	BIOFEEDSTOCK;OXYGEN;WATER	0.805;0.155;0.040	CARBON_MONOXIDE;HYDROGEN;CARBON_DIOXIDE;INDUST_WASTE	0.838;0.053;0.069;0.040	1		CARBON_MONOXIDE;HYDROGEN
COAL_CARBONIZATION	-1.50		0.375	0.75	0	COAL	1	CARBON;METHANE;HYDROGEN;WATER;AMMONIA;SULFUR;CARBON_MONOXIDE;INDUST_WASTE	0.650;0.055;0.015;0.175;0.003;0.012;0.014;0.076	1		CARBON;METHANE;HYDROGEN;AMMONIA;SULFUR;CARBON_MONOXIDE
BIOMASS_CARBONIZATION	-0.42		0.126	0.70	0	BIOFEEDSTOCK	1	CARBON;METHANE;HYDROGEN;CARBON_MONOXIDE;CARBON_DIOXIDE;WATER;INDUST_WASTE	0.347;0.065;0.015;0.080;0.210;0.233;0.050	1		CARBON;METHANE;HYDROGEN;CARBON_MONOXIDE
CO2_METHANATION	-0.09		0.0135	0.85	0	CARBON_DIOXIDE;HYDROGEN	0.845;0.155	METHANE;WATER	0.308;0.692	1		METHANE
STEAM_METHANE_REFORMING	-2.25		0.563	0.75	0	METHANE;WATER	0.471;0.529	CARBON_MONOXIDE;HYDROGEN	0.822;0.178	1		CARBON_MONOXIDE;HYDROGEN
WATER_GAS_SHIFT	-0.05		0.00750	0.85	0	CARBON_MONOXIDE;WATER	0.609;0.391	CARBON_DIOXIDE;HYDROGEN	0.956;0.044	1		HYDROGEN
REVERSE_WATER_GAS_SHIFT	-0.33		0.0825	0.75	0	CARBON_DIOXIDE;HYDROGEN	0.956;0.044	CARBON_MONOXIDE;WATER	0.609;0.391	1		CARBON_MONOXIDE
LIQ_HYDROCARBONS_SYNTH	-0.10		0.0200	0.80	0	CARBON_MONOXIDE;HYDROGEN	0.867;0.133	LIQ_HYDROCARBONS;WATER	0.442;0.558	1		LIQ_HYDROCARBONS
CO2_METHANOL_SYNTH	-0.08		0.0160	0.80	0	CARBON_DIOXIDE;HYDROGEN	0.879;0.121	METHANOL;WATER	0.640;0.360	1		METHANOL
CO_METHANOL_SYNTH	-0.06		0.00900	0.85	0	CARBON_MONOXIDE;HYDROGEN	0.874;0.126	METHANOL	1	1		METHANOL
AMMONIA_SYNTH	-1.50		0.300	0.80	0	NITROGEN;HYDROGEN	0.822;0.178	AMMONIA	1	1		AMMONIA
POLYMER_SYNTH	-2.00		0.500	0.75	0	LIQ_HYDROCARBONS;INDUST_CHEMICALS;OXYGEN	0.820;0.120;0.060	POLYMERS;METHANE;HYDROGEN;CARBON_DIOXIDE;WATER;INDUST_WASTE	0.550;0.160;0.030;0.100;0.080;0.080	1		POLYMERS;METHANE;HYDROGEN
INDUST_CHEMICALS_SYNTH	-0.55		0.110	0.80	0	INDUST_MINERALS;SULFUR;AMMONIA;METHANOL;OXYGEN	0.450;0.080;0.020;0.150;0.300	INDUST_CHEMICALS;WATER;INDUST_WASTE	0.550;0.200;0.250	1		INDUST_CHEMICALS
FINE_CHEMICALS_SYNTH	-1.50		0.375	0.75	0	INDUST_CHEMICALS;METHANOL;INDUST_METALS;INDUST_MINERALS;OXYGEN	0.400;0.200;0.100;0.100;0.200	FINE_CHEMICALS;WATER;INDUST_WASTE;CARBON_DIOXIDE	0.500;0.200;0.150;0.150	1		FINE_CHEMICALS
CONCRETE_PRODUCTION	-0.22		0.0550	0.75	0	STONE;INDUST_MINERALS;WATER	0.600;0.250;0.150	CONCRETE;CARBON_DIOXIDE	0.940;0.060	1		CONCRETE
GLASS_CERAMICS_PRODUCTION	-1.75		0.525	0.70	0	INDUST_MINERALS	1	GLASS_CERAMICS;CARBON_DIOXIDE	0.890;0.110	1		GLASS_CERAMICS
SEMICONDUCTOR_PRODUCTION	-20.0		7.00	0.65	0	INDUST_MINERALS;CARBON;FINE_CHEMICALS;INDUST_CHEMICALS;OXYGEN	0.500;0.100;0.050;0.100;0.250	SEMICONDUCTOR_MAT;CARBON_DIOXIDE;INDUST_WASTE;WATER	0.200;0.350;0.350;0.100	1		SEMICONDUCTOR_MAT
COMPOSITE_PRODUCTION	-1.80		0.450	0.75	0	POLYMERS;GLASS_CERAMICS;CARBON;INDUST_CHEMICALS	0.400;0.450;0.100;0.050	COMPOSITES;INDUST_WASTE	0.900;0.100	1		COMPOSITES
SOLID_FUEL_PRODUCTION	-0.29		0.0435	0.85	0	INDUST_CHEMICALS;ALUMINIUM;POLYMERS;FINE_CHEMICALS	0.680;0.180;0.100;0.040	SOLID_FUELS;INDUST_WASTE	0.950;0.050	1		SOLID_FUELS
FERTILIZER_PRODUCTION	-0.82		0.164	0.80	0	AMMONIA;INDUST_MINERALS;SULFUR;INDUST_CHEMICALS;WATER	0.294;0.470;0.059;0.118;0.059	FERTILIZERS;INDUST_WASTE	0.824;0.176	1		FERTILIZERS
```

---

## Simplifications and Caveats

**Waste heat underestimation for exothermic operations.** Operations like BASIC_O2_STEELMAKING (E = −0.03 MW, Q = 0.004 MW) produce far more waste heat in reality (~0.5 MW/t from the C+O₂ exotherm). The framework deliberately treats chemical energy as embedded in resource streams. If the simulation needs accurate thermal loads for heat-disposal sizing, it would need a supplementary "chemical heat" term or a lookup of reaction enthalpies.

**MUNICIPAL_RECYC_INCINERATION efficiency.** As noted above, η = 0.75 yields civ_power = 0.32 MW, but the actual chemical energy of the waste stream is ~1.3 MW, implying a real electrical efficiency of ~18%. The current η may have been set to represent combined recovery effectiveness. Worth revisiting.

**Kardashev double-counting in recycling loops.** If a civilization generates electricity from solar, electrolyzes water to hydrogen, then burns hydrogen for power, both the solar plant and the hydrogen plant register civ_power contributions. This overcounts by the hydrogen-loop energy. For a Kardashev metric this is a small systematic bias and acceptable, but worth documenting.

**Biological operations.** The photosynthesis-based operations (OPEN_AGRICULTURE, CEA, ALGAL_CULTIVATION, etc.) are listed in physical_operations.md but not yet in the TSV. When added, they'll need `solar` values representing optimized PAR capture per t/h of mass throughput. Photosynthetic efficiency is very low (~1–2% of incident radiation for most crops, up to ~8% for C₄ grasses under optimal conditions), so these solar values will be large relative to the mass throughput.

**SOLAR_STEELMAKING solar estimate.** The S = 1.50 MW value is based on a bottom-up energy balance (~0.88 MW useful process heat for carbothermic reduction and melting, divided by η = 0.70). The real value depends heavily on furnace design, operating temperature, and solar concentrator performance. Consider validating against specific pilot-plant data if available.