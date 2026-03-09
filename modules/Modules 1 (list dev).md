*Claude-Opus-4.6 (output effort = high). March, 2026.*

---
---

I want you to develop the context file modules.md. The goal is to include (as abstract simplifications) all of the physical infrastructure needed to carry out operations listed in physical_operations.md.

The file modules.md already has some listed items, some with notes in brackets. Existing items are provided as examples. You can modify these if needed. (In the case of "drilling rigs" and "mines", the exact strata targets are necessary as an implementation requirement.) Consider and then delete the individual notes in brackets. Each item needs a very brief description analogous to descriptions in other files, following General Instructions 6. Where it makes sense, described the module as a unit (e.g., a utility-scale power plant). Where that doesn't make sense, describe it in some other way (e.g., "Represents distributed ...").

This is a first pass only. We'll improve the list in later passes. I don't have specific number requirements at this time, but try to keep the abstraction level similar to that in other files.

---
---

*Thinking...*

> The user wants me to develop the `modules.md` file by filling in all the physical infrastructure modules needed to carry out the operations listed in `physical_operations.md`. Let me analyze the operations and figure out what modules are needed.
> 
> Let me go through each category of operations and think about what physical infrastructure (modules) enables them:
> 
> ## Energy Operations
> The energy operations include various power generation types. The existing modules cover most of these. Let me map them:
> 
> - Solar Power → Solar Farm, Solar Arrays
> - Geothermal Power → Geothermal Plant
> - Hydropower → Hydroelectric Dam
> - Wind Power → Wind Farm
> - Tidal Power → Tidal Power Station
> 
> 
> - Fossil fuel and hydrogen combustion operations all map to a Combustion Power Plant that handles multiple fuel types, while fuel cell operations need both Utility and Distributed Fuel Cell modules. For nuclear power, I need separate modules for LEU, HEU, and Thorium fission reactors, plus fusion plants for D-T and D-³He reactions.
> 
> For extraction, I'm seeing a pattern where atmosphere separation, volatiles extraction, hydrocarbon collection, desalination, and brine processing each need their own dedicated modules, while drilling and mining operations can be consolidated—drilling rigs handle both gas and oil extraction across different strata, and mines cover coal, iron, aluminium, industrial metals, and precious metals mining. Radioisotope generators round out the power options, and I still need to map uranium extraction. Mining → Mine (various strata)
> - Rare Earths Mining → Mine (various strata)
> - Sulfur Mining → Mine (various strata)
> - Organics Mining → Mine (various strata)
> - Industrial Minerals Mining → Mine (various strata)
> - Stone Quarrying → Quarry
> - Regolith Quarrying → Quarry
> 
> So for extraction, in addition to the existing drilling rigs, mines, and quarry, I need:
> - Atmosphere Processing Plant (or similar) for Atmosphere Separation
> - Volatiles Extraction Facility
> - Hydrocarbon Collection Facility (for Titan)
> - Desalination Plant
> - Brine Processing Plant
> 
> Wait, let me think about this differently. The modules are physical infrastructure. Multiple extraction operations can use the same mining module (e.g., Iron Mining and Industrial Metals Mining both use Mine modules at various strata). The drilling rigs handle Gas Drilling and Oil Drilling. But Atmosphere Separation needs its own infrastructure since it's fundamentally different from mining. Same with Volatiles Extraction—that's thermal processing of ice and regolith, not traditional mining. Hydrocarbon Collection on Titan is surface-based, Desalination and Brine Processing each require their own specialized facilities.
> 
> Now looking at refining operations, I'm seeing that Oil Refining already exists, but I need dedicated modules for Iron-Nickel Refining, Iron Ore Smelting, and multiple steelmaking variants—Basic O₂, H₂ Reduction, Electric, and Solar. Aluminium Refining and Industrial Metals Refining also need their own modules.
> 
> For steelmaking specifically, I'm thinking the first three methods—Basic O₂, H₂ Reduction, and Electric—could potentially share a single Steel Mill module since they're convertible processes. Solar Steelmaking would be separate infrastructure given its reliance on concentrated solar power. Then there's Precious Metals Refining, Rare Earths Refining, Uranium Refining, Regolith Processing, Deuterium Purification, and two recycling/incineration variants for both municipal and industrial waste, plus Uranium and Thorium Fuel Reprocessing—each needing its own module.
> 
> Now I'm reconsidering the iron and steel chain more carefully. Iron Ore Smelting uses blast furnaces or direct reduction and sits upstream of steelmaking, but it's distinct infrastructure from the steel mill itself. Iron-Nickel Refining is specialized for asteroidal sources. So I could organize this as a Smelter module for Iron Ore Smelting (and possibly Aluminium Refining), a Steel Mill module for the three main steelmaking methods, and a Solar Furnace module for Solar Steelmaking.
> 
> But wait—the modules documentation suggests operations can shift between different processes within a module. So maybe I should think of Steel Mill as flexible enough to handle Iron Ore Smelting, Basic O₂ Steelmaking, Electric Steelmaking, and H₂ Reduction Steelmaking all together, with Solar Furnace as its own dedicated module for solar-based steel production.
> 
> Now for non-ferrous metals, I'm mapping out the refineries: Aluminium Smelter handles aluminium refining through electrolysis, Base Metals Smelter covers industrial metals refining with shared hydrometallurgical infrastructure, Precious Metals Refinery handles specialized small-scale precious metals work, and Rare Earths Refinery handles rare earth refining. I'm wondering if some of these should be grouped together based on their physical infrastructure similarities, but looking at how granular the operations file is, the modules should probably stay at a similar level of detail. For now I'll keep them separate and move on to oil refining. I'm realizing the isotope separation plant comment about uranium enrichment belongs in fuel manufacturing, not here. Let me also reconsider the steel production chain—blast furnaces, basic oxygen furnaces, and electric arc furnaces often operate in different contexts, so I should consolidate them under a single Steel Mill facility that handles multiple steelmaking processes rather than treating them as separate buildings.
> 
> Iron-Nickel Refining could reasonably share infrastructure with a steel mill since they're both metal processing, so I'll include that capability. The key insight is that modules represent physical infrastructure with flexible capacity—a Steel Mill can shift between different steelmaking methods through refitting, whereas a Solar Furnace is fundamentally location-dependent and can't be repurposed the same way.
> 
> Now I need to map out the Conversions and Synthesis operations, starting with water processing.
> 
> Looking at the synthesis and conversion operations, I'm seeing a lot of chemical processes that could cluster into a few infrastructure types. Water electrolysis gets its own plant, but CO₂ electrolysis might use the same facility. Methane pyrolysis, coal gasification, and biomass gasification seem like they could share a gasification plant infrastructure, though I need to think about whether biomass carbonization belongs with coal carbonization in a coke works or if it needs separate handling. The various synthesis reactions—methanation, steam methane reforming, water-gas shift, liquid hydrocarbon synthesis, and methanol synthesis—all look like they'd fit in a chemical synthesis plant. Ammonia synthesis is specialized enough to warrant its own Ammonia Plant, and polymer synthesis clearly needs its own facility.
> 
> Now I'm grouping the remaining processes: industrial chemicals synthesis fits into the broader chemical plant category, though fine chemicals might need their own specialized plant. Concrete, glass, ceramics, semiconductors, composites, solid fuels, and fertilizers all have distinct production requirements that justify their own plant types. The key insight is that many of these chemical operations share similar core infrastructure like reactors and separation equipment, so I can consolidate them under a chemical plant umbrella while keeping specialized processes separate. I'm grouping Chemical Plant to handle CO₂ Methanation, Steam Methane Reforming, Water-Gas Shift, RWGS, Methanol Synthesis, and Industrial Chemicals Synthesis since they share similar reactor infrastructure. For thermal decomposition processes, I'm keeping Gasification/Pyrolysis separate from Carbonization since coal coke ovens operate quite differently from gasifiers, even though they're related. FT Synthesis fits under Chemical Plant rather than its own module since it uses standard chemical reactor equipment. I'm mapping out the remaining specialized plants—synthesis, ammonia, petrochemical, and various production facilities—each handling distinct chemical or manufacturing processes. It's a comprehensive list, and while it seems extensive, it matches the operational granularity I'm aiming for.
> 
> Now I'm thinking about which operations can actually share the same physical infrastructure, and comparing against the existing module density from other sectors. Energy runs about one module per one to two operations, while Extraction consolidates multiple operations into shared Mine infrastructure. Using that as a calibration, I'm estimating I'll need roughly 8-12 modules for Refining, 10-15 for Conversions and Synthesis, 8-15 for Manufacturing, and 5-10 for Biological operations. Let me consolidate the full module list based on these ratios.
> 
> Now I'm reviewing the Energy modules to see what's already established and what needs adjustment. The list covers Solar, Geothermal, Hydroelectric, Wind, Tidal, and Combustion power generation across various scales and configurations.
> 
> Continuing through the remaining energy sources—Fuel Cells at both utility and distributed scales, Nuclear fission options including LEU and HEU reactors plus Thorium cycles, and fusion power systems.
> 
> Now I'm cataloging the extraction modules for hydrocarbon resources, starting with drilling rigs at different depths from surface down to subsurface formations. I'm continuing through the extraction infrastructure categories, covering underground mining at various depths, surface quarrying operations, and specialized processing facilities for atmospheric gases, volatile compounds from ice and permafrost, and liquid hydrocarbons.
> 
> Now I'm realizing that drilling rigs support multiple distinct operations—gas and oil drilling are separate but share the same module type. Similarly, mines can support various mining operations across different commodities and strata depths, with capacity constraints determining what each mine can handle.
> 
> I'm mapping out the extraction and processing chain: atmosphere separation, volatiles extraction, hydrocarbon collection, desalination, and brine processing each have their corresponding facility modules. Commercial fishing rounds out the biological resource operations.
> 
> For refining, I'm looking at specialized facilities—oil refineries for crude processing, iron-nickel refineries for asteroidal metals, and smelting complexes for converting oxide ores into finished metals. Continuing through the remaining facilities: rare earths processing uses acid and alkali digestion to separate concentrates, uranium plants employ leaching and fluorination to produce hexafluoride, regolith processors extract oxygen and metals from lunar material, isotope separation plants use cryogenic distillation for deuterium, recycling centers handle municipal waste with energy recovery, and industrial recycling facilities process end-of-life equipment through mechanical and chemical methods.
> 
> Now I'm reconsidering whether the Smelting Complex should be a single module or split into separate ones. An integrated steel mill with a blast furnace is fundamentally different from an electric arc furnace mini-mill, but the module framework allows shifting capacity between enabled operations—which could represent major refitting. So treating it as one Steel Mill module that can toggle between iron ore smelting, basic oxygen steelmaking, hydrogen reduction steelmaking, and electric steelmaking seems like a reasonable abstraction, though I'm wondering if iron ore smelting should actually be upstream of the steelmaking operations themselves.
> 
> Looking at this more carefully, a blast furnace produces pig iron, a BOF converts that to steel, an EAF melts scrap or DRI, and hydrogen DRI is a separate reducing step. I think the cleaner split is an Ironworks module handling iron ore smelting through blast furnaces and DRI, while a Steel Mill module handles the downstream steelmaking operations—basic oxygen, hydrogen reduction, and electric arc—since those share common mill infrastructure like furnaces, casters, and rolling mills. The hydrogen reduction steelmaking operation itself includes both the direct reduction step and the electric arc furnace refining, so it bridges both stages.
> 
> Actually, I'm overcomplicating this. Real steelmaking facilities are often integrated, so I should just create one unified Steel Mill module that encompasses blast furnaces, direct-reduction units, basic oxygen furnaces, and electric arc furnaces all together, with capacity that can shift between different reduction and refining methods. That handles iron ore smelting, basic oxygen steelmaking, hydrogen reduction steelmaking, and electric steelmaking in one place. Solar furnaces are distinct enough to stay separate, and iron-nickel refining is specialized enough to warrant its own module, though I'm wondering if a steel mill could theoretically handle it with some conversion cost, or if the asteroidal metal input makes it fundamentally different.
> 
> For the conversion and synthesis modules, I'm starting to map out the physical infrastructure needed—an electrolysis plant for water splitting and CO₂ reduction, and then gas compression facilities.
> 
> Now I'm working through the thermal conversion processes. Coal gasification and biomass gasification fit together as a gasification plant, but when I look at pyrolysis, the different reactor types are pulling me in different directions. Coke ovens for coal carbonization operate at a completely different scale than methane pyrolysis reactors, even though they're both thermal decomposition. I need to decide whether to keep them grouped under one pyrolysis facility or split them based on the actual operations listed.
> 
> For the synthesis plant, I'm seeing that methanation, reforming, shift reactions, Fischer-Tropsch, and methanol synthesis all rely on similar catalytic reactor technology with fixed-bed systems and heat management, so grouping them makes sense.
> 
> Ammonia synthesis is specialized enough with its high-pressure Haber-Bosch requirements that it deserves its own dedicated facility. Polymer production and commodity chemicals like chlor-alkali and sulfuric acid can each be their own modules, but I'm wondering if fine chemicals should split out since they need clean rooms and more precision equipment than large-scale commodity production.
> 
> Looking at the remaining facilities—concrete, glass, semiconductors, composites, and propellants—each has distinct processing requirements that warrant separate categorization.
> 
> Now I'm mapping the manufacturing operations to their corresponding infrastructure modules, organizing everything from construction and shipbuilding through to specialized electronics and robotics production.
> 
> I'm noticing there's significant overlap across all these manufacturing facilities—I could consolidate metal fabrication operations, combine electronics and chip production under a shared semiconductor infrastructure, and group machinery manufacturing together. The nuclear fuel production pathways might share facilities too, though they'd need careful separation depending on enrichment levels. of light manufacturing. Keep separate.
> 
> Electrical Equipment Manufacturing involves specialized processes like generator winding and transformer assembly that are distinct enough to warrant their own category, though I'm considering whether they could merge with Heavy Machinery Manufacturing since both use heavy manufacturing infrastructure — but the end products are quite different.
> 
> I'm reconsidering my approach here. At this level of abstraction, many manufacturing operations could share general factory infrastructure like machine tools and cranes, even though their specialized tooling varies significantly. So consolidating some categories makes sense.
> 
> I'm settling on a few core manufacturing hubs: Construction Yard for on-site assembly and installation of fixed infrastructure, Shipyard for large-scale vehicle and vessel fabrication, and Metal Fabrication Works for cutting, welding, and forming operations.
> 
> Now I'm mapping out the remaining specialized facilities — separate categories for steel and aluminium structures, composite layup and assembly, technical textiles, heavy machinery and electrical equipment, precision instruments and medical devices, and electronics manufacturing.
> 
> I'm also considering the Advanced Processor Fab and whether it overlaps with the Semiconductor Processing operation. The distinction is that Semiconductor Processing handles the upstream wafer fabrication and material creation, while Tensor and Neuromorphic Processor Fabrication focuses on the actual chip packaging and system assembly — though in practice these operations often occur within the same facility. I'm realizing these operations represent distinct supply chain stages rather than overlapping infrastructure—Semiconductor Processing handles raw wafer creation, while Tensor and Neuromorphic Processor Fabrication handle the advanced lithography work, and Electronics Manufacturing covers the final assembly and integration. Though in reality these steps often happen in the same facility, the operations are deliberately separating them into distinct functional tiers.
> 
> Now I'm mapping out the remaining manufacturing operations: Solar Panel, Battery, and Nuclear Fuel fabrication plants, plus Consumer Goods and Robotics factories. I'm wondering whether Robotics should stand alone or integrate with general manufacturing since it combines structural, electronic, and mechanical components.
> 
> Looking at specialization levels, Solar Panel and Battery manufacturing are distinct enough to warrant separate entries given their unique processes. Consumer Goods and Specialty Goods could consolidate into one factory, though I should distinguish this from the Artisanal Workshop already in the Biological section—these are industrial-scale operations rather than handcrafted work.
> 
> Now I'm mapping out the biological modules more systematically. I see the existing categories like Industrial Farm, Controlled-Environment Farm, and Artisanal Farm, plus Artisanal Workshop and Commercial Fishing. I need to fill in gaps for aquaculture, forestry, algal cultivation, cellular agriculture, and fermentation operations—each deserves its own module based on how specialized they are. Now I'm mapping out the remaining biological modules—forestry, algal cultivation, bioreactors, food processing, biogas, biomass processing, pharmaceuticals, soil conditioning, and wastewater treatment—each tied to their specific production outputs. I'm noticing there's significant overlap and wondering if I should consolidate some of these facilities to keep the system more manageable, particularly around aquaculture systems and their different cultivation methods. Biomass Liquefaction uses chemical engineering processes like catalytic pyrolysis, which could fit in Conversions, but since it's classified as Biological, I'll keep it separate from Biogas Plant given their fundamentally different mechanisms. The module sections are flexible for organization anyway, so I can place things where they make the most sense structurally while maintaining the functional links to operations.
> 
> Now I'm listing out the biological modules I'm settling on: Industrial Farm for large-scale conventional agriculture, Controlled-Environment Farm for sealed greenhouse operations, Artisanal Farm for small-scale specialty production, Artisanal Workshop for handcraft goods, Commercial Fishing Fleet for wild-capture fishing and processing, Aquaculture Farm for farmed aquatic organisms, Forestry Operation for managed timber and biomass harvesting, and Algal Cultivation Facility for photobioreactor-based production.
> 
> Continuing through the production and processing infrastructure: Bioreactor Facility for cellular agriculture and fermentation, Food Processing Plant for cooking and preservation, Biogas Digester for converting organic waste to methane, Biomass Processing Plant for converting biomass to liquid fuels, Pharmaceutical Plant for drug and vaccine synthesis, and Soil Conditioning Facility for composting and agricultural media preparation.
> 
> Now I'm adding the Wastewater Treatment Plant to handle sewage, greywater, and organic waste with water reclamation and nutrient recovery. That brings me to 15 biological modules covering 19 operations.
> 
> I'm reconsidering whether Ethanol Fermentation needs its own dedicated facility or if it can be absorbed into the Bioreactor Facility since fermentation tanks can be repurposed for different fermentation products. I'll fold it into the Bioreactor Facility to keep things streamlined.
> 
> Now I'm going through each operation systematically to verify coverage, starting with the energy operations like Solar Power, Geothermal Power, and Hydro Power. I'm mapping out the nuclear and fusion power sources to their corresponding facilities, then moving into extraction methods for various resources like atmospheric processing, volatiles, hydrocarbons, and desalination systems.
> 
> Now I'm working through the resource extraction infrastructure—drilling rigs for gas and oil at various depths, and mines for coal, metals, and uranium, each with depth variants from surface to extreme conditions.
> 
> Continuing with rare earths, sulfur, organics, and industrial minerals all mapped to mines, plus stone and regolith quarries, then moving into the refining stage where oil goes to refineries, iron-nickel to its own refinery, and iron ore gets smelted into steel mills and basic oxygen steelmaking facilities.
> 
> Now I'm mapping out the rest of the refining processes—hydrogen reduction and electric steelmaking both feed into steel mills, while solar steelmaking uses a solar furnace instead. Aluminium, industrial metals, and precious metals each get their own specialized smelters or refineries, rare earths go to a dedicated refinery, uranium gets processed at a uranium plant, and regolith processing has its own facility. Deuterium purification happens at an isotope separation plant, and municipal recycling feeds into a recycling center, with industrial recycling still being categorized.
> 
> Continuing with the industrial recycling and fuel reprocessing—both uranium and thorium fuel reprocessing routes through a nuclear fuel reprocessing plant. Then I'm looking at the conversion and synthesis processes: water and CO₂ electrolysis both use an electrolysis plant, methane and coal pyrolysis go to a pyrolysis facility, and biomass carbonization also routes there. Coal and biomass gasification both feed into a gasification plant, while CO₂ methanation is still being mapped.
> 
> Now I'm working through the synthesis pathways—steam methane reforming, water-gas shift reactions, and reverse water-gas shift all converge at a synthesis plant. Liquid hydrocarbon synthesis and various methanol synthesis routes (from CO₂ and CO) also use the synthesis plant. Ammonia synthesis has its own dedicated ammonia plant, polymer synthesis goes to a polymer plant, and then I'm categorizing the chemical production: industrial chemicals use a chemical plant while fine chemicals get their own specialized facility. For materials production, concrete has a concrete plant, glass and ceramics share a glass/ceramics plant, semiconductors route through a semiconductor fab, and I'm still mapping out composite materials.
> 
> Now I'm continuing with the remaining synthesis processes—composites production goes to a composites plant, solid fuel production to a propellant plant, and fertilizer production to a fertilizer plant. Then I'm shifting into manufacturing operations: infrastructure and construction work at a construction yard, vehicle and spacecraft assembly at an assembly yard, and steel and aluminum structures both route through metal fabrication works. Composite structures get their own composite fabrication works, technical textiles go to a textiles mill, and heavy machinery uses a heavy manufacturing plant. For electrical and precision equipment, I'm considering whether electrical manufacturing should be separate or combined with heavy manufacturing, while precision equipment has its own dedicated plant. Electronics and robotics each get their own specialized factories.
> 
> Continuing down the list, I'm mapping tensor and neuromorphic processor fabrication to an advanced processor fab, solar panels to their own factory, batteries to a battery factory, and all nuclear fuel types—LEU, HEU, and thorium—consolidate into a single nuclear fuel fabrication plant. Consumer and specialty goods both route to a consumer goods factory, though I'm noting that specialty goods might warrant separation. I'm deciding to keep electrical equipment manufacturing distinct from heavy machinery since generators, transformers, motors, and cables are fundamentally different from turbines and engines.
> 
> Now I'm stepping back to count the total module inventory across all categories—energy, extraction, refining, conversions, manufacturing, and biological—and I'm realizing I'm approaching 88 to 90 modules total, which is nearly a one-to-one ratio with the 95 operations in the system. That level of granularity might be excessive, though some modules do handle multiple operations, so I need to reconsider whether I'm over-segmenting the manufacturing tier.
> 
> Looking at consolidation opportunities, I could merge the Precious Metals Refinery with the Industrial Metals Smelter since precious metals refining often occurs as a secondary process in base metal operations, but the technical differences between fire assay and flash smelting are substantial enough to warrant keeping them distinct. Similarly, Gasification and Pyrolysis are both thermal processes but produce fundamentally different outputs—syngas versus solid carbon—so combining them doesn't make sense. The Ammonia Plant and Synthesis Plant are another pair I'm weighing, though I haven't fully resolved whether they should merge.
> 
> For manufacturing, Solar Panel and Electronics factories have some process overlap in semiconductor-like fabrication, but solar manufacturing involves distinct materials like glass and aluminum framing that push me toward keeping them separate. Battery and Robotics factories are each too specialized to consolidate with other operations. On the biological side, I'm considering whether the Soil Conditioning Facility could be absorbed into the farm operations as an extension of their capacity, or if it's distinct enough to remain standalone. The Biomass Processing Plant and Biogas Digester are another pair I'm evaluating for potential consolidation.
> 
> Both handle biomass but through fundamentally different processes—thermochemical versus anaerobic digestion—so they should stay separate. I'm now reconsidering whether Biomass Liquefaction belongs in the Biological category at all, since it's really a chemical engineering operation. It might make more sense to move it under Conversions/Synthesis alongside the Pyrolysis Facility, since hydrothermal liquefaction and pyrolysis are related thermochemical processes. But Pyrolysis already covers methane pyrolysis, coal carbonization, and biomass carbonization, so adding liquefaction would make it quite broad. I need to decide whether to keep biomass liquefaction as its own module or consolidate it with pyrolysis.
> 
> I'm also looking at manufacturing consolidation opportunities—Heavy Machinery Factory and Electrical Equipment Factory could potentially merge into a single "Heavy Manufacturing Plant" that shifts capacity between mechanical and electrical equipment. And I'm wondering if Consumer Goods Factory could absorb Specialty Goods Manufacturing, though the operations file seems to distinguish between mass-production and specialty items.
> 
> Now I'm examining whether Composites Plant and Composite Fabrication Works are actually distinct—one handles the material production side (fiber-reinforced polymer layup, filament winding, pultrusion) while the other focuses on finished structures, but the line between them feels blurry.
> 
> Since composite manufacturing typically integrates both steps directly into the final shape, I'll merge them into a single Composites Fabrication Facility under Manufacturing. I'm keeping Propellant Plant separate though, since solid fuel production has specialized infrastructure requirements that warrant its own module despite being niche.
> 
> Now I'm working through the final module count—aiming for around 70-80 total to match the abstraction level in the existing files. Energy looks solid at 16 modules covering the 22 operations, and Extraction at 15 modules. I'm noticing the drilling rig and mine entries in the existing file have multiple strata variants, so I need to account for how those break down across the different depth categories.
> 
> For Refining, I'm tallying up the processing facilities—Oil Refinery, various smelters for different metals, specialized plants for uranium and isotopes, recycling centers, and nuclear fuel reprocessing. That's coming to around 14 entries so far.
> 
> Now looking at Conversions and Synthesis operations—electrolysis, gasification, pyrolysis, synthesis, ammonia, polymers, chemicals, concrete, glass, semiconductors, propellants, and fertilizers. I'm getting 14 modules here too, though I need to sort out where Composites belongs since I was considering moving it between categories. the operations has:
> - Water Electrolysis
> - CO₂ Electrolysis
> - Methane Pyrolysis
> - Coal Gasification
> - Biomass Gasification
> - Coal Carbonization
> - Biomass Carbonization
> - CO₂ Methanation
> - Steam Methane Reforming
> - Water-Gas Shift
> - Reverse Water-Gas Shift
> - Liquid Hydrocarbons Synthesis
> - CO₂ Methanol Synthesis
> - CO Methanol Synthesis
> - Ammonia Synthesis
> - Polymer Synthesis
> - Industrial Chemicals Synthesis
> - Fine Chemicals Synthesis
> - Concrete Production
> - Glass/Ceramics Production
> - Semiconductor Processing
> - Composite Production
> - Solid Fuel Production
> - Fertilizer Production
> 
> That's 24 operations. Now I'm organizing these into 14 specialized modules—electrolysis, gasification, pyrolysis, synthesis, and dedicated plants for ammonia, polymers, chemicals, concrete, glass, semiconductors, composites, propellants, and fertilizers. Beyond these conversion operations, I also need to account for manufacturing infrastructure like yards, transport equipment, and various structural fabrication capabilities. I'm working through whether to keep Composite Production and Composite Structures Manufacturing as separate operations or combine them, since they'd likely share similar equipment like autoclaves and layup facilities.
> 
> I think merging them into a single Composites Facility makes sense organizationally. I'll place it in Manufacturing rather than Conversions/Synthesis, which means removing the separate Composites Plant from Conversions/Synthesis. This brings Conversions/Synthesis down to 13 modules and keeps Manufacturing at 13 modules as well, with the new combined facility handling both operations.
> 
> But I should double-check whether these operations actually overlap enough to justify combining them. Let me look at what each one covers.
> 
> Composite Production focuses on the raw materials side—making the composite materials themselves through fiber reinforcement and matrix forming. Composite Structures Manufacturing is about taking those materials and assembling them into finished products like panels, pressure vessels, and habitat shells. They do share equipment and infrastructure, so consolidating them into one module makes sense.
> 
> Now looking at the Biological operations, I'm seeing a lot of overlap across agriculture, aquaculture, forestry, and food production that I need to sort through.
> 
> For the processing side, I'm mapping out fermentation, biogas, pharmaceuticals, and waste treatment operations. I'm organizing these into distinct modules—Industrial Farm, Controlled-Environment Farm, Artisanal Farm, Artisanal Workshop, Commercial Fishing Fleet, Aquaculture Farm, Forestry Operation, and Algal Cultivation Facility—each handling one or two specific operations to keep them focused and manageable.
> 
> Now I'm considering whether to split aquaculture into two separate modules since open ocean systems and sealed recirculating systems are fundamentally different in infrastructure and operation.
> 
> I should keep Controlled-Environment Aquaculture distinct from Controlled-Environment Farm even though both use sealed systems—the equipment and biological requirements are too different. Algal Cultivation also needs to stay separate since photobioreactors operate differently from traditional farms. The Bioreactor Facility grouping makes sense for cellular agriculture and fermentation operations since they all rely on large-scale bioreactor technology. I'm questioning whether Soil Conditioning warrants its own module or if it's substantial enough given it covers composting, amendment blending, and regolith processing.
> 
> For Biomass Conversion, I'm seeing it as distinct from general chemical plants since it processes biological feedstock through different methods than industrial chemicals like chlor-alkali or sulfuric acid production. Looking at the full count across all categories—16 energy modules, 15 extraction, 14 refining, 13 conversions, 13 manufacturing, and 16 biological—I'm at 87 total modules, which feels slightly inflated. I should explore a few more consolidation opportunities to trim this down.
> 
> The Isotope Separation Plant is highly specialized for deuterium production, which matters for fusion applications, so I'll keep that separate despite the overlap with chemical engineering. The Propellant Plant is similarly specialized and worth maintaining. For the Biogas Digester, there's a real case for merging it with Wastewater Treatment since many facilities handle both anaerobic digestion and biowaste treatment together, though biogas production can also process crop residues independently of wastewater streams, so I'm still weighing whether to combine them or keep them distinct.
> 
> The Soil Conditioning Facility is critical for off-Earth agriculture but also functions differently in terrestrial contexts, so that stays separate. Fine Chemicals is trickier—while a single Chemical Plant module could theoretically handle both industrial and fine chemicals synthesis with conversion costs, in practice these are fundamentally different operations with different facility requirements and input streams. Fine chemicals align more closely with pharmaceutical production anyway, so I'm leaning toward keeping them separate rather than forcing a consolidation that doesn't reflect how these industries actually work.
> 
> Now I'm verifying my module counts against the full operation list to make sure nothing's falling through the cracks. I've got 87 modules covering 122 operations across all six categories, and the coverage looks solid. Let me recount the operations themselves to confirm I haven't made an error there. Now I'm working through the extraction methods, continuing from atmosphere separation and volatiles extraction through various mining operations—I count 19 total. Moving into refining processes next, starting with oil refining and metal processing techniques.
> 
> Continuing with the refining category, I'm listing out different steelmaking approaches, metal refinement methods, and recycling processes—that brings me to 18 refining operations. Now I'm moving into conversions and synthesis, starting with electrolysis and gasification methods.
> 
> I'm working through the synthesis pathways next, covering everything from methane production and reforming to methanol synthesis, ammonia, and various chemical syntheses, plus materials like concrete, glass, and semiconductors—that's 24 conversion processes total. Then I'm starting on the manufacturing category with infrastructure, transport, and different structural materials.
> 
> Now I'm continuing through manufacturing with technical textiles, heavy machinery, electrical and precision equipment, electronics including specialized processors, renewable energy components like solar and batteries, nuclear fuel types, and consumer goods—20 manufacturing processes in all. Then moving into biological systems, listing agricultural methods from open farming to controlled environments, aquaculture, fishing, forestry, and emerging approaches like cellular agriculture and algal cultivation.
> 
> Finishing out the biological category with fermentation operations, biogas and biomass conversion, pharmaceutical synthesis, soil conditioning, and biowaste treatment—18 total biological processes. That brings me to 121 operations across all six categories. Now I need to refine the documentation, particularly around heat generation—I should add the note about providing facility or district heat to all power plants that produce thermal energy, not just combustion ones, since nuclear and geothermal can also supply heat to surrounding systems. me think about which ones:
> 
> - Solar Farm → PV primarily. Concentrated solar thermal does produce heat. "May provide facility or district heat" for CST variants.
> - Solar Arrays → PV only. Not really heat-producing.
> - Geothermal Plant → Definitely produces heat. Yes.
> - Hydroelectric Dam → No significant heat.
> - Wind Farm → No heat.
> - Tidal Power Station → No heat.
> - Combustion Power Plant → Yes, definitely.
> - Utility Fuel Cells → Yes, produces waste heat.
> - Distributed Fuel Cells → Yes, produces waste heat.
> - LEU Nuclear Plant → Yes.
> - HEU Nuclear Reactor → Yes.
> - Thorium Nuclear Plant → Yes.
> - D-T Fusion Plant → Yes.
> - D-³He Fusion Plant → Yes.
> - ³He-³He Fusion Plant → Yes.
> - Radioisotope Generator → Yes, primarily produces heat.
> 
> Now I need to figure out which modules should include the "May provide facility or district heat" note. The guideline says all electricity- and heat-generating modules should have it, but that really applies to the thermal generators—combustion, nuclear, fusion, fuel cells, and geothermal. Solar thermal variants and the radioisotope generator fit too. Wind, hydro, and PV don't produce usable heat, so they shouldn't get the note. I'll add it to geothermal, combustion, both fuel cell types, all the nuclear variants, all the fusion types, radioisotope, and mention it for the CST option on solar farms.
> 
> For the combustion and fuel cell modules, the note mentions they can convert between fuel types—the module design already handles this through shifting capacity between different fuel operations, so I don't need to explicitly call it out as "convertible" but I could reference the fuel variety. For utility fuel cells, the conversion note just clarifies that switching between units happens over time, which is already implicit in how the module works.
> 
> Now I'm ready to write the descriptions following the real-world framing rather than model mechanics. I'm refining the descriptions to match the style from physical_operations.md—keeping them brief and technical while avoiding implementation details. I need to add notes about facility or district heat where relevant and clean up any bracketed annotations from the existing items. I'm continuing through the energy infrastructure options, covering wind installations across multiple planetary environments, tidal power specific to Earth, then moving into combustion and fuel cell systems at both utility and distributed scales.
> 
> Now I'm cataloging the nuclear and fusion reactor variants—light-water and heavy-water reactors on LEU fuel, compact high-enrichment designs for space and naval use, thorium-cycle breeders, and fusion approaches using deuterium-tritium and deuterium-helium-3 fuel cycles, each with potential for facility or district heating applications.
> 
> I'm also noting the advanced fusion concepts like helium-3 fusion that produce no neutrons but require extreme ignition temperatures, and radioisotope generators using plutonium-238 or strontium-90 for deep space missions where reliability and longevity matter more than power density.
> 
> Now moving into extraction equipment—surface and near-surface drilling rigs for accessing hydrocarbon formations at varying depths. I'm continuing through the extraction infrastructure categories, detailing the depth ranges for underground mining operations, then moving into specialized processing facilities like atmosphere processors for gas extraction and volatiles plants for ice-based resource recovery. deposits, permafrost, regolith, and brine reservoirs.
> - **Hydrocarbon Harvester** — Surface collection and primary separation infrastructure for liquid hydrocarbon bodies, principally Titan's methane–ethane seas.
> - **Desalination Plant** — Reverse osmosis, multi-stage flash distillation, or electrodialysis facility producing fresh water from seawater, brackish water, or brine. On ocean worlds, applicable to melt-probe or plume-sourced water.
> - **Brine Processing Plant** — Ion exchange, fractional crystallization, solvent extraction, and evaporation facility recovering dissolved minerals and salts from brines, desalination reject, and ocean water.
> 
> For refining operations, I'm looking at facilities that process raw materials into usable products—oil refineries that break down crude into fuels and solvents, iron-nickel refineries extracting valuable metals from asteroidal sources, steel mills with multiple furnace types for different reduction methods, solar furnaces leveraging concentrated sunlight for thermal processing at high-insolation sites, and aluminium smelters using electrolytic processes.
> 
> Beyond that, there are specialized refineries for base metals like copper and zinc, precious metals including gold and platinum-group elements, rare earths requiring multi-stage separation, and uranium processing facilities that are still being detailed. shredding, pyrometallurgical smelting, and hydrometallurgical processing to recover materials from end-of-life equipment and industrial waste. Then there's nuclear fuel reprocessing using aqueous or pyrochemical separation to extract fissile material from spent fuel assemblies. Moving into conversion and synthesis facilities now — electrolysis plants for splitting water and CO₂ to produce hydrogen, oxygen, and synthesis gas, which are fundamental across ISRU operations. Gasification converts solid carbonaceous feedstocks into synthesis gas, and pyrolysis is next in this sequence.
> 
> Pyrolysis operates without oxygen to thermally decompose coal and biomass into coke, biochar, and solid carbon while recovering gases and liquids. Then synthesis plants use catalytic reactors for conversions like Fischer–Tropsch and methanol synthesis, ammonia plants run the Haber–Bosch process for fertilizer production, and polymer plants polymerize various feedstocks into plastics, thermosets, and fibers.
> 
> Now I'm moving through commodity chemical production facilities—chemical plants handling electrolysis and acid processes, fine chemicals plants for specialty reagents and pharmaceuticals, concrete plants with calcination and clinker grinding including off-Earth variants, glass and ceramics furnaces, and semiconductor fabs for crystal growth and wafer fabrication.
> 
> Continuing with composites fabrication for fiber-reinforced and metal-matrix materials, propellant plants for solid rocket grain casting, and fertilizer compounding facilities blending ammonia and phosphate derivatives.
> 
> Then there's the manufacturing infrastructure—construction yards for on-site assembly of buildings, roads, habitats and launch facilities, plus vehicle and spacecraft assembly operations.
> 
> Now I'm detailing the specialized fabrication works: metal fabrication for structural steel and aluminum products, composite structures for pressure vessels and habitat shells, and a textiles mill producing high-performance aramid and carbon-fiber fabrics.
> 
> Continuing through the heavy manufacturing and precision plants that handle turbines, engines, and scientific instruments, then the electronics factory for circuit boards and semiconductors, an advanced processor fab for tensor processors using extreme-ultraviolet lithography, and a robotics factory for fabricating robotic manipulators.
> 
> Now I'm mapping out the remaining production facilities—solar panel manufacturing with photovoltaic cell fabrication and encapsulation, battery production for lithium-ion and solid-state chemistries, nuclear fuel fabrication including uranium enrichment and fuel assembly, and consumer goods mass-production.
> 
> Then moving into biological systems: large-scale mechanized farms, controlled-environment greenhouses with active climate and nutrient management, artisanal food production for specialty items, handcraft workshops for pottery and textiles, and commercial operations.
> 
> Now I'm detailing aquatic and forestry production — fishing fleets with processing infrastructure, open-water and controlled aquaculture systems for fish and shellfish, managed timber plantations, algal cultivation facilities that produce biomass and oxygen, and industrial bioreactors.
> 
> Continuing with the conversion and processing infrastructure — bioreactors for cultured meat and fermentation products, food processing plants, biogas digesters that convert organic waste to methane, biomass conversion plants producing liquid fuels, and pharmaceutical manufacturing facilities.
> 
> Then there's soil conditioning for preparing growth media and wastewater treatment plants that handle sewage and greywater while recovering nutrients and water — both critical for closed-loop space habitats.
> 
> Now I'm tallying up the total modules across all these categories and getting 89, which feels substantial. I'm reconsidering whether some facilities might be redundant or if I should consolidate certain operations.
> 
> Looking more closely, I notice the composites work appears in two places — once as a conversions plant and again as structural manufacturing. Since composite fabrication and assembly blur the line between materials processing and structures anyway, I should merge these into a single composites facility that handles everything from fiber layup to finished pressure vessels and inflatable structures. That brings conversions down to 13 modules and keeps manufacturing at 14, bringing my total to 88.
> 
> Let me verify the manufacturing count with this consolidation: construction yard, vehicle assembly, metal fabrication, the combined composites facility...
> 
> actually, that's still 14 modules total, just with a cleaner organization.
> 
> Now I'm weighing whether controlled-environment aquaculture and open aquaculture farms should be separate. The infrastructure really is fundamentally different — ocean pens versus sealed recirculating systems — so I think keeping them distinct makes sense even though off-Earth operations would only use the controlled-environment version.
> 
> I'm also considering whether biogas digesters and wastewater treatment should merge, since they often work together in practice.  But the operations are genuinely separate — biogas from agricultural residues is a different process than treating wastewater — so I'll keep them as distinct modules.
> 
> Similarly, biomass conversion and pyrolysis both involve thermochemical processing, but one is a wet process and the other is dry, requiring different equipment. They should stay separate.
> 
> 88 modules feels like a solid starting point for this first pass. Now I'm reconsidering whether I really need both a general Chemical Plant and a Fine Chemicals Plant, since they handle very different scales and types of synthesis.
> 
> Looking at the distinction: industrial chemicals are heavy commodity production like chlor-alkali and sulfuric acid, while fine chemicals are precision specialty work — catalysts, coatings, adhesives, electronic-grade reagents. These are fundamentally different operations, so keeping them separate makes sense.
> 
> For the semiconductor modules, I notice the operations file treats Semiconductor Processing as a conversion step and Tensor/Neuromorphic Processor Fabrication as manufacturing, which mirrors how raw material smelting differs from finished product assembly. Even though real fabs do both, the separation in the operations file justifies having distinct modules.
> 
> I've now mapped all operations across energy, extraction, refining, conversions, manufacturing, and biological categories, so the module structure is complete.
> 
> Now I'm reviewing the naming conventions to ensure consistency—checking that terms like "Farm," "Plant," and "Station" are applied uniformly across the energy modules and that descriptions align throughout the list.
> 
> Looking at the extraction and refining categories, the terminology holds up well: "Rig" for drilling operations, "Mine" and "Quarry" for solid resource extraction, and specialized terms like "Processor," "Harvester," and "Plant" for their respective functions. The refining section follows the same pattern with "Refinery," "Mill," and facility-based naming.
> 
> Now moving through the rest of the refining options—furnaces, smelters, and processing plants for various metals and materials—they all fit the established naming convention. The conversion and synthesis section continues this with "Plant" and "Facility" as the primary suffixes, covering everything from electrolysis through chemical synthesis and polymer production. I've finished reviewing the biological production facilities, so now I'm thinking through whether there are any infrastructure categories I've overlooked—things that might not fit neatly into a single operation type but still represent distinct production or processing capabilities.
> 
> Looking at energy storage, power distribution, spaceports, and life support systems, I'm realizing most of these are either handled at the systems level or already covered by existing modules like the Construction Yard and various specialized operations. The only one that might warrant its own consideration is pipeline infrastructure, but I should think through whether that's truly necessary or if it's already accounted for.
> 
> Now I'm double-checking the non-operation items from the operations file to see if any of them need dedicated modules. Infrastructure and large vehicles are tracked separately from the resource system but use modules for construction. Populations, natural systems, and waste disposal don't require additional modules since they're either handled outside the system or don't need explicit simulation. I think I've covered everything comprehensively now.
> 
> Before I finalize the document, I need to remove all the bracketed notes as the user requested and weave their intent directly into the descriptions. I'm going through each one: the solar entries already have their scale distinctions built in, the power plants and fuel cells need the "may provide facility or district heat" language added where applicable, and I need to finish reviewing the remaining bracketed notes to ensure nothing gets lost in the consolidation. I've finished addressing all the notes about the different facility types. Now I need to be more selective about which power generation modules should include the "may provide facility or district heat" capability—I'm reviewing which ones actually produce significant waste heat worth capturing, like geothermal and combustion plants, versus those where heat recovery is less practical. I'm refining the Solar Farm description to clarify that concentrated solar thermal actually produces heat as part of the generation process rather than waste heat, so I should note it can provide facility or district heating. Now I need to finalize the complete file and consider whether to include a dedicated energy storage module, since battery manufacturing is listed but operating grid-scale storage systems is a separate operational concern. There are several other gaps worth noting: water infrastructure beyond desalination and wastewater treatment isn't explicitly modeled, telecommunications is missing entirely, and the composites modules have been merged into one. For improving the context files, I should document which modules enable which operations, and consider whether utility modules might be needed for water distribution, telecommunications, or energy storage that don't fit neatly into the current structure. The operations file also groups related processes like iron ore smelting and steelmaking, which hints that module boundaries could be refined based on how the simulation chains inputs and outputs together.
> 
> Now I'm reconsidering whether to keep Chemical Plant and Fine Chemicals Plant separate. They're fundamentally different—industrial chemicals uses large-scale electrolysis cells while fine chemicals relies on batch reactors with precise control, and fine chemicals is downstream of industrial chemicals anyway, so I'll maintain the distinction. I'm also thinking through whether to add a dedicated water cycle module since the operations file covers desalination, brine processing, and water electrolysis, but potable water treatment isn't listed as an operation and wastewater treatment already has its own plant.
> 
> Now I need to finalize the document and make sure I'm describing individual units where appropriate versus aggregate capacity where it makes more sense. I'm settling on consistent terminology for the final version: centralized facilities like plants and smelters, distributed systems that aggregate capacity, and individual units for things like the Radioisotope Generator. I'm also reconsidering whether deep subsurface mines make sense given the physical constraints—drilling can reach 5-12 km, but mining operations realistically max out around 2-5 km, so keeping mines limited to that depth range seems appropriate. I'm noticing the depth ranges differ between drilling and mining operations — drilling extends to extreme subsurface at 5+ km, but mining only goes to deep subsurface at 2-5 km since there aren't practical mining operations beyond that depth. For the descriptions, I want to keep them consistent at 1-2 sentences each, with a third sentence reserved for important notes like heat cogeneration or applicability across different celestial bodies.

