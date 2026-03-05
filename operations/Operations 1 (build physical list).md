*Claude-Opus-4.6 (output effort = high). March, 2026.*

---
---

I want you to develop the context file physical_operations.md. The goal is to include (as abstract simplifications) all industrial activities that involve extraction, refining, production, and/or conversions among resources listed in physical_resources.md, considering both current Earth activities and future solar system development. In short, we need to represent all of the industrial activities that convert our "extracted" resources into all of the other resources.

Consider the problem as a graph with nodes (physical_resources.md items) and edges (physical_operations.md items). However, I don't want to see lists of the resource terms (exactly as they appear in the context file) in the developed physical_operations.md file.

This is a first pass only; we'll improve the list in later passes. I don't have specific number requirements for operations at this time. I expect there to be more operations than resources: the resource graph needs more "edges" than "nodes" for a robust and flexible economy. Focus on broad inclusion now, but use abstractions/simplifications where possible to keep the number manageable.

Please also complete the 3 incomplete descriptions in physical_resources.md for Iron, Uranium and Thorium. Suggest additions for this file only if they fill major gaps in the abstraction (I don't want to grow this list more than necessary).


Notes:

- Add items, complete items, and/or modify items in physical_operations.md as needed. The existing items are intended to help you understand the desired level of abstraction and specificity.
- Items in physical_operations.md need brief descriptions analogous to items in the physical_resources.md context file (without elemental compositions). Include chemical conversions where it might be especially helpful (e.g., Steam Reforming, Haber–Bosch Process, etc.) but not where conversions are basic knowledge and implicit (e.g., combustion in Coal Power). Do mention valuable co- or by-products using regular language with compound or element names as appropriate. Don't list waste products (these are usually obvious).
- Some existing items have notes in brackets. Consider the notes for individual items and then delete. 
- Use existing abstractions and simplifications in physical_resources.md. For example, there is no "Yellowcake" resource, so one refining operation converts Uranium Ores to Uranium, another manufacturing operation converts Uranium plus other resources to LE Uranium Fuel, etc.
- Resource conversions can be more complex than the short name might imply. For example, Industrial Metals Mining produces other ores in addition to Industrial Metal Ores (the name here refers the main target). Rare Earths Refining produces Thorium in addition to Rare Earths. Etc.
- Be general where possible. For example, "Iron Mining" can be implemented to handle any targeted extraction of Iron/Nickle or Iron Ores in any strata that has relevant concentrations. "Volatiles Extraction" can handle extraction of Water or Helium-3 from appropriate strata. Etc.
- Be specific where necessary. An example here might be conversion of Martian regolith and atmosphere to rocket propellants or other useful resources. If an operation exploits a unique stratum potential (and isn't covered by a more generic term), then add it as such. We want these.
- The context file strata.md is included mainly to help when developing extraction operations. Consider different extraction operations to handle asteroids as bulk strata. These may or may not parallel the existing "mining" operations (these were added in consideration of crustal extraction on Earth or similar bodies).
- Energy is obviously a big part of simulation implementation. You can consider it in terms of plausible conversions, but you don't need to reference it for the most part since it is universal. 
- Be creative and speculative! But do note cases where you might be stretching plausibility.

---
---

*Thinking...*

> The user wants me to:
> 
> 1. Develop the `physical_operations.md` context file with all industrial activities that convert extracted resources into other resources
> 2. Complete 3 incomplete descriptions in `physical_resources.md` (Iron, Uranium, Thorium)
> 3. Suggest additions to `physical_resources.md` only if they fill major gaps
> 
> Let me think through this systematically.
> 
> ## Completing physical_resources.md entries
> 
> ### Iron
> Looking at the existing entry:
> - **Iron** — Industrial pig iron. [Needs completion!]
> 
> Pig iron is the intermediate product from blast furnace reduction of iron ores. It's the commodity form before steelmaking.
> 
> 
> Now I'm refining the Iron entry with typical composition data—roughly 93.5% iron, 4% carbon, and trace amounts of silicon, manganese, phosphorus, sulfur, and other elements like chromium and titanium that naturally occur in blast furnace pig iron.
> 
> ### Uranium
> Looking at context: "Purified, unenriched UF₆.
> 
> UF₆ (uranium hexafluoride) is the standard commodity form for enrichment feedstock. Molecular formula UF₆, molecular weight 352.02. U = 238.03/352.02 = 67.6%, F = 6×19.00/352.02 = 32.4%.
> 
> **Uranium** — Purified, unenriched UF₆ (natural assay: 0.711% ²³⁵U); ref. conversion-plant product meeting ASTM C787 specifications. U 67.6 (isotopic: 0.711% U-235, 99.289% U-238), F 32.4.
> 
> ### Thorium
> Looking at context: "[Needs completion! This is supposed to be the 'commodity' compound that is subsequently processed/
> 
> The standard thorium commodity is thorium dioxide (ThO₂), also called thoria. It's the form in which thorium is typically stored and traded. ThO₂: Th = 232.04/264.04 = 87.9%, O = 32.00/264.04 = 12.1%.
> 
> **Thorium** — Purified thorium dioxide (ThO₂); ref. nuclear-grade thoria powder meeting ASTM C28-standard purity. Th 87.9, O 12.1. Byproducts: trace U, total REE (from co-processing with rare earth ores; tracked under Rare Earths).
> 
> ## Now for physical_operations.md
> 
> I need to map out the conversion pathways from extracted resources through refining and manufacturing stages to reach the final fuel forms.
> 
> For the energy group, I'm listing all the power generation operations that convert various fuel sources into electricity. These range from renewable sources like solar, geothermal, hydropower, wind, and tidal that require no fuel input, to combustion-based systems using methane, coal, and oil, through to nuclear fission with different uranium enrichment levels and thorium, and fusion power using deuterium and tritium.
> 
> Then I'm considering a few additional possibilities like hydrogen fuel cells, biofuels, and radioisotope thermoelectric generators that might fit into this category.
> 
> For extraction, I'm mapping out the operations that pull raw resources from planetary surfaces and atmospheres—everything from separating atmospheric gases and mining water or helium-3 from various deposits, to drilling for hydrocarbons and mining different categories of metals from iron and aluminum to precious metals.
> 
> I'm also including uranium and rare earth mining, stone and regolith quarrying, and then some maybes like organics extraction for kerogen and tholins, sulfur mining, ocean or brine mining for dissolved minerals, ice mining from polar regions, and bulk asteroid mining for undifferentiated bodies.
> 
> Now I'm moving into the refining and conversion operations—the chemical and metallurgical processing that transforms raw materials into usable forms, starting with oil refining and then the various metal refining processes for iron, nickel, and aluminum.
> 
> Continuing with precious metals and rare earths refining, uranium conversion to UF₆, and then the key chemical synthesis processes like water electrolysis to produce hydrogen and oxygen, Fischer-Tropsch for making hydrocarbons, Sabatier for methane production, steam reforming, water-gas shift reactions, and reverse water-gas shift to cycle between carbon monoxide and dioxide.
> 
> Now I'm considering various steelmaking routes—basic oxygen, electric arc, hydrogen reduction, and solar-powered approaches—along with a longer list of potential industrial processes like cement, glass, polymers, semiconductors, uranium enrichment, fuel fabrication for different reactor types, coal processing, sulfur chemistry, methanol synthesis, ammonia-based fertilizers, and isotope separation.
> 
> Then I'm organizing manufacturing into its own category, covering everything from structural steel and aluminum fabrication through composites, heavy machinery, electrical and precision equipment, electronics including specialized processors, transport systems, robotics, renewable energy components like solar panels and batteries, and general consumer and specialty goods production.
> 
> Finally, I'm mapping out biological production systems—both open and closed variants of agriculture, aquaculture, and forestry—plus potential bioprocessing applications like fermentation, pharmaceuticals, and fertilizer production.
> 
> Now I need to verify the resource graph is complete by ensuring every non-extractable resource has at least one production method and every extractable resource has a corresponding extraction pathway. Looking at energy resources specifically: hydrogen can be extracted or produced through electrolysis and steam reforming, methane comes from drilling or atmosphere separation or the Sabatier process, refined fuels from oil refining or Fischer-Tropsch synthesis, biofuels from biological processes, coal and oil from mining and drilling respectively, and enriched uranium fuel through manufacturing.
> 
> For the ore category, I'm checking that all the metallic and mineral resources have extraction pathways—iron and nickel from metallic asteroid mining, various ore types from their corresponding mining operations like aluminum mining, industrial metals mining, precious metals mining, rare earths mining, and uranium mining.
> 
> Then there's sulfur as a byproduct of oil refining, industrial minerals from dedicated mining, organics and tholins needing extraction operations, and stone and regolith from quarrying operations.
> 
> For volatiles, I'm mapping out the extraction sources: water from volatiles extraction or chemical reactions, oxygen from atmosphere separation or water electrolysis, nitrogen and carbon dioxide from atmosphere separation, carbon monoxide from various chemical processes, ammonia from the Haber-Bosch process or direct extraction, and ethane from gas drilling or volatiles extraction.
> 
> Now I'm looking at additional volatiles like sulfur dioxide from Venus atmosphere separation, helium and argon from atmosphere separation or gas drilling, heavy noble gases from atmosphere separation, and then industrial and fine chemicals both coming from chemical synthesis operations. For refined materials, I'm tracking iron and steel from their respective refining processes, aluminium from refining, industrial metals from their refining operations, precious metals and rare earths from dedicated refineries, uranium from uranium refining, thorium as either a byproduct of rare earths processing or its own operation, and concrete as a constructed material.
> 
> For the next tier of materials, I need to identify production operations for glass and ceramics, semiconductor materials, polymers, composites, technical textiles, while industrial waste and radioactive waste are byproducts from various operations rather than standalone production chains. Then moving into manufactured goods, I'm mapping structures made from steel, aluminium, and composites through fabrication, heavy machinery and electrical equipment through manufacturing, precision and electronic equipment including specialized processors through manufacturing, transport systems and robotics through manufacturing, and energy-related products like solar panels and batteries through manufacturing, along with consumer and specialty goods.
> 
> Now I'm working through biological materials and products—crops and animal products come from agriculture or aquaculture, packaged meals from food processing, while artisanal foods and goods need their own operations. Biofibers, lumber, and wood products flow through forestry, algal and microbial products through bioprocessing, biofeedstock through agriculture or forestry, biochemicals and pharmaceuticals through their respective processing operations, fertilizers from chemical production, and biowaste as a byproduct. I'm starting to organize these into a comprehensive operations framework, beginning with energy systems that convert various inputs into power generation.
> 
> I'm listing out the energy conversion operations—solar, geothermal, hydropower, wind, tidal, methane combustion, coal, oil, hydrogen fuel cells, and various fission and fusion pathways including radioisotope generators for space applications. I'm considering whether biofuel power should be separate or consolidated with methane and oil power, and I'm checking this against what the user already has to avoid redundancy.
> 
> Now I'm mapping extraction operations to their specific contexts—atmosphere separation for gases, volatiles extraction from regolith and ice, subsurface drilling for gas and oil, mining operations for various mineral types from iron and aluminum to uranium and rare earths, each tied to where they'd actually occur.
> 
> But I need to reconsider the scope here. Something like iron mining should be general enough to cover targeted extraction of iron or nickel ores across any strata, including asteroids. The real distinction might be whether we're doing targeted mining versus whole-body processing of undifferentiated asteroids as a bulk operation.
> 
> For C-type asteroids specifically, thermal desorption to drive off volatiles could be its own operation, separate from the mechanical breakup and sorting. Though thinking about it more, the existing extraction categories probably handle most asteroid scenarios if they're properly stratified—iron mining on a metallic asteroid's bulk stratum, volatiles extraction on a C-type, regolith quarrying on any surface. The question is whether there's enough value in a dedicated bulk thermal processing operation to warrant its own category. For sulfur, I need to decide whether it's better handled as a byproduct of refining operations or as its own extraction type—given that Io makes it a special case with surface sulfur, I think a dedicated Sulfur Mining operation makes sense. For organics and tholins, I'm working through the extraction pathways across different bodies: kerogen from oil shale on Earth, surface tholins on Titan, and I need to figure out how to handle them on C-type asteroids.
> 
> I also need to account for ocean extraction on Earth since the strata document mentions dissolved minerals and salt refining as distinct operations. For the specialized ISRU processes like Mars's CO₂ conversion chain and Titan's liquid hydrocarbon harvesting from the methane-ethane seas, most of these fit into existing chemical operations, though I'm still considering whether "Volatiles Extraction" adequately covers Titan's case or if it needs its own category.
> 
> Now I'm mapping out the core refining operations: oil refining to produce fuels and polymers, metal refining for iron and nickel, and aluminium processing from ore.
> 
> Beyond those, I need to account for specialized ore refineries—industrial metals, precious metals, rare earths with thorium byproducts, and uranium enrichment to UF₆. Then there are the synthetic fuel pathways: water electrolysis for hydrogen and oxygen, Fischer-Tropsch for hydrocarbon synthesis, Sabatier for methane production, and steam reforming to generate syngas. I'm continuing through the industrial processes list, adding cement, glass, polymers, semiconductors, and other specialty chemical productions. But I'm realizing I should reorganize this by thinking about which resources actually need production pathways versus extraction—focusing on what can't be mined directly and what requires manufacturing steps like oil refining, Fischer-Tropsch synthesis, and biofuel production.
> 
> Now I'm mapping out the refined fuels and manufactured materials: uranium and thorium fuels need fabrication, various metals require refining or smelting from their ores, and rare earths produce thorium as a byproduct during their own refining process. Finishing up the biological category with biofibers through forestry and bioprocessing operations, then moving into organizing everything into final operation groups—energy around 15 items, extraction around 18, and refining/conversions expanding to roughly 25 to keep things comprehensive but workable.
> 
> Now I'm structuring the full operations file with notes about how the five groupings are organizational only, and how extraction operations get subdivided by strata in the actual simulation even though the labels here are more generic.
> 
> I'm also noting that natural resource cycles like Earth's water system are handled separately, and there's a sixth services group for intangible operations that isn't included in this file.
> 
> Now I'm detailing the energy operations—solar power as the primary source for inner solar system work, geothermal for planetary bodies with accessible heat, and hydropower which is Earth-focused but theoretically applicable to places like Titan with its methane rivers.
> 
> I'm continuing through the atmospheric and tidal energy sources: wind power works across multiple bodies depending on atmospheric conditions, tidal power is Earth-only, and then the fuel-based pathways like methane combustion (which can use ISRU-produced or harvested sources off-Earth), coal combustion with ash and sulfur byproducts, and oil combustion.
> 
> Now I'm moving into hydrogen fuel cells and combustion turbines as a key off-Earth power option, biofuel and biogas generators for combined heat and power, and then the fission pathways—LEU reactors producing radioactive waste with deuterium as a co-product from heavy-water variants, and HEU reactors for compact naval and space applications that also generate radioactive waste.
> 
> I'm also considering thorium-based thermal-breeder reactors that breed uranium-233 in-situ, deuterium-tritium fusion with lithium blankets for tritium breeding and minimal long-lived waste, helium-3 fusion as a speculative aneutronic option with the lowest waste profile, and radioisotope power systems.
> 
> For radioisotope power, I'm looking at electricity and heat generation from radioactive decay using plutonium-238 or strontium-90 from reactor operations—these have low power output but exceptional reliability and longevity, making them essential for deep space missions and surface operations where solar power isn't viable.
> 
> On the extraction side, I'm examining atmospheric separation techniques like cryogenic distillation and pressure-swing adsorption to pull oxygen, nitrogen, and other gases from planetary atmospheres, with gas giants yielding hydrogen, helium, methane, and ammonia through atmospheric skimming or balloon collection. I'm also considering volatiles extraction through thermal processing and sublimation trapping.
> 
> I'm now looking at how to harvest volatiles from ice-bearing regolith, permafrost, and subsurface reservoirs to produce water, carbon dioxide, ammonia, methane, and helium-3, then moving into hydrocarbon collection from liquid seas like those on Titan to extract methane and ethane, and finally ocean extraction for dissolved minerals and salts. Oil extraction completes the hydrocarbon picture, while Coal Mining is straightforward surface and underground extraction on Earth. Iron Mining gets more complex—on Earth it's conventional open-pit work with oxide and sulfide ores, but on asteroids I'm considering mechanical excavation of metallic bodies and magnetic separation, and on the Moon and Mars, beneficiation of regolith minerals like ilmenite and hematite. Aluminium Mining follows a similar pattern, targeting bauxite and other ore sources.
> 
> Now I'm moving through the industrial and precious metals—these operations target specific ore concentrates like copper, manganese, chromium, and lithium, with iron and rare earth elements as co-products depending on the geological strata. Precious metals mining is currently Earth-focused but could shift off-world through refining asteroidal metal. Uranium mining is next on the list. I'm continuing through the extraction methods—stone and regolith quarrying for bulk materials on airless bodies, then moving into organics recovery from oil shale and carbonaceous sources, and sulfur extraction from volcanic or evaporite deposits, with Io's native sulfur as a special case.
> 
> Now I'm mapping out the refining and conversion processes: oil refining produces fuels and polymers through distillation and cracking, while coal processing via gasification and coking yields syngas and liquefied fuels that feed into downstream synthesis chains like Fischer-Tropsch.
> 
> For metallurgical operations, I'm considering Iron and Nickel refining from asteroidal sources, which co-produces precious metals and industrial metals as significant byproducts, alongside Iron Ore smelting through blast furnaces or direct reduction, and Aluminium refining via the Bayer process. Precious Metals Refining uses fire assay and electrorefining to extract gold, platinum, and related metals from ore concentrates and processing byproducts. Rare Earths Refining then separates individual rare earth elements through acid digestion and solvent extraction, with thorium and uranium as co-products. Uranium Refining follows a similar leaching and extraction pathway, producing uranium hexafluoride and intermediate yellowcake forms.
> 
> Uranium Enrichment then uses centrifuge cascades to concentrate the fissile U-235 isotope for fuel applications, creating both enriched and depleted uranium streams. Water and CO₂ electrolysis represent fundamental chemical processes—splitting water into hydrogen and oxygen, and carbon dioxide into carbon monoxide and oxygen—that serve as building blocks for in-situ resource utilization across space-based operations.
> 
> From there, I'm looking at how synthesis gas gets converted into usable fuels through Fischer-Tropsch catalysis, which chains carbon monoxide and hydrogen into longer hydrocarbons while producing water as a byproduct. The Sabatier reaction then takes carbon dioxide and hydrogen to generate methane and water, forming a core process for Mars ISRU operations.
> 
> Beyond that, I'm considering the supporting reactions: steam reforming breaks down methane with steam to create synthesis gas, the water-gas shift reaction optimizes hydrogen yield by converting carbon monoxide and steam, and the reverse water-gas shift specifically produces carbon monoxide feedstock for Fischer-Tropsch by reacting carbon dioxide with hydrogen. These interconnected pathways create a complete ISRU chain where outputs from one process feed into the next. Electric arc furnaces can recycle scrap steel and industrial waste with lower carbon emissions than basic oxygen processes. Hydrogen reduction of iron ore produces water instead of CO₂, while speculative solar-thermal methods could work at permanently sunlit locations like lunar poles or on mobile foundries, yielding steel and oxygen as byproducts.
> 
> Now I'm looking at isotope separation techniques like cryogenic distillation to extract deuterium and helium-3 for fusion fuel, alongside broad chemical synthesis pathways covering everything from acid-base reactions to specialty chemicals, and polymerization processes for creating polymers from various feedstocks.
> 
> Continuing with biofuel production through fermentation and transesterification of biological materials, and fertilizer manufacturing that combines ammonia with mineral sources into compound products.
> 
> Now I'm mapping out concrete production, which involves heating limestone to create clinker, then grinding and mixing it with aggregates and water—with off-Earth variants using sintered regolith or geopolymer binders. Glass and ceramics production follows a similar pattern of melting silicate minerals with various additives to create different product types. Then there's semiconductor processing, which starts with crystal growth techniques...
> 
> I'm also considering whether to include recycling and material recovery as a separate process category, since that would capture the mechanical and chemical methods for reclaiming materials from waste and end-of-life products. ies. Includes pellet sintering, fuel rod loading, and assembly welding. Then there's solid fuel production, where I'm mixing and casting propellant composites from ammonium perchlorate, oxidizers, aluminum powder, and polymer binders. For structural components, I'm detailing steel fabrication through cutting and welding into vessels and modules, aluminum fabrication via extrusion and rolling into lightweight structures, and composite fabrication through layup and curing processes.
> 
> Moving into heavier industrial manufacturing, I'm covering machinery like turbines and engines assembled from multiple material types, electrical equipment such as generators and transformers, and precision instruments including scientific and medical devices that require careful assembly of steel, aluminum, electronics, and specialized materials like glass and ceramics.
> 
> Then there's the semiconductor and advanced processor space—standard electronics manufacturing through PCB and display production, followed by specialized tensor processor packaging with cutting-edge techniques, and emerging neuromorphic processor fabrication that uses similar advanced semiconductor methods. I'm also starting to outline transport systems manufacturing, which handles everything from ground vehicles to aircraft.
> 
> Continuing through robotics, which combines structural materials with electronics and specialized processors, solar panel production drawing on semiconductor materials and various metals, and battery manufacturing covering the full cell-to-pack integration pipeline. Technical textiles use high-performance polymers and fibers for specialized applications like spacesuits and insulation. Moving into biological systems, I'm mapping out agricultural and aquacultural production methods—open farming and aquaculture that generate crops, animal products, and biofeedstock, along with forestry operations for timber and wood products. Closed Forestry can produce oxygen and biowaste too, though scaling it would be speculative and might only work in large rotating habitats. Bioprocessing covers fermentation and microbial synthesis to create biochemicals and algal products, while pharmaceutical production takes those biochemicals further through synthesis and purification into finished drugs and vaccines.
> 
> Now I'm mapping out the downstream industries—food processing handles preservation and packaging of crops and animal products, artisanal production creates small-batch foods and handcrafted goods from raw materials, and soil conditioning prepares growth media from regolith and other amendments.
> 
> I'm also realizing I need to account for recycling as a major industrial loop, spent fuel reprocessing for the nuclear cycle, and possibly cryogenic processing for operations in the outer solar system—though rare earths refining already covers thorium as a co-product. I'm noticing I left out recycling as a refining operation, and I should verify the major conversion chains are all accounted for—ethane to polymers through cracking and polymerization, ammonia to industrial chemicals, sulfur to sulfuric acid, and biomass to biofuels all seem covered by existing operations. Let me re-examine the original prompt to make sure I'm structuring this correctly as a graph problem.
> 
> The instruction is clear: I shouldn't just list resource names from the context file in formulaic input-output patterns. I need to use natural language descriptions instead, even though the existing operations do reference some resource names contextually. Let me look at how the current physical_resources.md is structured to understand the right balance between naming resources naturally versus avoiding rote lists.
> 
> Now I'm reconsidering the scope—I'm looking at roughly 95 operations across energy, extraction, refining, manufacturing, and biological categories, which might be more than intended.
> 
> I need to review the original file more carefully, particularly those bracketed notes like "[Earth-only]" that were attached to certain operations, and decide whether to fold them into descriptions or remove them entirely. The user also encouraged creative speculation about physically plausible future operations that don't exist yet, so I should identify which ones fit that bill—like deep-ocean mining or atmospheric extraction from gas giants—and make sure I'm not duplicating coverage across categories. I'm realizing I should add a Regolith Processing operation for the Moon and Mars, though looking at it more carefully, the existing framework already covers the key aspects—oxygen extraction through Volatiles Extraction, metal extraction through the strata-subdivided mining operations, and volatile release through heating. Now I'm wondering whether I need a dedicated Waste Processing operation to handle the Industrial, Radioactive, and Biowaste streams that the simulation tracks, or if that's already implicit in the current design.
> 
> I think I'll add Recycling as an explicit operation to cover waste recovery, with disposal and storage handled implicitly. Now I'm ready to finalize the documents and flesh out the physical resources specifications with detailed compositions and standards.
> 
> Looking at what might be missing from the resource list, I'm considering whether propellant oxidizers, cryogens, and specialty carbons need separate entries or if they're already adequately covered by existing categories like Oxygen, Hydrogen, and Solid Fuels. I'm realizing animal feed is probably just a simplification—crops convert to livestock products without needing a separate resource category. For other potential gaps like fiber optics or habitat modules, they're likely already covered under existing categories like electronics or structural materials. There's also no explicit propellant resource combining fuel components, though that might be intentional or covered elsewhere in the model. resources rather than a separate resource category. Now I'm mapping out the conversion pathways to make sure there aren't any gaps in the operations. For Mars, I can trace the full chain from atmosphere separation through water electrolysis to the Sabatier and Fischer-Tropsch processes. For the lunar pathway, I'm starting to work through what that would look like as well. I'm mapping out the remaining extraction pathways—regolith collection from asteroids feeds into the broader resource chain, then moving through Titan's unique atmospheric and hydrocarbon opportunities, gas giant extraction methods, and finally Earth-based industrial processes for coal, oil, and natural gas.
> 
> Now I'm verifying that ethane is properly accounted for across all sourcing methods—it comes from gas drilling on Earth, hydrocarbon harvesting on Titan, and volatiles extraction there as well. The coverage looks comprehensive, so I'm ready to wrap this up.
> 
> Let me double-check sulfur dioxide and carbon monoxide too. Sulfur dioxide can be extracted directly or produced through atmosphere separation on Venus and Io, plus it's a natural byproduct when smelting sulfide ores. Carbon monoxide is trickier since it's rarely found naturally, but I've got multiple production pathways covered through RWGS, steam reforming, CO₂ electrolysis, and coal processing.
> 
> Before I finalize the document, I should verify the user's point about needing more operations than resources—I'm counting 13 energy resources, 13 ores, and 14 volatiles, which should give me enough nodes to work with for building out a robust economy with plenty of edges between them.
> 
> Looking at my operations inventory, I've got around 16 energy operations, 18 extraction processes, roughly 30 refining and conversion steps, about 18 manufacturing operations, and 11 biological processes. That puts me at roughly 93 operations against 87 resources, which is solid, though I'm wondering if I should add a few more to really maximize the interconnectedness of the resource graph.
> 
> Each operation connects multiple resource pairs as inputs and outputs, so even with similar counts of operations and resources, I'm generating hundreds of edges in the graph—that should satisfy the requirement for more edges than nodes. I'm considering whether to add methane cracking or ammonia cracking as hydrogen production alternatives, and I think I'll keep my chemical synthesis operations consolidated rather than splitting them into separate commodity and fine chemical categories.
> 
> For the metallurgical processes, I'm confident that iron ore smelting and iron/nickel refining should remain distinct since they handle fundamentally different material transformations. Now I'm reviewing what resource gaps actually matter for the simulation—the user wants me to be selective about additions, so I need to focus on what's truly missing rather than edge cases like spent nuclear fuel, which can be adequately represented through the existing radioactive waste and reprocessing framework.
> 
> Looking at potential gaps: noble gases are grouped broadly enough for now, lime and coke are implicitly handled through their source materials, but natural gas is a notable omission since it's a primary feedstock for methane and other hydrocarbons on Earth.
> 
> Actually, the simulation sidesteps this by having Gas Drilling produce methane, ethane, and helium directly as separate resources rather than bundling them as natural gas first—that's a reasonable abstraction. Similarly, biowaste production flows implicitly from agricultural and food processing operations into its consumption pathways, which seems adequate. I should finalize the document now, but I need to preserve the existing note about potentially adding a services category as a sixth group.
> 
> Let me review the guidelines once more: I need brief descriptions without elemental breakdowns, include chemical equations for named processes like Steam Reforming or Haber–Bosch, skip equations for obvious conversions like combustion, mention valuable co-products, and avoid listing waste. I should stick with the abstractions already established in the resources file and keep resource conversions aligned with that framework.
> 
> The key constraint is avoiding formulaic lists of resource names—I need to weave them into natural prose descriptions instead of bullet points. So rather than "Produces: Hydrogen, Oxygen, Nitrogen," I'd describe the separation process in flowing language that mentions these outputs contextually.
> 
> Now I'm ready to compile the final deliverables: the completed physical_operations.md file with all sections filled in, the three resource entries for physical_resources.md, any suggested additions, and a summary of the work. I also notice there are placeholder items in the biological section that need to be replaced with actual operations, so I'll fill those with appropriate entries before presenting everything. Now I'm verifying the extraction operations—pulling resources from various sources like the atmosphere, oceans, and geological formations across 18 different methods. Then there are the refining and conversion processes, which appear to continue beyond what's shown here, transforming raw materials into usable forms through smelting, refining, and chemical processing. I'm finishing out the chemical processing list and moving into manufacturing categories—looks like I have 34 chemical processes total, then 18 manufacturing sectors, and I'm starting to map out biological processes but that section got cut off.
> 
> Let me verify the full count across all categories: 16 energy operations, 18 manufacturing, 34 chemical, 18 more manufacturing subcategories, and 11 biological operations adds up to 97 total. That's a good ratio—more operations than resources, which means the graph will have solid connectivity with each operation drawing from multiple resource types.
> 
> I'm double-checking that I haven't missed any critical production pathways, particularly for materials like technical textiles, composites, and concrete—all of which need their own dedicated manufacturing operations to close the loop. I'm continuing through the production methods for the remaining materials—biofuels, solid and refined fuels, agricultural products like biofibers and soil media, artisanal goods, and biochemical feedstocks all map to their respective production processes.
> 
> Now I'm working through the rest of the product categories: packaged meals come from food processing, lumber from forestry operations, algal and microbial products from bioprocessing and aquaculture, pharmaceuticals from their own production systems, and fertilizers from fertilizer production. I'm also noting how waste streams work—industrial waste gets recovered through recycling, radioactive waste through spent fuel reprocessing, and biowaste cycles back into soil conditioning, biofuel production, or bioprocessing.
> 
> Everything looks consistent. I'm ready to write this up. Though I'm second-guessing whether I should include a dedicated regolith processing operation—it's a significant ISRU technology that could directly produce oxygen, iron, silicon, and aluminum from regolith. But looking at how the strata document defines regolith across different bodies, I think the existing mining operations already handle this when applied to regolith layers, so I don't need a separate entry.
> 
> Actually, molten regolith electrolysis is different—it's a single refining process that produces multiple products at once, not just extraction. I should add it as a refining operation since it takes raw regolith and converts it into useful materials. That would make the list more complete and reflect an actual technology being developed. I'm also wondering if I should include carbothermal reduction as another regolith processing method, though I'm still thinking through whether that's necessary. I'm considering whether asteroid capture deserves its own operation, but I think it's really a logistics step rather than a resource conversion process—the actual processing would just use my existing extraction and refining operations. I briefly considered adding a "Bulk Material Separation" operation for sorting heterogeneous materials, but that's typically part of the mining step itself, so it's already covered.
> 
> Now I'm ready to write the document. Let me verify the format requirements one more time: keep descriptions brief without listing elemental compositions, include chemical conversions where they're useful, mention valuable co-products naturally, skip waste products and resource term lists, preserve bracketed notes from the original source material, and embrace some creative speculation while flagging any plausibility stretches.
> 
> Before I draft the main content, I should finalize the physical_resources.md entries and any suggested additions. I don't think major changes are needed, but let me review the propellant category to see if anything's missing there.
> 
> Looking at the nuclear fuel cycle, I'm considering whether depleted uranium needs its own entry or if it can stay implicit as a byproduct tracked within the enrichment operation. For the simulation purposes, I think treating it as a variant of uranium is workable. Superconductors are probably better represented as components of existing categories like Heavy Machinery rather than as standalone resources.
> 
> I'm also noticing there's no explicit intermediate step for some materials—like cement as distinct from concrete—but the Production operation handles those conversions, so that abstraction level seems fine. One thing I want to verify: using Iron as pig iron makes sense as the intermediate between ore and steel, even though in reality pig iron rarely trades as a final product. It's a reasonable abstraction, similar to how Aluminium represents refined metal before fabrication.
> 
> I think the resource list is solid as-is without major additions. Time to finalize my response.
> 
> Now I'm organizing what I've completed: the full physical_operations.md file, entries for Iron, Uranium, and Thorium in physical_resources.md, notes on minimal suggested additions, and feedback. I also need to make sure my operation descriptions use natural language rather than listing resource names directly—so instead of "Produces Hydrogen, Oxygen, and Nitrogen," I'd write something like "Primary products are the separated atmospheric components."
> 
> Actually, looking at how the existing entries work, they reference resources naturally within prose descriptions without making bulleted lists, so I can do the same. Let me structure my response to show what's completed, what entries I've added, any suggestions, and clarifications about the approach. Now I'm reviewing the rest of the bracket notes to understand the context and constraints for each operation. The notes clarify scope limitations like Earth-only operations for oil and coal, the speculative nature of solar foundries at lunar poles, and that manufacturing should prioritize Manufactured resources but remain flexible. I'll incorporate these insights while removing the brackets from the final version.
> 
> For the biological operations, I'm seeing that Open and Closed variants exist for Agriculture, Aquaculture, and Forestry—where Open applies to standard or terraformed environments and Closed applies to sealed systems like space stations or pre-terraformed Mars. I need to decide whether "Closed" is the right terminology or if there's a clearer alternative, then fill in the two empty items in that section.
> 
> I also notice the typo "Iron/Nickle" should be corrected to "Iron/Nickel" throughout. Beyond that, I'm considering whether to add Methane Pyrolysis or Thermal Cracking as an operation, since it's a real process that could fit the simulation's scope. I'm weighing whether steam reforming adequately represents the various methane conversion pathways, and deciding that it does as a useful abstraction. For propellant production, solid fuel needs its own operation, but liquid propellant doesn't require a separate step since it's just storage. Now I have my final categorized list across energy, extraction, refining, manufacturing, and biological operations totaling 98 items.
> 
> Let me verify the refining count by going through the full list—I'm seeing oil, coal, various metal refinings, uranium processes, electrolysis reactions, Fischer-Tropsch, Sabatier, steam reforming, methane pyrolysis, water-gas shift reactions, Haber-Bosch, methanol synthesis, and chemical synthesis, though I need to finish tallying to confirm the exact number. I'm reconsidering the categorization of a few items—Biofuel Production, Fertilizer Production, and Concrete Production feel like they belong in Refining/Conversions since they're fundamentally chemical transformations rather than fabrication of finished goods, even though there's some overlap with Manufacturing.
> 
> Now I'm checking for redundancy between operations like Iron/Nickel Refining versus Iron Ore Smelting to see if any can be consolidated.
> 
> I'm also reconsidering whether Ammonia Synthesis and the Haber-Bosch Process are distinct enough to keep separate, and whether Sulfuric Acid Production warrants its own category given how fundamental it is to industrial processes.
> 
> Actually, they're the same thing, and Sulfuric Acid fits fine under Chemical Synthesis since it's treated as an intermediate in the simulation anyway—I can just mention it in the description.
> 
> Now I'm ready to write the full document. I'm thinking about description length based on what I see in physical_resources.md—aiming for 1-3 sentences per operation with chemical equations where relevant. I should also be specific about co-products, mentioning things like "platinum-group metals" rather than generic category names when it adds clarity, while still using resource category names where they're the natural fit. And I'll skip listing waste products since those are usually obvious.
> 
> Here's my structure: intro, the complete physical_operations.md, finished entries for Iron, Uranium, and Thorium in physical_resources.md, any suggested additions, then feedback covering simplifications, plausibility notes, and improvement suggestions.
> 
> One more consideration—the resource file has detailed compositional notes, so the operations descriptions need to match that level of detail and context. Let me start composing now.
> 
> Actually, I'm second-guessing the energy operation names. "Oil Power" works fine since it refers to burning refined fuels in power plants, and "Methane Power" is consistent with the resource name in the simulation, so both are clear enough.
> 
> Writing the files now...
> 
> Wait, I should verify the resource counts first. The physical_resources.md file has 87 total items across six categories, and I'm documenting 99 operations, which gives me good coverage with more edges than nodes. For the descriptions, I'll keep simple operations like "Wind Power" brief, but expand on complex ones like "Regolith Processing" or "Steelmaking (solar)" to make them clearer. I also noticed the original file had "Iron/Nickle Refining" that I need to check on.
> 
> I'm catching a typo—it should be "Iron/Nickel Refining." And for consistency with how chemical formulas are formatted elsewhere in the resources file, I'll use "H₂ reduction" instead of "H2 reduction" in the operation names. The user wrote "H2" but proper subscript formatting matches the style used for things like "H₂O" and "CO₂" already in the file. Now I'm ready to write the response.
> 
> I'm organizing this into four main sections: the complete physical_operations.md file with all five groups and roughly 99 operations, the finished entries for Iron, Uranium, and Thorium in physical_resources.md, any suggested additions to resources, and then feedback covering simplifications, plausibility, and open questions. I need to be careful not to enumerate resource terms directly in the operation descriptions—I'll weave them naturally into the text instead. I'm settling on the right balance—naming resources directly in prose is fine for clarity in a simulation context, as long as I'm not falling into mechanical list formatting. The key is weaving them naturally into descriptions rather than using rigid "Produces: X. Co-produces: Y" structures. Now I'm drafting the Energy section, starting with solar, geothermal, and hydroelectric power sources and how they apply across different planetary bodies. Oil-based power generation feeds into broader energy infrastructure, then I'm moving through hydrogen as a critical off-world energy vector via fuel cells, followed by biofuel and biomass systems. The nuclear pathways branch into LEU fission as Earth's dominant reactor type, HEU fission for compact space and naval applications, and thorium fission as an emerging alternative.
> 
> Now I'm covering fusion approaches—D-T fusion with in-situ tritium breeding from lithium, He-3 fusion producing cleaner neutron profiles, and radioisotope power systems using plutonium-238 or strontium-90 for deep-space missions where reliability and longevity matter more than power density.
> 
> Moving into the Extraction section, I'm detailing atmosphere separation techniques like cryogenic distillation and pressure-swing adsorption for terrestrial worlds, plus atmospheric skimming on gas giants to harvest hydrogen and helium-3. Then there's volatiles extraction from ice and regolith through thermal processing and sublimation trapping to recover water, carbon dioxide, and other frozen compounds.
> 
> I'm also covering hydrocarbon harvesting from Titan's methane-ethane seas, ocean extraction for minerals and fresh water from liquid bodies or subsurface oceans accessed via plumes and melt probes, and gas drilling operations.
> 
> Now moving through subsurface well drilling for gaseous hydrocarbons with methane and ethane as primary yields, plus helium and nitrogen co-production, then oil drilling for crude petroleum on Earth, coal mining operations, and iron mining from native metal or oxide ores across terrestrial and extraterrestrial sources.
> 
> Continuing with lunar ilmenite beneficiation and Martian hematite collection, then aluminium extraction from bauxite on Earth or plagioclase from lunar highlands and asteroids, followed by industrial metals mining for copper, manganese, chromium, zinc, nickel, cobalt, lithium and others with associated iron, precious metals, sulfur, and rare-earth byproducts, and precious metals mining for gold and silver.
> 
> Now I'm covering platinum-group metals, which are primarily Earth-sourced but expected off-world mainly as byproducts from asteroidal iron-nickel processing rather than dedicated extraction. Uranium mining remains Earth-only through open-pit, underground, and in-situ leach methods, sometimes yielding vanadium, copper, and rare-earth concentrates. Rare-earth mining targets bastnäsite, monazite, xenotime, ion-adsorption clays, and lunar KREEP minerals, co-producing thorium and uranium from certain deposits. Industrial minerals mining is next on the list. Sulfur extraction works differently depending on the body—native collection on Io versus thermal decomposition of iron sulfide on asteroids to get both sulfur and metallic iron as byproducts. That brings the extraction operations to eighteen. For refining and conversions, oil refining uses fractional distillation and catalytic cracking to produce liquid fuels, petrochemical feedstock, and solvents, with hydrodesulfurization yielding elemental sulfur as a co-product, while coal processing involves coking and gasification.
> 
> Iron and nickel refining separates asteroidal metallic alloys into refined iron while recovering cobalt, chromium, copper, and platinum-group metals as valuable off-world sources. Iron ore smelting then converts iron oxides through blast furnaces or direct reduction into pig iron or direct-reduced iron.
> 
> Aluminium refining follows the Bayer-Hall-Héroult process, with off-Earth alternatives like carbothermal reduction or molten-salt electrolysis of anorthositic material that co-produces oxygen. Industrial metals refining covers the full suite of base metals—copper, nickel, zinc, chromium, manganese, titanium, tin, tungsten, molybdenum, cobalt—through smelting, solvent extraction, and electrorefining, where sulfide smelting yields sulfuric acid and electrorefining produces precious-metal-bearing anode slimes.
> 
> Precious metals are refined via fire assay, chlorination, solvent extraction, and electrorefining to isolate gold, silver, and platinum-group metals from ores and anode slimes. Rare-earth refining uses acid or alkali digestion and solvent extraction to separate individual oxides and metals, co-producing thorium dioxide and uranium from monazite feeds. Uranium refining progresses from leaching and solvent extraction through yellowcake to purified uranium hexafluoride at natural assay, often recovering vanadium, molybdenum, and rare-earth byproducts.
> 
> Uranium enrichment then uses gaseous centrifuge cascades or laser isotope separation to boost the ²³⁵U fraction above natural levels, producing enriched fuel material and depleted tails as an essential step before nuclear fuel fabrication. Regolith processing thermally or electrochemically reduces raw lunar or planetary material to extract oxygen and metal separates like iron, silicon, and titanium without prior beneficiation, making it a foundational ISRU technique. Water electrolysis splits H₂O electrochemically to yield hydrogen and oxygen. The water byproduct cycles back into the system, making this a key route for generating storable propellant and industrial feedstock from in-situ resources. I'm also considering the Sabatier reaction—converting CO₂ and hydrogen into methane and water, which is central to Mars operations and helps recycle carbon dioxide in life support. Then there's steam reforming to break down methane into synthesis gas, plus methane pyrolysis as a thermal decomposition method to extract hydrogen and leave behind solid carbon.
> 
> Now I'm looking at how methane pyrolysis produces hydrogen cleanly without CO₂ emissions, and the carbon residue becomes useful for composites or electrodes—something that could be especially valuable in space where carbon is limited. I'm also tracking the water-gas shift reaction to fine-tune syngas composition for maximum hydrogen output, the reverse water-gas shift to generate carbon monoxide from atmospheric CO₂ on Mars, and the Haber-Bosch process for synthesizing ammonia from nitrogen and hydrogen.
> 
> Moving into methanol synthesis, where carbon monoxide or carbon dioxide gets hydrogenated to produce methanol as a chemical intermediate and potential fuel, with downstream conversion to olefins replacing petrochemical feedstocks. I'm also considering the broader category of chemical synthesis processes like chlor-alkali and sulfuric acid production.
> 
> Then there's polymer synthesis from various monomers derived through petrochemical cracking or methanol conversion, producing plastics and fibers. For metals, I'm looking at both basic oxygen steelmaking from pig iron and electric arc furnace approaches.
> 
> Electric arc furnaces melt scrap steel and direct-reduced iron with lower carbon intensity than the oxygen route. Hydrogen reduction offers another pathway—using H₂ to directly reduce iron ore to produce water instead of CO₂, enabling near-zero-emission steel where clean hydrogen is available. There's also a speculative solar steelmaking concept using concentrated sunlight as the heat source, which could work at permanently sunlit locations like lunar crater rims or on mobile platforms tracking sunlight on slowly rotating bodies.
> 
> Moving into more speculative territory, I'm considering isotope separation through cryogenic distillation or electrolysis to extract deuterium and helium-3 for fusion fuel, biofuel production via fermentation and hydrothermal liquefaction of biological feedstock, fertilizer compounding from ammonia and phosphate sources, and concrete production starting with limestone calcination.
> 
> For off-world applications, I'm noting that regolith can be sintered into binders or used directly as glass feedstock, and sulfur concrete becomes viable in vacuum environments. Glass and ceramics production follows conventional melting and forming processes, while semiconductor fabrication demands crystal growth, wafer processing, and ultra-high-purity refining of silicon and compound semiconductors in clean-room conditions.
> 
> Now I'm moving through composite manufacturing—fiber-reinforced polymers and metal-matrix composites that layer or wind fibers like carbon or aramid with a matrix material. Recycling becomes critical for closed-loop systems, using mechanical and chemical separation to recover metals, polymers, and rare earths from waste. I'm also considering spent nuclear fuel reprocessing, where aqueous or pyrochemical methods separate irradiated fuel to recover uranium and plutonium for refabrication.
> 
> I notice I need to correct "clinite" to "clinker" in the concrete section. Moving into manufacturing now—nuclear fuel fabrication converts enriched uranium or thorium into sintered pellets loaded into zirconium cladding for reactor assemblies, and solid rocket propellant production mixes oxidizers, aluminum, and polymer binders into finished grains.
> 
> Now covering structural materials: steel fabrication through cutting and welding, aluminum through extrusion and machining, composites via autoclave layup and filament winding, heavy machinery like turbines and pumps cast and assembled from various alloys, and electrical systems. Neuromorphic processors are still largely theoretical at scale, but they'd follow a similar manufacturing path to tensor processors as a specialized AI hardware capability. I'm also mapping out transport systems—vehicles, aircraft, spacecraft—which integrate multiple complex subsystems, plus robotics manufacturing for manipulators and autonomous platforms with advanced processors. Solar panels and batteries round out the energy infrastructure, with solar requiring semiconductor-grade silicon and battery production still being outlined. I'm finishing out the technical textiles category with the remaining materials and processes, then moving into biological operations—starting with open agriculture, aquaculture, and forestry systems that work across different environments including space habitats. That brings the manufacturing count to 18 distinct operations.
> 
> Now I'm detailing the sealed-environment alternatives: closed agriculture in pressurized modules, closed aquaculture in recirculating systems, and closed forestry in controlled facilities, which become increasingly important for isolated off-world settlements. I'm also introducing bioprocessing operations like fermentation and photobioreactor cultivation.
> 
> Continuing with the downstream applications—pharmaceutical production through synthesis and fermentation, food processing that converts fresh inputs into shelf-stable meals with packaging, and artisanal production for small-batch specialty foods.
> 
> Then soil conditioning, which prepares growth media from regolith and amendments, including critical steps like pH adjustment and toxin remediation for extraterrestrial agriculture. That brings the biological operations to 11, making 99 total across all categories. Now I'm deciding whether 99 is the right scope or if I should adjust the count.
> 
> Let me verify the pathways are complete — checking that organics can flow into polymer synthesis, bioprocessing, or chemical synthesis, and that each manufactured resource has a clear production route. Going through the list to confirm there are no critical gaps in the system. I'm verifying that all the remaining material resources have production pathways—industrial metals, precious metals, rare earths, uranium, and thorium all route through their respective refining processes, while concrete, glass, ceramics, semiconductors, polymers, composites, and textiles each have dedicated manufacturing chains, plus industrial waste gets recovered through recycling.
> 
> Now I'm checking the volatile resources to confirm they're either extractable or have synthesis pathways, and doing the same verification for energy resources to ensure refined fuels, solid fuels, biofuels, and uranium fuel variants all have production routes available. Fuel Fabrication ✓
> - Thorium Fuel ← Nuclear Fuel Fabrication ✓
> - Deuterium ← Isotope Separation ✓
> All other energy resources are extractable. ✓
> 
> 6. Is there a pathway for each Biological resource?
> - Crop Products ← Open/Closed Agriculture ✓
> - Animal Products ← Open/Closed Agriculture, Open/Closed Aquaculture ✓
> - Packaged Meals ← Food Processing ✓
> - Artisanal Foods ← Artisanal Production ✓
> - Artisanal Goods ← Artisanal Production ✓
> - Biofibers ← Open/Closed Agriculture ✓
> - Lumber/Wood Products ← Open/Closed Forestry ✓
> - Algal/Microbial Products ← Bioprocessing, Open/Closed Aquaculture ✓
> - Biofeedstock ← Open/Closed Agriculture, Open/Closed Forestry, Bioprocessing ✓
> - Biochemicals ← Bioprocessing ✓
> - Pharmaceuticals ← Pharmaceutical Production ✓
> - Fertilizers ← Fertilizer Production ✓
> - Soil/Growth Media ← Soil Conditioning ✓
> - Biowaste ← byproduct of biological operations ✓
> 
> All biological resources have production pathways. ✓
> 
> The operations list is complete and verified. Now I'm moving on to finalize the physical resources documentation, starting with iron specifications. I'm considering whether there are any notable gaps in the resource list. Carbon and graphite come up frequently in various materials, but they're probably better handled as intermediates within composite and polymer production rather than standalone resources. Similarly, lithium is critical for batteries but makes sense to keep abstracted within the industrial metals basket given the simulation's level of detail. Overall, the resource coverage seems comprehensive enough.
> 
> Now I'm ready to compile the final response with the completed operations file, the three resource entries, notes on resource adequacy, and feedback clarifications. I want to make sure my descriptions stay concise and natural throughout, and I'm doing a quick review to trim any wordiness. I should also consider whether the existing notes in the operations file need any additions, though the current set seems solid.
> 
> For the feedback section, I'm thinking through the key simplifications and omissions: Chemical Synthesis is really a broad umbrella for thousands of distinct industrial processes, several operations like Recycling and Regolith Processing aggregate many sub-processes into single entries, the off-Earth co-products from operations like Aluminium Refining aren't fully specified in terms of mass balance, and the biological section could be more granular in distinguishing between different types of processes.
> 
> I'm also noting what's missing from the framework: there's no explicit construction or assembly operations for building infrastructure, no repair or maintenance workflows, no demolition or decommissioning, and transport logistics between locations aren't captured. I'm also flagging some clarification questions about whether Recycling should handle end-of-life manufactured goods conversion or just industrial waste, and whether Art operations need further definition. I'm filling in the gaps in the original file—the Manufacturing section needs a proper description, and those blank biological operation placeholders need to be replaced with actual entries like Bioprocessing, Pharmaceutical Production, and Food Processing to give the biological domain proper coverage. Now I'll put together the complete, polished response.

Here is the developed `physical_operations.md`, followed by the three completed resource entries and feedback.

---

## Completed `physical_operations.md`

```markdown
# Physical Operations

Notes:

1. The 5 groupings are for organization and display only (it's a somewhat heterogeneous classification); functional effects in the simulation are handled by individual data table properties.
2. Extraction operations are further subdivided within the simulation by strata. For example, Iron Mining is subdivided into Surface Iron Mining, Near-Surface Iron Mining, Subsurface Iron Mining, etc. The terms here are sometimes generic but can be implemented to handle very different contexts (e.g., Iron Mining on a metallic asteroid BULK stratum represents mechanical disaggregation and magnetic separation, not conventional open-pit mining).
3. "Natural" resource conversions (e.g., Earth's water cycle) are handled by a separate system in the simulation.
4. There is a 6th group for "services" (not in this file) that handles intangible operations (i.e., conversions among non-physical resources).


## Energy

- **Solar Power** — Photovoltaic and concentrated solar thermal electricity generation. No fuel input; output scales with solar flux and collector area. Primary power source for inner solar system operations.
- **Geothermal Power** — Electricity and process heat from planetary interior heat via hydrothermal or hot-rock systems. Applicable on Earth; potentially on tidally heated bodies such as Io or Enceladus.
- **Hydropower** — Electricity from gravitational potential energy of flowing water. Primarily Earth-based; conceivable on Titan using methane–ethane river systems at speculative scale.
- **Wind Power** — Electricity from atmospheric kinetic energy via turbines. Applicable on Earth, potentially Mars (low atmospheric density limits output), Venus (high-altitude platforms), and Titan (dense, slow atmosphere).
- **Tidal Power** — Electricity from gravitational tidal forcing on oceans and seas. Earth-only under foreseeable conditions.
- **Methane Power** — Electricity and heat from methane combustion or methane fuel cells. Covers natural gas power on Earth and ISRU-derived or harvested methane power off-Earth (Mars, Titan, gas giant sourcing).
- **Coal Power** — Electricity and heat from coal combustion. Earth-only. Fly ash and bottom ash are recoverable as construction feedstock.
- **Oil Power** — Electricity and heat from combustion of liquid petroleum fuels in generators and industrial boilers. Earth-dominated.
- **Hydrogen Power** — Electricity from hydrogen fuel cells or direct-combustion gas turbines. Key power pathway for off-Earth facilities using electrolysis-derived hydrogen and oxygen.
- **Biofuel Power** — Electricity and heat from biofuel combustion, biogas generators, and biomass combined heat-and-power systems.
- **LEU Fission Power** — Electricity from thermal-neutron fission of low-enriched uranium fuel in light-water and heavy-water reactor types. Dominant nuclear power pathway on Earth.
- **HEU Fission Power** — Electricity and propulsion heat from compact high-enrichment uranium reactors for naval propulsion and space-rated kilowatt-to-megawatt power systems.
- **Thorium Fission Power** — Electricity from thorium-cycle thermal-breeder reactors, breeding ²³³U from ²³²Th in-situ.
- **D-T Fusion Power** — Electricity from deuterium–tritium fusion. Tritium is bred from lithium blankets in-situ. Requires deuterium fuel and a lithium supply.
- **He-3 Fusion Power** — Electricity from D-³He or ³He-³He fusion, producing substantially fewer neutrons than D-T cycles. Requires helium-3 fuel.
- **Radioisotope Power** — Electricity and heat from radioactive decay in radioisotope thermoelectric generators (RTGs) and Stirling radioisotope generators. Uses plutonium-238 or strontium-90 produced in reactor operations. Low power density but extreme reliability and longevity; critical for deep outer solar system probes and surface stations with limited sunlight.


## Extraction

- **Atmosphere Separation** — Cryogenic distillation, pressure-swing adsorption, membrane separation, or centrifugal processing of planetary atmospheres. On terrestrial worlds, separates oxygen, nitrogen, carbon dioxide, argon, and noble gases. On gas giants, atmospheric skimming or balloon-based systems harvest hydrogen, helium, helium-3, and methane from the upper atmosphere.
- **Volatiles Extraction** — Thermal processing, sublimation trapping, or mechanical extraction of volatile species from ice-bearing strata, regolith, permafrost, and brine reservoirs. Target products depend on source composition and include water, carbon dioxide, carbon monoxide, ammonia, methane, sulfur dioxide, helium-3 (from solar-wind-implanted regolith), and other trapped volatiles. Applicable to polar ice deposits, cometary bodies, icy moon shells, and subsurface oceans.
- **Hydrocarbon Harvest** — Collection and separation of liquid hydrocarbons from surface bodies, principally Titan's methane–ethane seas. Yields methane, ethane, and dissolved nitrogen. Distinct from gas drilling (subsurface reservoirs) and atmosphere separation (gaseous phases).
- **Ocean Extraction** — Recovery of dissolved minerals, salts, and fresh water from liquid ocean or brine bodies. On Earth, encompasses desalination, brine evaporation, and selective ion exchange for magnesium, lithium, bromine, and other dissolved species. On ocean worlds, targets water and dissolved solutes accessed via plumes, melt probes, or ice penetrators.
- **Gas Drilling** — Subsurface well drilling and completion for gaseous hydrocarbon extraction. Yields methane and ethane as primary products with significant helium, nitrogen, and carbon dioxide co-production; hydrogen sulfide is captured and converted to elemental sulfur. Applicable on Earth; conceivably adaptable to subsurface volatile reservoirs elsewhere.
- **Oil Drilling** — Subsurface well drilling and completion for liquid petroleum extraction. Yields crude oil and associated gas. Earth-only.
- **Coal Mining** — Surface strip mining and underground extraction of coal seams. Earth-only.
- **Iron Mining** — Targeted extraction of native iron–nickel metal or iron oxide–silicate ores from any strata with exploitable concentrations. On Earth, conventional open-pit and underground mining of magnetite, hematite, and goethite deposits. On asteroids, mechanical disaggregation of metallic and stony-iron bodies. On the Moon, magnetic beneficiation of ilmenite from regolith. On Mars, collection of hematite-enriched surface material.
- **Aluminium Mining** — Targeted extraction of aluminium-bearing ores. On Earth, open-pit mining of bauxite. Off-Earth, beneficiation of aluminium-rich plagioclase from lunar highland regolith or asteroidal silicate bodies.
- **Industrial Metals Mining** — Targeted extraction of base-metal sulfide, oxide, and silicate ores (copper, manganese, chromium, zinc, nickel, cobalt, lithium, and others). Co-produces iron ores, precious-metal-bearing concentrates, sulfur, and rare-earth-bearing heavy mineral fractions depending on deposit type.
- **Precious Metals Mining** — Targeted extraction of gold, silver, and platinum-group-metal-bearing ores from crustal deposits. Earth-dominated; off-Earth PGM sourcing is expected primarily as a co-product of asteroidal iron–nickel refining rather than dedicated mining.
- **Uranium Mining** — Extraction of uranium ores from crustal deposits using open-pit, underground, and in-situ leach methods. Some deposit types co-produce vanadium, copper, and rare-earth concentrates. Earth-only for the foreseeable future.
- **Rare Earths Mining** — Extraction of rare-earth-bearing ores: bastnäsite, monazite, xenotime, ion-adsorption clays, and KREEP mineral separates (off-Earth). Co-produces thorium and uranium concentrates from monazite-bearing deposits.
- **Industrial Minerals Mining** — Extraction of non-metallic minerals used in construction, agriculture, and chemical processing: phosphate rock, potash, limestone, gypsum, clays, salt, sand and gravel, fluorspar, and borates. Co-produces rare-earth concentrates from heavy mineral sands and phosphate deposits.
- **Stone Quarrying** — Extraction of bulk silicate rock as dimension stone, crushed aggregate, or industrial process feedstock.
- **Regolith Quarrying** — Collection of loose surface fines and impact-generated debris. Primary bulk material source on airless bodies and Mars. Output serves as feedstock for construction, sintering, regolith processing, and soil conditioning.
- **Organics Extraction** — Recovery of solid organic and carbonaceous material from natural strata. Includes oil shale retorting on Earth, tholin collection on Titan, and separation of insoluble organic matter from carbonaceous chondrite, D-type, and U-type asteroid material and cometary bodies.
- **Sulfur Extraction** — Recovery of elemental or mineralogical sulfur from volcanic, evaporite, or iron-sulfide-bearing strata. On Io, surface collection of native sulfur deposits. On asteroids, thermal decomposition of troilite (FeS → Fe + S), co-producing metallic iron.


## Refining/Conversions

- **Oil Refining** — Fractional distillation, catalytic cracking, and hydroprocessing of crude oil. Primary products are liquid fuels, petrochemical naphtha for polymer feedstock, and industrial solvents. Hydrodesulfurization recovers elemental sulfur as a co-product. Earth-dominated.
- **Coal Processing** — Coking, gasification (C + H₂O → CO + H₂), and coal-to-liquids conversion. Produces metallurgical coke for steelmaking, synthesis gas for downstream chemical processing, and liquid fuels via indirect Fischer-Tropsch. Rare-earth elements concentrate ~5–10× in combustion ash and may be recoverable.
- **Iron/Nickel Refining** — Pyrometallurgical and hydrometallurgical processing of native iron–nickel alloy from asteroidal and other metallic sources. Separates the alloy into refined pig iron with recovery of cobalt, chromium, and copper, plus platinum-group metals and gold — potentially a primary off-Earth precious-metal source.
- **Iron Ore Smelting** — Blast-furnace or direct-reduction processing of iron oxide ores to pig iron or direct-reduced iron using coke, coal, or carbon monoxide as reductant.
- **Aluminium Refining** — Bayer-process alumina extraction followed by Hall-Héroult electrolytic smelting on Earth. Off-Earth alternatives include carbothermal reduction and molten-salt electrolysis of anorthositic feedstock, which co-produce oxygen as a valuable by-product.
- **Industrial Metals Refining** — Smelting, solvent extraction, and electrorefining of base-metal ores to produce refined copper, nickel, zinc, chromium, manganese, titanium, tin, tungsten, molybdenum, cobalt, and other industrial metals. Sulfide smelting generates sulfuric acid as a valuable co-product; electrorefining yields precious-metal-bearing anode slimes.
- **Precious Metals Refining** — Fire assay, chlorination, solvent extraction, and electrorefining of precious-metal ores and intermediate concentrates (including anode slimes from base-metal refining) to produce refined gold, silver, and platinum-group metals.
- **Rare Earths Refining** — Acid or alkali digestion, solvent extraction, and selective precipitation to separate rare-earth ore concentrates into individual oxide and metal products. Co-produces thorium dioxide and uranium concentrates from monazite-bearing feeds.
- **Uranium Refining** — Leaching, solvent extraction, and fluorination of uranium ores through yellowcake (U₃O₈) to purified uranium hexafluoride at natural isotopic assay. Some ore types yield recoverable vanadium, molybdenum, and rare-earth co-products.
- **Uranium Enrichment** — Gaseous centrifuge cascade or advanced laser isotope separation to increase the ²³⁵U fraction in UF₆ above natural assay. Produces enriched material for fuel fabrication and depleted tails. Required intermediate step between uranium refining and nuclear fuel fabrication.
- **Regolith Processing** — Bulk thermal, electrochemical, or carbothermic reduction of raw regolith to extract oxygen and metal–metalloid separates (iron, silicon, aluminium, titanium). Principal techniques include molten-oxide electrolysis (FFC Cambridge process or similar) and carbothermal reduction. Operates on unsorted regolith without prior ore beneficiation, making it a key early-stage ISRU pathway on the Moon, Mars, Mercury, and asteroids.
- **Water Electrolysis** — Electrochemical splitting of water: 2H₂O → 2H₂ + O₂. PEM, alkaline, and solid-oxide variants. Fundamental ISRU process throughout the solar system for producing propellant, industrial hydrogen, and breathing oxygen.
- **CO₂ Electrolysis** — Solid-oxide electrochemical splitting of carbon dioxide: 2CO₂ → 2CO + O₂. Key Mars ISRU process; also applicable to Venus atmospheric processing. Produces carbon monoxide for downstream Fischer-Tropsch synthesis and oxygen for life support or propellant use.
- **Fischer-Tropsch Process** — Catalytic conversion of synthesis gas to liquid hydrocarbons: nCO + (2n+1)H₂ → CₙH₂ₙ₊₂ + nH₂O. Produces synthetic liquid fuels from any CO + H₂ source, with recyclable water as a co-product. Key pathway for synthesizing storable propellant and chemical feedstock from ISRU-derived syngas.
- **Sabatier Process** — Catalytic methanation: CO₂ + 4H₂ → CH₄ + 2H₂O. Converts carbon dioxide and hydrogen to methane and water. Core Mars propellant-production process; also useful as a CO₂ recycling step in closed life-support systems.
- **Steam Reforming** — Catalytic conversion of methane and steam to synthesis gas: CH₄ + H₂O → CO + 3H₂. Primary industrial hydrogen production pathway on Earth.
- **Methane Pyrolysis** — Thermal or catalytic decomposition of methane: CH₄ → C(s) + 2H₂. Produces hydrogen without CO₂ emissions; solid carbon co-product is usable in composites, electrodes, or carbon-fiber precursor stock. Emerging technology on Earth; potentially valuable off-Earth where solid carbon is scarce.
- **Water-Gas Shift** — Catalytic equilibrium reaction: CO + H₂O → CO₂ + H₂. Adjusts syngas composition to maximize hydrogen yield downstream of gasification or reforming.
- **Reverse Water-Gas Shift** — Catalytic conversion: CO₂ + H₂ → CO + H₂O. Mars ISRU route to produce carbon monoxide feedstock for Fischer-Tropsch synthesis from atmospheric CO₂ and electrolysis-derived hydrogen.
- **Haber-Bosch Process** — High-pressure catalytic synthesis: N₂ + 3H₂ → 2NH₃. Produces ammonia from atmospheric or extracted nitrogen and hydrogen. Foundational for fertilizer and industrial chemical production. Off-Earth, nitrogen supply (from Titan, Mars, or Venus atmospheres, or from ammonia-bearing icy bodies) is the principal constraint.
- **Methanol Synthesis** — Catalytic hydrogenation of carbon monoxide or carbon dioxide: CO₂ + 3H₂ → CH₃OH + H₂O (or CO + 2H₂ → CH₃OH). Produces methanol as a versatile chemical intermediate, solvent, and potential fuel. Downstream methanol-to-olefins conversion can substitute for petrochemical naphtha in polymer production.
- **Chemical Synthesis** — Broad category encompassing commodity and specialty chemical production: chlor-alkali electrolysis, sulfuric acid contact process (2SO₂ + O₂ → 2SO₃, then H₂SO₄), nitric acid oxidation, phosphoric acid digestion, and downstream organic and inorganic synthesis. Consumes ammonia, sulfur, mineral feedstocks, and hydrocarbon intermediates to produce the full range of industrial and fine chemicals.
- **Polymer Synthesis** — Polymerization of monomers (ethylene, propylene, vinyl chloride, styrene, and others) derived from petrochemical cracking, Fischer-Tropsch intermediates, or methanol-to-olefins conversion. Produces thermoplastics, thermosets, elastomers, and synthetic fibers.
- **Biofuel Production** — Fermentation (ethanol), transesterification (biodiesel), and hydrothermal liquefaction of biological feedstock and crop-derived sugars into liquid biofuels. Co-produces organic residues usable as soil amendments or fermentation feedstock.
- **Fertilizer Production** — Compounding and granulation of ammonia, phosphate rock, potash, sulfur-derived acids, and micronutrients into blended NPK and specialty fertilizer products.
- **Concrete Production** — Calcination of limestone to clinker, grinding with gypsum and pozzolanic additives, and mixing with aggregate and water. Off-Earth variants include sintered-regolith binders and sulfur concrete (elemental sulfur as binder, viable in vacuum and low-temperature environments). Conventional calcination co-produces CO₂.
- **Glass/Ceramics Production** — Melting and forming of silicate feedstock with fluxes and modifiers to produce flat glass, fiber glass, technical ceramics, and refractory products. Off-Earth, basaltic regolith can serve as a near-complete glass and ceramic feedstock.
- **Semiconductor Processing** — Crystal growth (Czochralski or float-zone), wafer fabrication, epitaxial deposition, and ultra-high-purity refining of silicon, gallium arsenide, silicon carbide, and compound semiconductors. Requires fine chemicals and ultra-clean processing environments.
- **Composite Production** — Fiber-reinforced polymer layup, filament winding, pultrusion, and curing; metal-matrix composite forming. Combines polymer or metal matrix with glass fiber, carbon fiber (pyrolyzed from polymer precursors or organic feedstock), or aramid reinforcement.
- **Isotope Separation** — Cryogenic distillation, electrolysis cascade, or thermal-cycling methods to separate deuterium from hydrogen or water, and to purify helium-3 from bulk helium. Produces fusion-grade isotopic fuel.
- **Steelmaking (basic oxygen)** — Conversion of pig iron to carbon and alloy steel in basic oxygen furnaces by blowing an oxygen lance through molten iron. Dominant primary steelmaking route on Earth.
- **Steelmaking (electric)** — Electric arc furnace melting and refining of pig iron, scrap steel, and/or direct-reduced iron. Lower carbon intensity than the basic oxygen route; primary recycling-based steelmaking pathway.
- **Steelmaking (H₂ reduction)** — Direct reduction of iron ores with hydrogen (Fe₂O₃ + 3H₂ → 2Fe + 3H₂O) followed by electric arc furnace refining. Produces water vapor rather than CO₂ as the reduction by-product.
- **Steelmaking (solar)** — Speculative direct solar-thermal reduction and melting using concentrated reflected sunlight as the primary heat source. Applicable at permanently or near-permanently sunlit locations such as lunar and Mercurian polar crater rims, or on mobile solar-rail platforms that track continuous sunlight on slowly rotating bodies.
- **Recycling** — Mechanical shredding, magnetic and eddy-current separation, pyrometallurgical recovery, and chemical dissolution of end-of-life equipment, process scrap, and mixed waste streams. Recovers ferrous and non-ferrous metals, polymers, glass, and rare earths. Essential for mass-closure in space habitats and off-Earth settlements where resupply is constrained.
- **Spent Fuel Reprocessing** — Aqueous (PUREX) or pyrochemical separation of irradiated nuclear fuel to recover uranium and transuranic actinides for re-fabrication into fresh fuel. Concentrates fission products into vitrified high-level waste. Extends fissile resource utilization by an order of magnitude or more.


## Manufacturing

- **Nuclear Fuel Fabrication** — Conversion of enriched UF₆ and thorium dioxide powder into finished reactor fuel: sintered oxide pellets loaded into zirconium-alloy cladding tubes and assembled into fuel bundles. Covers fabrication of low-enrichment, high-enrichment, and thorium-cycle fuel assemblies.
- **Solid Fuel Production** — Mixing, casting, and curing of solid rocket propellant from ammonium perchlorate oxidizer, aluminium powder, and polymer binder, including grain geometry design for thrust profiling.
- **Structural Steel Fabrication** — Cutting, welding, forming, galvanizing, and assembly of steel plate, beams, pipe, pressure vessels, and prefabricated modules for structural applications.
- **Aluminium Fabrication** — Extrusion, rolling, machining, and joining of aluminium alloys into structural members, pressure shells, and lightweight frames.
- **Composite Fabrication** — Autoclave and out-of-autoclave layup, filament winding, and assembly of composite panels, overwrapped pressure vessels, fairings, and inflatable habitat shells.
- **Heavy Machinery Manufacturing** — Design, casting, machining, and assembly of turbines, engines, pumps, compressors, heat exchangers, mining and construction equipment, and industrial furnaces from steel, base-metal alloys, and specialty materials.
- **Electrical Equipment Manufacturing** — Fabrication and assembly of generators, transformers, electric motors, power cables, switchgear, and fuel-cell stacks. Heavy copper consumption.
- **Precision Equipment Manufacturing** — Fabrication and assembly of scientific instruments, medical devices, CNC machine tools, lithography systems, optical systems, and navigation and guidance hardware. Requires tight tolerances and specialty materials.
- **Electronics Manufacturing** — Semiconductor packaging, printed-circuit-board fabrication, surface-mount assembly, display production, and system integration for general-purpose computing, communications, and sensor hardware.
- **Tensor Processor Manufacturing** — Advanced-node semiconductor lithography, high-bandwidth memory stacking, and advanced packaging for GPU and TPU accelerator modules. Consumes ultra-high-purity semiconductor wafers and precious-metal interconnects.
- **Neuromorphic Processor Manufacturing** — Fabrication of programmable neuromorphic computing hardware using advanced semiconductor and novel device architectures. Speculative at production scale; input profile similar to tensor processors.
- **Transport Systems Manufacturing** — Design, fabrication, and final assembly of ground vehicles, aircraft, marine vessels, spacecraft, EVA mobility units, and spacesuits. Integrates structural, propulsion, avionics, and life-support subsystems.
- **Robotics Manufacturing** — Fabrication and assembly of robotic manipulators, autonomous mobile platforms, and AI-hardware systems. Integrates structural elements, actuators, sensors, and advanced processors.
- **Solar Panel Manufacturing** — Production of photovoltaic modules: silicon ingot slicing and cell fabrication (or thin-film deposition), glass encapsulation, aluminium frame assembly, and copper interconnect wiring.
- **Battery Manufacturing** — Electrode coating, cell stacking, electrolyte filling, formation cycling, module assembly, and battery-pack integration. Covers lithium-ion (NMC, LFP, NCA) and successor solid-state chemistries.
- **Consumer Goods Manufacturing** — Mass production of furniture, clothing, housewares, paper products, hygiene products, and building finishes from polymers, steel, textiles, wood products, and glass.
- **Specialty Goods Manufacturing** — Production of high-value, low-mass items: premium personal electronics, jewelry, art objects, luxury fashion, and recreational equipment.
- **Technical Textile Manufacturing** — Weaving, knitting, and coating of high-performance fabrics: aramid, UHMWPE, carbon-fiber cloth, PTFE membranes, multilayer insulation blankets, filtration media, and spacesuit soft-goods assemblies.


## Biological

- **Open Agriculture** — Conventional cultivation of crops and raising of livestock on open land under ambient atmospheric conditions. Produces plant-based foods, animal-based foods, fiber crops, and biomass feedstock. Applicable on Earth and in any sufficiently terraformed open environment, including O'Neill cylinder interiors.
- **Open Aquaculture** — Marine and freshwater farming of fish, shellfish, seaweed, and microalgae in open water bodies.
- **Open Forestry** — Managed cultivation and harvest of trees for timber, pulp, and biomass in open conditions.
- **Closed Agriculture** — Sealed, controlled-environment crop cultivation and animal husbandry in pressurized greenhouses, space station grow modules, or planetary surface habitats. Requires artificial or filtered lighting, active atmosphere management (CO₂ supply, humidity control), water recycling, and nutrient delivery. Co-produces oxygen via photosynthesis. Synonymous with controlled-environment agriculture (CEA).
- **Closed Aquaculture** — Recirculating sealed-system aquatic farming for fish, shellfish, and algae. Essential protein and micronutrient source for isolated off-Earth habitats.
- **Closed Forestry** — Sealed-environment tree cultivation in pressurized facilities. Speculative at meaningful timber-production scale; potentially practical in large rotating habitats. Co-produces oxygen.
- **Bioprocessing** — Industrial fermentation, photobioreactor algal cultivation, and enzymatic processing. Produces microbial and algal biomass, biochemicals (enzymes, amino acids, biopolymers, organic acids), and industrial biofeedstock from water, carbon-source substrates, nitrogen, and trace mineral nutrients.
- **Pharmaceutical Production** — Chemical synthesis, fermentation, purification, and formulation of drugs, vaccines, biologics, and medical compounds. Consumes biochemical intermediates, fine chemicals, water, and packaging materials (glass vials, polymer closures, foil laminates).
- **Food Processing** — Cooking, preservation (retort, freeze-drying, dehydration, thermostabilization), and packaging of agricultural and aquacultural products into shelf-stable prepared meals. Critical for space operations where fresh-food supply is intermittent.
- **Artisanal Production** — Small-scale handcraft production of specialty foods (artisan cheeses, charcuterie, preserves, craft beverages, baked goods) and non-food goods (pottery, handmade textiles, woodcraft, leatherwork). Represents culturally significant, high-value-per-mass production that persists alongside industrial manufacturing.
- **Soil Conditioning** — Preparation of growth media from raw regolith, mineral amendments, organic waste, water, and microbial inoculants. Includes composting, pH adjustment, nutrient loading, and toxin remediation (e.g., perchlorate removal for Mars regolith). Critical ISRU step for establishing agriculture on extraterrestrial surfaces.
```

---

## Completed `physical_resources.md` Entries

These three entries replace the incomplete stubs in the Materials section:

**Iron:**
> **Iron** — Industrial pig iron; ref. blast-furnace pig iron, global production-weighted. Fe 93.5, C 4, Si 1, Mn 0.7, P 0.15, S 0.05, other (Cr, Ti, V, Cu) 0.6.

**Uranium:**
> **Uranium** — Purified, unenriched uranium hexafluoride (UF₆) at natural isotopic assay (0.711% ²³⁵U); ref. conversion-plant product meeting ASTM C787 specification. U 67.6 (isotopic: 0.711% U-235, 99.289% U-238), F 32.4.

**Thorium:**
> **Thorium** — Nuclear-grade thorium dioxide (ThO₂) powder; ref. purified thoria separated from monazite-process residues, meeting nuclear-purity specifications. Th 87.9, O 12.1.

---

## Suggested Additions to `physical_resources.md`

I don't see major structural gaps in the resource list. A few borderline cases I considered and rejected:

The list has no standalone "Carbon" or "Graphite" commodity. Carbon fiber, graphite electrodes, and activated carbon are industrially important, but carbon appears as a constituent of Polymers, Composites, Organics/Tholins, and as a by-product of Methane Pyrolysis. At this abstraction level, I think carbon is adequately captured within existing categories — adding it would create awkward double-counting issues in compositions. If downstream simulation reveals that carbon as a standalone feedstock is a bottleneck (particularly off-Earth, where solid carbon is genuinely scarce), it could be added later.

Xenon for electric propulsion is folded into Heavy Noble Gases. This is fine unless the simulation needs to price ion-drive propellant independently, in which case it would need its own entry.

No other gaps appear significant enough to warrant growing the list.

---

## Feedback and Simplifications

**Simplifications and important omissions in this pass:**

Chemical Synthesis is a very broad catch-all aggregating thousands of real-world chemical processes (chlor-alkali, acid production, organic synthesis, etc.) into one operation. If specific chemical pathways emerge as economic bottlenecks in the simulation (e.g., sulfuric acid production constraining phosphate fertilizer output), they may need to be broken out.

Recycling is similarly broad — in reality, metal recycling, polymer recycling, and e-waste processing are quite different industries with different economics and yields. A single Recycling operation is adequate for a first pass but may need subdivision.

No explicit "construction" or "assembly" operation consumes manufactured goods (structures, machinery, electronics) and produces habitable infrastructure. If infrastructure is tracked as a resource or capital asset in the simulation, a Construction operation would be needed. This may belong in the services group or in a separate system entirely.

Transport and logistics operations (moving mass between locations) are absent, as expected — I assume this is handled by a separate system in the simulation.

There is no explicit "waste disposal" or "long-term storage" operation. The simulation may need one if waste accumulation has gameplay consequences.

I treated Artisanal Production as a single combined operation for both food and non-food artisanal goods. If the simulation needs distinct labor pools, facility types, or input profiles for these, they could be split.

**Plausibility notes for speculative items:**

Steelmaking (solar) is physically sound but stretches engineering plausibility — maintaining optical alignment and thermal cycling on a moving solar-rail foundry is a major unsolved problem. Radioisotope Power using strontium-90 (as opposed to Pu-238) is technically feasible but not current practice for deep-space missions — it's more relevant to Earth remote applications and is included for breadth. Closed Forestry at meaningful scale requires habitat volumes orders of magnitude larger than anything currently planned. Hydropower on Titan assumes methane rivers with sufficient and sustained flow, which is highly uncertain given Titan's slow methane cycle.

**Questions for clarification:**

How should "electricity" as a universal intermediary be handled? It's not in the resource list, yet virtually every operation either produces or consumes it. If the simulation tracks energy flows explicitly, the energy-group operations need to produce a trackable energy commodity and all other operations need to consume it. If energy is instead handled as an implicit cost (e.g., via an energy-price scalar applied to each operation), the current setup is fine. This design choice significantly affects how the operations are parameterized.

Should Recycling accept end-of-life Manufactured goods directly (e.g., scrapping old transport systems to recover steel and copper), or only process the intermediate Industrial Waste resource? The former is more realistic but creates many more input edges in the graph.

Is there a need for an explicit "Demolition" or "Decommissioning" operation that converts infrastructure or capital goods into Industrial Waste and Radioactive Waste, feeding into Recycling and Spent Fuel Reprocessing? This would close the lifecycle loop.

The 99 operations listed above versus ~87 resources should provide a well-connected graph with multiple production pathways for most downstream resources. In subsequent passes, the main refinement areas I'd prioritize are parameterization (input/output ratios, energy requirements, capital intensity) and potential subdivision of the broadest catch-all operations (Chemical Synthesis, Recycling, Bioprocessing) based on simulation needs.
