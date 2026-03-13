*Claude-Opus-4.6 (output effort = high). March, 2026.*

---
---

Construct a tab-delimited table with physical operations as row entities. This will become model input data so follow provided table instructions. Include the following column fields:

- name — Operation name per table instructions.
- electricity — Electricity produced (as positive value) or used (as negative value) by the operation in MW.
- efficiency — Efficiency of the operation, such that 1 minus efficiency equals proportional waste heat. Select an efficiency value that is somewhat forward looking but within the range of current Earth operations.
- input_resources — A semicolon-delimited list of input resources for the operation (resource names per table instructions).
- input_rates — A semicolon-delimited list of input resource rates (in t/h) for the operation (each corresponds to a resource above).
- output_resources — A semicolon-delimited list of output resources for the operation.
- output_rates — A semicolon-delimited list of output resource rates (in t/h) for the operation.
- mass_flow — This should equal the sum of input_rates which should equal the sum of output_rates.

Notes:

- For Extraction group operations, leave all fields blank except the name field (these are handled very differently).
- SOLAR_POWER and RADIOISOTOPE_POWER have no input or output resources.
- For Energy group operations, normalize the operation so that `electricity` is 1 MW. For others, normalize the operation so that `mass_flow` is either 1 t/h or 1 kg/h (use the latter for very high value to weight products such as PRECIOUS_METALS and TENSOR_PROCESSORS).
- Use resources as they are defined in physical_resources.md. E.g., input rate for LE_URANIUM_FUEL should consider the assembled fuel weight, not just the uranium content.
- For chemical conversions, consider only stoichiometric quantities for inputs and outputs. For fabricated/manufactured items, consider product composition and subtractive processes (the latter producing INDUSTRIAL_WASTE, etc.).
- Don't consider plant maintenance or repair parts used to run the operation; that will be handled separately.
- Don't have the same resource as input and output. E.g., there should be no "excess oxygen" in combustion.
- Don't worry about water use in industrial processes, except as a stoichiometric input or output.
- Don't worry about the source of input resources (e.g., OXYGEN from atmosphere versus purified supply) or the fate of output resources (e.g., collected versus exhaust). An amendment to the latter is that you can consider different potential output compositions and how they might be more useful or less harmful. For example, you could consider outputting SULFER rather than SULFUR_DIOXIDE from COAL_POWER.
- As for efficiency, be forward looking if the particular operation or consideration is not well constrained.
- Report if you find major gaps in either the resources or operations lists.

For now, I only want you to construct table rows for the Energy and Extraction groups. I don't want you to analyze other operation groups at this time, but tell me if you have suggestions that might improve results when moving on to the remaining operations.

---
---

*Thinking...*