Here's the developed `modules.md`:

```markdown
# Modules

Modules are the physical infrastructure that implement operations.

Notes:

1. Modules can have a many-to-many relationship with operations. Each module provides capacity for one or more operations. Each operation is enabled by one or more modules. (In practice, many specific cases are one-to-one.)
2. Although described here in terms of individual units, modules are implemented as a continuous "capacity" value.
3. A module's capacity can be shifted ("converted") among its enabled operations. E.g., Combustion Power Plant (which really represents a capacity of combustion plants) could be shifted from 70% Coal Power / 30% Methane Power to 40/60 over time. Speed and cost of conversion are fixed properties of each module. This could represent anything from a major refitting/retooling/relocation to a minor operational adjustment, depending on the module.


## Energy

- **Solar Farm** — Utility-scale photovoltaic and concentrated solar thermal installation. Primary power source across the inner solar system. Concentrated solar thermal variants may provide facility or district heat.
- **Solar Arrays** — Distributed and small-scale photovoltaic systems including rooftop, building-integrated, and portable deployments. Represents aggregate capacity of many individual installations.
- **Geothermal Plant** — Power station extracting heat from planetary interiors via hydrothermal wells or engineered hot-rock systems. Applicable on Earth and tidally heated bodies. May provide facility or district heat.
- **Hydroelectric Dam** — River impoundment or run-of-river hydropower installation.
- **Wind Farm** — Utility-scale wind turbine array, onshore or offshore. Applicable on Earth, Mars, Venus upper atmosphere, and Titan.
- **Tidal Power Station** — Tidal barrage or tidal stream turbine installation exploiting gravitational tidal forcing. Earth-only under foreseeable conditions.
- **Combustion Power Plant** — Utility-scale thermal power station with boilers and gas turbines for burning hydrogen, methane, methanol, ethanol, liquid hydrocarbons, oil, or coal to generate electricity. May provide facility or district heat.
- **Utility Fuel Cells** — Centralized stationary fuel cell installation operating on hydrogen, methane, or methanol for grid-scale or large-facility power. May provide facility or district heat.
- **Distributed Fuel Cells** — Small-scale fuel cell units for buildings, vehicles, and remote installations, operating on hydrogen, methane, or methanol. Represents aggregate capacity of many individual units. May provide facility heat.
- **LEU Nuclear Plant** — Light-water or heavy-water reactor power station operating on low-enriched uranium fuel. May provide facility or district heat.
- **HEU Nuclear Reactor** — Compact high-enrichment uranium reactor for naval propulsion, space power, and other applications requiring high power density at small scale. May provide facility heat.
- **Thorium Nuclear Plant** — Thermal-breeder reactor power station operating on thorium-cycle fuel with in-situ breeding of fissile uranium-233. May provide facility or district heat.
- **D-T Fusion Plant** — Deuterium–tritium fusion power station with lithium tritium-breeding blankets. May provide facility or district heat.
- **D-³He Fusion Plant** — Deuterium–helium-3 fusion power station producing substantially fewer neutrons than D-T designs. May provide facility or district heat.
- **³He-³He Fusion Plant** — Helium-3 fusion power station producing no neutrons but requiring the highest ignition temperature of practical fusion cycles. May provide facility or district heat.
- **Radioisotope Generator** — Radioisotope thermoelectric or Stirling generator unit using plutonium-238 or strontium-90. Low power density but extreme reliability and longevity; critical for deep outer solar system installations. May provide facility heat.


## Extraction

- **Drilling Rig (surface)** — Well drilling and completion equipment for gaseous and liquid hydrocarbon extraction from surface-accessible formations to approximately 100 m depth on land.
- **Drilling Rig (near-surface)** — Well drilling and completion equipment for hydrocarbon extraction from formations at approximately 0.1–0.5 km depth, including offshore continental shelf.
- **Drilling Rig (subsurface)** — Well drilling and completion equipment for hydrocarbon extraction from formations at approximately 0.5–2.0 km depth.
- **Drilling Rig (deep subsurface)** — Well drilling and completion equipment for hydrocarbon extraction from formations at approximately 2.0–5.0 km depth.
- **Drilling Rig (extreme subsurface)** — Well drilling and completion equipment for hydrocarbon extraction from formations at approximately 5.0–12.0 km depth.
- **Mine (surface)** — Open-pit, strip, and placer mining capacity for solid resource extraction from surface strata to approximately 100 m depth. Target ore proportions adjustable over time.
- **Mine (near-surface)** — Underground and deep open-pit mining capacity for strata at approximately 0.1–0.5 km depth, including sub-seafloor operations on continental shelves.
- **Mine (subsurface)** — Underground mining capacity for strata at approximately 0.5–2.0 km depth.
- **Mine (deep subsurface)** — Underground mining capacity for deep strata at approximately 2.0–5.0 km depth.
- **Quarry** — Surface extraction operation for bulk stone and loose regolith, producing construction aggregate, dimension stone, and industrial or ISRU feedstock.
- **Atmosphere Processor** — Cryogenic distillation, pressure-swing adsorption, membrane separation, or centrifugal processing plant for separating target gases from planetary atmospheres. On gas giants, represents atmospheric skimming or balloon-based harvesting systems.
- **Volatiles Extraction Plant** — Thermal processing, sublimation trapping, and mechanical extraction facility for recovering water, carbon dioxide, ammonia, and other volatile species from ice deposits, permafrost, regolith, and brine reservoirs.
- **Hydrocarbon Harvester** — Surface collection and primary separation infrastructure for liquid hydrocarbon bodies, principally Titan's methane–ethane seas.
- **Desalination Plant** — Reverse osmosis, multi-stage flash distillation, or electrodialysis facility producing fresh water from seawater, brackish water, or brine. On ocean worlds, applicable to melt-probe or plume-sourced water.
- **Brine Processing Plant** — Selective ion exchange, fractional crystallization, solvent extraction, and evaporation facility for recovering dissolved minerals and salts from ocean water, desalination reject, and subsurface brines.


## Refining

- **Oil Refinery** — Fractional distillation, catalytic cracking, and hydroprocessing facility for crude oil. Produces liquid fuels, petrochemical naphtha, and industrial solvents with elemental sulfur recovery.
- **Iron-Nickel Refinery** — Pyrometallurgical and hydrometallurgical facility for processing native iron–nickel alloy from asteroidal and other metallic sources, with recovery of cobalt, platinum-group metals, and other by-products.
- **Steel Mill** — Integrated iron and steel production facility encompassing blast furnaces, direct-reduction units, basic oxygen furnaces, and electric arc furnaces. Capacity adjustable among ore-reduction and steelmaking methods over time.
- **Solar Furnace** — Concentrated-sunlight thermal processing facility for direct solar reduction and melting of metals. Sited at high-insolation locations such as lunar or Mercurian polar crater rims, or on solar-tracking platforms on slowly rotating bodies.
- **Aluminium Smelter** — Electrolytic smelting facility for aluminium production, covering Hall–Héroult, carbothermal reduction, and molten-salt electrolysis processes. Off-Earth variants processing anorthositic feedstock co-produce oxygen.
- **Industrial Metals Smelter** — Smelting, solvent extraction, and electrorefining facility for base metals including copper, nickel, zinc, chromium, manganese, titanium, and others. Sulfide smelting generates sulfuric acid as a co-product.
- **Precious Metals Refinery** — Fire assay, chlorination, solvent extraction, and electrorefining facility for gold, silver, and platinum-group metals from ores, concentrates, and intermediate feeds such as anode slimes.
- **Rare Earths Refinery** — Acid or alkali digestion and multi-stage solvent extraction facility for separating rare-earth concentrates into individual oxide and metal products.
- **Uranium Processing Plant** — Leaching, solvent extraction, and fluorination facility for converting uranium ore through yellowcake to purified uranium hexafluoride at natural isotopic assay.
- **Regolith Processing Plant** — Bulk electrochemical or carbothermic reduction facility for extracting oxygen and metal–metalloid separates from unsorted regolith without prior ore beneficiation. Key early-stage ISRU installation on airless bodies and Mars.
- **Isotope Separation Plant** — Cryogenic distillation and electrolysis cascade facility for separating deuterium from natural-abundance hydrogen or water to produce fusion-grade fuel.
- **Recycling Center** — Municipal waste collection, mechanical and optical sorting, and material recovery facility with waste-to-energy incineration or plasma gasification for non-recyclable residual. Essential for mass-closure in space habitats.
- **Industrial Recycling Facility** — Mechanical shredding, pyrometallurgical smelting, and hydrometallurgical processing facility for end-of-life equipment, manufacturing scrap, spent catalysts, and mixed industrial waste. Essential for mass-closure in space habitats.
- **Nuclear Fuel Reprocessing Plant** — Aqueous or pyrochemical separation facility for recovering fissile material from irradiated uranium and thorium fuel assemblies and concentrating fission products into vitrified high-level waste.


## Conversions/Synthesis

- **Electrolysis Plant** — Electrochemical facility for splitting water into hydrogen and oxygen or reducing carbon dioxide to carbon monoxide and oxygen. PEM, alkaline, and solid-oxide cell types. Fundamental ISRU installation throughout the solar system.
- **Gasification Plant** — Thermal gasification facility converting coal, biomass, and other solid carbonaceous feedstocks to synthesis gas under controlled oxygen and steam injection.
- **Pyrolysis Facility** — Thermal decomposition facility operating under oxygen-excluded or oxygen-limited conditions. Produces coke, biochar, solid carbon, and co-product gases and liquids from coal, biomass, or methane.
- **Synthesis Plant** — Catalytic reactor facility for gas-phase chemical conversions including Sabatier methanation, methane reforming, water-gas shift, Fischer–Tropsch hydrocarbon synthesis, and methanol synthesis from carbon monoxide or carbon dioxide.
- **Ammonia Plant** — High-pressure Haber–Bosch catalytic synthesis facility producing ammonia from hydrogen and nitrogen. Foundational for fertilizer and industrial chemical supply chains.
- **Polymer Plant** — Polymerization reactor facility producing thermoplastics, thermosets, elastomers, and synthetic fibers from petrochemical, Fischer–Tropsch, or methanol-to-olefins feedstocks.
- **Chemical Plant** — Large-scale process facility for commodity chemical production including chlor-alkali electrolysis, sulfuric acid contact process, nitric acid oxidation, phosphoric acid digestion, and downstream inorganic intermediates.
- **Fine Chemicals Plant** — Controlled-environment process facility for specialty chemicals, catalysts, coatings, adhesives, electronic-grade reagents, and pharmaceutical intermediates requiring high-purity inputs and precise reaction control.
- **Concrete Plant** — Calcination kiln, clinker grinding mill, and batching facility for cement and concrete production. Off-Earth variants include sintered-regolith binder and sulfur-concrete processing.
- **Glass/Ceramics Plant** — Melting furnace and forming facility for flat glass, fiber glass, technical ceramics, and refractory products. Off-Earth, basaltic regolith serves as a near-complete feedstock.
- **Semiconductor Fab** — Ultra-clean crystal growth, wafer fabrication, and epitaxial deposition facility for silicon, gallium arsenide, silicon carbide, and compound semiconductor materials.
- **Propellant Plant** — Energetic materials mixing, casting, and curing facility for solid rocket propellant production with grain geometry design for thrust profiling.
- **Fertilizer Plant** — Compounding and granulation facility blending ammonia, phosphate, potash, sulfur-derived acids, and micronutrients into NPK and specialty fertilizer products.


## Manufacturing

- **Construction Yard** — On-site assembly, civil engineering, and installation capacity for fixed infrastructure including buildings, roads, tunnels, landing pads, launch facilities, power distribution networks, pipelines, pressurized habitats, and radiation shielding.
- **Vehicle/Spacecraft Assembly Yard** — Large-scale fabrication and final assembly facility for ground vehicles, aircraft, marine vessels, spacecraft, EVA mobility units, and spacesuits, integrating structural, propulsion, avionics, and life-support subsystems.
- **Metal Fabrication Works** — Cutting, welding, forming, machining, galvanizing, and assembly facility for steel and aluminium structural products including beams, plate, pipe, pressure vessels, extrusions, and prefabricated modules.
- **Composites Facility** — Fiber-reinforced polymer and metal-matrix composite fabrication and assembly facility for layup, filament winding, pultrusion, curing, and integration of composite materials into panels, overwrapped pressure vessels, fairings, and inflatable habitat shells.
- **Textiles Mill** — Weaving, knitting, and coating facility for high-performance fabrics including aramid, UHMWPE, carbon-fiber cloth, PTFE membranes, multilayer insulation blankets, filtration media, and spacesuit soft goods.
- **Heavy Manufacturing Plant** — Casting, forging, machining, winding, and assembly facility for turbines, engines, pumps, compressors, heat exchangers, generators, transformers, electric motors, and other heavy mechanical and electrical equipment.
- **Precision Manufacturing Plant** — Controlled-environment fabrication and assembly facility for scientific instruments, medical devices, CNC machine tools, lithography systems, optical systems, and navigation hardware requiring tight tolerances and specialty materials.
- **Electronics Factory** — Printed-circuit-board fabrication, semiconductor packaging, surface-mount assembly, display production, and system integration facility for general-purpose computing, communications, and sensor hardware.
- **Advanced Processor Fab** — Extreme-ultraviolet lithography, high-bandwidth memory stacking, and advanced-packaging facility for fabrication of high-performance tensor processors and neuromorphic computing hardware.
- **Robotics Factory** — Fabrication and integration facility for robotic manipulators, autonomous mobile platforms, and AI-hardware systems, combining structural, actuator, sensor, and advanced processor subsystems.
- **Solar Panel Factory** — Photovoltaic module production line covering silicon ingot slicing, cell fabrication or thin-film deposition, glass encapsulation, aluminium framing, and copper interconnect wiring.
- **Battery Factory** — Electrode coating, cell stacking, electrolyte filling, formation cycling, and battery-pack assembly facility for lithium-ion and successor solid-state chemistries.
- **Nuclear Fuel Fabrication Plant** — Uranium enrichment cascades, fuel pellet sintering, cladding tube loading, and fuel bundle assembly for low-enriched, high-enriched, and thorium-cycle reactor fuels. Produces depleted uranium hexafluoride tails as a by-product of enrichment.
- **Consumer Goods Factory** — Mass-production facility for furniture, clothing, housewares, paper products, personal care items, building finishes, personal electronics, and other consumer and specialty goods.


## Biological

- **Industrial Farm** — Large-scale mechanized farm for conventional crop cultivation and livestock raising on open land under ambient atmospheric conditions.
- **Controlled-Environment Farm** — Sealed pressurized greenhouse or habitat grow module for crop cultivation and animal husbandry with artificial or filtered lighting, active atmosphere management, water recycling, and nutrient delivery.
- **Artisanal Farm** — Small-scale family farm for artisanal and specialty food production including cheeses, preserves, craft beverages, and baked goods.
- **Artisanal Workshop** — Small-scale handcraft workshop for pottery, handmade textiles, woodcraft, leatherwork, and other artisanal non-food goods.
- **Commercial Fishing Fleet** — Marine and freshwater wild-capture fishing vessels with shore-side landing and processing infrastructure.
- **Aquaculture Farm** — Open-water marine or freshwater farming installation for fish, shellfish, and seaweed using net pens, longlines, or pond systems.
- **Controlled-Environment Aquaculture Facility** — Sealed recirculating aquaculture system for fish, shellfish, and algae production in isolated or off-Earth installations where open-water farming is unavailable.
- **Forestry Operation** — Managed tree plantation and harvest operation producing timber, pulp, and biomass feedstock under open atmospheric conditions.
- **Algal Cultivation Facility** — Photobioreactor and open-pond installation for microalgae and cyanobacteria cultivation. Produces food-grade biomass and industrial biofeedstock; co-produces oxygen via photosynthesis.
- **Bioreactor Facility** — Industrial bioreactor installation for cellular agriculture, ethanol fermentation, and general-purpose fermentation of enzymes, amino acids, organic acids, and biopolymers.
- **Food Processing Plant** — Cooking, preservation, and packaging facility for converting agricultural and aquacultural products into shelf-stable prepared meals and food products. Critical for space operations where fresh-food supply is intermittent.
- **Biogas Digester** — Anaerobic digestion installation converting organic waste, crop residues, and sewage sludge to methane-rich biogas and nutrient-dense digestate.
- **Biomass Conversion Plant** — Hydrothermal liquefaction, catalytic pyrolysis-upgrading, and transesterification facility converting biomass and biological lipids to liquid hydrocarbon fuels.
- **Pharmaceutical Plant** — Chemical synthesis, fermentation, purification, and formulation facility for drugs, vaccines, biologics, and medical compounds.
- **Soil Conditioning Facility** — Composting, amendment blending, pH adjustment, nutrient loading, and regolith remediation operation for preparing agricultural growth media. Critical ISRU step for establishing agriculture on extraterrestrial surfaces.
- **Wastewater Treatment Plant** — Biological, chemical, and membrane processing facility for sewage, greywater, and mixed organic waste, with water reclamation and nutrient recovery. Critical water- and mass-closure infrastructure for off-Earth habitats.
```

