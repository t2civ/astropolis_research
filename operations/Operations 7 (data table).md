Provide additions and/or modifications for operations.tsv as follows:

- Consider existing Refining group operations in operations.tsv. Should REGOLITH be an output in addition to INDUST_WASTE for some of these operations? If so, modify the data table rows accordingly.
- Add data table rows for Conversions/Synthesis group operations.

---
---

*Thinking...*

> Let me analyze this carefully. I need to:
> 
> 1. Consider whether REGOLITH should be an output in addition to INDUST_WASTE for some Refining operations
> 2. Add data table rows for all Conversions/Synthesis group operations
> 
> Let me first think about the REGOLITH question.
> 
> Looking at the existing refining operations, INDUST_WASTE is used as a catch-all waste product. The question is whether some of these operations would more accurately produce REGOLITH as an output.
> 
> Looking at the resource definitions:
> - **Industrial Waste** — Scrap metal, slag, fly ash, spent catalysts, recyclable mixed waste; ref. production-weighted blend (~40% fly ash, ~30% slag, ~20% scrap metal, ~10% other). 
> 
> - **Regolith** — Surface fines and debris (includes overburden and gangue from mining operations); ref. generic basalt.
> 
> The key distinction is that Industrial Waste has significantly higher metal content and recyclable value, while Regolith is more inert fines and debris from mining operations.
> 
> Now I'm working through how different refining operations map to these waste categories. Slag from smelting fits Industrial Waste, while gangue and tailings from ore processing could go either way depending on the operation. For oil refining, the petroleum residue stays as Industrial Waste, and iron-nickel refining produces small amounts of slag that also classify as Industrial Waste.
> 
> Looking at the iron and steel operations, smelting and BOF steelmaking both produce slag that's clearly Industrial Waste. But hydrogen reduction steelmaking is more complex—it takes iron ore as input and produces a large waste fraction that includes both gangue from the ore itself, which is more regolith-like, and process waste. For electric and solar steelmaking, the waste is slag classified as industrial waste at 0.094 and 0.175 respectively. For aluminium refining, the waste fraction is much larger at 0.587 — the input ore is only 22% aluminium, so from 0.938 t of ore I get about 0.206 t of aluminium output, with the remainder being red mud and bauxite residue containing iron oxides, silica, and titania, which despite being mineral-like in character is heavily processed with caustic soda residue making it hazardous.
> 
> Industrial metals refining produces 0.394 waste fraction from slag and tailings, while precious metals refining is almost entirely waste at 0.999 — the input ore contains only 0.087% precious metals, so the waste is essentially the entire ore minus trace metals, consisting of pyrite, silicates, and iron. This processed tailings material could arguably be classified as regolith-like since it's mostly rock with metals removed, but the chemical processing involved in leaching suggests it belongs in the industrial waste category.
> 
> Rare earths and uranium refining follow similar patterns — both produce large waste fractions from chemically processed ore residues, so industrial waste is the right classification. Regolith processing itself generates waste from the unextracted regolith remaining after oxygen, iron, and aluminum removal, which is still fundamentally regolith in character.
> 
> Municipal recycling incineration produces ash and residue, clearly industrial waste. Industrial recycling incineration is already outputting regolith at a high rate, which is interesting. Uranium fuel reprocessing outputs radioactive waste and water instead, with no industrial waste component.
> 
> Looking at regolith processing specifically, the waste from extracting oxygen, iron, and aluminium from raw regolith is essentially depleted regolith—still a silicate mineral material. The definitions suggest regolith should be "surface fines and debris from mining operations," which actually fits depleted regolith better than the industrial waste category of "scrap metal, slag, fly ash." So regolith processing waste should probably output regolith rather than industrial waste. I'm reconsidering the schema definitions more carefully. The notes distinguish between slag from smelting operations versus tailings from ore processing, and Industrial Waste specifically covers slag and fly ash. For REGOLITH_PROCESSING, since the waste is depleted regolith rather than a chemically transformed byproduct, I should change it to output REGOLITH instead of INDUST_WASTE.
> 
> Now I'm looking at ALUMINIUM_REFINING more closely—the 0.587 t waste is red mud from the Bayer process, which on Earth is a major industrial waste problem, but off-Earth when processing anorthositic rock, the composition would be more regolith-like.
> 
> Since this is a blended reference, INDUST_WASTE works fine. I'll only modify REGOLITH_PROCESSING by swapping INDUST_WASTE for REGOLITH in the output products, keeping the same proportions.
> 
> But wait—the schema says inputs and outputs shouldn't overlap. If REGOLITH appears as both input and output, I need to net them out. That means 1.0 input minus 0.380 output leaves 0.620 net input. The outputs stay at 0.440 oxygen, 0.110 iron, and 0.070 aluminium, which sum to 0.620, matching the net input. This changes the mass_flow value though.
> 
> To keep mass_flow at 1, I need to scale everything up by 1.6129. That gives me REGOLITH as 1.0 input with no regolith output, and the outputs become 0.710 oxygen, 0.177 iron, and 0.113 aluminium. I also need to rescale the electricity and heat values by the same factor to maintain consistency.
> 
> But wait—the instructions say not to have the same resource as both input and output. If regolith is coming in and depleted regolith is coming out, that violates this rule. So I need to reconsider what the actual waste product is. When you electrolyze regolith oxides, you're extracting oxygen and metals, leaving behind unreacted material—essentially depleted regolith. The question is whether I should net this out entirely or treat it as a separate waste stream.
> 
> Actually, I think the waste from this process isn't really regolith anymore. After molten oxide electrolysis, what's left is slag—a melted, processed residue that's fundamentally different from raw regolith. It's been through an industrial transformation, so calling it INDUST_WASTE makes more sense than regolith. That resolves the input/output conflict.
> 
> Now I'm looking back at the INDUST_RECYC_INCINERATION process, which outputs regolith at 0.782 t/h. The resource definitions describe industrial waste as scrap metal, slag, fly ash, and similar materials, so there's a question about whether incineration of industrial waste should actually produce regolith.
> 
> Looking at the schema notes, vitrified slag from incineration is treated as regolith—which is reasonable since vitrified slag is essentially artificial rock. This suggests that some smelting operations might also output regolith for their slag byproducts. Blast furnace slag, for instance, is a calcium-alumino-silicate material with a composition similar to natural minerals, and when ground it functions as aggregate or artificial stone.
> 
> But I'm noticing the convention is inconsistent right now. Industrial incineration outputs regolith for its slag, while smelting operations output industrial waste. The key distinction seems to be that industrial waste is recyclable feedstock with value for further processing, whereas regolith represents the final, non-recoverable residue.
> 
> So the processing chain makes sense: refining produces industrial waste → incineration processes it → regolith emerges as the end product along with recovered metals. This aligns with the simulation notes that infrastructure becoming obsolete turns into industrial waste, which then feeds into the recycling pathway. Though some refining slag might genuinely be inert and unsuitable for further processing, in which case regolith would be the right classification from the start.
> 
> Looking at the actual waste compositions from these operations, I'm seeing that iron ore smelting produces blast furnace slag—calcium oxide, silica, alumina, magnesium oxide—which is typically used as aggregate or cement supplement in real-world applications, so it might be better classified as regolith or stone rather than industrial waste. Aluminium refining generates red mud with iron oxide and silica, which raises similar questions about whether these materials should be treated as recyclable industrial waste or as final inert residue.
> 
> The key distinction seems to be whether the waste stream has economically recoverable metals worth processing through the recycling-incineration pathway, versus whether it's already depleted enough to treat as regolith. Since the current recycling process does extract steel and industrial metals from industrial waste, I need to check whether the composition of these specific waste streams justifies that recovery effort.
> 
> Looking at blast furnace slag specifically—it's primarily calcium silicate and alumina compounds, which are more valuable as construction aggregate than as a metal recycling feedstock. The metal content is too low to justify the recycling route, so that should shift to regolith.
> 
> For the oxygen and electric steelmaking processes, the slag compositions are richer in iron and manganese oxides, which makes them worth treating as industrial waste for potential metal recovery rather than just aggregate. The hydrogen reduction and solar steelmaking operations produce mostly gangue minerals similar to the blast furnace case, so those align more with regolith classification.
> 
> Moving through the refining operations, I'm seeing a pattern where the waste streams become increasingly complex and chemically processed. Aluminium refining generates massive volumes of red mud with recoverable metals, while the various metal and rare earth refining processes all produce slag or residue with some metallic content worth recovering, making industrial waste the appropriate category for all of them. The uranium refining waste is particularly voluminous from leached ore residue.
> 
> Now I'm reconsidering the fundamental distinction between the two waste sinks—industrial waste feeds into recycling chains while regolith acts as inert fill material. For smelting slags specifically, they're actually used as construction aggregate in real operations, so routing them as regolith might better reflect their practical end-use rather than treating them as recyclable materials. STONE might make sense. But the current operations already have this as INDUST_WASTE, and the INDUST_RECYC_INCINERATION operation handles the recycling of INDUST_WASTE into recovered metals + REGOLITH. So there's already a pathway for converting INDUST_WASTE to REGOLITH.
> 
> I think the question is whether it's a meaningful distinction. In practice:
> - If waste goes to INDUST_WASTE, it can be recycled (some metals recovered) before becoming REGOLITH
> - If waste goes directly to REGOLITH, the recycling step is skipped
> 
> For most refining slags, there IS some recyclable content, so going through INDUST_WASTE → recycling → REGOLITH is actually the more accurate path.
> 
> For REGOLITH_PROCESSING specifically, the waste is depleted regolith after oxygen and metal extraction via high-temperature electrolysis, leaving behind mostly MgO, SiO2, and CaO slag. It could logically be either INDUST_WASTE or REGOLITH, but the constraint against having the same resource as both input and output means I'd need to net out the REGOLITH if I change it—reducing the input from 1.0 to 0.620 and renormalizing the outputs accordingly.
> 
> Now I'm recalculating the heat relationship. The efficiency stays at 0.70, so the waste heat should be derived from the electricity consumption using the formula: heat equals electricity magnitude times (1 minus efficiency) divided by efficiency. Let me work through this more carefully to get the right value. So the formula is heat = |electricity| / efficiency. Let me verify this holds across all the operations—checking the calculations for oil refining, iron ore smelting, and aluminium refining confirms the pattern. This means the total energy input equals the heat output, which is counterintuitive but seems to be what the data shows. I'm seeing the pattern now—across both power generation and consuming operations, the heat value consistently equals electricity divided by efficiency. This represents the total thermal energy involved in the process, with waste heat being the difference between that total and the actual electricity output. The schema label "net waste heat" seems to describe the overall thermal budget rather than just the wasted portion. So the heat column represents the total thermal energy budget—electricity divided by efficiency. When I rescale an operation, the new heat value becomes the new electricity magnitude divided by the efficiency. For REGOLITH_PROCESSING with netting, scaling by 1.6129 gives me an electricity value of -8.06 MW and a heat value of about 11 MW.
> 
> But this raises a key question: should REGOLITH itself be listed as an output for REGOLITH_PROCESSING? The instructions say not to have the same resource as both input and output, but if the waste is truly depleted regolith, I'd need to account for it separately, which significantly changes the numbers. I'm trying to figure out what the physical waste product actually is here.
> 
> The waste from molten oxide electrolysis is slag, not raw regolith—it's been fundamentally transformed through processing. Since INDUST_WASTE already covers slag and INDUST_RECYC_INCINERATION converts it back to REGOLITH, the current setup actually handles this correctly. So REGOLITH_PROCESSING looks fine as is.
> 
> Now I'm wondering if there are other refining operations where REGOLITH should be an output. URANIUM_REFINING seems like a candidate—the waste stream is mostly leached ore residue, which is essentially depleted rock. I'm reconsidering the incineration pathway—INDUST_WASTE converts to roughly 20% recoverable metals and 78% inert REGOLITH, which suggests most industrial waste eventually becomes fill material. For operations like iron ore smelting, the blast furnace slag is actually well-characterized in practice and typically repurposed as cement supplement or aggregate rather than ending up as pure waste.
> 
> The red mud from aluminium refining does contain some iron, but the bulk is alumina and silica waste. I think for simulation purposes, keeping a single INDUST_WASTE stream that routes through recycling is acceptable—splitting outputs into both INDUST_WASTE and REGOLITH for each operation would add unnecessary complexity. Let me verify whether any refining operations already output both streams.
> 
> Looking at the power generation operations, they do output multiple waste products including INDUST_WASTE, while INDUST_RECYC_INCINERATION produces REGOLITH instead. For refining operations with large gangue fractions like slag, outputting REGOLITH would be more realistic, but maintaining the current INDUST_WASTE recycling chain keeps things simpler. That said, uranium refining seems like a case where splitting the output would actually make sense.
> 
> The uranium ore is only 6% uranium, so after leaching, most of that 0.906 t/h of waste is just processed rock and tailings—silica, iron, calcium, aluminium. Routing 90% of that mass through incineration to recover metals doesn't align with the actual chemistry. The same issue applies to precious metals refining at 99.9% waste, rare earths at 58%, and aluminium at 59%. Though in a space economy context, waste reuse would be critical and even low-grade streams would get processed for any recoverable content, so there's tension between physical accuracy and the simulation's recycling philosophy.
> 
> I'm going to stop overthinking this and make a practical call: keep all the refining operations as they are. The INDUST_WASTE classification accurately represents slag, tailings, and red mud, and it feeds into the recycling chain appropriately for this scenario.
> 
> Now I'm moving on to the Conversions/Synthesis operations and need to work through the electrolysis and pyrolysis reactions from the physical operations list.
> 
> Looking at the remaining gasification, carbonization, and reforming reactions — I need to map each one to appropriate resource types and outputs, keeping in mind the constraint that input and output resources must be different. Now I'm setting up the parameters I need to calculate for each process — electricity consumption or generation, heat requirements based on efficiency, and the input/output resource flows normalized to a 1 ton per hour basis. Let me start working through the stoichiometry and energy requirements for water electrolysis.
> 
> For the reaction, I'm checking the mass balance: the input water converts to hydrogen and oxygen in specific proportions. At a 1 t/h throughput, that gives me 0.112 t/h of hydrogen and 0.888 t/h of oxygen as outputs. The energy requirement for PEM or alkaline electrolysis is roughly 50 kWh per kilogram of hydrogen produced, so with 112 kg/h of hydrogen output, I'm looking at around 5,600 kWh/h of electricity consumption.
> 
> But let me verify this against the thermodynamic minimum. The enthalpy of water splitting is about 286 kJ per mole of water, which translates to roughly 15.88 kJ per gram of water. For a full ton of water per hour, that's a theoretical minimum of about 4.41 MW. The actual PEM systems operate at around 50 kWh/kg of hydrogen, though newer designs might push that down to 48-50 kWh/kg. At the 50 kWh/kg figure with my 1 t/h water input producing 112 kg/h of hydrogen, I'm getting close to that 5.6 MW estimate.
> 
> Now I'm working through the efficiency calculation. If the thermodynamic minimum is 4.41 MW and I'm actually consuming 5.6 MW, that gives me an efficiency of about 0.79, or roughly 80%. The waste heat generated would be the difference between the total energy input divided by efficiency and the actual electricity consumed—so 5.6 divided by 0.8 gives 7.0 MW total energy, meaning 1.4 MW is lost as waste heat.
> 
> But I need to reconsider what this actually represents physically. The 5.6 MW of electricity goes into the process, and with 80% efficiency, only 4.48 MW becomes useful chemical work while 1.12 MW is wasted. Let me check this against the solar power example to understand the convention: if solar has 25% efficiency with 1 MW electricity output, that means 4 MW of thermal input, which makes sense—the heat column represents the total energy input before conversion. I'm realizing the convention might be simpler than I thought—just use heat = |electricity| / efficiency across all operation types, where heat represents the thermal input for generators and the equivalent thermal dissipation for consumers. Now I'm working through the water electrolysis case, calculating the theoretical energy requirement based on the higher heating value of hydrogen to establish what the actual efficiency should be. H₂
> 
> Current best PEM: ~50 kWh/kg H₂ (including stack + balance of plant)
> Forward-looking: ~45 kWh/kg H₂ (solid oxide at high temperature could be even better)
> 
> Let me use 48 kWh/kg H₂ as forward-looking.
> 
> H₂ production = 0.112 t/h = 112 kg/h
> Energy = 112 × 48 = 5376 kWh/h = 5.38 MW
> 
> For this analysis, efficiency is the fraction of electrical input that converts to chemical energy in the products. So I'm calculating η = HHV of H₂ / electrical energy consumed = 39.4 / 48 = 0.82, which I'll use as my forward-looking efficiency.
> 
> Now I'm working through the energy balance: the H₂ produced contains 4.41 MW of chemical energy, and if I'm consuming 5.38 MW of electricity at 82% efficiency, the difference goes to waste heat.
> 
> But I'm realizing the heat column might represent something different depending on the process type—for oil refining, it seems to capture the total energy requirement rather than just waste heat, so I need to clarify what that column actually means in the context of different operations.
> 
> For water electrolysis, I'll work with an efficiency of 0.80 as a reasonable middle ground across different electrolyzer technologies, then calculate the actual energy consumption needed per kilogram of hydrogen produced.
> 
> At that efficiency, I need about 49.3 kWh per kilogram of H₂, which gives me roughly 5.5 MW of electricity for the 112 kg/h production rate. Let me verify this using the chemical energy of the products—the hydrogen output contains about 4.41 MW of chemical energy, and dividing by the efficiency confirms the 5.5 MW electrical input required.
> 
> Now I'm setting up the operation schema normalized to 1 t/h water input, which produces 0.112 t/h hydrogen and 0.888 t/h oxygen, with electricity consumption of 5.50 MW and heat rejection of 6.88 MW based on the energy balance.
> 
> But I should reconsider how electrolysis efficiency is actually defined—there's a difference between the thermodynamic minimum (237 kJ/mol from Gibbs free energy) and the thermoneutral voltage (286 kJ/mol from enthalpy), and real PEM systems typically require 50-55 kWh per kilogram of hydrogen produced.
> 
> Working through the system-level energy: at 50 kWh/kg H₂, that translates to about 180 MJ/kg, which for my water throughput gives roughly 5.60 MW of electricity input. This aligns with an efficiency of about 0.80 when I compare the thermoneutral enthalpy to the actual energy required. So I'll stick with electricity at -5.60 MW and calculate the heat requirement from there. Now I'm settling on -5.50 MW for electricity and 6.88 MW for heat output. But I should clarify whether to use the enthalpy of reaction (ΔH) or Gibbs free energy (ΔG) when defining efficiency—the electrical energy input represents the minimum thermodynamic work (ΔG), while the chemical energy stored in hydrogen is the total enthalpy (ΔH), with the difference accounting for heat generation. Now I'm setting up the water electrolysis parameters with input and output rates, then moving on to model CO₂ electrolysis where I need to calculate the mass fractions for the products based on the stoichiometric reaction.
> 
> For the CO₂ electrolysis, I'm working through the energy requirements—the reaction needs about 283 kJ per mole of CO₂, which translates to roughly 1.79 MW of energy input. Accounting for a 75% efficiency typical of solid oxide electrolysis cells, the actual electricity demand comes to about 2.38 MW.
> 
> Now I'm setting up the process parameters: the system consumes 1 ton per hour of CO₂ and produces carbon monoxide and oxygen in a 0.636 to 0.364 mass ratio, with the electricity and heat requirements specified accordingly.
> 
> For methane pyrolysis, I'm calculating the stoichiometry where methane decomposes into solid carbon and hydrogen gas, working through the molecular weights to get output mass fractions of 74.9% carbon and 25.1% hydrogen, with the reaction requiring 74.9 kJ/mol of heat input.
> 
> Now I'm converting this to hourly energy requirements per ton of methane feed—about 4.67 million kJ per hour, which translates to roughly 1.3 megawatts of thermal energy. Accounting for a 70% process efficiency, I need about 1.85 megawatts of electricity input, though I'm realizing the actual energy demand is more complex since the process operates at extremely high temperatures (1000-2000°C depending on the method) and the total energy requirement includes both the reaction enthalpy and the sensible heat needed to bring the methane up to those temperatures.
> 
> Looking at industrial data for turquoise hydrogen production, the typical energy requirement is around 18-25 kilowatt-hours per kilogram of hydrogen produced. Using the thermoneutral value of about 20 kWh/kg as a reasonable forward-looking estimate, and given that one ton of methane per hour yields roughly 251 kilograms of hydrogen per hour, the total energy demand comes to about 5 megawatts.
> 
> Now I'm reconsidering what efficiency actually means in this context—the table's efficiency metric appears to relate to waste heat recovery rather than thermodynamic efficiency. For an endothermic process like methane pyrolysis that requires substantial heating, most of the energy input goes toward maintaining reaction temperature and process losses, so the chemical conversion efficiency relative to total energy input would indeed be quite low. I should look at how other industrial processes in the reference table define efficiency, particularly for catalytic operations like oil refining versus energy-intensive ones like iron ore smelting, to better calibrate what efficiency value makes sense here.
> 
> Given that the reaction enthalpy is only 74.9 kJ/mol but the total energy requirement is several times higher due to heating needs, an efficiency around 0.25-0.35 seems reasonable, accounting for potential heat recovery improvements. Using η = 0.35 as a forward-looking estimate with heat recovery, the actual electricity requirement would be around 3.71 MW per ton per hour of methane.
> 
> But checking against real-world data for molten metal methane pyrolysis—which requires about 5-10 kWh/kg of methane—suggests the actual energy demand is closer to 8 MW for 1 ton per hour, which would imply a much lower efficiency of only 0.16. This discrepancy makes me reconsider whether I'm framing the efficiency correctly—perhaps it's better understood as the ratio of electricity needed relative to the theoretical minimum, rather than as a simple fraction of useful work. I'm reconsidering those numbers—literature on catalytic methane pyrolysis suggests lower energy requirements around 3-5 kWh/kg, or even 2 kWh/kg for thermally-driven reactors with good heat recovery. Using 4 kWh/kg as a realistic forward-looking estimate gives me 4.0 MW for 1 t/h throughput. Now I'm checking the thermodynamic efficiency by comparing the reaction enthalpy of 4.67 MJ/kg against the actual energy input of 14.4 MJ/kg.
> 
> That gives me an efficiency of about 32%, but I should really be looking at this from an energy balance perspective—comparing the higher heating values of the products against the input. The products (hydrogen and solid carbon) contain about 60.2 MJ/kg total, while the methane input has 55.5 MJ/kg, so the reaction adds roughly 4.7 MJ/kg of chemical energy, which represents the useful work being done.
> 
> But I'm only getting 4.7 MJ/kg of useful output while needing to input about 14.4 MJ/kg, giving an efficiency around 33%. The rest becomes waste heat—which actually makes sense for a high-temperature thermal process like methane pyrolysis operating at ~1000°C. Now I'm checking this against the aluminium refining numbers to see if it's reasonable. Aluminium smelting requires about 15 kWh per kilogram, so for the 186 kg of aluminium produced per ton of mass flow, that's roughly 2790 kWh of energy input. 2.79 MW. ✓ And η = 0.85.
> 
> So η = 0.85 for aluminium smelting. That means 85% of the electricity goes to chemical work (decomposing alumina) and 15% is waste heat. That makes sense for electrolytic processes which are quite efficient.
> 
> For methane pyrolysis, with η = 0.33, only 33% goes to chemical work and 67% is waste heat from heating the reactor. This seems physically reasonable for a high-temperature thermal process.
> 
> Actually, let me reconsider. If methane pyrolysis is done electrically with electric heating, the furnace efficiency might be 70-80% with good insulation. But I need to account for heating the gas to ~1000°C plus the mildly endothermic reaction. Working through the energy balance—heating the methane requires about 2.15 MJ/kg, the reaction itself needs 4.67 MJ/kg, so the total useful energy is around 6.82 MJ/kg. At 80% furnace efficiency that translates to roughly 2.37 kWh/kg. Though realistically, some of that heating energy is recoverable through heat exchange and the carbon exits at high temperature, so I shouldn't overthink the exact numbers. Let me just settle on reasonable estimates for methane pyrolysis around 4 kWh/kg. I'm working through the framework where waste heat is proportional to (1 - efficiency), and total heat input equals electricity divided by efficiency. For methane pyrolysis with an estimated efficiency of 0.55, the reaction enthalpy is about 1.30 MW, but I'm uncertain whether the "useful work" should include just the reaction enthalpy or also account for heating the feedstock to reaction temperature.
> 
> I think the most consistent approach is to define useful work as the reaction enthalpy plus any net sensible heat that can't be recovered through heat exchange, then divide by efficiency to get the actual electricity requirement, and finally calculate waste heat from that.
> 
> For methane pyrolysis with effective heat recovery, the net energy demand is around 5 MJ/kg, which translates to roughly 1.39 MW for a 1 t/h feed rate. But when I apply the efficiency factor of 0.55, I'm getting results that don't align—I need to reconsider how efficiency is being interpreted for endothermic processes. Looking at the iron ore smelting example, where efficiency is 0.85 with minimal electricity input, I think I'm misunderstanding the relationship between the reaction thermodynamics and how the efficiency parameter actually scales the energy requirements. The blast furnace is largely self-heated by the exothermic coke combustion. The electricity is mainly for blowers and auxiliary equipment. So η = 0.85 means 85% of that small electrical input goes to useful work.
> 
> BASIC_O2_STEELMAKING: η = 0.88, electricity = -0.03 MW
> The BOF process is exothermic (carbon in iron reacts with oxygen). Again, electricity is mainly for auxiliary equipment.
> 
> H2_REDUCTION_STEELMAKING: η = 0.85, electricity = -0.24 MW
> Fe₂O₃ + 3H₂ → 2Fe + 3H₂O. This is slightly endothermic. The electrical energy is for heating and H₂ supply.
> 
> ELECTRIC_STEELMAKING: η = 0.80, electricity = -0.40 MW
> EAF melts material directly with electricity, so 80% of the electrical input translates to useful melting work.
> 
> ALUMINIUM_REFINING: η = 0.85, electricity = -2.80 MW
> Hall-Héroult is highly electrical, with 85% of the input going to the electrochemical process.
> 
> REGOLITH_PROCESSING: η = 0.70, electricity = -5.00 MW
> Molten oxide electrolysis has lower efficiency at 70%.
> 
> DEUTERIUM_PURIFICATION: η = 0.80, electricity = -100 MW
> Isotope separation demands massive energy input, but maintains 80% efficiency.
> 
> I'm noticing a pattern here: electrochemical processes cluster around 80-85% efficiency, while electrical heating processes tend toward 70%.
> 
> For methane pyrolysis with electrical heating to 1000°C, I'm estimating η = 0.70 as reasonable for a thermal process. Now I need to figure out the actual electricity consumption—the reaction enthalpy is about 4.67 MJ/kg CH₄, but accounting for sensible heat and losses, the practical energy requirement is probably 2-3 times that, putting it in the 10-15 MJ/kg range.
> 
> Converting this to power: at 10-12 MJ/kg, processing 1 t/h of methane requires roughly 2.8-3.3 MW of electricity. I'll use 3.0 MW as my working value, which means the total thermal energy needed is 4.29 MW, leaving about 1.29 MW as waste heat.
> 
> Now I'm checking the energy balance: the chemical energy gain from converting methane to hydrogen and carbon is only 1.30 MW, but I've allocated 2.10 MW as "useful" electricity. The discrepancy suggests the extra 0.80 MW goes into sensible heating that gets partially recovered, which seems reasonable to account for in the overall efficiency picture.
> 
> Looking at the process parameters more carefully, I notice the deuterium purification requires an enormous 100 MW of electricity for just 1 ton per hour of water input—that's because deuterium is such a tiny fraction of natural hydrogen. This reminds me that energy requirements can vary wildly across different processes, so I should stick with my current estimates rather than overthinking them.
> 
> Now I'm moving through the synthesis operations. For methane pyrolysis, I'm calculating the heat requirement by dividing the energy input by the efficiency, which gives me 4.29 units. The process splits methane into carbon and hydrogen with a 70% thermal efficiency. Next I need to work through coal gasification, which is more involved since coal has multiple components—carbon, hydrogen, oxygen, and other elements—and the reaction produces a mixture of carbon monoxide, hydrogen, carbon dioxide, and ash.
> 
> For a modern entrained-flow gasifier, I'm looking at typical syngas compositions around 60-65% CO and 25-30% H₂ by volume, with a cold gas efficiency of 75-80%. The main reactions happening are the water-gas reaction (endothermic), partial oxidation with oxygen (exothermic), and combustion providing the necessary heat. I need to work out the mass balance using the full coal composition and determine how much of each product forms from the input coal.
> 
> Now I'm setting up the stoichiometry for the overall gasification process—coal, oxygen, and steam going in to produce syngas (CO and H₂), CO₂, and ash residue. Since steam is a key stoichiometric input in the water-gas reaction, I should account for it explicitly. Let me break down 1 kg of coal by its elemental composition and trace through what happens to each component during gasification.
> 
> For the carbon, hydrogen, and oxygen in the coal, I'm calculating the molar amounts and then determining the product yields—assuming roughly 90% of the carbon converts to CO with the remainder forming CO₂, while the hydrogen becomes H₂. Converting these back to mass gives me the product stream composition.
> 
> Now I'm checking the oxygen balance to see how much additional O₂ is needed beyond what's already in the coal. But I'm realizing I should factor in steam input, since the water-gas reaction introduces more hydrogen into the system. Let me reconsider this with a more straightforward approach focused on syngas production for downstream processes.
> 
> For a simplified model, I'm working with coal inputs of oxygen and steam to produce syngas components. The key reactions are partial oxidation of carbon to CO for heat generation, the water-gas reaction consuming heat to produce both CO and H₂, and some over-oxidation creating CO₂. Breaking this down per kilogram of coal, I'm calculating that roughly 93% of the carbon converts to CO while 7% becomes CO₂, which gives me specific mass outputs for each component.
> 
> Now I'm determining the oxygen requirements by working through the stoichiometry—the CO production needs about half its molar amount in O₂, while CO₂ production requires a 1:1 molar ratio. After accounting for the oxygen already present in the coal itself, I'm finding that external oxygen supply is needed to make up the difference. The hydrogen content from the coal will contribute to the H₂ output through the water-gas reaction. Now I'm setting up the balance equations for oxygen, hydrogen, and sulfur. For oxygen, I need to account for what comes in from external O₂, the coal's oxygen content, and the steam, then match it against what's in the CO and CO₂ products. The hydrogen balance is simpler—the coal and steam both contribute hydrogen atoms that end up in the H₂ output. For sulfur, the 0.015 kg converts to H₂S during gasification, which will consume some of the hydrogen produced.
> 
> To make this tractable, I'm assuming CO₂ represents about 10% of the carbon output while CO takes the rest, then working backward to find the oxygen balance constraint. For the steam-to-oxygen ratio, typical gasifiers run around 3:1 to 4:1 by mass, so I'm estimating a steam input of 0.20 kg per kg of coal and converting that to moles to solve for the oxygen requirement.
> 
> Now I'm realizing this manual approach is getting unwieldy, so I'm switching to empirical data from commercial oxygen-blown entrained-flow gasifiers like those from Shell, GE, or Siemens, which give me a clearer picture of realistic input and output ranges.
> 
> Using representative values per kilogram of coal input—0.65 kg oxygen and 0.15 kg steam—I can map out the mass balance: the products break down into syngas components (CO and H₂), CO₂, water vapor, sulfur compounds, and ash residue, which together account for the total input mass of 1.80 kg.
> 
> Now I'm checking the mass balance more carefully. The output totals around 1.66 kg, leaving a gap of about 0.14 kg that corresponds to nitrogen from the coal converting to N₂ or ammonia in the syngas. I need to track the carbon and hydrogen flows separately to ensure everything balances—the carbon in the coal becomes CO and CO₂, while the hydrogen from both the coal and steam produces H₂ and water vapor.
> 
> Rather than working through this manually, I'll reference literature values for a Shell/GE gasifier running Illinois No. 6 coal, which gives me more reliable input and output ratios to work from.
> 
> Now I'm verifying the mass balance checks out—the total inputs and outputs both equal 1.922 units. Scaling everything down to a 1 ton per hour basis gives me the normalized flow rates for coal, oxygen, and water inputs, with corresponding outputs for the syngas components.
> 
> I notice water appears on both sides of the equation, so I need to net them out—the output is slightly higher than the input, giving a net water output of 0.027 tons per hour. But I'm second-guessing my initial estimates since coal composition varies significantly. Let me recalculate this more rigorously using the actual coal properties we have: 66% carbon, 5.5% hydrogen, 18% oxygen, 1.5% sulfur, 1.4% nitrogen, and about 7.6% ash. For an oxygen-blown gasifier, I should be working with typical operating ratios of around 0.6 to 0.8 for the oxygen-to-coal mass ratio and 0.05 to 0.20 for steam-to-coal ratio.
> 
> Now I'm looking at the performance targets—cold gas efficiency around 75-80% and carbon conversion exceeding 99%—and I'll use a more systematic approach by calculating the stoichiometry of the main reactions and then applying empirical adjustment factors. The partial oxidation reaction releases heat that drives the water-gas reaction, so I need to account for that energy balance. Starting with the coal composition, I have roughly 660 grams of carbon per kilogram, which is about 55 moles, and 55 grams of hydrogen or 27.5 moles to work with.
> 
> Now I'm working through the oxygen balance—the coal already contains some oxygen equivalent, but I need additional oxygen from air or steam to drive the reactions. I'm assuming 95% of the carbon converts to CO and 5% to CO₂, which gives me the oxygen requirements. To boost hydrogen production, I'll introduce steam into the system to shift more carbon toward the water-gas reaction. Now I'm balancing the heat from partial oxidation against the energy needed for the water-gas reaction to find the equilibrium point where the system operates autothermally. Setting these equal gives me s = 23.85 mol, but accounting for practical losses and inefficiencies, I should apply a correction factor of around 0.65 to get a more realistic steam flow rate.
> 
> With that adjusted steam input of about 15.5 mol, I can calculate the actual oxygen and water requirements in mass terms. Rather than continuing with these detailed stoichiometric calculations, I'm going to shift to an empirical approach based on typical coal gasification data—using mass ratios of oxygen and steam per kilogram of coal to estimate the product composition more directly.
> 
> Now I'm working through the output products and how they map to the available resources in the schema. The nitrogen trace amounts from coal gasification are tricky since they typically form ammonia and hydrogen cyanide that get scrubbed out, leaving only small amounts of N₂ in the syngas. I could either include it in the gas stream or treat it separately, but I need to clarify whether steam should be counted as a stoichiometric input given the schema's guidance on water use in industrial processes.
> 
> Let me step back and use a more straightforward approach based on literature values for modern gasifiers like the Shell process. I'm going to work with approximate mass balances per kilogram of coal input, accounting for oxygen and steam inputs, then map the products to our resource categories. Now I'm breaking down the carbon allocation more carefully—splitting the 0.660 kg of carbon into two pathways: most of it forming carbon monoxide, with a smaller portion becoming carbon dioxide. I'm calculating the oxygen requirements for each reaction and comparing them against the oxygen available from the coal itself, which leaves a significant deficit that needs to be supplied externally.
> 
> For the hydrogen balance, I'm tracking where it comes from—the coal provides most of it, steam adds more, and some gets consumed in forming hydrogen sulfide—then working out how much molecular hydrogen ends up in the final output.
> 
> Now I'm settling on a steam-to-coal ratio of about 0.15, which gives me 150 grams of steam or roughly 8.33 moles. This means the steam contributes 133 grams of oxygen, so I need about 647 grams of external oxygen gas to make up the remaining deficit. The hydrogen sulfide produced gets converted to elemental sulfur, yielding about 15 grams of solid sulfur product.
> 
> When H₂S is processed through the Claus reaction to recover sulfur, the hydrogen ends up as water rather than staying as H₂, so I'm actually losing that hydrogen from my final output. This means my net hydrogen production drops to about 35.36 moles or 71.3 grams after accounting for the sulfur recovery process. Now I'm checking the nitrogen balance—14 g of nitrogen converts to 0.5 mol N₂, which is 14 g output. Let me verify the overall mass balance by comparing inputs against outputs: coal, oxygen, and steam total 1796.7 g going in, while the products—carbon monoxide, hydrogen, carbon dioxide, water from sulfur treatment, elemental sulfur, and nitrogen—need to account for everything.
> 
> Looking at the output total of 1804.7 g, there's an 8 g discrepancy that's likely from rounding errors, so I should recalculate the carbon monoxide amount to close the gap. Let me go back and work through the atomic inventory more systematically: the coal contains carbon, hydrogen, oxygen, sulfur, and nitrogen; the external oxygen and steam add more atoms; I need to track where all these atoms end up in the products.
> 
> Now I'm checking the oxygen balance across inputs and outputs—the coal, air, and steam provide about 60 moles of oxygen total, and I'm seeing that distributed between carbon monoxide, carbon dioxide, and water from sulfur removal, but the numbers aren't quite lining up yet. Now I'm calculating the hydrogen output by subtracting the hydrogen consumed in the H₂S reaction from the total hydrogen available, which gives me 35.09 mol or about 70.8 grams. Then I'm verifying the mass balance by adding up all the inputs and outputs to make sure everything accounts for the 1804.3 grams total.
> 
> For the normalization, I need to scale everything to a 1 ton per hour basis using the ratio 1000/1804.3. But I'm also reconsidering the water—since it appears as both an input (150 g steam) and output (8.5 g from the H₂S treatment), I should net these out according to the instructions, leaving a net steam input of 141.5 grams rather than counting both separately. Now I'm normalizing each output stream by dividing by the total mass, then verifying that both input and output sides sum to 1.0 to confirm the mass balance is correct.
> 
> For the electricity requirements, I need to account for the oxygen plant separately since it's modeled as its own unit operation. The gasifier itself needs power for compression and gas cleanup, but I should avoid double-counting the ASU energy since that's handled elsewhere in the system. - Feed preparation, gasifier operation, heat recovery, gas cooling, acid gas removal
> - Typically ~0.10-0.20 kWh/kg coal
> 
> For 0.557 t/h coal at 0.15 kWh/kg:
> Electricity = 557 × 0.15 = 83.6 kWh/h = 0.0836 MW → ~0.08 MW
> 
> efficiency: The gasifier is quite efficient thermally. η = 0.85?
> 
> heat = 0.08/0.85 = 0.094
> 
> The electricity requirement is actually quite low because coal gasification is self-powered—the exothermic reaction between coal and oxygen provides the main process heat, so electricity only covers auxiliary equipment like fans and pumps. This is similar to oxygen steelmaking where the coke-oxygen reaction generates its own heat. So I'm setting electricity to -0.08 MW with an auxiliary efficiency of 0.85, giving a heat requirement of 0.094 MW, and the inputs are coal, oxygen, and water.
> 
> Now I'm looking at the output composition and mass flows. The syngas contains carbon monoxide, hydrogen, carbon dioxide, sulfur, nitrogen, and industrial waste. I'm reconsidering whether to include nitrogen since it's only 0.8% of the output—it passes through the gasifier largely inert and doesn't significantly affect the process, though the coal itself contains about 1.4% nitrogen that converts to ammonia and hydrogen cyanide during gasification.
> 
> Since ammonia synthesis is a separate downstream operation and these nitrogen compounds get removed and end up in waste streams anyway, I'm consolidating the nitrogen into the industrial waste category. This simplifies the model by combining the 0.8% nitrogen output with the existing 4.2% industrial waste, bringing the total to 5.0%.
> 
> Now I'm verifying the mass balance across all outputs—carbon monoxide, hydrogen, carbon dioxide, sulfur, and industrial waste sum to 100%. But I'm reconsidering whether carbon dioxide should be included as a target product. While it's a significant 12.3% of the output and could theoretically be converted further in a modern gasifier, the definition of target products suggests focusing on the primary valued outputs rather than everything that isn't waste. So I'm leaning toward listing just carbon monoxide, hydrogen, and sulfur as the core targets, though carbon dioxide is worth noting as a secondary output. Now I'm working through biomass gasification, which has significantly higher oxygen content than coal—about 42% compared to coal's lower oxygen. This means less external oxygen is needed for the process. I'm calculating the molar composition per kilogram of biofeedstock and assuming similar conversion ratios where most carbon converts to CO with a small fraction forming CO₂.
> 
> I'm now determining the oxygen balance by calculating how much oxygen is required for the products, comparing it against what's already present in the feedstock, and figuring out how much additional oxygen needs to be supplied through steam and external O₂. So with 50 g of steam providing 2.78 mol of oxygen, I need 6.03 mol of external O₂—about 193 g. That gives me 32.54 mol of hydrogen output, or 65.6 g. Running through the mass balance: 1000 g feedstock plus 193 g O₂ plus 50 g steam totals 1243 g in, and my products—CO, H₂, CO₂, and ash—add up to essentially the same amount.
> 
> Now I'm normalizing everything to a 1 t/h basis by dividing through by 1.243, which gives me the mass fractions for each input and output stream.
> 
> The mineral content in the biofeedstock—nitrogen, potassium, calcium, silicon—converts to ash in the industrial waste stream at 0.040 t/h. Let me verify the balance: inputs sum to 1.000 and outputs to 0.999, so I'll adjust the carbon monoxide slightly to 0.838 to close the gap perfectly.
> 
> For electricity consumption, biofeedstock gasification is similar to coal but with higher reactivity, so I'm estimating around 0.06 MW with an 80% efficiency factor.
> 
> Now I'm setting up the biomass gasification process parameters with the balanced mass flows and moving on to coal carbonization, where coal breaks down into coke and volatile products like tar, ammonia, and synthesis gases. I'm trying to map coal tar and benzole to the available resources in the model. Coal tar is a complex hydrocarbon mixture, so it could fit under LIQ_HYDROCARBONS or ORGANICS, though ORGANICS is technically defined as kerogen rather than coal tar. For now, I might just combine the coal tar output with the coke to keep things simple.
> 
> Let me work out the actual yields from coal carbonization: coke makes up the bulk at around 750 g/kg coal, with the coal gas components (methane, hydrogen, carbon monoxide) contributing smaller amounts, plus ammonia from nitrogen content, water vapor, coal tar and light oil as liquid hydrocarbons, some CO₂, and sulfur compounds from the hydrogen sulfide in the gas stream.
> 
> Now I need to account for the ash content properly—the coke itself contains about 10% ash by weight, so I should split that 750 g of coke into its carbon component and the industrial waste portion that represents the ash and other inert material.
> 
> Wait, I'm getting confused about the carbon balance. Let me trace where all the carbon goes: some stays in the coke, but significant amounts leave as methane, carbon monoxide, carbon dioxide, and coal tar during the coking process. I'm working through the numbers to make sure they add up correctly across all the output streams.
> 
> Actually, I realize I've been estimating these values without solid data. Let me look at actual coke oven mass balance figures instead. For every 1000 kg of coal fed in, I get roughly 720-760 kg of coke, plus coke oven gas that's produced at around 300 cubic meters. The gas has a density of about 0.44 kg/m³, so that's roughly 132 kg of gas total. The composition breaks down by volume as hydrogen, methane, carbon monoxide, carbon dioxide, and some heavier hydrocarbons, and I need to convert these to mass fractions to properly track where the carbon ends up. Now I'm calculating the average molecular weight of the coal-derived gas using those volume fractions, which comes out to about 10.32 g/mol, and then converting that to the typical production rate of around 300-350 cubic meters per tonne of coal.
> 
> Working through the conversion, that translates to roughly 140 kilograms of coal-derived gas per tonne of coal. From there, I'm breaking down the mass composition of each component in the gas by multiplying its mole fraction by its molecular weight and scaling it to that 140 kg total.
> 
> I'm realizing this level of detail is getting unwieldy, so I'm stepping back to create a simplified material balance—just the key outputs per tonne of coal: solid carbon residue around 650 kg, methane at roughly 55 kg, hydrogen at 15 kg, ammonia from the coal's nitrogen content at about 3 kg, plus smaller quantities of carbon monoxide, carbon dioxide, and water vapor.
> 
> Now I'm adding the remaining streams: ethane, sulfur compounds extracted from the coal gas, industrial waste from ash, and coal tar which I'll categorize as liquid hydrocarbons. When I sum everything up I'm getting close to 950 kg out of 1000 kg in, which means there's roughly a 50 kg gap I need to account for—likely from underestimating moisture content or other minor streams I haven't fully captured.
> 
> Actually, I need to reconsider the mass balance more carefully since coking is a closed system with only coal as input. The 1000 kg of coal must fully account for all outputs, so I should work backward from coal composition: the carbon content becomes coke, while hydrogen and oxygen largely convert to water and volatile gases. Let me recalculate the coke yield and trace where each element ends up more systematically. Now I'm working through the stoichiometry of water formation from the volatile matter—the hydrogen and oxygen in the coal can combine to form water, but I'm constrained by which element is limiting. With 55 g of hydrogen available, I could theoretically make nearly 500 g of water, but the coal only contains 180 g of oxygen, which limits water production to around 200 g and consumes about 22.5 g of the available hydrogen.
> 
> Since the volatile formation is too complex to model from first principles, I'm switching to empirical mass yields from published data on coal carbonization. For every 1000 kg of coal, I'm looking at the breakdown: metallurgical coke yields about 720 kg with a specific composition of carbon, ash, sulfur, and other elements, while coke oven gas produces roughly 140 kg of product containing hydrogen, methane, carbon monoxide, carbon dioxide, hydrocarbons, nitrogen, hydrogen sulfide, and trace components.
> 
> The remaining outputs include coal tar at 40 kg of carbon-rich heavy hydrocarbons, ammoniacal liquor at 55 kg of water with dissolved ammonia, and light oil at 10 kg of light hydrocarbons, which accounts for 965 kg total—so I'm attributing the missing 35 kg to moisture driven off as water. Now I need to translate these mass yields into model resource flows, starting with how much pure carbon actually ends up in the coke versus how much is bound up in ash and sulfur. ...so I need to account for sulfur splitting between the coke and gas phases. Let me take a practical approach: for every ton of coal input, I'm getting 637 kg of carbon product, 76 kg of industrial waste ash, plus the gaseous outputs of methane, hydrogen, and carbon monoxide at 55 kg, 15 kg, and 22 kg respectively.
> 
> Adding in carbon dioxide, water from the ammoniacal liquor, ammonia, sulfur from the H₂S in the coal gas, liquid hydrocarbons from tar and light oil, and ethane brings me to about 984 kg total—I'm 16 kg short of the full ton, so I need to adjust the water content upward or redistribute some of the lighter products to close the mass balance.
> 
> Now I'm reconsidering my approach since I've been pulling yields from different sources and they're not aligning properly. Let me work through a cleaner mass balance starting with the coal composition and tracking where each element ends up in the products—the carbon in the coke, the hydrogen in the gases and liquids, and the oxygen and sulfur distributed across the various outputs. Now I'm finishing the hydrogen balance by calculating the water content from the remaining hydrogen, then cross-checking with the oxygen balance—both methods converge on approximately 178 grams of water, which confirms the calculation is consistent.
> 
> Let me finalize the mass distribution across all the product streams: carbon solids, methane, hydrogen gas, carbon monoxide, carbon dioxide, water vapor, ethane, ammonia, and sulfur compounds, with the industrial waste accounting for ash and residual sulfur in the coke. I'm 7 grams short of 1000, so I'll adjust the CARBON to 560 and INDUST_WASTE to 81 to balance it out. Now I'm realizing ETHANE is actually a resource in the system, so I need to account for that. This is getting complicated with all these small outputs, so I should consolidate them into the main products: coke, methane, hydrogen, water, ammonia, and coal tar liquids.
> 
> For the coal carbonization model, the key outputs are coke as the primary product, methane and hydrogen from coal gas, water vapor, small amounts of ammonia and sulfur, and industrial waste as ash. I'll lump the minor gases like CO, CO₂, and ethane into a single category rather than tracking each separately. Working through the stoichiometry per 1000 grams of coal: 650 grams of coke, 55 of methane, 15 of hydrogen, 175 of water, 3 of ammonia, 12 of sulfur, 76 of waste, and about 14 grams of other minor gases combined.
> 
> Now I'm verifying the mass balance and converting to normalized rates where 1 unit of coal input produces 0.650 coke, 0.055 methane, 0.015 hydrogen, 0.175 water, 0.003 ammonia, 0.012 sulfur, 0.014 combined minor gases, and 0.076 waste. The electricity requirement for heating the coke ovens needs to be accounted for separately unless the process uses self-generated coal gas for heat. usually come from electricity." But coking is often heated by burning COG. However, since the model assumes electricity, let me calculate:
> 
> Heat needed: Coking requires ~3.5-4.5 GJ per tonne of coal.
> At 4 GJ/t = 4000 MJ/t → 4000/3600 = 1.11 MW per t/h
> 
> η = 0.75 (thermal process with heat recovery)
> electricity = -1.11 MW
> heat = 1.11/0.75 = 1.48
> 
> Wait, but coking is actually heated by burning the COG byproduct. The schema says there are exceptions for "cases where energy is derived directly from operation inputs." COG is technically a product of the operation itself, used to heat the ovens. But in this model, the COG is output as methane, hydrogen, etc., so the electricity needs to cover the full heating requirement instead. If I model the COG as an output, then the heating has to come from electricity. The alternative would be to reduce the gas outputs and assume some is burned for heat, but the schema prefers electricity as the energy source.
> 
> So I'll go with electricity = -1.11, η = 0.75, heat = 1.48. Actually, let me double-check the calculation: the coking process needs about 3.5-4.5 GJ of heat per tonne of coal, and at 4 GJ/t with η = 0.75 for electrical heating, the useful heat needed is 4 GJ/t = 1.11 MW per tonne per hour.
> 
> Now I'm checking the convention against existing operations like aluminium refining to see if my calculation makes sense.
> 
> Looking at the pattern: η represents useful energy divided by total electricity input, so heat output equals electricity divided by η. For coal carbonization with 1.11 MW of useful heat needed and 75% efficiency, that gives me 1.48 MW of electricity input and 1.97 MW of heat output, which I can round to electricity = -1.50 MW.
> 
> Now I'm mapping out the coal carbonization process itself—the primary product is metallurgical coke from the carbon, while the coal off-gas products like methane and hydrogen are valuable co-products, with ammonia and sulfur as minor byproducts.
> 
> Looking at the instructions for valued outputs, I should exclude waste streams and neutral products like water, so I'm including everything except water and industrial waste—that means carbon, methane, hydrogen, ammonia, sulfur, and carbon monoxide all count as target products.
> 
> Moving on to biomass carbonization next, which follows a similar process...
> 
> For slow pyrolysis of biofeedstock at 400-500°C, I'm working through the mass balance: starting with 1000g of biofeedstock that's roughly 47% carbon, 42% oxygen, 6% hydrogen, and 5% ash, this breaks down into biochar (about 30-35% yield), bio-oil with significant water content (25-30%), and syngas gases (25-35%), with the biochar itself being mostly carbon and mineral ash.
> 
> Now I'm calculating the carbon distribution: the biochar yields about 244g of carbon with 49g of ash going to waste, leaving 226g of carbon in the volatile phase that forms syngas and water. Since biofeedstock has much higher oxygen content than coal, I'm expecting more CO₂ and water vapor in the gas products—I'm estimating CO₂ around 100g and CO around 50g as starting points for the gas composition.
> 
> Let me reconsider this with a standard slow pyrolysis yield profile instead: for every kilogram of biomass, I'd expect roughly 280g of carbon in the biochar, 30g of methane, 5g of hydrogen gas, 60g of carbon monoxide, and 180g of carbon dioxide. The carbon balance isn't working out—I'm getting 363 g of carbon in the outputs versus 470 g in the input, so I'm short about 107 g. Let me step back and reconsider the yields for slow pyrolysis of wood at around 500°C: biochar typically makes up 25-30% of the mass, bio-oil and water account for 40-50%, and gases comprise 20-25%. Working with 1000 g of biomass, I should get about 280 g of biochar, but I need to think more carefully about how to represent this—biochar isn't quite the same as the CARBON resource I've been modeling.
> 
> For the biochar portion, I can split it into roughly 230 g of actual carbon content and 50 g of ash and minerals that would go to INDUST_WASTE. The liquid phase is trickier: I have about 450 g total (250 g bio-oil and 200 g pyrolysis water), and I'm uncertain whether to classify the bio-oil as LIQ_HYDROCARBONS, METHANOL if there's downstream processing, or just decompose it further into carbon dioxide and water.
> 
> Looking at the operation description more carefully, the primary output is biochar with volatiles as co-products. The bio-oil could reasonably map to LIQ_HYDROCARBONS, though the composition is quite different—bio-oil runs about 55% carbon, 35% oxygen, 7% hydrogen, while LIQ_HYDROCARBONS is much richer in carbon at 85.5% and hydrogen at 14%.
> 
> Rather than trying to decompose bio-oil into exact model resources, I'm going to simplify by treating the liquid phase as a combination of WATER and LIQ_HYDROCARBONS. Now I'm working through the mass balance per 1000 g of biomass input, allocating outputs across CARBON, LIQ_HYDROCARBONS, METHANE, CARBON_MONOXIDE, CARBON_DIOXIDE, HYDROGEN, WATER, and other industrial products.
> 
> I'm verifying the carbon balance by calculating the actual carbon content in each output stream against the 470 g of carbon in the input biomass, and I'm finding I'm about 36 g short, so I'm adjusting the CARBON output upward to close the gap.
> 
> Now I'm hitting a constraint—increasing CARBON pushes the total mass over 1000 g, so I need to compensate by reducing WATER from 345 g down to 307 g to keep everything balanced.
> 
> With those adjustments locked in, I'm now working through the hydrogen balance, checking how much hydrogen is distributed across the liquid hydrocarbons, methane, and other streams.
> 
> The numbers aren't working out—I'm getting 71.5 g of hydrogen when I only have 60 g available, which means my assumption about the hydrogen content in bio-oil is off. Bio-oil is fundamentally different from the liquid hydrocarbons category, so I should probably drop that approach entirely and reconsider the composition breakdown.
> 
> Instead of treating bio-oil as a single product, I'll decompose the biomass carbonization into its basic components: biochar, methane, hydrogen, carbon monoxide, carbon dioxide, and water. Starting with 1000 g of biomass containing 470 g carbon, 60 g hydrogen, 420 g oxygen, and 50 g ash, I'm allocating 250 g to solid carbon and 50 g to waste, leaving 232.5 g of carbon to distribute among the volatile products. Now I need to work through how to allocate the remaining hydrogen and oxygen across these gaseous outputs.
> 
> I'm setting up mass balance equations to properly distribute the carbon, hydrogen, and oxygen across methane, carbon monoxide, and carbon dioxide, since my initial allocation attempt didn't conserve all the elements correctly. Now I'm working through the hydrogen and oxygen balances to solve for the product gas composition. I have three equations relating the methane, carbon monoxide, carbon dioxide, and water vapor outputs, along with a mass balance constraint that accounts for the char and ash residue. Let me set up these simultaneous equations to find the individual gas flow rates. Now I'm setting up the mass balance equations for each element and the total mass, which gives me four equations but five unknowns—I need to constrain one more variable. Since hydrogen gas production in biomass pyrolysis is typically minimal, I'll use that as my additional constraint to solve the system. Let me set m_H2O = 5 g.
> 
> From (2): 60 = 0.251×m_CH4 + 5 + 0.112×m_H2O
> → 55 = 0.251×m_CH4 + 0.112×m_H2O ... (2')
> 
> From (3): 420 = 0.571×m_CO + 0.727×m_CO2 + 0.888×m_H2O
> 
> From mass: 705 = m_CH4 + m_CO + m_CO2 + m_H2O
> 
> Let me express m_H2O from (2'): m_H2O = (55 - 0.251×m_CH4)/0.112 = 491.1 - 2.241×m_CH4
> 
> Testing m_CH4 = 40 g gives m_H2O = 401.5 g, which means m_CO + m_CO2 = 263.5 g. But when I work through the carbon balance equation, I get m_CO = 821.2 g, which is impossible since it exceeds the total volatile mass available.
> 
> The issue is that the biomass contains too much oxygen—most of it has to be released as CO₂ and H₂O. Let me try a smaller methane value of 15 g instead. This gives m_H2O = 457.5 g and m_CO + m_CO2 = 232.5 g. Working through the oxygen balance with these values...
> 
> Solving the oxygen equation gives m_CO = 995.5 g, which is way too high and clearly wrong. The real problem is that the water product alone accounts for 406.3 g of the 420 g total oxygen, leaving almost nothing for CO and CO₂. This constraint is too tight—there's barely any oxygen left to distribute between those two products while still hitting the required mass of 232.5 g combined. I see the fundamental issue now—I'm allocating too much oxygen to water formation. Let me recalculate the molar composition of the biomass and trace where each element actually goes. If I use all 26.25 moles of oxygen for water, that consumes 52.5 moles of hydrogen, leaving only 7 moles of hydrogen for other products, which severely constrains what can form from the remaining carbon. 0 mol → 3.5 mol H₂ = 7.0 g H₂, or some CH₄.
> 
> And all C would need to exit as solid carbon or CO/CO₂ using external O. But there's no external O input in carbonization!
> 
> So the C either stays as solid carbon or forms CO/CO₂ using O from the biomass.
> 
> If O goes to: H₂O and CO₂
> x mol H₂O + y mol CO₂, where x + 2y ≤ 26.25 mol O
> 
> Actually, let me think in moles:
> 
> Available: 39.1 mol C, 26.25 mol O, 59.5 mol H (29.75 mol H₂)
> 
> Products: solid C (a mol), CO (b mol, uses b C and b O), CO₂ (c mol, uses c C and 2c O), CH₄ (d mol, uses d C and 4d H), H₂ (e mol, uses 2e H), H₂O (f mol, uses 2f H and f O)
> 
> Now I'm setting up the mass balance equations: carbon accounts for a + b + c + d = 39.1 mol, oxygen for b + 2c + f = 26.25 mol, hydrogen for 4d + 2e + 2f = 59.5 mol, and the total mass constraint including ash gives 12a + 28b + 44c + 16d + 2e + 18f + 50(ash) = 1000 g.
> 
> Checking against literature values for slow pyrolysis around 500°C, I'm seeing char yields typically in the 25-35% range, which suggests roughly 20 mol of solid carbon (about 240 g of pure carbon plus ash bringing it to around 252 g total).
> 
> For the gas phase, I'd expect CO₂ to dominate with some CO, methane, and hydrogen present.
> 
> Testing with a = 20, d = 2, e = 3: the hydrogen balance gives f = 22.75 mol H₂O (409.5 g), but then the oxygen constraint becomes impossible—I'd need b + 2c = 3.5 while b + c = 17.1, which yields negative CO₂. The fundamental issue is that there isn't enough oxygen in the biomass to convert all the hydrogen to water while also producing significant CO₂ from the remaining carbon.
> 
> Reducing water production to f = 5 (90 g) frees up oxygen for CO₂ formation, giving b + 2c = 21.25 paired with b + c = 17.1, which now yields c = 4.15 mol CO₂—finally a feasible solution. 3.5 + 24 = 59.5 ✓
> 
> So with a = 14.0 mol of solid carbon, d = 8.0 mol of methane, e = 1.75 mol of hydrogen gas, and f = 12.0 mol of water, I'm verifying the carbon balance across all species to make sure everything checks out.
> 
> Working through the oxygen constraint: if b + c = 17.1 from the carbon balance, but the oxygen balance requires b + 2c = 14.25, then solving these simultaneously gives c = -2.85, which is impossible. This reveals a fundamental issue—the biomass simply doesn't have enough oxygen relative to its carbon content to form significant amounts of CO and CO₂ while also producing water. In a pyrolysis scenario without external oxygen, the oxygen-poor feedstock severely limits how much carbon can gasify into these oxygenated products.
> 
> So I'm shifting my approach: most of the carbon should remain as solid char, with the volatile fraction forming methane instead. Testing this with 25 moles of solid carbon, 12 moles of methane, and 1.5 moles of hydrogen gives only 2.1 moles of carbon in volatile gases. The hydrogen balance then yields 4.25 moles of water, and checking the oxygen balance: b + 2c = 22.0, which is now feasible since I have much less volatile carbon to account for.
> 
> But wait—this creates a contradiction: b + c = 2.1, so c can't equal 19.9. The real problem is that there's far too much oxygen relative to what the volatile carbon species can accommodate. Most of the oxygen must end up as CO₂ or H₂O. Let me test the opposite extreme: all volatile carbon converts to CO₂ with no other carbon-containing gases. This uses 28.2 moles of oxygen just for CO₂, but I only have 26.25 moles total—so even this extreme scenario is impossible.
> 
> Since CO uses only one oxygen per carbon while CO₂ uses two, I should try converting all volatile carbon to CO instead. This uses just 14.1 moles of oxygen for CO, leaving 12.15 moles for water, which corresponds to 218.7 grams. The remaining hydrogen after accounting for water in H₂O would then distribute between methane and hydrogen gas, but I'm running into another issue here.
> 
> If I've already committed all the volatile carbon to CO, there's nothing left for methane formation. That leaves me with 35.2 moles of hydrogen going entirely to H₂, which gives 17.6 moles or 35.2 grams of hydrogen gas—an unrealistically high yield. Plus having zero methane doesn't match what actually happens in biomass pyrolysis.
> 
> The fundamental problem is that real pyrolysis isn't a simple closed-system thermal decomposition. The actual process involves complex tar formation and secondary reactions that don't follow basic stoichiometry, so I should probably just reference the empirical product distributions from literature instead of trying to force this calculation.
> 
> Looking at slow pyrolysis data for woody biomass around 500°C, the typical yields per kilogram of dry biomass are biochar at 350 grams, bio-oil around 300 grams, water from the process at 100 grams, and non-condensable gases totaling 250 grams—which breaks down into carbon dioxide, carbon monoxide, methane, hydrogen, and heavier hydrocarbons. These proportions add up to the full kilogram and represent what actually happens in practice.
> 
> Now I'm mapping these outputs to available model resources. Biochar converts straightforwardly into carbon and industrial waste for the ash content, but bio-oil is trickier since it's a complex oxygenated liquid with roughly 55% carbon, 7% hydrogen, and 35% oxygen. I don't have a perfect model resource match for this—I could force it into liquid hydrocarbons but that's not accurate, or I could try decomposing it into components, or potentially use the organics resource which has a similar elemental profile with carbon, hydrogen, nitrogen, oxygen, and sulfur.
> 
> Looking at the numbers more carefully, if I conceptually break down 300 grams of bio-oil by its elemental composition, I get about 165g carbon, 21g hydrogen, and 105g oxygen. Methanol could work as a proxy since it's an oxygenated hydrocarbon, but the carbon-to-oxygen ratio doesn't quite match—bio-oil sits at 1.57 while methanol is only 0.80, so there's a significant mismatch in how oxygen-rich each compound is.
> 
> Since the pyrolysis process explicitly produces methane, carbon monoxide, carbon dioxide, and bio-oil as co-products, I need some way to represent the bio-oil in the simulation. Methanol seems like the most reasonable approximation among available resources—it's an oxygenated liquid that's already in the system and has practical applications. I could alternatively decompose the bio-oil further into carbon and water as if it undergoes additional processing, but for now, treating it as methanol feels like a workable simplification.
> 
> Let me just commit to a straightforward output profile: the carbonization operation yields biochar as the main product, with methane, hydrogen, carbon monoxide, carbon dioxide, water, and methanol as co-products, plus industrial waste for the ash residue.
> 
> I'm setting the yields per kilogram of feedstock—carbon at 300 grams, methanol at 200 grams to represent the bio-oil, then smaller quantities of the gaseous outputs and water, with 50 grams of ash waste. The numbers balance to 1000 grams total, which works as a reasonable approximation even though treating bio-oil as pure methanol is simplified.
> 
> For the energy side, slow pyrolysis requires moderate heating around 500°C, and while the char formation is exothermic, the volatile release is endothermic, so the net energy demand is roughly 1-2 megajoules per kilogram of biomass. The biomass carbonization process produces quite a few different outputs, which makes sense given what's actually happening chemically. Now I'm moving on to the CO₂ methanation reaction using the Sabatier process, where I need to work through the stoichiometry and mass balance to determine the input and output fractions.
> 
> I'm calculating the mass fractions for both inputs and outputs at a 1 ton per hour flow rate, then determining the energy released since this reaction is highly exothermic.
> 
> Now I'm working through the auxiliary power requirements for the process—compression, pumping, and catalyst heating consume about 0.085 MW for this scale—and factoring in the overall efficiency of around 85% for an established catalyst-based system. The heat output calculation is getting complex though, since I need to account for both the exothermic reaction and the electricity input.
> 
> Looking at how the model handles exothermic processes like steelmaking, it seems the electricity value captures just the auxiliary power needs, while the reaction heat gets folded into the overall heat budget. But the Sabatier reaction generates 0.88 MW of heat, which is substantially more than the 0.085 MW of electrical input—I need to figure out how the schema actually captures this larger thermal output. less than the reaction heat. So it seems like reaction heat is NOT included in the "heat" column. Only electrical waste heat.
> 
> Actually, let me check if efficiency already factors this in. If η = 0.88 and electricity = 0.03, then heat = 0.03/0.88 = 0.034, which matches the table exactly. So the heat column is just the electrical energy divided by efficiency—it's not capturing the chemical reaction heat at all. The model appears to treat reaction heat separately from the electrical waste heat, likely as part of the thermodynamic calculations elsewhere in the simulation. For processes like BOF steelmaking that are chemically driven, the electricity is just auxiliary power, and the exothermic reaction energy is handled independently.
> 
> Looking at the schema notes, it's clear now: the electricity column covers auxiliary requirements for exothermic reactions, while the heat column captures only the thermal equivalent of that electrical input. For endothermic processes powered by electricity, the electricity covers the full energy need. So for CO₂ Methanation, which is exothermic, the electricity is just for compression and controls at around -0.09, and the heat would be calculated from that auxiliary power divided by the efficiency.
> 
> Now I'm setting up the parameters for Steam Methane Reforming, which is endothermic and requires significant heat input. I need to work through the stoichiometry and mass balances for the methane and water inputs converting to carbon monoxide and hydrogen. Now I'm realizing the schema specifies electricity rather than natural gas combustion for heat, so I need to recalculate based on actual SMR energy requirements—around 10-12 kWh thermal per kilogram of hydrogen produced, which would give roughly 1.8-2.1 MW for the 178 kg/h output. (but ~3.5-4 kWh/kg from the fuel combustion).
> 
> Wait, let me use the thermodynamic approach:
> ΔH = 206 kJ/mol CH₄ = 206/16.043 = 12.84 kJ/g CH₄ = 12.84 MJ/kg CH₄
> 
> Per t/h: 471 × 12.84 = 6048 MJ/h = 1.68 MW (matches above)
> 
> η = 0.75 (high-temperature reforming with heat recovery)
> electricity = 1.68/0.75 = 2.24 MW
> heat = 2.24/0.75 = 2.99
> 
> Wait, I'm confused again. Let me be clear:
> - Useful energy needed = ΔH = 1.68 MW
> - η = fraction of electricity that does useful work
> - electricity = useful_energy / η = 1.68/0.75 = 2.24 MW
> - heat = |electricity|/η = 2.24/0.75 = 2.99 MW
> 
> This feels circular though. Let me check the water electrolysis case instead: the enthalpy requirement is 4.41 MW with 80% efficiency, so I'd need 5.51 MW of electricity input, and the waste heat would be around 1.38 MW.
> 
> Actually, I think I'm overcomplicating this. The waste heat is just the input electricity minus the useful work: 5.51 - 4.41 = 1.10 MW. And when I calculate heat as |E|/η, that's really the total thermal energy budget, not the waste. For the electrolysis case, useful work equals the enthalpy change (4.41 MW), waste is the inefficiency loss (1.10 MW), and that total thermal budget of 6.89 MW represents something different depending on the context—for power generation it's the thermal input, but for consuming operations like this, I'm still not sure what it represents exactly.
> 
> I think heat = |E|/η is just a convention in the schema, and the actual waste heat is calculated differently. For the SMR case, I need to figure out whether to treat it as purely electrical input or account for the methane combustion that traditionally provides the heat. The schema says to use electricity, but SMR normally burns methane for about 30-40% of the thermal energy, so I'm trying to figure out if this is an exception where I should use the direct operation inputs instead.
> 
> For an electrified SMR where electricity provides all the heat, I'll set electricity to -2.25 MW and heat to 3.00 MW with an efficiency of 0.75. Now I'm defining the full process parameters including the methane and water inputs that produce carbon monoxide and hydrogen.
> 
> Moving to the water-gas shift reaction, I'm calculating the stoichiometry where carbon monoxide and water convert to carbon dioxide and hydrogen. The molecular weights balance out to 46.025 on both sides, giving me input fractions of 0.609 for CO and 0.391 for water, which produce 0.956 CO₂ and 0.044 H₂ at a mass flow of 1.
> 
> Now I'm working through the energy requirements—this reaction is mildly exothermic at -41.2 kJ/mol, so it mainly needs auxiliary electricity for pumping and heating to reaction temperature. I'm setting electricity at -0.05 with an efficiency of 0.85, which gives me a heat requirement of about 0.059. The key insight here is that while CO₂ is produced, the real goal of this step is maximizing hydrogen output, so that's where the focus should be. I'm going back and forth on whether to include CO₂ as a target product—the schema suggests including everything except waste, but CO₂ is genuinely unwanted in this reaction and typically captured or recycled rather than valued. I'll stick with HYDROGEN as the sole target. Now moving on to the reverse water-gas shift reaction with CO₂ input of 0.956 and H₂ at 0.
> 
> 044, producing CO and water. This is endothermic and requires significant heating to 400-600°C, so I'm calculating the energy demand: about 895 MJ per hour, which translates to roughly 0.33 MW of electricity needed at 75% efficiency, with heat requirements around 0.44 MW.
> 
> Now I'm moving on to the Fischer-Tropsch synthesis step, where carbon monoxide and hydrogen combine to form liquid hydrocarbons—I'll use octane as a representative product, which requires 8 moles of CO and 17 moles of H₂ to produce one mole of octane and 8 moles of water.
> 
> Calculating the molecular weights and mass balance, the reactants total 258.35 mass units, which matches the products perfectly. This gives me input fractions of 0.867 for carbon monoxide and 0.133 for hydrogen, while the outputs split into 0.442 for liquid hydrocarbons and 0.558 for water.
> 
> Now I'm accounting for the fact that real Fischer-Tropsch synthesis produces a distribution of products rather than just octane—typically some methane, lighter C₂-C₄ hydrocarbons, and heavier waxes following an Anderson-Schulz-Flory distribution. With a cobalt catalyst around α = 0.85, I'd expect roughly 80% C₅+ liquids, 10% methane, and 10% lighter gases, though I can treat the lighter fractions as recyclable and use the octane stoichiometry as a reasonable baseline approximation.
> 
> Since the Fischer-Tropsch reaction is strongly exothermic at around -150 kJ/mol CO, I'm setting up the process with minimal auxiliary electricity for compression, pumping, and separation—about 0.10 units—and accounting for the heat recovery with an 80% efficiency factor, which gives me 0.125 units of usable heat output. I'm defining the LIQ_HYDROCARBONS_SYNTH process with these energy parameters and the stoichiometric inputs and outputs.
> 
> Now moving to CO₂ methanol synthesis, which follows a similar pattern but is much less exothermic at -49.5 kJ/mol CO₂. I'm calculating the mass balance from the stoichiometry: 44 parts CO₂ plus 6 parts H₂ yields 32 parts methanol and 18 parts water, giving me input fractions of 0.879 for CO₂ and 0.121 for H₂, with output fractions of 0.640 for methanol and 0.360 for water. The process requires about 0.08 units of auxiliary electricity for compression.
> 
> For the actual synthesis parameters, I'm accounting for the compression, heating to around 250°C, and separation stages with an 80% efficiency, which means the heat requirement comes out to 0.10 units when I divide the electricity input by the efficiency factor. I'm now setting up the CO methanol synthesis reaction, which uses CO instead of CO₂ with a 2:1 hydrogen ratio, and the molecular weights work out to give a clean 32-unit mass balance.
> 
> Now I'm working through the CO methanol synthesis where the reaction is simpler since there's no water byproduct—just pure methanol output. The input fractions are CO at 0.874 and hydrogen at 0.126, with the reaction being exothermic at -90.5 kJ/mol CO. I'm estimating the auxiliary electricity at 0.06 units with an 85% efficiency, giving me a heat requirement of about 0.0706 units. Moving on to ammonia synthesis next.
> 
> For the Haber-Bosch process, I'm looking at the stoichiometry where nitrogen and three parts hydrogen produce two parts ammonia. The mass balance checks out at 34.062 units on both sides. The input fractions are nitrogen at 0.822 and hydrogen at 0.178, producing pure ammonia. This reaction is also exothermic at -92.2 kJ/mol N₂, but it demands significant energy for compression to 150-200 bar and heating to 400-500°C, so the energy consumption is much more substantial than the methanol synthesis.
> 
> The total energy requirement for ammonia synthesis is around 8-10 kWh per kilogram of ammonia produced, which includes the compression work. At a production rate of 1 ton per hour, that translates to 9 megawatts. I need to be careful here though—that figure includes the energy to produce the hydrogen feedstock via steam methane reforming. If I'm looking at just the synthesis step itself with hydrogen and nitrogen already available, the compression and heating alone would be more like 1-2 kWh per kilogram of ammonia.
> 
> Breaking down the synthesis step further: compression takes about 1-2 kWh/kg, the reactor heating around 0.5-1 kWh/kg, and refrigeration for product separation another 0.3-0.5 kWh/kg, bringing the total to roughly 2-3 kWh/kg. At 2.5 kWh/kg for 1 ton per hour, I'm looking at 2.5 megawatts of electricity. Since the reaction is exothermic, there's heat being released that I can partially recover, and assuming an efficiency of about 80% with some heat recovery, the electricity demand would be around 2.5 megawatts with roughly 3.1 megawatts of thermal energy to manage. I should verify this against the actual single-pass conversion efficiency of the Haber-Bosch process to make sure my numbers are reasonable.
> 
> Actually, accounting for the recycling loop and using a more conservative estimate for an advanced electric process, I'm settling on about 1.5-2 kWh/kg for ammonia synthesis. Using 1.5 kWh/kg as a forward-looking figure, that gives me 1.88 megawatts of thermal energy at 80% efficiency. I'm setting up the ammonia synthesis parameters with electricity at -1.50, heat at 1.88, and the stoichiometric ratio of nitrogen to hydrogen feeding into ammonia production at a 1 ton per hour mass flow.
> 
> Now moving on to polymer synthesis from monomers.
> 
> For polymer production, I need to account for the hydrocarbon feedstock coming from petrochemical cracking or alternative routes, plus the industrial chemicals for catalysts and additives. The key inputs are liquid hydrocarbons as naphtha, industrial chemicals for the polymerization process, and potentially carbon monoxide or methanol if using alternative feedstock pathways. The resulting polymer composition breaks down to roughly 73% carbon, 10% hydrogen, with smaller fractions of chlorine, oxygen, nitrogen, sulfur, and fluorine depending on the specific polymer type.
> 
> Now I'm working through the mass balance—naphtha cracking yields ethylene and propylene as primary monomers, with polymerization converting those monomers at high efficiency. The overall conversion from raw naphtha to finished polymer involves significant losses to byproducts and off-gases, so I'm estimating roughly 1.5 to 2 tonnes of naphtha feedstock needed per tonne of final polymer product.
> 
> For the process model with a 1 t/h throughput, I need to account for all inputs and outputs: naphtha and industrial chemicals going in, with polymer as the main product alongside various byproducts like methane, hydrogen, carbon dioxide, water from condensation, and industrial waste from catalyst residue. The key is balancing these flows so the total input mass equals total output mass. I'm balancing the water flows—there's more water coming out than going in, so I need to adjust the inputs to account for that net production. Let me recalibrate the feedstock composition to make the mass balance work properly.
> 
> Now I'm verifying the totals: the inputs and outputs both sum to 0.850, which confirms the stoichiometry is consistent. Since the process needs to operate at a mass flow of 1, I'm scaling everything up by a factor of about 1.176 to normalize the system.
> 
> After scaling, the polymer yield comes out to around 53%, which initially seems low, but I'm realizing this operation only represents the polymerization step itself—the naphtha cracking that would precede it would be modeled as a separate unit operation upstream.
> 
> Actually, looking at the operation description more carefully, it does include the cracking step as part of the overall process, so that 53% yield reflects the reality that naphtha cracking only converts 40-50% of the feed into usable olefin monomers, with the remainder becoming fuel gases, heavy oil, and other byproducts. Since this is meant to be a general operation, I'll stick with naphtha as the primary feedstock and bump the polymer yield up slightly to 55% to better reflect typical industrial performance.
> 
> Now I'm setting up the mass balance with the inputs and outputs, checking that everything sums to 1.0 across both sides. For the energy requirements, steam cracking is the energy-intensive step at around 20 GJ per ton of ethylene produced, while polymerization itself is mildly exothermic, so the combined process energy demand is dominated by the cracking phase.
> 
> Scaling this to the actual throughput: with 300 kg of ethylene coming from 820 kg of naphtha input at a 1 ton per hour mass flow, the cracking step requires about 1.67 MW, and polymerization adds roughly 0.7 MW, bringing the total energy demand to approximately 2.37 MW.
> 
> Now I'm realizing steam cracking traditionally relies on fired heaters burning fuel gas byproducts, but since our model treats methane and hydrogen as separate outputs, I need to account for that heat electrically instead. Using 2.0 MW of electricity with a 0.75 thermal efficiency gives me about 2.67 MW of heat for the polymer synthesis process.
> 
> For industrial chemicals, I'm looking at the composition breakdown and the various synthesis pathways—chlor-alkali, sulfuric acid, nitric acid, and phosphoric acid processes—each with their own input and output streams that I need to balance against the target chemical composition. Looking at the major industrial chemical pathways, I'm mapping out how each one connects to the available inputs—chlor-alkali producing sodium hydroxide and chlorine from salt and water, nitric acid from ammonia oxidation, phosphoric acid from phosphate rock and sulfuric acid, and soda ash through either the Solvay process or direct mining. Now I need to work backward from the elemental composition of this industrial chemicals blend to figure out what raw material inputs are actually required across all these interconnected processes. I'm realizing this initial blend is too rough—I need to recalculate based on the actual elemental composition of the output. So for 1000 kg of industrial chemicals, I need to work backwards from the required amounts of each element: 450 kg oxygen, 180 kg carbon, 100 kg chlorine, and so on, then figure out which source materials provide those elements most efficiently.
> 
> Now I'm mapping out the input resources systematically—sulfur provides most of the sulfur content I need, ammonia covers the nitrogen while also contributing some hydrogen, and industrial minerals can supply oxygen, calcium, phosphorus, and other trace elements. I'm calculating the exact quantities needed from each source by dividing the required element mass by its concentration in that material.
> 
> The challenge is that industrial minerals has very low sodium and chlorine content—only 2.1% each—which means I'd need an enormous amount (around 3,800 kg) just to get the 80 kg of sodium I need. In practice, salt is the primary feedstock for these processes and contains much higher concentrations of sodium and chlorine, but my model groups it into industrial minerals, so I'll need to work with a large quantity of that resource to meet the sodium and chlorine requirements. 2.1% Na: need 3810 kg
>    For Cl (100 kg) at 2.1% Cl: need 4762 kg
>    For P (17 kg) at 2.1% P: need 810 kg
> 
>    This would dominate the inputs (>3000 kg for 1000 kg output), and the excess elements (Si, Ca, Fe, Al, etc.) would need to be output as waste.
> 
>    Actually, this tells me the model's INDUST_MINERALS composition is not appropriate for chemical salt feedstock. The INDUST_MINERALS resource is a blend heavily weighted toward construction minerals. For the chemical industry, the key minerals are salt (NaCl), phosphate rock, limestone, etc.
> 
>    In the model, I should use INDUST_MINERALS as the best available resource for these inputs, accepting that the model is an approximation. Let me take a more pragmatic approach and work with what's available, even if the stoichiometry isn't perfect. I think the right way to handle these complex manufacturing operations is to think about where each element in the final product comes from—breaking down INDUST_CHEMICALS by composition and sourcing the oxygen from water or minerals, the carbon from hydrocarbons or methanol, the chlorine and sodium from industrial minerals, sulfur from dedicated sulfur resources, and hydrogen from water.
> 
>    For a 1-ton output, I need roughly 4000 kg of industrial minerals to get the required sodium and chlorine, which produces about 3000 kg of waste—a realistic 4:1 input-to-output ratio for industrial chemical production. more than the 17 kg needed, so there's significant waste. At a normalized mass flow of 1 t/h, I'd only get about 0.2 t/h of INDUST_CHEMICALS output, which is reasonable for a low-yield refining process. Let me work through a complete mass balance per ton of product, accounting for minerals, sulfur, ammonia, and methanol as carbon and hydrogen sources.
> 
> Calculating the elemental contributions from methanol and summing all inputs—minerals, sulfur, ammonia, methanol, water, and oxygen—I get roughly 5 kg of input for every 1 kg of INDUST_CHEMICALS produced, meaning about 4 tons of waste per ton of product.
> 
> Now I'm scaling these values down to a 1 ton per hour mass flow rate, which gives me the fractional inputs and outputs that should balance to 1.0 total. The inputs add up to 0.999 and outputs to 1.0, which checks out, though something about this ratio seems off.
> 
> The waste output is way too high at 79.9% - that's because industrial minerals as a feedstock aren't really suited for chemical production since they're mostly construction-grade materials. I need to rethink this. Maybe I should treat the industrial minerals input as just the relevant fractions like salt, phosphate rock, and limestone rather than the full blend. Or perhaps I'm overestimating how much mineral feedstock is needed, since the industrial chemicals output already includes all the mass from other sources like oxygen, carbon, and hydrogen. Let me try adjusting the input proportions.
> 
> I'm testing a leaner mix with sulfur, ammonia, methanol as the main carbon source, some industrial minerals, water, and oxygen. But I'm running into the same problem - the chlor-alkali process needs massive amounts of purified salt brine, and at only 2.1% sodium and chlorine content in the industrial minerals, I'd need thousands of kilograms just to get the 80 kg sodium and 100 kg chlorine the model requires.
> 
> I'm realizing that while salt production is huge globally and likely part of the industrial minerals blend, the composition table shows it at just 2.1% for both elements. Rather than trying to justify an unrealistic input quantity or oversimplify the output, I think the most practical approach is to estimate a reasonable input-to-output ratio for the whole industrial chemicals production process - something in the 2-3 tonne range seems workable.
> 
> Now I'm sketching out a simpler mass balance model where I specify the inputs needed per tonne of output: industrial minerals at 0.4 t/h to cover the sodium, chlorine, phosphorus and calcium; sulfur at 0.06; ammonia at 0.015; methanol at 0.12 for the carbon and organic components; plus water and oxygen to balance things out. The outputs would be the industrial chemicals product itself at 0.5 t/h, water at 0.3 t/h, and waste at 0.2 t/h. I notice water appears on both sides - 0.2 t/h in but 0.3 t/h out - which means there's a net water generation of 0.1 t/h from the process.
> 
> Now I'm consolidating this into a net balance and checking that everything adds up. The inputs total 0.8 t/h and outputs also total 0.8 t/h, so the mass balance closes. To scale this to a 1 t/h throughput, I need to multiply everything by 1.25, which gives me the final input requirements of industrial minerals at 0.5 t/h, sulfur at 0.075, ammonia at 0.019, and so on.
> 
> The output side scales proportionally—industrial chemicals at 0.625 t/h, water at 0.125, and waste at 0.25—and both sides now sum to exactly 1.0 t/h. This represents a 62.5% product yield, which feels reasonable for a simplified industrial process. However, I'm now thinking through the elemental composition to make sure the chemistry actually makes sense. Starting with the industrial minerals input at 0.5 t/h, I'm breaking down what elements that contains and checking whether the outputs could plausibly come from those inputs. Now I'm checking the material balance for the output streams and I'm seeing a critical issue—sodium and chlorine are severely undersupplied. The INDUST_MINERALS source only contains 2.1% of each, which falls far short of what the INDUST_CHEMICALS product requires, creating deficits of about 40 units for sodium and 52 units for chlorine.
> 
> I could scale up INDUST_MINERALS significantly to meet these demands, but that would require nearly 2.4 tons of input to balance just the sodium, which would bloat the overall mass flow. I think I need to accept this as a model constraint and either work with reduced INDUST_CHEMICALS output or acknowledge that the current feedstock composition can't support the target product specifications.
> 
> Actually, I'm overcomplicating this. The schema allows for catchall resources and the model is meant to be approximate anyway. Let me just define reasonable input and output proportions for the synthesis process without getting hung up on perfect elemental balance. I'll set up the INDUST_CHEMICALS_SYNTH reaction with INDUST_MINERALS, SULFUR, AMMONIA, METHANOL, and OXYGEN as inputs, producing INDUST_CHEMICALS, WATER, and INDUST_WASTE as outputs, using proportions that make practical sense for an industrial synthesis.
> 
> Now I'm checking the energy requirements for industrial chemical production. Different processes have different intensities—chlor-alkali is particularly demanding at around 2.5 kWh per kilogram of product, while sulfuric acid is actually exothermic and generates steam, and nitric acid falls somewhere in between. When I weight these together across typical industrial chemical production, the energy requirements roughly balance out.
> 
> For the specific synthesis pathway producing 0.550 tonnes per hour with an estimated energy intensity of 1.0 kWh per kilogram, that translates to 0.55 megawatts of electricity demand. Accounting for an 80% process efficiency, I need to supply 0.688 megawatts of heat input to achieve that output. I'm now defining the industrial chemicals synthesis process with these energy parameters, the material flows I calculated earlier, and specifying industrial chemicals as the target product.
> 
> Next I'm looking at fine chemicals synthesis and examining the elemental composition of fine chemical products. Now I'm calculating the energy requirements—at 3 kWh per kilogram for 0.5 tons per hour, that's 1.5 megawatts of electricity needed, which translates to 2 megawatts of heat input at 75% efficiency. I'm defining the fine chemicals synthesis process with these energy parameters and specifying the input and output material flows.
> 
> For concrete production, I'm looking at the composition and the key process: limestone calcination to produce calcium oxide and carbon dioxide, then mixing with aggregates and water. Per ton of concrete, I need about 150 kilograms of cement from the limestone, 700 kilograms of stone aggregate, and 100 kilograms of water.
> 
> The calcination process itself generates roughly 75 kilograms of CO₂ per ton of concrete produced. The inputs are stone for aggregate, industrial minerals like limestone and gypsum for the cement component, and water, which combine to create the final concrete product.
> 
> Now I'm working through the mass balance—the inputs total 1 ton and the outputs (concrete plus the CO₂ released) also balance to 1 ton. For the energy requirements, cement production is energy-intensive at around 3.5 gigajoules per ton, so for the 150 kilograms of cement in each ton of concrete, that's roughly 0.15 megawatts, plus additional energy for mixing and curing brings the total to about 0.17 megawatts with a 75% efficiency factor.
> 
> For the electrified kiln process, I need to account for the electricity input and the heat requirement—dividing the total energy by the efficiency gives me about 0.23 megawatts of heat needed. But I'm realizing I need to be more careful about what exactly I'm calculating here: the concrete production operation encompasses both the cement calcination in the kiln and the actual concrete mixing, so I should clarify whether I'm looking at the energy per ton of cement produced or per ton of final concrete output.
> 
> Looking at the mass flows, the INDUST_MINERALS stream at 0.250 t/h contains the limestone that undergoes calcination, while the STONE is inert aggregate. If I assume roughly 60% of those minerals are limestone requiring the full calcination energy of 3.5 GJ/t, that works out to about 0.146 megawatts of thermal energy, which translates to roughly 0.195 megawatts of electricity demand at 75% efficiency—plus whatever additional energy the mixing step requires.
> 
> So the concrete production process needs about 0.22 megawatts of electricity input, which means I'm drawing 0.22 megawatts from the grid, and the heat requirement comes to about 0.293 megawatts at that efficiency level. Now I'm moving on to characterize the glass and ceramics production pathway, which will draw from similar industrial mineral inputs like sand, soda ash, and limestone, producing the glass-ceramics product along with carbon dioxide as a byproduct.
> 
> For the glass production specifically, I'm calculating the material balance per ton of output—about 700 kilograms of sand, 200 kilograms of soda ash, and 60 kilograms of limestone, with minor additives making up the rest. The decomposition reactions release carbon dioxide from both the soda ash and limestone, totaling roughly 109 kilograms of CO₂ per ton of glass produced.
> 
> Now I'm setting up the model parameters where 1 ton of industrial minerals input yields 0.890 tons of glass and 0.110 tons of CO₂ as outputs, which balances perfectly. For the energy requirements, glass melting demands around 5 gigajoules per ton, and accounting for a 70% efficiency factor, I'm calculating that 0.890 tons of glass output requires approximately 1.77 megawatts of electricity input.
> 
> Refining those numbers, I get electricity at -1.75 and heat at 2.50 with the 0.70 efficiency ratio. So the glass ceramics production model takes industrial minerals as input, produces glass ceramics and carbon dioxide in those proportions, and requires those specific electricity and heat parameters.
> 
> Now I'm moving on to semiconductor production, which involves a much more complex material composition—primarily high-purity silicon along with arsenic, gallium, carbon, and trace elements.
> 
> The challenge is figuring out where to source the silicon from. The model doesn't have a dedicated silicon resource, so I'm looking at what's available: industrial minerals contain about 18% silicon, stone has 21%, and regolith has 22%. The production process itself requires starting with very pure quartz, reducing it with carbon, then purifying it further—but the available resources are too dilute for this to work cleanly. I'd need massive quantities of industrial minerals to get the silicon I need, which would generate enormous waste. This is the same issue I ran into with industrial chemicals. I think the pragmatic solution here is to just use industrial minerals as the input source and accept the model's limitations.
> 
> Now I'm checking the mass balance on the recipe I've sketched out: taking 0.5 tons of industrial minerals, 0.1 tons of carbon, 0.05 tons of fine chemicals, 0.1 tons of industrial chemicals, and 0.25 tons of oxygen to produce 0.2 tons of semiconductor material, 0.35 tons of carbon dioxide, 0.35 tons of industrial waste, and 0.1 tons of water. The inputs and outputs both sum to 1.0, so the balance works. A 20% yield seems low, but semiconductor production really is that wasteful in reality.
> 
> For energy requirements, semiconductor-grade silicon production is extremely energy-intensive—around 130 kilowatt-hours per kilogram when you factor in the full Czochralski process and multiple purification steps. At a production rate of 0.2 tons per hour, that's 26 megawatts, which is enormous but actually realistic for this kind of operation.
> 
> Breaking down the full chain: carbothermal reduction takes about 12 kilowatt-hours per kilogram, the Siemens purification process adds roughly 120 kilowatt-hours per kilogram, and Czochralski crystal growth adds another 30 kilowatt-hours per kilogram, totaling around 160 kilowatt-hours per kilogram for electronic-grade silicon. At 200 kilograms per hour, that's 32 megawatts. Since the operation description includes the entire chain from raw silicon through refining, crystal growth, and wafer fabrication, this high energy demand makes sense. I need to figure out whether this energy scales with the total mass flow of 1 ton per hour or just the 0.2 ton per hour of semiconductor product, and then calculate the corresponding heat requirement using an efficiency factor. I'm settling on these parameters because the 20% yield is realistic for semiconductor manufacturing—the Siemens process generates substantial byproducts like silicon tetrachloride and hydrochloric acid. So I'm defining the production process with electricity at -20 MW, heat output of 30.8 MW, and 0.65 efficiency, taking inputs from industrial minerals, carbon, fine chemicals, and oxygen to produce semiconductors, carbon dioxide, industrial waste, and water.
> 
> Now I'm moving into composite materials. These are primarily glass fiber reinforced polymers at about 70% of the market, with carbon fiber composites making up another 20%. I need to map out how the inputs—polymers and glass ceramics for the fiberglass route, plus carbon precursors for carbon fiber—combine to create the final composite products. ERS (PAN fiber → carbon fiber)
> - Polymer matrix: ~40%
> 
> For a production-weighted blend:
> - POLYMERS: 0.400 (matrix material + PAN precursor for carbon fiber)
> - GLASS_CERAMICS: 0.450 (glass fiber)
> - CARBON: 0.100 (carbon fiber for CFRP fraction)
> - INDUST_CHEMICALS: 0.050 (curing agents, sizing, adhesion promoters)
> 
> Outputs:
> - COMPOSITES: 0.900
> - INDUST_WASTE: 0.100 (trimming waste, process scrap)
> 
> Input: 0.400 + 0.450 + 0.100 + 0.050 = 1.000
> Output: 0.900 + 0.100 = 1.000 ✓
> 
> Energy: Composite production involves mixing, layup, and curing. Autoclave curing runs 5-10 kWh/kg while out-of-autoclave processes use 2-5 kWh/kg, so I'm settling on 2 kWh/kg as a more realistic estimate for the curing energy requirement at this production scale.
> 
> At 900 kg/h throughput, that's 1.8 MW of thermal energy needed, which translates to 1.8 MWe of electricity input with a 0.75 efficiency factor, requiring 2.4 MW of heat output.
> 
> Now I'm moving on to solid fuel production, which uses ammonium perchlorate-based composite propellant with aluminum powder and HTPB binder, and I need to map out the chemical inputs and mass fractions for this process. Now I'm verifying the mass balance checks out, then working through the energy requirements for the mixing and casting process. I initially estimated around 0.5-1 kWh per kilogram, but that seems high for mainly mechanical mixing, so I'm revising down to 0.3 kWh/kg, which gives me about 0.29 MW of electricity needed and roughly 0.341 MW of heat output accounting for the 0.85 efficiency factor.
> 
> Now I'm setting up the solid fuel production process with its inputs from industrial chemicals, aluminum, polymers, and fine chemicals, producing solid fuels as the main output with a small waste stream. Moving on to fertilizer production next, where I need to work out the composition and input requirements for a blended NPK fertilizer.
> 
> I'm calculating the ammonia requirement at 310 kg per ton of fertilizer to meet the nitrogen content, but running into a problem with the industrial minerals approach—the phosphate and potash concentrations are too dilute, requiring impractically large quantities of raw material. I need to shift to using more concentrated phosphate rock and potash sources directly instead of relying on a generic industrial minerals blend. Potash (KCl): ~200 kg (at ~52% K₂O → 43% K)
> - Sulfur/H₂SO₄: ~50 kg
> - Limestone: ~50 kg
> - Water: ~100 kg
> 
> Total: ~1010 kg
> Product: 1000 kg fertilizer + some waste
> 
> In the model, phosphate rock, potash, and limestone all map to INDUST_MINERALS. Potash at 43% K vs INDUST_MINERALS at 0.95% K means we'd need ~100× the mass. This really doesn't work.
> 
> I think for the model, I should just use INDUST_MINERALS as the mineral input and accept the approximation. The model isn't meant to capture every elemental balance perfectly.
> 
> Now I'm setting up the mass flows at 1 t/h: ammonia, industrial minerals, sulfur, industrial chemicals, and water going in, with fertilizers, water, and waste coming out. The water balance is tricky though—I'm getting net water output, but the schema says to consider inputs and outputs as a net reaction, so I should account for the water that's actually consumed in the process rather than treating input and output water separately.
> 
> After netting the water flows and scaling everything to a mass flow of 1, I'm checking that the inputs and outputs balance. Now I need to estimate the energy requirements for the fertilizer compounding and granulation process. Now I'm finishing the fertilizer production entry with the remaining input rates and output specifications, then I need to format all the conversion and synthesis operations as TSV rows following the established table structure where fuel_rate stays empty for refining operations and target_products gets populated instead. I'm finishing up the production processes list and now need to verify the mass balances for each operation using three significant figures. I'm also reconsidering whether the current refining operations need adjustments—particularly REGOLITH_PROCESSING, where the depleted regolith slag output as INDUST_WASTE seems appropriate since it can then flow into the incineration recycling pathway. Now I'm finalizing the coal and biomass gasification parameters, specifying their inputs like coal with oxygen and water, and their outputs including carbon monoxide, hydrogen, and various byproducts with their respective proportions. Continuing through the process definitions, I'm looking at the output compositions and efficiency metrics for CO2 methanation, which converts carbon dioxide and hydrogen into methane and water, followed by steam methane reforming that breaks down methane and water to produce carbon monoxide and hydrogen.
> 
> For the water-gas shift reaction, I'm debating whether to include CO₂ as a target output alongside hydrogen. The reaction converts carbon monoxide and water into carbon dioxide and hydrogen, but since the primary value is in the hydrogen production, I'll keep the target focused there rather than listing all products.
> 
> Moving on to reverse water-gas shift and the liquid hydrocarbons synthesis reaction, I'm setting up their respective parameters with carbon monoxide as the target for the former and continuing with the synthesis inputs. Continuing through the synthesis process parameters, I'm looking at the industrial chemicals synthesis which takes minerals, sulfur, ammonia, methanol, and oxygen as inputs to produce chemicals, water, and waste. Then there's the fine chemicals synthesis that builds on industrial chemicals with methanol, metals, minerals, and oxygen as feedstocks.
> 
> Moving into material production now—concrete production uses stone, minerals, and water to create concrete with some carbon dioxide byproduct, while glass and ceramics production relies primarily on industrial minerals and generates glass ceramics with carbon dioxide emissions. Semiconductor production is significantly more complex, requiring minerals, carbon, fine chemicals, industrial chemicals, and oxygen in specific proportions to manufacture semiconductor materials. Now I'm verifying the mass balances for the remaining production processes—checking that inputs and outputs sum to 1.0 for each step to ensure the model is internally consistent. I'm verifying the remaining production processes and catching a rounding error in the fertilizer production inputs—adjusting the industrial minerals coefficient down by 0.001 to get the total to exactly 1.000.
> 
> Now I'm checking the heat-to-electricity conversion calculations by dividing each electricity value by its corresponding efficiency factor across all ten production processes. Finishing up the calculation checks, and they all look correct. Now I need to organize the output into two sections—one assessing REGOLITH's performance in existing refining operations, and another with new rows for conversions and synthesis. I'm also second-guessing a few of my estimates and need to reconsider those assumptions.
> 
> For the methane pyrolysis electricity requirement, I'm reconsidering whether -3.00 MW is too high. The reaction itself requires about 1.30 MW, and with a 70% efficiency that's roughly 1.86 MW. But accounting for the sensible heat needed to raise methane to 1000°C and process losses, 3.00 MW total seems more realistic—leaving about 1.70 MW for heating, which is reasonable for that temperature range. Let me work through the sensible heat calculation to verify this is sound.
> 
> Looking at the specific heat capacity of methane and the temperature rise, the heating requirement comes to around 0.61 MW, but the carbon product also exits at 1000°C carrying significant sensible heat. With effective heat recovery, the net energy demand drops to roughly 1.60 MW, which at 70% efficiency translates to about 2.29 MW of electricity. I'll adjust the electricity requirement down to -2.30 MW to be more conservative.
> 
> Actually, reconsidering the heat balance—my original -3.00 MW estimate already factored in real-world losses and the reality that perfect heat recovery isn't achievable, so I should stick with that more conservative figure. I'm also noticing that representing bio-oil as methanol in the carbonization operation is quite a simplification, since bio-oil is much heavier and more chemically complex than methanol.
> 
> Looking at the actual composition of bio-oil from slow pyrolysis—roughly 55% carbon, 7% hydrogen, 35% oxygen—it doesn't map cleanly onto any single product in my model. I could output it as methanol, liquid hydrocarbons, or break it down further into simpler components, but each approach involves trade-offs between accuracy and model complexity.
> 
> The real insight is that bio-oil is inherently a mixture that gets further processed in practice. So for BIOMASS_CARBONIZATION, I should probably assume the bio-oil undergoes thermal cracking to produce a cleaner set of outputs: carbon, methane, hydrogen, carbon monoxide, carbon dioxide, water, and industrial waste. This way the model captures the practical downstream processing rather than trying to represent the intermediate bio-oil directly.
> 
> Now I'm working through the mass balance per kilogram of biofeedstock to make sure everything adds up to 1000 grams, then checking that the carbon atoms are conserved across all the products.
> 
> I'm verifying the hydrogen balance now—accounting for hydrogen in methane, molecular hydrogen, water, and the carbon product—and I'm getting 49.1 grams total versus 60 grams input, so there's a discrepancy of about 10.9 grams I need to investigate.
> 
> Let me try adjusting the product amounts: increasing methane to 60 and water to 252 while reducing carbon monoxide to 90 should help balance things out. After recalculating with these new values and fine-tuning the carbon amount to 348, I'm getting much closer to the target—carbon at 468.7 versus 470 input, and hydrogen now at 60.
> 
> But I'm still coming up short on hydrogen even after bumping H₂ to 13 and lowering CO to 85. The calculation shows 56.8 hydrogen atoms versus the 60 I need, leaving a gap of 3.2. Let me check the oxygen balance with these latest adjustments. I'm adjusting the water content down to 233 to bring the total back to 1000, and now checking the elemental balances—carbon is matching at 470, hydrogen is close to the target of 60, and I'm verifying the oxygen content. I'm satisfied with this approach—no need to complicate it with methanol. The bio-oil gets implicitly cracked into those gaseous and carbon products. Let me finalize the response with this revised BIOMASS_CARBONIZATION definition, then I'll verify the stoichiometry on a few more operations like CO₂ methanation to make sure the mass balances check out. Now I'm working through the methanol synthesis reactions. For CO₂ + 3H₂, the mole fractions come out to methanol at 0.640 and water at 0.360. Then for the CO + 2H₂ reaction, I'm calculating the input ratios as CO at 0.874 and H₂ at 0.126, which produces pure methanol. I'm starting on the ammonia synthesis reaction next.
> 
> For the ammonia reaction, N₂ + 3H₂ produces 2NH₃, and I'm verifying the stoichiometry checks out with N₂ at 0.822 and H₂ at 0.178 as inputs, yielding pure ammonia as output. All the stoichiometric calculations are consistent so far.
> 
> Now I'm reconsidering the energy requirements, particularly for semiconductor production at -20 MW, which seems extremely demanding. I'm checking against known industrial processes like the Siemens polysilicon method, which typically requires 60-80 kWh per kilogram, plus additional energy for crystal growth stages.
> 
> Calculating the total energy demand for 200 kg/h output across all processing steps comes to around 24 MW, so my -20 MW estimate is actually on the conservative side. I could justify pushing it to -25 MW, but I'll stick with -20 MW as a forward-looking assumption. For water electrolysis at -5.50 MW, the math checks out too—producing 112 kg/h of hydrogen at 50 kWh/kg gives roughly 5.6 MW, so that's consistent. Now I need to verify the naming conventions align with the table specifications.
> 
> Standardizing the process names... I'm verifying that all the resource outputs follow the RESOURCE_ prefix pattern correctly, and they all check out through nitrogen. I've confirmed the remaining resource mappings are correct, and I understand now that the prefix system works as expected—the table stores unprefixed names and the prefixes get added during import. Let me finalize the output and address the REGOLITH question.
> 
> I'm also reconsidering whether to note the bio-oil simplification in BIOMASS_CARBONIZATION, since I'm cracking it into simpler products within the operation itself, which aligns with how real facilities typically handle condensate processing. Before I write the final rows, I want to double-check my target_products against the schema requirements—specifically verifying that I've identified the valued outputs correctly for each conversion operation, excluding waste products like INDUST_WASTE and CO₂.
> 
> Looking at OIL_REFINING as a reference point, it includes METHANE, CARBON, and SULFUR alongside the main liquid hydrocarbons, so I should probably apply the same logic to POLYMER_SYNTH and include METHANE and HYDROGEN as target products since they're valuable byproducts from the cracking process, not just waste. So INDUST_WASTE is clearly waste and shouldn't be included. For POLYMER_SYNTH, I'm setting the targets to POLYMERS, METHANE, and HYDROGEN while excluding the waste streams. I'm doing the same for the chemical synthesis operations—INDUST_CHEMICALS_SYNTH targets just the industrial chemicals, and FINE_CHEMICALS_SYNTH targets the fine chemicals, both excluding water and waste. Now I need to figure out which outputs from biomass carbonization are actually valued products versus waste.
> 
> Looking at the carbonization outputs, carbon monoxide is a useful syngas component so it counts as a target, but CO₂ and water aren't valued here. For the methanol synthesis, methanol is the sole product of value. And for the water-gas shift reaction, only hydrogen is worth capturing—the CO₂ is just a byproduct. I think my target assignments are solid now, so I can finalize the output. . target = HYDROGEN ✓
> 
> For REVERSE_WATER_GAS_SHIFT: output = CO + H₂O. Only CO is valued. target = CARBON_MONOXIDE ✓
> 
> These look correct.
> 
> Let me also consider whether there should be any ETHANOL_SYNTH or ETHANOL_FERMENTATION in the Conversions/Synthesis group. Looking at the physical_operations.md, I see "Ethanol Fermentation" is listed under Biological, not Conversions/Synthesis. And there's no chemical ethanol synthesis listed. So I don't need to include it.
> 
> Alright, I'm confident in my data. Let me format the response.
> 
> Now I'm working through the CO₂ emissions from concrete production. Tracing back through the cement calcination process—cement makes up about 13% of concrete, and limestone decomposition produces roughly 0.5 kg CO₂ per kg of clinker. That works out to about 6.2% CO₂ in the final concrete, so my initial 8% estimate was too high. I'll adjust the output to CONCRETE 0.930 and CO₂ 0.070, which keeps the inputs balanced at 1.000.
> 
> Now I'm recalculating the energy requirements for cement production within the concrete manufacturing process. The cement portion accounts for about 13% of the concrete mass, and with cement requiring roughly 3.5 GJ per ton, I need to figure out how much of the industrial minerals input actually goes toward cement versus aggregate supplementation.
> 
> I'm settling on keeping the electricity consumption at -0.22 MW as before, but I need to reconsider the mass balance—specifically how water hydration factors into the concrete output and whether the CO₂ release happens during calcination or elsewhere in the process. into clinker, then reacts with water during curing.
> 
> So: Input = stone (aggregate) + minerals (limestone, clay, gypsum) + water
> Output = concrete (contains hydrated cement + aggregate + water) + CO₂ (from calcination)
> 
> The CO₂ comes from the calcium carbonate in the mineral input:
> If 0.250 t INDUST_MINERALS contains ~40% CaCO₃ (after considering it's a blend):
> CaCO₃ = 0.250 × 0.40 = 0.100 t
> CO₂ from calcination: 0.100 × 44/100 = 0.044 t
> 
> But Ca from INDUST_MINERALS is 13.5%:
> Ca in 0.250 t = 0.034 t
> If all Ca is from CaCO₃: CaCO₃ = 0.034 × 100/40 = 0.085 t, giving CO₂ = 0.037 t
> 
> That's only 3.7% of the mass flow, so my initial 7-8% estimate was too high. I'll settle on a middle ground that accounts for the cement content being around 13%, using concrete output of 0.940 and CO₂ of 0.060, which balances the inputs of 0.600 + 0.250 + 0.150 = 1.000 perfectly.
> 
> I'm satisfied with that balance. Now I need to finalize the complete list of all 24 operations, double-checking for obvious errors and keeping in mind that elemental compositions don't need to balance perfectly given the abstractions involved. I should also verify naming conventions—like whether to use GLASS_CERAMICS or GLASS/CERAMICS based on the table format. Then I'll compile the final response with the assessment on REGOLITH, the new data rows, and notes on simplifications. I'm verifying the abbreviation rules against the instructions—MATERIALS does map to MAT, so SEMICONDUCTOR_MAT is correct. I'm also noting that some operations like COAL_CARBONIZATION and POLYMER_SYNTH have multiple outputs, which is physically justified, and for conversion operations I need to make sure the target_products column captures all the valued products, not just the primary one. Now I'm ready to write up the response. I'm verifying the naming conventions for these resources against the existing patterns—Industrial Metals uses the full word rather than abbreviating to MAT, so Semiconductor Materials should follow the same logic as SEMICONDUCTOR_MAT. Let me double-check the other resource names like Industrial Chemicals and Industrial Minerals to make sure they're consistent with the abbreviation rules I'm seeing.
> 
> Now I'm reconsidering the electricity value for the solid fuel production step—at -0.29 MW for propellant mixing and vacuum casting, the energy requirement seems reasonable given the temperature control needed, so I'll keep that value as is.
> 
> For the regolith question, the existing approach makes sense: refining byproducts should stay classified as industrial waste since they've been chemically processed, and the industrial waste then feeds into the recycling-incineration chain that converts it back to regolith plus recovered metals. This two-step pathway is more physically realistic than having refining output regolith directly. The regolith processing operation is a borderline case since its waste is depleted regolith, but since it's been through molten oxide electrolysis and using regolith as both input and output would violate the constraint, industrial waste is the better classification.
> 
> Now I'm ready to generate all 24 operation rows with the correct column structure.

## Assessment: REGOLITH as Output in Existing Refining Operations

After reviewing all Refining group operations, I recommend **no changes** to existing rows. The INDUST_WASTE classification is appropriate for the following reasons:

Refining waste products (slag, tailings, red mud, leach residue) are all chemically processed materials that fit the INDUST_WASTE definition ("scrap metal, slag, fly ash, spent catalysts, recyclable mixed waste"). Crucially, the existing INDUST_RECYC_INCINERATION operation already models the conversion of INDUST_WASTE → REGOLITH + recovered metals (STEEL, INDUST_METALS). This two-step chain — refining → INDUST_WASTE → recycling → REGOLITH — is more physically accurate and captures the residual metal recovery that occurs in practice, especially in mass-constrained off-Earth contexts.

The one borderline case is REGOLITH_PROCESSING, where the waste is depleted regolith after molten oxide electrolysis. However, the material has been transformed into slag (qualitatively different from raw regolith), and outputting REGOLITH would violate the "no same resource as input and output" rule, requiring a net-reaction reformulation that substantially changes the operation's normalization. The current treatment is defensible.

## Conversions/Synthesis Operations — New Rows

```tsv
WATER_ELECTROLYSIS	-5.50	6.88	0.80	WATER	1	HYDROGEN;OXYGEN	0.112;0.888	1		HYDROGEN;OXYGEN
CO2_ELECTROLYSIS	-2.38	3.17	0.75	CARBON_DIOXIDE	1	CARBON_MONOXIDE;OXYGEN	0.636;0.364	1		CARBON_MONOXIDE;OXYGEN
METHANE_PYROLYSIS	-3.00	4.29	0.70	METHANE	1	CARBON;HYDROGEN	0.749;0.251	1		CARBON;HYDROGEN
COAL_GASIFICATION	-0.08	0.0941	0.85	COAL;OXYGEN;WATER	0.557;0.364;0.079	CARBON_MONOXIDE;HYDROGEN;CARBON_DIOXIDE;SULFUR;INDUST_WASTE	0.780;0.039;0.123;0.008;0.050	1		CARBON_MONOXIDE;HYDROGEN;SULFUR
BIOMASS_GASIFICATION	-0.06	0.075	0.80	BIOFEEDSTOCK;OXYGEN;WATER	0.805;0.155;0.040	CARBON_MONOXIDE;HYDROGEN;CARBON_DIOXIDE;INDUST_WASTE	0.838;0.053;0.069;0.040	1		CARBON_MONOXIDE;HYDROGEN
COAL_CARBONIZATION	-1.50	2.00	0.75	COAL	1	CARBON;METHANE;HYDROGEN;WATER;AMMONIA;SULFUR;CARBON_MONOXIDE;INDUST_WASTE	0.650;0.055;0.015;0.175;0.003;0.012;0.014;0.076	1		CARBON;METHANE;HYDROGEN;AMMONIA;SULFUR;CARBON_MONOXIDE
BIOMASS_CARBONIZATION	-0.42	0.60	0.70	BIOFEEDSTOCK	1	CARBON;METHANE;HYDROGEN;CARBON_MONOXIDE;CARBON_DIOXIDE;WATER;INDUST_WASTE	0.347;0.065;0.015;0.080;0.210;0.233;0.050	1		CARBON;METHANE;HYDROGEN;CARBON_MONOXIDE
CO2_METHANATION	-0.09	0.106	0.85	CARBON_DIOXIDE;HYDROGEN	0.845;0.155	METHANE;WATER	0.308;0.692	1		METHANE
STEAM_METHANE_REFORMING	-2.25	3.00	0.75	METHANE;WATER	0.471;0.529	CARBON_MONOXIDE;HYDROGEN	0.822;0.178	1		CARBON_MONOXIDE;HYDROGEN
WATER_GAS_SHIFT	-0.05	0.0588	0.85	CARBON_MONOXIDE;WATER	0.609;0.391	CARBON_DIOXIDE;HYDROGEN	0.956;0.044	1		HYDROGEN
REVERSE_WATER_GAS_SHIFT	-0.33	0.44	0.75	CARBON_DIOXIDE;HYDROGEN	0.956;0.044	CARBON_MONOXIDE;WATER	0.609;0.391	1		CARBON_MONOXIDE
LIQ_HYDROCARBONS_SYNTH	-0.10	0.125	0.80	CARBON_MONOXIDE;HYDROGEN	0.867;0.133	LIQ_HYDROCARBONS;WATER	0.442;0.558	1		LIQ_HYDROCARBONS
CO2_METHANOL_SYNTH	-0.08	0.10	0.80	CARBON_DIOXIDE;HYDROGEN	0.879;0.121	METHANOL;WATER	0.640;0.360	1		METHANOL
CO_METHANOL_SYNTH	-0.06	0.0706	0.85	CARBON_MONOXIDE;HYDROGEN	0.874;0.126	METHANOL	1	1		METHANOL
AMMONIA_SYNTH	-1.50	1.88	0.80	NITROGEN;HYDROGEN	0.822;0.178	AMMONIA	1	1		AMMONIA
POLYMER_SYNTH	-2.00	2.67	0.75	LIQ_HYDROCARBONS;INDUST_CHEMICALS;OXYGEN	0.820;0.120;0.060	POLYMERS;METHANE;HYDROGEN;CARBON_DIOXIDE;WATER;INDUST_WASTE	0.550;0.160;0.030;0.100;0.080;0.080	1		POLYMERS;METHANE;HYDROGEN
INDUST_CHEMICALS_SYNTH	-0.55	0.688	0.80	INDUST_MINERALS;SULFUR;AMMONIA;METHANOL;OXYGEN	0.450;0.080;0.020;0.150;0.300	INDUST_CHEMICALS;WATER;INDUST_WASTE	0.550;0.200;0.250	1		INDUST_CHEMICALS
FINE_CHEMICALS_SYNTH	-1.50	2.00	0.75	INDUST_CHEMICALS;METHANOL;INDUST_METALS;INDUST_MINERALS;OXYGEN	0.400;0.200;0.100;0.100;0.200	FINE_CHEMICALS;WATER;INDUST_WASTE;CARBON_DIOXIDE	0.500;0.200;0.150;0.150	1		FINE_CHEMICALS
CONCRETE_PRODUCTION	-0.22	0.293	0.75	STONE;INDUST_MINERALS;WATER	0.600;0.250;0.150	CONCRETE;CARBON_DIOXIDE	0.940;0.060	1		CONCRETE
GLASS_CERAMICS_PRODUCTION	-1.75	2.50	0.70	INDUST_MINERALS	1	GLASS_CERAMICS;CARBON_DIOXIDE	0.890;0.110	1		GLASS_CERAMICS
SEMICONDUCTOR_PRODUCTION	-20.0	30.8	0.65	INDUST_MINERALS;CARBON;FINE_CHEMICALS;INDUST_CHEMICALS;OXYGEN	0.500;0.100;0.050;0.100;0.250	SEMICONDUCTOR_MAT;CARBON_DIOXIDE;INDUST_WASTE;WATER	0.200;0.350;0.350;0.100	1		SEMICONDUCTOR_MAT
COMPOSITE_PRODUCTION	-1.80	2.40	0.75	POLYMERS;GLASS_CERAMICS;CARBON;INDUST_CHEMICALS	0.400;0.450;0.100;0.050	COMPOSITES;INDUST_WASTE	0.900;0.100	1		COMPOSITES
SOLID_FUEL_PRODUCTION	-0.29	0.341	0.85	INDUST_CHEMICALS;ALUMINIUM;POLYMERS;FINE_CHEMICALS	0.680;0.180;0.100;0.040	SOLID_FUELS;INDUST_WASTE	0.950;0.050	1		SOLID_FUELS
FERTILIZER_PRODUCTION	-0.82	1.03	0.80	AMMONIA;INDUST_MINERALS;SULFUR;INDUST_CHEMICALS;WATER	0.294;0.470;0.059;0.118;0.059	FERTILIZERS;INDUST_WASTE	0.824;0.176	1		FERTILIZERS
```

## Key Design Decisions and Simplifications

**Stoichiometric operations** (WATER_ELECTROLYSIS, CO2_ELECTROLYSIS, METHANE_PYROLYSIS, CO2_METHANATION, STEAM_METHANE_REFORMING, WATER_GAS_SHIFT, REVERSE_WATER_GAS_SHIFT, LIQ_HYDROCARBONS_SYNTH, CO2_METHANOL_SYNTH, CO_METHANOL_SYNTH, AMMONIA_SYNTH): Mass ratios follow pure stoichiometry. Fischer-Tropsch uses octane (C₈H₁₈) as the representative hydrocarbon product.

**Exothermic vs. endothermic handling**: For exothermic reactions where energy is derived from input chemistry (CO2_METHANATION, WATER_GAS_SHIFT, LIQ_HYDROCARBONS_SYNTH, CO2_METHANOL_SYNTH, CO_METHANOL_SYNTH, AMMONIA_SYNTH), electricity covers only auxiliary requirements (compression, pumping, separation), consistent with the BASIC_O2_STEELMAKING precedent. For endothermic reactions (WATER_ELECTROLYSIS, CO2_ELECTROLYSIS, METHANE_PYROLYSIS, STEAM_METHANE_REFORMING, REVERSE_WATER_GAS_SHIFT), electricity provides the full process energy.

**COAL_GASIFICATION and BIOMASS_GASIFICATION**: Oxygen-blown autothermal gasification where partial oxidation provides process heat. Electricity covers auxiliaries only. Coal gasification includes SULFUR recovery from coal's sulfur content. The nitrogen content of coal (~1.4%) is lumped into INDUST_WASTE.

**COAL_CARBONIZATION**: Yields based on typical coke-oven mass balance for bituminous coal. Eight output resources capture the major product streams (coke, coke-oven gas components, pyrolysis water, ash). Electricity covers full heating requirement since coke-oven gas is modeled as a product output rather than burned internally.

**BIOMASS_CARBONIZATION**: Bio-oil (the condensable liquid from pyrolysis) is not cleanly representable by any single model resource. Rather than using METHANOL as an imperfect proxy, I assumed integrated pyrolysis with condensate cracking, decomposing the bio-oil fraction into CARBON, METHANE, HYDROGEN, CO, and CO₂. This avoids introducing misleading resource flows.

**Complex multi-input operations** (POLYMER_SYNTH, INDUST_CHEMICALS_SYNTH, FINE_CHEMICALS_SYNTH, FERTILIZER_PRODUCTION): These represent highly aggregated, production-weighted blends of many distinct industrial processes. Exact elemental balance against resource compositions is not achievable due to the abstracted nature of catchall resources (especially INDUST_MINERALS, whose blend composition is heavily weighted toward construction minerals rather than chemical feedstocks like NaCl, phosphate rock, and potash). These operations should be understood as approximate representations where the input/output resource types and rough mass ratios are physically motivated but not elementally precise.

**SEMICONDUCTOR_PRODUCTION**: Extremely energy-intensive at −20 MW per t/h mass flow, reflecting the combined energy of Siemens-process silicon purification (~60–80 kWh/kg), Czochralski crystal growth (~30–50 kWh/kg), and wafer processing (~10 kWh/kg). The 20% product yield reflects the large amount of process waste inherent in semiconductor manufacturing.

## Feedback on Context Files

**INDUST_MINERALS composition limitation**: The production-weighted INDUST_MINERALS blend has only 2.1% Na, 2.1% Cl, 2.1% P, and 0.95% K. This makes it a poor proxy for the salt, phosphate rock, and potash feedstocks that dominate the chemical and fertilizer industries. Consider either creating a separate "Chemical Minerals" resource (salt, phosphate, potash concentrate) or adjusting the INDUST_MINERALS blend to increase these elements. This would significantly improve the fidelity of INDUST_CHEMICALS_SYNTH and FERTILIZER_PRODUCTION.

**Missing Ethanol Synthesis**: There is no chemical ethanol synthesis operation in the Conversions/Synthesis group (only biological fermentation). If synthetic ethanol (e.g., from ethylene hydration) is desired as an alternative pathway, it could be added.

**Bio-oil resource gap**: There is no model resource that well represents pyrolysis bio-oil (a highly oxygenated liquid: ~55% C, ~35% O, ~7% H). This creates modeling difficulty for biomass carbonization and any future biomass liquefaction operations. A "Bio-crude" or "Pyrolysis Oil" resource could fill this gap if finer resolution in the biorefinery chain is desired.