> Let me carefully work through this task. I need to construct a tab-delimited table for Energy and Extraction group operations from physical_operations.md.
> 
> Let me first enumerate all Energy operations:
> 1. Solar Power
> 2. Geothermal Power
> 3. Hydropower
> 4. Wind Power
> 5. Tidal Power
> 6. Hydrogen Power
> 7. Methane Power
> 8. Methanol Power
> 9. Ethanol Power
> 10. Liquid Hydrocarbons Power
> 11. Oil Power
> 12. Coal Power
> 13. Hydrogen Fuel Cells
> 14. Methane Fuel Cells
> 15. Methanol Fuel Cells
> 
> 
> 16. LEU Fission Power
> 17. HEU Fission Power
> 18. Thorium Fission Power
> 19. D-T Fusion Power
> 20. D-³He Fusion Power
> 21. ³He-³He Fusion Power
> 22. Radioisotope Power
> 
> Now I'm compiling the Extraction operations list, which includes various resource extraction methods like atmospheric and volatiles separation, drilling, and mining operations for different materials.
> 
> For the Energy operations, I need to normalize everything to 1 MW of electricity output. Looking at solar power specifically—it has no fuel input, just converts intercepted solar energy to electricity. Current panels are around 20-25% efficient, with high-efficiency variants reaching up to 25%. For a forward-looking simulation, I should pick something realistic but slightly optimistic, so I'll go with 0.25 as the efficiency value, where the remaining 75% represents unconverted solar energy rather than traditional waste heat.
> 
> For geothermal, wind, hydro, and tidal power—these all work similarly to solar in that they have no tracked fuel inputs or resource consumption. Geothermal plants typically operate at 10-23% efficiency depending on the type, so I'll use 0.15 as a forward-looking estimate. Like solar, these renewable sources don't consume tracked resources, so their input and output resource fields should remain blank.
> 
> Now I'm working through hydrogen power, which is different since it actually consumes fuel. Hydrogen combustion with oxygen produces water, and I need to calculate the stoichiometric ratios and energy requirements. At around 45% efficiency for a gas turbine, producing 1 MW would require roughly 0.067 tonnes of hydrogen per hour and 0.533 tonnes of oxygen per hour, yielding 0.6 tonnes of water as output.
> 
> Now I'm checking the mass balance and considering the constraints—oxygen is fine as an input resource, and water as the sole output is straightforward. For methane combustion, I'm working through the stoichiometry where methane reacts with oxygen to produce carbon dioxide and water, with a lower energy content around 50 MJ/kg compared to hydrogen's 120 MJ/kg.
> 
> For methanol, the combustion reaction is similar but with lower energy density at 20 MJ/kg, so it requires more fuel mass to generate the same power output. I'm calculating the consumption rates and verifying the mass balance checks out for both pathways.
> 
> Now moving to ethanol, which has higher energy content at 27 MJ/kg, so it needs less fuel mass than methanol but more oxygen due to its molecular structure. The stoichiometry shows it produces slightly different ratios of CO₂ and H₂O compared to methanol.
> 
> For liquid hydrocarbons, I'm using a representative composition approximating the fuel as a C₁₂ compound based on typical crude oil characteristics.
> 
> Now I'm working through the combustion chemistry for hydrocarbons by breaking down the carbon and hydrogen content separately—carbon oxidizes to CO₂ while hydrogen forms H₂O, and I'm calculating the oxygen requirement and products per kilogram of fuel.
> 
> The mass balance checks out at around 4.4 kg total (fuel plus oxygen yields products), though there's a tiny discrepancy from trace elements like sulfur and nitrogen. Since sulfur makes up only 0.15% of the fuel, it's negligible for this calculation, so I'll focus on the main hydrocarbon combustion and track only the defined resources.
> 
> For a 1 MW power plant running at 42% efficiency, I need about 2.38 MJ/s of fuel energy, which translates to roughly 0.055 kg/s of liquid hydrocarbons—about 199 kg/hour. This consumes 0.677 t/h of oxygen and produces 0.625 t/h of CO₂ and 0.251 t/h of water vapor.
> 
> Now for crude oil, the composition is heavier with more carbon and sulfur content. The energy density is slightly lower at around 40 MJ/kg, and typical oil-fired boiler systems run at 35-38% efficiency, so I'll need to recalculate the fuel consumption and emission rates accordingly. I'm simplifying the approach here—I don't need to track every oxygen source since the instructions say to ignore input resource origins. For coal specifically, I should focus on the main combustible elements (carbon, hydrogen, sulfur) and potentially output sulfur as a separate product rather than converting it to SO₂, given the explicit guidance in the instructions.
> 
> Now I'm working through the stoichiometry: calculating CO₂ and H₂O from the carbon and hydrogen content, determining the oxygen needed for complete combustion while accounting for the oxygen already present in the coal itself, and handling the sulfur output separately as indicated.
> 
> For the ash component, I'm recognizing that the mineral elements like silicon, aluminum, iron, and calcium make up roughly 8-12% of the coal by mass, and these non-combustible materials should map to the INDUSTRIAL_WASTE resource in the simulation. The nitrogen content will convert to N₂ or NOₓ during combustion. So the mass balance framework is: coal plus oxygen yields carbon dioxide, water, sulfur compounds, and ash as the main outputs. I'm realizing I need to step back and use a simpler approach rather than trying to account for every fraction of oxygen. Let me just work with the basic combustion reactions for carbon and hydrogen, use the coal's total oxygen content to reduce the external oxygen requirement, and treat everything else—the ash, sulfur, and nitrogen—as separate output streams. I'm realizing the oxygen calculation needs refinement—only the organic oxygen in the coal reduces external O₂ demand, not the mineral oxygen bound in ash compounds. After accounting for roughly 5% mineral oxygen, the external O₂ requirement comes to about 2.07 kg per kilogram of coal. Now I need to properly categorize the ash: it's everything that isn't carbon, hydrogen, oxygen, nitrogen, or sulfur, which totals around 7.6% by weight, though the actual ash collected will include oxygen from the mineral oxides formed during combustion.
> 
> Rather than getting bogged down in these details, I should take a more straightforward approach—calculate fuel consumption based on energy content and efficiency, then work through the stoichiometry for O₂, CO₂, and H₂O outputs, handling sulfur, nitrogen, and ash as needed. I'll normalize everything to 1 MW output and use three significant figures throughout to keep the calculations consistent and manageable.
> 
> Now I'm setting up the parameters for each renewable energy source. Solar, geothermal, and hydropower have no fuel inputs or mass flows since they're drawing from natural sources. Wind is a bit trickier since the "efficiency" represents the fraction of wind energy converted to electricity, but the remainder isn't really waste heat—it's just kinetic energy that stays in the atmosphere. For renewables without fuel inputs, I need to think about what fraction of the energy they capture actually becomes electricity versus waste heat. Solar panels convert about 25% of photons to electricity, wind turbines mechanically convert ~95% of the wind energy they capture (though the Betz limit means they can only capture ~59% of available wind energy), hydro systems are around 90% efficient, geothermal is limited by Carnot efficiency at ~15%, and tidal is similar to hydro at 85-90%. So I'm settling on 0.25 for solar, 0.15 for geothermal, 0.90 for hydropower, and 0.45 for wind as a compromise that accounts for the total available energy rather than just the mechanical conversion efficiency.
> 
> Now I'm realizing the simulation treats electricity and heat separately, so the waste heat calculation matters less than I initially thought—the unconverted energy from wind doesn't necessarily manifest as literal heat in the system. I'll stick with these standard efficiency definitions and not overthink the waste heat implications, since the simulation handles thermal energy outside the resource list anyway.
> 
> For the combustion operations, efficiency is more directly tied to waste heat production, so I need to work through each fuel-burning process carefully. Starting with hydrogen power using a gas turbine, I'm calculating the stoichiometric ratios based on the reaction of hydrogen and oxygen to produce water, using hydrogen's lower heating value of 120 MJ/kg and an efficiency of 0.45. This gives me the mass flow rates for fuel consumption and product generation. Now I'm moving through the other fuel-based operations with the same approach. I'm refining the methanol power calculations with consistent significant figures—recalculating the fuel consumption rate using the more precise LHV value of 19.9 MJ/kg, then deriving the stoichiometric oxygen, carbon dioxide, and water vapor flows to verify mass balance.
> 
> Now moving to ethanol combustion, where I'm computing the fuel rate from its 26.8 MJ/kg energy content and 0.42 efficiency, then scaling up the oxygen, carbon dioxide, and water requirements based on the stoichiometric ratios from the combustion equation.
> 
> For liquid hydrocarbons, I'm treating the fuel as a representative kerosene/diesel mixture with roughly 43 MJ/kg energy content and calculating the fuel consumption rate needed to deliver 1 megawatt of power at the same 0.42 efficiency.
> 
> Now working through crude oil combustion with its more complex composition including sulfur and nitrogen, using a lower 0.38 efficiency for the boiler and steam turbine system, then tracking the oxygen requirements and product streams while accounting for the oxygen already present in the fuel itself. Now I'm calculating the mass flow rates by multiplying each per-kilogram value by 0.237 t/h, and verifying that inputs and outputs balance at 0.997 t/h. I'm keeping the composition simple with sulfur as a distinct output and bundling nitrogen and other trace elements into industrial waste. I'm settling on a pragmatic threshold—I'll include sulfur and industrial waste only when they exceed about 1% of the mass flow, which means they'll show up for coal but not for oil. Now I'm working through the coal combustion stoichiometry from first principles, tracking each element's transformation: carbon to CO₂, hydrogen to water, and the mineral components converting to ash oxides. I need to account for the oxygen already present in the coal itself to avoid double-counting when balancing the combustion equation. So the external oxygen needed works out to 2.065 kg, and the ash combines the mineral cations with their bound oxygen for a total of 0.121 kg. Let me verify the mass balance: inputs of 1 kg coal plus 2.065 kg oxygen equal 3.065 kg total, and my outputs—carbon dioxide, water, sulfur, nitrogen, and industrial waste—sum to exactly that. Scaling everything by 0.343 gives me the hourly flow rates, and the nitrogen output is small but worth including since coal nitrogen content is actually significant at 1.4%.
> 
> Actually, I'm going to fold the nitrogen into industrial waste instead to keep things simpler, since it's such a minor stream. That means my waste output becomes 0.135 kg per kg of coal, combining the mineral cations, their oxygen, and the nitrogen. The oxygen balance still holds—I need 2.065 kg external oxygen after accounting for the organic oxygen already in the coal. My revised outputs are carbon dioxide at 2.42 kg, water at 0.495 kg, sulfur at 0.015 kg, and industrial waste at 0.135 kg, which totals back to 3.065 kg perfectly. Scaling to hourly rates gives me 0.343 t/h coal input, 0.708 t/h oxygen, and outputs of 0.830 t/h CO₂, 0.170 t/h water, 0.00515 t/h sulfur, and 0.0463 t/h waste—all balancing to 1.051 t/h.
> 
> Now moving to hydrogen fuel cells, which use a 2H₂ + O₂ electrochemical reaction with 55% efficiency and 120 MJ/kg LHV. That gives me 0.0545 t/h hydrogen input, 0.436 t/h oxygen, and 0.491 t/h water output. For methane fuel cells running on SOFC technology at 55% efficiency with 50 MJ/kg LHV, I need 0.131 t/h methane, 0.524 t/h oxygen, producing 0.360 t/h CO₂ and 0.295 t/h water. With methanol fuel cells, the reaction is 2CH₃OH + 3O₂ → 2CO₂ + 4H₂O, and I'm weighing efficiency assumptions—DMFCs typically run around 30% but methanol-fed SOFCs can reach higher efficiency.
> 
> For methanol at 45% efficiency and 19.9 MJ/kg LHV, I get 0.402 t/h methanol consumption, 0.603 t/h oxygen, 0.553 t/h CO₂, and 0.452 t/h water. Now I'm shifting to nuclear fission, which doesn't have chemical inputs or outputs in the traditional sense—it consumes enriched uranium fuel and produces spent fuel. The mass conversion is negligible due to E=mc², so I'm tracking fuel consumption over time. A typical light water reactor achieves about 50 GWd/tHM burnup, and with fuel assemblies around 660 kg total mass with roughly 70.5% heavy metal content, I can calculate the fuel requirements from there.
> 
> Recalculating with a forward-looking thermal efficiency of 34% for Gen III+ reactors: 1 tonne of heavy metal requires about 17,000 days to fully consume at the thermal power needed for 1 MW electrical output, which translates to a fuel consumption rate of roughly 3.48 g/h. The spent fuel output is essentially the same mass since fission losses are negligible—only about 50 kg per tonne of heavy metal converts to energy.
> 
> For the operation, I'm normalizing to 1 MW electrical output with a 34% efficiency, so the input and output mass flows both sit at 3.48e-6 t/h. Now I need to define the HEU fission power operation similarly, though naval reactors typically operate at lower burnup rates despite their long operational lifespans. Now I'm calculating the fuel consumption rates for HEU and thorium reactors. For HEU with 150 GWd/tHM burnup and 30% efficiency, I get about 3.70e-6 t/h consumption, which feeds into the spent fuel output at the same rate. For thorium fuel with lower burnup of 40 GWd/tHM but slightly better efficiency at 34%, the consumption rate comes to 4.36e-6 t/h, also matching the spent fuel output. Now I'm moving on to the D-T fusion calculations.
> 
> For fusion, the reaction is deuterium plus tritium producing helium and a neutron, but tritium gets bred in-situ from lithium blankets, so the net fuel input is really deuterium and lithium. The challenge is figuring out which resource in the simulation represents the lithium for the blankets—it's not explicitly listed as a separate fuel, but it could come from industrial metals or the batteries resource, though both have very small lithium fractions.
> 
> Now I'm working through the stoichiometry and energy calculations. For the net D + ⁶Li reaction producing two helium nuclei, I need to verify the energy output per unit mass of deuterium consumed. I'm converting the 22.4 MeV per reaction into joules per gram and per kilogram, but I'm getting some inconsistencies in my calculations that I need to work through more carefully. I need to recalculate using the correct energy value for D-T fusion at 17.6 MeV per reaction rather than 22.4 MeV, which gives me about 1.698 × 10¹² joules per mole. Now I'm working through the energy density by accounting for the mass of deuterium and tritium separately to get the energy per unit mass of fuel. I'm realizing the tritium breeding complicates the resource model—the lithium blanket gets consumed as it converts to helium, but it might be treated as infrastructure maintenance rather than an operational resource. For the simulation, I need to figure out whether to track lithium consumption as an input alongside deuterium, or handle it separately as part of reactor upkeep.
> 
> Let me work through the actual consumption rates for a 1 MW_e reactor to see what makes sense.
> 
> Now I'm calculating the mass flows: deuterium gets consumed at about 0.01 grams per hour, lithium-6 at roughly 0.03 grams per hour, and helium-4 is produced at twice the reaction rate.
> 
> The mass balance checks out when accounting for the energy released—the inputs and outputs align as expected. But I'm realizing there's a fundamental issue with the resource model: the simulation doesn't have a dedicated lithium resource, only a broad industrial metals category that lumps together many different commodities, which makes it awkward to represent lithium as a specific fusion fuel input.
> 
> I think the cleanest approach is to list only deuterium as the input and helium as the output, treating the lithium blanket as part of the reactor's infrastructure that gets replaced periodically—which aligns with how the instructions handle capital equipment anyway. This sidesteps the resource categorization problem while still capturing the main fuel cycle. I'm realizing the tritium breeding happens in-situ from lithium blankets, so from the simulation's perspective the process is self-contained—deuterium and lithium go in, helium comes out. Since INDUSTRIAL_METALS feels like too broad a proxy for lithium, I'll flag this as a resource gap and model the operation with deuterium input and helium output, adjusting the helium to balance the mass equation, then note in feedback that a dedicated lithium resource should be defined.
> 
> Now looking at D-³He fusion, the key difference is that it produces protons as the primary energy carrier rather than neutrons, which makes it much better suited for direct energy conversion and potentially higher efficiency than D-T cycles.
> 
> For the calculations, I'm working through the reaction stoichiometry and energy output to determine consumption rates—deuterium and helium-3 inputs producing helium-4 and protons, with an 18.3 MeV energy release and assuming 50% efficiency due to the direct conversion advantage. Running the numbers for 1 MW of electrical output gives me the mass consumption rates for both fuel inputs.
> 
> Now I'm verifying the mass balance between inputs and outputs, accounting for the mass-energy deficit from the reaction. The total fuel consumption comes to about 2.05e-8 tonnes per hour, with helium-4 as the primary product and a smaller hydrogen output.
> 
> Moving on to the helium-3 to helium-3 fusion reaction, which produces helium-4 and two protons with 12.86 MeV per reaction.  With a 55% efficiency for this aneutronic process, I'm calculating that 1 megawatt of electrical power requires about 8.82e17 reactions per second. The mass flows work out to roughly 3.18e-8 tonnes per hour of helium-3 consumed, producing about 2.11e-8 tonnes of helium-4 and 1.06e-8 tonnes of hydrogen, which balances within the expected mass-energy loss.
> 
> For radioisotope power, I'm using plutonium-238 or strontium-90 as the decay source, which comes built into the unit rather than as an input resource. I'll set the efficiency at 0.10 to reflect forward-looking Stirling generator technology, which is more promising than the current 6-7% standard RTG performance.
> 
> Now I need to revisit the oil power calculations and tighten up the numbers across the board.
> 
> For oil, I'm working through the combustion chemistry—breaking down the fuel composition by element and calculating the CO₂, water, and other outputs produced per kilogram of oil burned. With a lower heating value around 40 MJ/kg and an efficiency of 0.38, that gives me a fuel consumption rate of about 0.237 tonnes per hour to generate 1 MW. I'm also tracking the oxygen requirements for each combustion reaction and accounting for the nitrogen and sulfur content in the fuel.
> 
> Now I'm verifying the mass balance—the oxygen needed externally is 3.207 kg per kg of oil after accounting for the oxygen already present in the fuel itself. I'm grouping the nitrogen and other trace elements into industrial waste since they're negligible. The outputs check out: CO₂, water, sulfur, and industrial waste sum to exactly the inputs of oil plus oxygen. Converting these per-kilogram values to hourly rates based on the fuel consumption gives me the resource flows for the simulation. Now I'm mapping all the resource names to their standardized format and recalculating the values with consistent significant figures. Let me work through each operation systematically, starting with the renewable power sources and then moving into the fuel-based conversions.
> 
> For hydrogen power, I'm calculating the input rates based on the combustion reaction and energy content, then deriving the stoichiometric ratios to get the water output. The mass balance checks out at 0.600 t/h.
> 
> Now I'm working through methane combustion...
> 
> Methane requires a 50% efficiency assumption, giving me a consumption rate of 0.144 t/h. The stoichiometric ratios show I need 4 times the mass of oxygen, producing CO₂ and water as outputs. The mass balance confirms: inputs and outputs both total 0.720 t/h.
> 
> For methanol, I'm using a lower heating value of 19.9 MJ/kg with 42% efficiency. The calculation gives 0.431 t/h of methanol consumption, but I'm catching a rounding discrepancy in my initial math. Let me recalculate more precisely using exact values—this gives me 0.4307 t/h, which rounds to 0.431. The stoichiometric products work out to 0.646 t/h oxygen, 0.592 t/h CO₂, and 0.485 t/h water, all balancing to 1.077 t/h.
> 
> Now moving into ethanol combustion with a heating value of 26.8 MJ/kg and the same 42% efficiency...
> 
> Finishing the ethanol calculation, I get a fuel rate of 0.320 t/h. The stoichiometric ratios give me oxygen at 0.668 t/h, CO₂ at 0.612 t/h, and water at 0.376 t/h, with inputs and outputs both totaling 0.988 t/h.
> 
> For liquid hydrocarbons, I'm approximating the composition as 85.5% carbon and 14% hydrogen by mass. With a heating value around 43 MJ/kg, the fuel consumption comes to 0.199 t/h. Working through the combustion stoichiometry, I need 3.4 kg of oxygen per kg of fuel, producing 3.14 kg of CO₂ and 1.26 kg of water. The slight discrepancy from trace elements gets absorbed into the CO₂ adjustment, and the final mass balance checks out at 0.876 t/h for both inputs and outputs.
> 
> Now I'm working through the oil combustion. The composition includes carbon, hydrogen, sulfur, nitrogen, and oxygen, with a lower heating value of about 40 MJ/kg. This gives a fuel rate of 0.237 t/h. The oxygen requirement is 3.207 kg per kg of oil, and the products include CO₂, water, sulfur dioxide, and industrial waste from the mineral content. The mass balance confirms at 0.997 t/h.
> 
> For coal, I'm using a detailed elemental composition with carbon at 66%, oxygen at 18%, and various mineral components. The lower heating value is around 25 MJ/kg, resulting in a fuel rate of 0.343 t/h. I'm calculating the external oxygen needed at 2.065 kg per kg coal, with CO₂ production at 2.42 kg and water at 0.495 kg per kg coal.
> 
> Now I'm looking at hydrogen fuel cells, which are much cleaner—just hydrogen and oxygen producing water with 55% efficiency. The hydrogen rate comes out to 0.0545 t/h, requiring 0.436 t/h of oxygen and producing 0.491 t/h of water.
> 
> For methane fuel cells, I'm using the combustion reaction with 50 MJ/kg lower heating value and 55% efficiency, giving a methane rate of 0.131 t/h. The oxygen requirement is 0.524 t/h, producing 0.360 t/h of CO₂ and 0.295 t/h of water.
> 
> Methanol fuel cells operate at lower efficiency (45%) with a lower energy density (19.9 MJ/kg), so the fuel rate is higher at 0.402 t/h, requiring 0.603 t/h of oxygen and producing 0.553 t/h of CO₂ and 0.452 t/h of water.
> 
> For the nuclear options, I'm calculating fuel consumption rates based on thermal efficiency and burnup rates. Low-enriched uranium fuel gets consumed at 3.47e-6 t/h with 34% efficiency and 50 GWd/tHM burnup. High-enriched uranium allows much higher burnup at 150 GWd/tHM but with slightly lower efficiency at 30%, and I'm working through the consumption rate calculation.
> 
> For fusion, the D-T reaction releases 22.4 MeV per reaction, which translates to needing about 6.968e17 reactions per second to generate 2.5 MW thermal. This means deuterium consumption is extremely low at 8.39e-9 t/h, with lithium as the other fuel source.
> 
> For the lithium side, I'm calculating that each deuterium nucleus consumes one lithium-6 nucleus in the reaction. Using the mass of lithium-6, the consumption rate comes to about 2.51e-8 t/h. Since fusion reactors would likely use enriched lithium-6 rather than natural lithium (which is mostly lithium-7), I'm using the stoichiometric mass for lithium-6 only.
> 
> Now I'm working through the helium production—each reaction produces two helium-4 nuclei, which gives an output rate of about 3.34e-8 t/h. When I add up the inputs (deuterium plus lithium), I get 3.35e-8 t/h, which nearly matches the helium output. The small discrepancy comes from the mass defect—the binding energy released in the reaction accounts for about 4e-29 kg per reaction, equivalent to roughly 22.4 MeV.
> 
> For the mass balance in the table, I'll treat the energy loss as negligible at this precision and set the output equal to the input. The reaction rate is 6.968e17 per second at 1 MW_e, which gives me deuterium consumption of 8.39e-9 t/h and lithium consumption of 2.51e-8 t/h, for a total input of 3.35e-8 t/h that becomes helium output.
> 
> The tricky part is how to represent the lithium input—using INDUST_METALS feels like a workaround since lithium for fusion is quite specific, and neither the Batteries resource nor other proxies feel right. The operations document mentions that tritium is bred from lithium blankets in-situ, which suggests the lithium is part of the reactor infrastructure rather than a consumed resource. I think the best approach is to flag this as a gap in the resource model and use INDUST_METALS as a placeholder for now, noting that a dedicated lithium resource could be added later if needed. The abstraction is acceptable given the negligible mass scales involved. Now I'm working through the D-³He fusion reaction, calculating the reaction rate from the thermal power output and then deriving the mass flow rates for deuterium, helium-3, helium-4, and protons based on their atomic masses.
> 
> For the ³He-³He fusion pathway, I'm setting up the reaction with its higher efficiency and working through the mass consumption and production rates similarly.
> 
> Now I'm moving on to radioisotope power, which operates independently with no material inputs or outputs, just a fixed electricity output at 10% efficiency.
> 
> I'm compiling the extraction operations next—listing out all the resource extraction categories like atmospheric separation, mining operations, and quarrying to build the complete system table.
> 
> Now I need to format everything as a TSV with columns for operation name, electricity consumption, efficiency, input and output resources with their rates, and mass flow. I'm also reconsidering whether renewable energy sources like geothermal, hydro, wind, and tidal should be treated the same way as solar and radioisotope power since they don't consume tracked resources either.
> 
> Let me verify my fusion calculations for the D-T reaction, working through the energy per reaction, thermal power requirements at 40% efficiency, and then the consumption rates for deuterium and lithium-6 based on the reaction frequency.
> 
> Now I'm converting those mass flow rates into metric tons per hour and checking that the input and output masses balance correctly, accounting for the mass-energy deficit from the reaction.
> 
> Generating markdown table... I need to be careful with tab delimiters and use 3 significant figures as instructed.
> 
> For the efficiency values, I'm working through each energy source systematically. Wind power is tricky since the "waste heat" concept doesn't quite apply—the unconverted kinetic energy stays in the wind rather than becoming heat. The actual waste is just generator losses, so 0.45 captures the overall conversion efficiency reasonably well, accounting for the Betz limit and real turbine performance. I'm settling on the efficiency values across all the power sources, from renewables like solar and geothermal through fossil fuels and advanced options like fusion and fuel cells, and they all seem defensible. Now I'm checking whether any energy operations are missing or need refinement, particularly around biofuels and liquid hydrocarbons combustion products. Generating the operations table... Generating the data table... Now I'm checking the mass balance for the hydrogen power system more carefully. The hydrogen input rate calculation gives me 0.066667 t/h, and I need to verify this matches the oxygen consumption to ensure the stoichiometry is correct.
> 
> I'm working through the rounding precision issue—using 3 significant figures creates a small discrepancy where 0.0667 + 0.533 = 0.5997 instead of exactly 0.600. I think the best approach is to use 3 decimal places consistently so the inputs sum exactly to the mass flow output, which means rounding the hydrogen value to 0.067 t/h to make everything balance properly. Now I'm verifying the mass balance for the remaining fuel types. The methane, methanol, ethanol, and liquid hydrocarbon reactions all check out perfectly. For the oil combustion, the inputs sum to exactly 0.997, and the outputs including CO₂, H₂O, sulfur, and inert water also total 0.997, so the balance is solid across all reactions. The output sum is slightly lower than the input sum due to rounding in my fuel rate calculation. Let me verify the per-kilogram balance holds exactly, then recalculate using more precise values to close that small gap.
> 
> Now I'm working through the exact arithmetic with the fuel rate of 0.23684 t/h, computing each product term carefully to ensure the input and output masses match. After breaking down the calculations, I get an input sum of 0.99639 and an output sum of 0.99639—they balance perfectly. The discrepancy I saw earlier was just from rounding errors. Let me round everything to three significant figures for the final values.
> 
> When I round to three sig figs and add them back up, I'm running into cumulative rounding error—the individual rounded values don't quite sum to the same total as the unrounded calculation. The real issue is that I need to ensure consistency across the table by computing the mass flow from the unrounded values once, then rounding that result, rather than rounding intermediate values and summing them. I should adjust my approach to report values that are self-consistent to three significant figures, using the unrounded mass flow of 0.99639 rounded to 0.996 as the reference point.
> 
> Now I'm verifying the coal power plant calculations to make sure the input and output streams balance correctly, and checking that the hydrogen fuel cell rates are also internally consistent. Now I'm verifying the methane fuel cell calculations—the stoichiometric ratios check out with CO₂ and H₂O as products, and the mass balance confirms the input and output flows match at 0.655 t/h. Moving into methanol fuel cells, I'm working through similar calculations where the reactants are methanol and oxygen, producing CO₂ and water with a total flow around 1.005 t/h.
> 
> Now I need to verify the resource names for the fusion reactions—checking that HYDROGEN is correct for the D-³He and ³He-³He reactions since the proton products form hydrogen gas, and confirming the HELIUM resource designation for the ⁴He fraction.
> 
> For the extraction operations, I'm formatting them with just the name field populated and empty tab-separated cells for the remaining columns. I'm also thinking through some suggestions for improving the model, particularly around the lithium resource gap since D-T fusion requires lithium for tritium breeding but there's no dedicated lithium resource currently defined.
> 
> For manufacturing, biological, and refining operations, I need to carefully track mass balances and stoichiometric ratios based on resource compositions. I'm also considering whether Oil Power should output crude oil byproducts like vanadium and nickel that end up in ash, though the quantities are quite small.
> 
> For coal power, I'm thinking through the nitrogen content—coal has about 1.4% N that combusts into N₂ and NOₓ, which modern plants reduce back to N₂ through SCR/SNCR, so it exits as atmospheric nitrogen in the flue gas. I'm ready to construct the final operations table now, being careful with the formatting. goes back to the atmosphere. It's not really a useful output or a waste product. I think it's appropriate to fold it into the general mass balance without explicitly listing it. Since I already put it in INDUST_WASTE, let me reconsider.
> 
> Actually, N₂ released to atmosphere is different from industrial waste. N in coal at 1.4% of 0.343 t/h coal = 0.0048 t/h, which is small but not negligible. Where should it go?
> 
> Options:
> 1. NITROGEN output (N₂ released)
> 2. INDUST_WASTE (doesn't really make sense for a gas)
> 3. Fold into rounding/simplification
> 
> I think NITROGEN is most correct. The N leaves the system as N₂ gas. Let me adjust the outputs: CO₂ at 2.42, H₂O at 0.495, SULFUR at 0.015, and NITROGEN at 0.014. For industrial waste, I need to remove the nitrogen I previously included, so that's 0.135 minus 0.014, leaving 0.121 for the mineral matter. Checking the mass balance: 2.42 + 0.495 + 0.015 + 0.014 + 0.121 equals 3.065, which matches the input of 1.000 coal plus 2.065 oxygen. I'm deciding to fold nitrogen into industrial waste for both coal and oil since the amounts are negligible, keeping the table simpler while still capturing the stoichiometric products. Coal ash is appropriately classified as industrial waste even though it's recoverable—the downstream recycling is handled by other operations. Now I'm doing a final mass balance check across all power generation operations to ensure inputs and outputs match.
> 
> All the combustion and fuel cell rows check out. The fission operations are straightforward single-isotope inputs with minimal outputs. For the fusion reactions, the mass balances work for D-T, D-³He, and ³He-³He. But I'm realizing that using INDUST_METALS as a proxy for lithium in D-T fusion is problematic since lithium isn't even in that resource's composition—it would artificially inflate demand for copper and manganese. The cleanest approach is to acknowledge this gap and either model D-T fusion without the lithium input for now, or flag that a dedicated LITHIUM resource should be added to the model. I'm realizing that Option A actually loses critical information about lithium consumption, which the model needs to know fusion requires. But the mass balance constraint is non-negotiable, so I'm forced back to Option A: tracking just the deuterium-to-helium conversion with forced mass balance, while prominently noting that lithium consumption of 2.51e-8 t/h isn't being captured and should be added once a lithium resource is defined.
> 
> Actually, I can work around this by using INDUST_METALS as a proxy input alongside deuterium—it's not chemically accurate, but at least the mass balances and the model knows fusion needs multiple inputs. I'll flag this compositional mismatch in my notes so it can be corrected later.
> 
> Now I'm reviewing the resource naming conventions to make sure everything follows the abbreviation rules before finalizing the table. Continuing through the rest of the operation names for mining and extraction processes. I'm also reconsidering the liquid hydrocarbons power calculation—I simplified the fuel composition to just carbon and hydrogen, then adjusted the CO₂ output coefficient to account for the trace elements like sulfur and nitrogen that would produce additional emissions.
> 
> The sulfur content in liquid hydrocarbons is much lower than crude oil, so absorbing it into rounding seems reasonable rather than adding another output resource. Now I'm working through the final table format, though I'm second-guessing the efficiency assumptions for methanol fuel cells since direct methanol fuel cells typically operate at 20-30% efficiency.
> 
> Looking at the broader fuel cell landscape, SOFCs running on methanol can reach 50-55%, which is significantly higher. Given the forward-looking nature of this scenario, I'll weight toward the DMFC pathway at 0.45 efficiency. For the other fuel cells, PEM hydrogen at 0.55 seems solid, and I could push SOFC on methane to 0.58 since current technology already sits in the 55-65% range.
> 
> Actually, let me settle on a cleaner set: hydrogen fuel cells at 0.55, methane fuel cells at 0.55 using SOFC technology, and methanol fuel cells at 0.45 as a DMFC/SOFC blend. These all fit the "forward-looking but within current range" criterion.
> 
> Now reconsidering methane power generation more broadly—the description mentions gas turbines and reciprocating engines, and combined cycle gas turbines can already hit 63-64% efficiency. While advanced CCGT reaches 0.63, I need to account for the production-weighted average across simple cycle (35-40%) and combined cycle systems. Keeping 0.50 as a representative blend makes sense within current Earth operations, even if it's conservative.
> 
> For hydrogen, I'm distinguishing between combustion at 0.45 efficiency versus fuel cells at 0.55—fuel cells are inherently more efficient with the same fuel, which tracks physically.
> 
> Generating the efficiency table... Now I'm looking at the efficiency and fuel specifications for fuel cell and nuclear power generation methods, comparing their input requirements and waste outputs across hydrogen, methane, and methanol fuel cells, as well as various fission and fusion reactor types.
> 
> I'm cross-referencing the extraction operations list against what I've documented—I count 19 total operations from atmosphere separation through regolith quarrying, which matches the source material. But I'm noticing a gap: D-T fusion requires lithium for tritium breeding, yet I don't have a dedicated lithium resource defined, so I've been using industrial metals as a proxy even though that doesn't accurately represent lithium's composition.
> 
> Beyond lithium, there's a broader issue with how I'm handling trace elements and simplifications across the energy and extraction chains. I'm folding sulfur and nitrogen from fossil fuels into waste streams rather than tracking them separately, treating coal ash as generic industrial waste despite its recoverable rare earth content, and essentially ignoring mass-energy conversions in fusion and fission since they're negligible at the scale I'm modeling. Renewable sources like solar and wind have no resource inputs tracked—just efficiency-based waste heat.
> 
> For the remaining operations, I need to lean on stoichiometry for conversions and synthesis, use the defined resource compositions to back out manufacturing inputs and track subtractive waste, apply ore grades to determine refining yields with gangue going to waste, and be careful with biological operations to balance CO₂, O₂, and water flows.
> 
> Now I'm realizing there's a tension with operations that both consume and produce the same resource—like water in methanation or the water-gas shift—where the instruction prohibits having the same resource as both input and output, but the chemistry genuinely requires it. I should track only the net flow in these cases rather than trying to represent both directions simultaneously.
> 
> For the final compilation, I'm also reconsidering whether "other" components in crude oil should map to industrial waste. The trace metals and organometallics in oil (vanadium, nickel, and similar impurities) end up as ash during combustion, so industrial waste is the right category. The coal ash fraction I calculated—around 4.4% of the total mass flow—aligns with typical coal ash content, which gives me confidence in that classification.
> 
> Now I'm second-guessing the efficiency assumption for wind power. If I set efficiency at 0.45, the simulation would calculate waste heat as 1.22 MW per MW of output, but a wind turbine doesn't actually generate that much waste heat—the unconverted wind energy just stays in the wind itself. For thermal management purposes, I should use much higher efficiencies for renewable sources that don't produce significant waste heat. I'm thinking wind at 0.95, hydro at 0.92, and tidal at 0.90 to better reflect the actual mechanical-to-electrical conversion losses rather than implying large thermal outputs.
> 
> The real question is how to define "efficiency" consistently across different energy sources. For wind and hydro, the input energy is the kinetic or potential energy they actually capture, not the total available in the environment. For solar, I need to decide whether the input is the total incident light or just the absorbed portion. Since the simulation uses efficiency to calculate waste heat, I should define it as the ratio of electricity output to the total energy the device processes from the environment. For solar panels in space, that means accounting for the ~90% absorption rate and ~25% conversion to electricity, giving an efficiency of about 0.28 relative to absorbed light. I think the cleanest approach is to define efficiency relative to total incident or available energy, which keeps the calculation consistent across all sources. I'm verifying these efficiency values against real-world standards—wind and hydro at 90-95% are already achieved today, so they're within the "forward looking but current" range. For a solar system-wide simulation, the waste heat calculations become physically meaningful too: solar panels in space would need to radiate that thermal energy, and the same logic applies to wind turbines on Mars or Titan. I think this approach is consistent across all energy types. The actual waste heat from solar is about 2.8 MW per MW_e when accounting for reflected light, which is close enough to the 3.0 MW estimate from the efficiency formula. I'm settling on these efficiency values: solar at 0.25, geothermal at 0.18 (accounting for modern binary cycle plants and flash steam systems), hydropower at 0.92, wind at 0.95, and tidal at 0.90. For geothermal specifically, I should clarify whether the 0.18 represents just electrical output or includes process heat generation as well.
> 
> Now I'm realizing the extraction operations should have blank electricity fields per the instructions, and I need to verify my fission calculations for LEU burnup rates before finalizing the table. The thorium calculation checks out at 4.36e-6 t/h. Now I'm preparing the final output with a table and some important notes about the assumptions—particularly that the oxygen splits in coal are estimates, and that renewable sources have a different physical meaning for efficiency even though the mathematical treatment stays consistent. I should also clarify that solid fuels aren't in the operations list because they're used for propulsion, not power generation, and I'm reconsidering whether to distinguish between concentrating solar thermal and PV efficiency.
> 
> For wind power, I'm realizing the 0.95 efficiency needs context: the Betz limit means real turbines only capture about 45% of wind energy, so a 1 MW turbine intercepts roughly 2.22 MW of wind energy. The remaining 1.22 MW stays as kinetic energy in the wind itself, not heat. The actual waste heat comes from bearing friction and generator losses—about 5% of the captured mechanical energy.
> 
> With 0.95 efficiency, the waste heat calculation gives roughly 53 kW for a 1 MW turbine, which is physically sound. Using 0.45 would incorrectly suggest 1.22 MW of waste heat, so 0.95 is the right choice for this model.
> 
> For solar at 0.25, the waste heat works out to about 3 MW per 1 MW of electrical output, which makes sense given how much heat solar panels actually dissipate. Geothermal at 0.15 yields roughly 5.67 MW of rejected thermal energy per 1 MW electrical, consistent with Carnot-limited efficiency in geothermal plants.
> 
> Now I'm formatting the response table with proper tab separation for TSV compatibility.
> 
> Generating markdown table...
> 
> For the manufacturing improvements, I need to think about defining inputs based on actual product composition from the resources file, then accounting for waste from subtractive processes. The key insight is decomposing finished products into their raw material components.
> 
> For chemical conversions like the water-gas shift reaction, I need to make sure stoichiometric inputs like water are explicitly included in the tracking. Then I'll normalize manufacturing operations to 1 t/h output, except for high-value items like precious metals or processors which should be at 1 kg/h.
> 
> Many industrial processes consume electricity rather than produce it—aluminium refining, electric steelmaking, semiconductor fabrication—so those need negative electricity values. Refining operations often yield multiple useful products, so I need to carefully allocate outputs across streams rather than treating them as single-product operations.
> 
> Biological systems are particularly complex since agriculture involves soil, water, fertilizer, and CO₂ inputs, with crop products, oxygen, and biowaste as outputs. The photosynthesis reaction means CO₂ input and O₂ output should be stoichiometrically tied to the carbon content of what's being grown.
> 
> Now I'm working through the renewable energy sources—geothermal, hydro, wind, and tidal all have efficiency values but no fuel input. I can derive waste heat from the efficiency ratio: if electricity output is 1 unit and efficiency is e, then waste heat equals electricity × (1 - e) / e. This gives me physically sensible values across all the renewables, from solar's 3.0 MW waste heat down to wind's 0.053 MW. Everything checks out, so I'm ready to finalize the response and verify the oil output rates.
> 
> Generating the final table...

Here is the table for the Energy and Extraction groups.

```tsv
name	electricity	efficiency	input_resources	input_rates	output_resources	output_rates	mass_flow
Unit	MW				t/h		t/h	t/h
SOLAR_POWER	1	0.25					0
GEOTHERMAL_POWER	1	0.15					0
HYDROPOWER	1	0.92					0
WIND_POWER	1	0.95					0
TIDAL_POWER	1	0.90					0
HYDROGEN_POWER	1	0.45	HYDROGEN;OXYGEN	0.0667;0.533	WATER	0.600	0.600
METHANE_POWER	1	0.50	METHANE;OXYGEN	0.144;0.576	CARBON_DIOXIDE;WATER	0.396;0.324	0.720
METHANOL_POWER	1	0.42	METHANOL;OXYGEN	0.431;0.646	CARBON_DIOXIDE;WATER	0.592;0.485	1.077
ETHANOL_POWER	1	0.42	ETHANOL;OXYGEN	0.320;0.668	CARBON_DIOXIDE;WATER	0.612;0.376	0.988
LIQ_HYDROCARBONS_POWER	1	0.42	LIQ_HYDROCARBONS;OXYGEN	0.199;0.677	CARBON_DIOXIDE;WATER	0.625;0.251	0.876
OIL_POWER	1	0.38	OIL;OXYGEN	0.237;0.760	CARBON_DIOXIDE;WATER;SULFUR;INDUST_WASTE	0.734;0.256;3.56e-3;3.32e-3	0.997
COAL_POWER	1	0.42	COAL;OXYGEN	0.343;0.708	CARBON_DIOXIDE;WATER;SULFUR;INDUST_WASTE	0.830;0.170;5.15e-3;0.0463	1.051
HYDROGEN_FUEL_CELLS	1	0.55	HYDROGEN;OXYGEN	0.0545;0.436	WATER	0.491	0.491
METHANE_FUEL_CELLS	1	0.55	METHANE;OXYGEN	0.131;0.524	CARBON_DIOXIDE;WATER	0.360;0.295	0.655
METHANOL_FUEL_CELLS	1	0.45	METHANOL;OXYGEN	0.402;0.603	CARBON_DIOXIDE;WATER	0.553;0.452	1.005
LEU_FISSION_POWER	1	0.34	LE_URANIUM_FUEL	3.47e-6	SPENT_URANIUM_FUEL	3.47e-6	3.47e-6
HEU_FISSION_POWER	1	0.30	HE_URANIUM_FUEL	3.70e-6	SPENT_URANIUM_FUEL	3.70e-6	3.70e-6
THORIUM_FISSION_POWER	1	0.34	THORIUM_FUEL	4.36e-6	SPENT_THORIUM_FUEL	4.36e-6	4.36e-6
D_T_FUSION_POWER	1	0.40	DEUTERIUM;INDUST_METALS	8.39e-9;2.51e-8	HELIUM	3.35e-8	3.35e-8
D_3HE_FUSION_POWER	1	0.50	DEUTERIUM;HELIUM_3	8.22e-9;1.23e-8	HELIUM;HYDROGEN	1.63e-8;4.20e-9	2.05e-8
3HE_3HE_FUSION_POWER	1	0.55	HELIUM_3	3.18e-8	HELIUM;HYDROGEN	2.11e-8;1.07e-8	3.18e-8
RADIOISOTOPE_POWER	1	0.10					0
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
```

## Calculation Basis

**Combustion and fuel-cell operations** are normalized to 1 MW electrical output. Fuel mass rate is derived from fuel LHV and efficiency: \(\dot{m}_\text{fuel} = P_e / (\eta \times \text{LHV})\). Oxygen demand and product masses (CO₂, H₂O) follow from stoichiometric combustion of the fuel's elemental composition as defined in physical_resources.md. Key LHVs used: H₂ 120 MJ/kg, CH₄ 50 MJ/kg, CH₃OH 19.9 MJ/kg, C₂H₅OH 26.8 MJ/kg, liquid hydrocarbons 43 MJ/kg, crude oil 40 MJ/kg, coal 25 MJ/kg.

**Coal ash** is computed from the mineral-cation fraction (Si, Al, Fe, Ca, K, Mg, Na, Ti, P, other = 7.6%) plus their stoichiometric mineral-bound oxygen (~4.5%), totaling ~12.1% of coal mass. Of the 18% total oxygen in coal, roughly 13.5% is organic (available for combustion) and 4.5% is mineral-bound. Fuel-bound nitrogen (1.4%) and sulfur (1.5%) are output as INDUST_WASTE and SULFUR respectively.

**Fission operations** use the burnup figures implied by the resources file: LEU 50 GWd/tHM, HEU 150 GWd/tHM, Th 40 GWd/tHM. Fuel assembly mass rates account for the heavy-metal fraction of total assembly mass (LEU 70.5%, HEU 25%, Th 70.3%). Mass-energy conversion from fission is negligible (~0.005%) and ignored.

**Fusion operations** use the combined reaction energies (D-T with lithium breeding: 22.4 MeV, D-³He: 18.3 MeV, ³He-³He: 12.86 MeV) and track atomic-mass-unit quantities per reaction. Mass-energy deficits (~0.1%) are absorbed into output rounding.

**Renewable sources** (solar, geothermal, hydro, wind, tidal) and radioisotope power have zero tracked mass flow. Efficiency values are set so that \((1-\eta)/\eta\) yields the correct waste-heat-to-electricity ratio: for hydro, wind, and tidal this reflects mechanical-to-electrical conversion losses only, whereas for solar and geothermal it reflects the full primary-to-electrical gap.

## Resource Gap Identified

**Lithium for D-T fusion.** The D-T blanket breeding reaction (⁶Li + n → T + ⁴He) consumes lithium as fuel at ~2.51 × 10⁻⁸ t/h per MW_e. No dedicated lithium resource exists in the current resources list; lithium appears only as a minor component of INDUST_METAL_ORES (0.2%) and BATTERIES (2%). I used INDUST_METALS as a mass-balance proxy, but its composition (29% Cu, 23% Mn, etc.) is entirely wrong for this use. Consider adding a LITHIUM resource (or at least a lithium-bearing intermediate such as lithium carbonate/hydroxide) before the full operation set is built, since lithium also figures prominently in battery manufacturing and certain industrial-chemical pathways.

## Simplifications

**Coal and oil byproducts.** Fuel-bound nitrogen in coal and oil is folded into INDUST_WASTE rather than tracked as a NITROGEN output (coal N = 1.4%, oil N = 0.4%). Coal ash is output as INDUST_WASTE despite containing recoverable REE and construction-grade material; downstream recovery would occur via INDUST_RECYC_INCINERATION.

**Liquid hydrocarbons trace elements.** The 0.5% non-C/H fraction (S, N, O, other) is absorbed into CO₂ rounding rather than explicitly tracked, since the sulfur content (0.15%) is an order of magnitude below oil and coal.

**Radioisotope source decay.** Pu-238 and Sr-90 consumption is not tracked; the isotope source is treated as part of the unit hardware per the "no maintenance/repair parts" instruction.

**Rounding.** All rates are reported to 3 significant figures. Cumulative rounding can produce input/output sum discrepancies up to ~0.1% in some rows (notably OIL_POWER and HYDROGEN_POWER/HYDROGEN_FUEL_CELLS).

## Efficiency Rationale

The "waste heat fraction = 1 − η" interpretation drives efficiency choices in two regimes. For mechanical-to-electrical converters (hydro, wind, tidal) where unconverted primary energy remains in the medium (not as heat), efficiency is set near the actual electromechanical conversion limit (0.90–0.95) so that waste heat isn't vastly overstated. For thermal converters (combustion, fission, geothermal, solar), efficiency represents the full primary-to-electrical ratio, and the complement genuinely becomes waste heat.

Fusion efficiencies are speculative by nature. D-T at 0.40 reflects first-generation magnetic-confinement plants limited by neutron-driven thermal cycles. D-³He at 0.50 and ³He-³He at 0.55 reflect the progressive advantage of charged-particle products amenable to direct energy conversion.

## Suggestions for Remaining Operations

**Conversions/Synthesis:** These are the most stoichiometrically constrained and should be straightforward. Watch for operations where water appears on both sides of the reaction (e.g., CO₂ methanation produces water; water-gas shift consumes water). Per the "no same resource as input and output" rule, only the net direction should appear.

**Refining:** Ore-to-product yield ratios should be derived from the grade percentages in the resources file (e.g., iron ores at 55% Fe grade). Gangue, slag, and tailings should go to INDUST_WASTE. Many refining operations produce valuable byproducts (e.g., sulfuric acid from sulfide smelting, PGM from base-metal anode slimes) that should appear as outputs. Electricity is a major input for electrolytic processes (aluminium, electric steelmaking).

**Manufacturing:** Product compositions from physical_resources.md can drive the input bill of materials. A subtractive-manufacturing waste fraction (machining chips, offcuts, overspray) of 5–15% of input mass flowing to INDUST_WASTE is typical, varying by operation. Electricity consumption will be significant and highly variable (e.g., semiconductor fabrication is extremely electricity-intensive per unit mass vs. steel structures manufacturing).

**Biological:** Photosynthetic operations (agriculture, algal cultivation) consume CO₂ and water and produce O₂ stoichiometrically with the carbon and hydrogen in the organic product. This needs careful mass balancing since the CO₂ and H₂O inputs are large relative to crop mass. Fermentation and digestion operations are effectively the reverse. Consider normalizing at 1 t/h for bulk operations (agriculture, food processing) and perhaps 1 kg/h for pharmaceuticals.

**General:** You may want to add a normalized "electricity" sign convention note to the table metadata—positive for generation, negative for consumption—so that all operations can be handled uniformly.