**Summary of key decisions and simplifications:**

The file contains 88 modules covering all 121 operations. The main many-to-many consolidations are: Combustion Power Plant (7 combustion power operations), Fuel Cell modules (3 fuel cell operations each across utility and distributed), Mine modules (11+ mining operations per stratum), Drilling Rig modules (gas and oil drilling per stratum), Steel Mill (iron ore smelting and three steelmaking routes), Synthesis Plant (7 catalytic gas-phase operations), Bioreactor Facility (cellular agriculture, ethanol fermentation, industrial fermentation), Nuclear Fuel Fabrication Plant (LEU, HEU, thorium), and Composites Facility (composite material production and composite structure assembly).

**Important omissions to flag for later passes:**

- Utility-scale energy storage infrastructure (pumped hydro, grid batteries, compressed air) has no dedicated module — it may need one if not handled at the systems level.
- Telecommunications and data infrastructure (data centers, ground stations, relay networks) has no dedicated module.
- Water treatment and distribution for potable supply (distinct from desalination and wastewater) is not explicitly modeled — it's presumably absorbed into Construction Yard.
- Orbital and deep-space construction (e.g., zero-g assembly platforms, dry docks) might warrant a separate module from surface Construction Yard.

**Suggestions for the context files:**

- A module-to-operation mapping table (TSV) would be valuable for making the many-to-many relationships explicit and machine-readable.
- Consider whether "conversion cost/speed" properties of modules should distinguish between shifting among operations within the same general technology (e.g., switching a mine's target ore — low cost) versus shifting between fundamentally different processes (e.g., converting a BOF steel mill to an EAF mill — high cost).
- The depth strata definitions for drilling rigs and mines are Earth-centric. It may be worth noting in the file header how these translate to other bodies (e.g., asteroid mining is effectively "surface" regardless of depth into the body).
