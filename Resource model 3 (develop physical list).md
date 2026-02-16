_Claude-Opus-4.6 (output effort = high)_

---
---

I need information for a simulation that focuses on resource streams: extraction, commodity trading, and downstream usage in industrial/economic activities. The simulation starts at present-day technology and operations on Earth, but runs forward in time to include highly speculative (yet physically plausible) future technologies and operations in the wider solar system. Abstractions and simplifications are necessary. However, please tell me if my instructions seem too oversimplified or have erroneous assumptions. Ask for clarification if it may help in subsequent queries. This is intended to be an iterative process. Summarize simplifications and important omissions in your analysis. State references and show your work.

The simulation has 6 broad resource classes:

- Energy
- Ores
- Volatiles
- Materials
- Manufactured
- Biological

I've included a list of 27 "extractable resources". All of these can be classified as energy, ores, or volatiles (they will make up the entirety or bulk of ores and volatiles). The class names aren't exclusive or exact. 

There are few resources I need to add for implementation reasons. These are "electricity" (top of the energy class), "LE uranium fuel", "HE uranium fuel", "steel structures", "aluminum structures", and "composite structures".

Please start by classifying the 27 extractable resources and the additional 6 above. We don't need a final classification now, so don't spend much effort on borderline cases.

We mainly want to focus on the latter 3 classes here (although additions to the first 3 are possible). Ultimately, we want roughly 10 to 15 items per class. I'll briefly describe the classes, followed by considerations and priorities.

- Energy. In addition to "electricity", include resources that are inputs for power generation or rocket propulsion (but exclude oxygen and non-energetic propellants like heavy noble gases). This class pulls from resources that could otherwise fit in classes below.
- Ores. Include solids from the extractable resources list, even ones that aren't strictly ores but fit best in that class (e.g., "stone" and "regolith").
- Volatiles. This includes volatiles from the extractable resources list and bulk synthesized liquids or gases.
- Materials. Include resources that are intermediates in industrial processes. These are outputs of refining or synthesis and (usually) inputs to subsequent refining, manufacturing, construction, or chemical synthesis. Examples might include "steel" or "fine chemicals". Also include byproducts of industrial processes that may have small or negative value but need to be accounted for; examples might include "industrial scrap", "slag/ash", or "radioactive waste", but try to keep the number of these as small as possible.
- Manufactured. Include more downstream, fabricated items. These may be end products or inputs to subsequent manufacturing. Examples might include "microcontrollers", "batteries", or "consumer goods".
- Biological. Include resources that are either products of, or primarily used in, agriculture or other biological processes. Examples might include "lumber", "fertilizers", or "pharmaceuticals". This class may include one or more unwanted byproducts (similar to materials), but keep these as few in number as possible.

Here are some considerations:

- Resource terms must be short and intuitive even at the expense of precision. Abbreviations are allowed if they are clear in context (e.g., "LE uranium fuel"). 
- We want commodities or commodity-like resources as much as possible. That might be challenging for manufactured (in particular) but it should be a consideration. Permanent constructions and infrastructure are handled elsewhere in the simulation, but we do need inputs for those activities here.
- Use catchall terms as needed, as we did for the "extractable resources" list.
- Take advantage of existing extractable resources. We don't need additional grades of oil. However, a catchall term "refined hydrocarbons" might be useful as both an energy resource and as input for other industrial processes.
- Prioritize based on total mass flow and/or economic value. "Concrete" and "pharmaceuticals" are examples on either end of that spectrum.
- Deprioritize resources used in only one application stream. An example might be "yellowcake". We can abstract that resource stream to "radioactive ores" + other stuff -> "LE uranium fuel". Prioritize resources that are used in multiple application streams.
- Consider both present-day Earth activities and a future space industry/economy that has advanced far enough to be independent of Earth.
- Try to be complete. This is an abstraction, but we want it to encompass all industrial and economic activity.

We don't want detailed descriptions of these new resources yet. For now, we want to focus on getting the abstractions right and filling out the lists. Don't worry too much about where to put resources that straddle the classes - we can sort that out later.

(FYI, there is a 7th class for "services". Don't worry about these here. I only mention it in case you are tempted to add intangibles due to their economic value.)

---
---

*Thinking...*

> The user wants me to classify the 27 extractable resources plus 6 additional resources into 6 classes, then flesh out the Materials, Manufactured, and Biological classes (and potentially add to Energy, Ores, Volatiles) to reach roughly 10-15 items per class.
> 
> Let me start by classifying the existing resources:
> 
> **27 Extractable Resources:**
> 
> Ores/Solids (13):
> 1. Iron/nickel
> 2. Iron ores
> 3. Aluminum ores
> 4. Industrial metal ores
> 5. Precious metal ores
> 6. Rare earth ores
> 
> 
> 7. Radioactive ores
> 8. Sulfur
> 9. Industrial minerals
> 10. Organics/tholins
> 11. Coal
> 12. Stone
> 13. Regolith
> 
> Volatiles (14):
> 14. Water
> 15. Hydrogen
> 16. Oxygen
> 17. Nitrogen
> 18. Carbon dioxide
> 19. Carbon monoxide
> 20. Ammonia
> 21. Methane
> 22. Ethane
> 23. Sulfur dioxide
> 24. Helium
> 25. Argon
> 26. Heavy noble gases
> 27. Oil
> 
> The 6 additional resources include electricity, uranium fuel, and heavy elements that I need to sort into the appropriate categories or create new ones for energy and specialized materials.
> 
> Now I'm working through the classification system, starting with energy resources. Electricity, uranium fuel variants, coal, and oil are clear energy sources, while hydrogen and methane can serve as energy inputs despite overlapping with other categories. Helium stays in volatiles since He-3 fusion is speculative and regular helium has broader industrial applications beyond energy. I'm realizing the classification isn't mutually exclusive in usage—each resource belongs to exactly one class but can serve multiple purposes across different processes. For Energy, I have eight core resources so far (electricity, uranium fuels, coal, oil, hydrogen, methane, and ethane), though ethane is borderline since it's primarily a petrochemical feedstock. I should consider whether to add refined hydrocarbons as the user suggested or explore other candidates to reach the target range of 10-15 resources in this category.
> 
> For Ores, I've identified twelve resources including iron/nickel, various ore types, sulfur, industrial minerals, organics, stone, and regolith—this feels complete for the initial pass. Volatiles also sits at ten with water, oxygen, nitrogen, carbon dioxide, carbon monoxide, ammonia, sulfur dioxide, helium, argon, and heavy noble gases, which gives me a solid foundation to build from.
> 
> Now I'm working through Materials as intermediate industrial products—the refined outputs that feed into manufacturing. I'm prioritizing by mass and economic value: steel and aluminum as structural metals, concrete as the largest mass flow, glass and ceramics, polymers, refined metals from smelting, precious metals, rare earth compounds, and semiconductors, aiming for around ten to fifteen categories.
> 
> I notice the user already has steel structures, aluminum structures, and composite structures listed as separate resources for implementation reasons, which suggests these belong in Manufactured rather than Materials—they're fabricated components, not raw refined materials. So I should keep Materials focused on the intermediate industrial products themselves, not their finished structural forms. I'm seeing that composites are already categorized as manufactured structures, so I should keep the Materials class focused on refined intermediates like steel and aluminum that then get fabricated into their corresponding structures. The flow makes sense: raw ores get refined into materials, which then get manufactured into finished structures.
> 
> For the Materials list, I'm settling on the core refined products—steel, aluminum, other metals, precious metals, concrete, glass, polymers, chemicals, rare earths, semiconductors, and composite materials themselves as the feedstock. Refined hydrocarbons fit better here as a material input for downstream processes rather than as energy, since the user noted they serve both purposes but classification doesn't need to be exclusive.
> 
> Now for Manufactured goods, I'm thinking about the downstream fabricated items like steel and composite structures, industrial machinery, engines, and electronics components—things that are assembled or processed from the raw materials rather than extracted or refined. I'm consolidating these into a more manageable set focused on what's actually needed for space simulation — power generation and storage, propulsion, computing, automation, and general machinery that can serve as inputs to construction and operations. The catch-all categories like vehicles and consumer goods can absorb the remainder without needing separate entries for every specific type. I'm reconsidering whether power systems like solar and nuclear are too heterogeneous to group together, and whether vehicles should include spacecraft or be separate. I'm also adding textiles as a category but need to decide if instruments warrant their own classification or should merge elsewhere.
> 
> The core question is what matters for the simulation—I need to think about what these manufactured goods are actually inputs to, like how structures and machinery feed into construction, while robots and electronics support multiple sectors like mining and manufacturing.
> 
> For biological resources, I'm looking at the outputs and inputs of agricultural and biological processes: fertilizers and soil amendments, food crops, animal products, animal feed, lumber and biomass, pharmaceuticals, and potentially biofuels or bioplastics if those make sense as distinct categories rather than folding into materials or energy.
> 
> I'm also considering space-specific biological needs like growth media for closed-loop agriculture, seeds and genetic stock for propagation, cultured proteins, and microbial products—though some of these might consolidate with existing categories. By mass, food crops dominate, but by value, pharmaceuticals and animal products rank higher, so I need to decide whether to weight the categorization by volume or economic importance. I keep cycling through different categorizations, so let me refocus on what actually matters for the simulation: the key resource flows from agriculture through to end products, plus the waste streams that cycle back. For a space context, I should emphasize closed-loop systems and what's actually harvestable or processable in that environment, rather than getting bogged down in present-day distinctions like cultured versus conventional proteins. I'm settling on a 10-item list that keeps food unified rather than splitting it, and includes soil/growth media as relevant for space agriculture even if it's lower volume on Earth. For the energy class, I need to distinguish between electricity as converted energy and primary fuel resources like uranium, then figure out what other energy forms belong in this category. I'm considering adding fusion fuel as a tenth category since it's physically plausible for the simulation's timeframe, though I'm uncertain whether to treat helium-3 separately or keep helium in the Volatiles category given that bulk helium is primarily industrial. The user's target of 10-15 resources per class suggests I should aim for around 10 in Energy, which fusion fuel would achieve.
> 
> For Materials, I'm working through the major categories by production volume—steel, concrete, aluminum, refined metals, and polymers are the dominant ones—but I need to decide whether concrete counts as a raw material or a manufactured product before finalizing the list. Concrete makes sense as a Material since it's an intermediate input for construction, similar to how steel functions. The user already distinguishes between materials and manufactured goods (like steel structures), so concrete would be a material input while prefabricated structural elements would be manufactured products. This aligns with their note that permanent infrastructure is handled elsewhere, but material inputs for those activities belong here.
> 
> Now I'm working through a comprehensive materials list that includes metals, rare earths, semiconductors, concrete, glass, polymers, composites, chemicals, hydrocarbons, and industrial waste — about 15 categories total. I'm considering whether some of these could merge together, like combining composites with polymers or consolidating glass and ceramics, to avoid the list becoming unwieldy. I'm moving refined hydrocarbons to Energy since most are burned as fuel, even though they also serve as chemical feedstock—the user expects some cross-class dependencies anyway. I'll keep composites separate from polymers because they're critical for lightweight space structures, bringing Materials down to 13 items and Energy up to 10. For edge cases like explosives, paints, and textiles, I'm folding them into existing categories since they're either low-priority or better suited to other classes.
> 
> Now I'm thinking through Manufactured goods—the simulation needs commodity groupings for building cities, factories, and space infrastructure, so I'm organizing around structures, machinery, electronics, and specialized systems like life support and propulsion.
> 
> I could keep all 13 categories or consolidate by merging electrical equipment into machinery, instruments into electronics, and textiles into consumer goods, though vehicles probably shouldn't split further into ground versus spacecraft variants.
> 
> Looking at actual manufacturing by value globally—motor vehicles, electronics, food processing, petroleum, chemicals, machinery, metals, plastics, aerospace, electrical equipment, fabricated metals, furniture, textiles, paper, and pharmaceuticals—I realize the user already has structures covered, so I need to focus on what's missing. For a simulation spanning both Earth and space economies, I should prioritize by mass flow and economic value: structural materials, solar arrays and power systems (critical for space), propulsion, life support, electronics, and vehicles. That suggests keeping solar panels separate from general electrical equipment given their dominance in space, though they could merge if I'm being lean. I'll settle on around 10-13 categories with solar panels grouped into electrical equipment but noted as significant.
> 
> For the extractable resources, I need to classify them functionally into Energy, Ores, and Volatiles. Hydrogen and methane belong in Energy since they're primarily valued as fuels and propellants, even though they're chemically volatile. Ethane follows the same logic. This leaves coal and oil in Energy, the metal ores and sulfur and organics in Ores, and the remaining gases—water, oxygen, nitrogen, carbon dioxide, ammonia, helium, and others—in Volatiles. That gives me roughly 5 items in Energy (plus electricity and uranium variants), 12 in Ores, and 10 in Volatiles, which feels balanced.
> 
> For Energy, I should add refined hydrocarbons as a processed output of oil refining—gasoline, diesel, kerosene—since about 85% of petroleum products are used as fuel. I could also include fusion fuel as a speculative but in-scope addition. That brings Energy to around 10 items total, which is reasonable. I'm reconsidering whether refined hydrocarbons belongs in Materials rather than Energy—the user's phrasing suggests it could work either way, but thinking about the processing chain, it makes more sense as a Materials output (refined from crude oil, then used as fuel or feedstock), similar to how uranium fuel is categorized as an Energy resource after refining from ores. Fusion fuel deserves its own category since the enrichment process for deuterium and tritium is significant enough to distinguish it from raw hydrogen or helium, similar to how uranium enrichment creates separate fuel types. I'm finalizing the Energy category with ten entries: electricity, fission fuels (LE and HE uranium), fusion fuel, coal, oil, hydrogen, methane, and ethane.
> 
> For the remaining resource categories, I'm organizing ores into twelve types covering metals, rare earths, radioactive materials, and industrial minerals. Volatiles include ten atmospheric and cryogenic gases. Materials span fourteen categories from basic metals and refined compounds to semiconductors, polymers, composites, and specialty chemicals. I'm consolidating waste streams into just two categories—industrial waste covering slag, tailings, and scrap, plus radioactive waste for spent fuel and contaminated materials—to keep byproducts minimal as requested. Materials waste comes from downstream processing, so I'm settling on a final list of 14 material types that covers the main categories: metals (steel, aluminum, refined, precious, rare earth), semiconductors, construction materials (concrete, glass/ceramics), polymers, composites, chemicals (bulk and fine), industrial scrap, and radioactive waste. Petrochemicals are intermediate products that fit implicitly within the bulk chemicals and polymers categories rather than needing separate tracking.
> 
> Now I'm moving to the Manufactured category with 12 items: structural materials (steel, aluminum, composite structures), machinery, electronics, electrical equipment, batteries, vehicles, robots, instruments, consumer goods, and textiles. I'm reconsidering whether some of these categories need adjustment.
> 
> Instruments should stay separate from electronics since they include non-electronic precision items and represent a distinct economic category in simulation. I'm skipping weapons/defense since the user hasn't mentioned it, and spare parts are already covered by existing categories. Packaging has low value and can fold into consumer goods or polymers. Construction components like pipes and fasteners can integrate into steel structures or machinery. I'm questioning whether electrical equipment needs to be split further, but I think 12 categories strikes the right balance. Food processing is part of machinery, so I don't need a separate category. For the biological section, I'm consolidating livestock and aquaculture into a single "animal products" category since they overlap significantly.
> 
> I'm reconsidering the overall structure—splitting food into crop and animal products gets me closer to the 10-15 range, and I should add algal products as a distinct category since algae serves multiple functions in space environments like oxygen production and food. I might also separate animal feed from human food, or consider seeds and cultures as their own category, though I need to be careful not to over-segment. Animal feed is a massive flow on Earth but at this level it's just crop products being redirected, so I don't need a separate category. Similarly, processed food like frozen meals could go in either section, but it makes more sense to keep food production unified in Biological rather than splitting it across categories. The user's definition of Biological resources already covers both agricultural products and their uses, so raw and minimally processed forms fit naturally there.
> 
> For agricultural chemicals, fertilizers have such a dominant mass flow that they deserve their own entry in Biological, while pesticides and herbicides—being specialty chemicals—fit better under fine chemicals in the Materials section.
> 
> On biomass versus lumber versus natural fibers, I'm distinguishing between them: lumber as a major Earth commodity for structural wood, natural fibers like cotton and hemp as moderate-volume inputs, and biomass as the broader category of raw plant material for energy or feedstock, though there's overlap since lumber is technically biomass from trees.
> 
> Now I'm checking whether I've covered the major economic sectors—agriculture, mining, manufacturing, construction, energy, transportation, communication, healthcare, and space industry all map to my resource categories. Water treatment and atmospheric processing are implicit in the Volatiles category since they're processes rather than separate resources, and radiation shielding inputs are already accounted for through water, regolith, and materials. I think the framework is complete, so I'm ready to finalize the response with a full accounting of the resource lists. Ethane is borderline as a fuel versus feedstock, but I'll keep it in Energy since the user grouped it with methane and it could fuel operations on Titan. Refined hydrocarbons are primarily fuels on Earth despite being refining outputs, so Energy is appropriate. Fusion fuel is speculative but fits the simulation's scope of plausible future tech, paralleling uranium fuel categories. Algal and microbial products need reconsideration for their category placement. Biomass serves multiple purposes—energy, materials, and feedstock—so it belongs in Biological. Industrial waste should be a single catch-all category for byproducts like slag, ash, and scrap rather than splitting them up, keeping the list manageable. For the Materials section, I'm settling on 14 entries: the productive resources (steel, aluminum, refined and precious metals, rare earths, semiconductors, concrete, glass, polymers, composites, and chemicals) plus industrial waste and radioactive waste as the two waste streams. Nuclear fuel intermediates like yellowcake don't need their own entry since they're too specialized. I'm flagging key simplifications like each resource appearing in only one class despite multiple uses, and broad categories like "refined metals" and "bulk chemicals" that aggregate diverse materials. I'm also noting important omissions—no explicit construction resources, propellants, software, or life support equipment—and some borderline classification decisions like placing hydrogen and refined hydrocarbons in Energy rather than elsewhere, with biochemicals still needing resolution between Biological and Materials. I'm considering whether processed forms like breathable air or potable water should be separate resources, but they're really just refined versions of existing volatiles—the processing itself is the transformation, not a new resource category.
> 
> Similarly, liquid oxygen is just oxygen in a different phase for storage purposes, and deuterium extraction is part of the fusion fuel production chain rather than a standalone volatile. I'm satisfied with the first three resource classes now and moving into the new categories: refined metals and alloys like steel, aluminum, and precious metals; processed inorganics including semiconductors and various concrete types. Finishing the biological section with biomass feedstocks, growth media, and organic waste streams. Then reviewing coverage gaps—most categories like cement, coatings, adhesives, and wiring are already accounted for under existing classifications, though I'm deliberately excluding ammunition and explosives.
> 
> Checking the remaining specialized materials: uranium fuel rods, heat exchangers, filters, 3D printing feedstock, magnets, optical components, bearings, and propellant tanks all map cleanly to existing categories, so the framework feels comprehensive. The one potential issue is overlap between natural fibers in the biological section and textiles in manufactured goods, since the supply chain flows from raw fibers through to finished textile products. Biofuels represent a process pathway where biomass gets converted into energy products, and the framework handles electrolysis cleanly—water and electricity produce hydrogen or oxygen depending on the output needed. The "Consumer goods" category functions as a massive sink for resources across the supply chain, which is appropriate for the simulation's abstraction level. I'm also considering space-specific manufactured items like habitats, airlocks, and radiation shielding as potential edge cases that need categorization.
> 
> These composite products break down into their constituent resources—space suits combine textiles, composites, and electronics—so the existing production processes handle them without needing separate entries. In-situ manufacturing and 3D printing are covered by treating feedstocks like metal powder and polymer filament as existing resources, with the printers themselves classified as machinery. Nuclear thermal rockets fit naturally since they use hydrogen propellant and uranium fuel, and concepts like solar sails are infrastructure rather than resources. The framework seems robust enough to handle these cases, so I'm ready to write up the response in an organized but conversational way.
> 
> I'm reconsidering whether fusion fuel deserves its own resource category. It's physically plausible and fits the simulation's scope for speculative future tech, plus it has distinct processing requirements like deuterium separation and tritium breeding that parallel fission fuel. But it's not yet commercialized, and I could model it as a derivative of water through processing rather than a standalone resource. Given the added complexity for something potentially irrelevant in the near term, I'm leaning toward keeping it simpler unless the user specifically wants it included.
> 
> For now I'll move forward with the full resource taxonomy, organizing everything into classifications and flagging fusion as speculative if I do include it. I'm planning to present this as a structured breakdown showing the existing 33 resources, proposed additions by category, borderline cases, and simplifications, so the user can see the full picture and decide what to keep or modify. I'm ready to write out the response now.

## Classification of the 33 Existing Resources

Here's how I'd distribute the 27 extractable resources plus your 6 additions across the first three classes. I then propose lists for Materials, Manufactured, and Biological.

**Energy (10)** — Five extractables pulled in as fuels/propellants, plus the three given additions and two new entries.

| # | Resource | Source |
|---|----------|--------|
| 1 | Electricity | given addition |
| 2 | LE uranium fuel | given addition |
| 3 | HE uranium fuel | given addition |
| 4 | Coal | extractable |
| 5 | Oil | extractable |
| 6 | Hydrogen | extractable (from Volatiles) |
| 7 | Methane | extractable (from Volatiles) |
| 8 | Ethane | extractable (from Volatiles) |
| 9 | Refined hydrocarbons | **new** |
| 10 | Fusion fuel | **new** |

**Ores (12)** — All 12 extractable solids, unchanged.

| # | Resource |
|---|----------|
| 1 | Iron/nickel |
| 2 | Iron ores |
| 3 | Aluminum ores |
| 4 | Industrial metal ores |
| 5 | Precious metal ores |
| 6 | Rare earth ores |
| 7 | Radioactive ores |
| 8 | Sulfur |
| 9 | Industrial minerals |
| 10 | Organics/tholins |
| 11 | Stone |
| 12 | Regolith |

**Volatiles (10)** — The remaining 10 extractable gases/liquids.

| # | Resource |
|---|----------|
| 1 | Water |
| 2 | Oxygen |
| 3 | Nitrogen |
| 4 | Carbon dioxide |
| 5 | Carbon monoxide |
| 6 | Ammonia |
| 7 | Sulfur dioxide |
| 8 | Helium |
| 9 | Argon |
| 10 | Heavy noble gases |

---

## Proposed New Classes

### Materials (14)

Refining and synthesis outputs that feed downstream manufacturing, construction, or chemical processes.

| # | Resource | Scope |
|---|----------|-------|
| 1 | Steel | Carbon steel, alloy steel, stainless steel |
| 2 | Aluminum | Refined Al metal and alloys |
| 3 | Refined metals | Cu, Ti, Zn, Ni, Sn, Pb, W, Mo, Co, Mn, Cr, etc. |
| 4 | Precious metals | Au, Ag, Pt, Pd, Rh, Ru, Ir, Os |
| 5 | Rare earth compounds | REE oxides, metals, alloys, permanent-magnet stock |
| 6 | Semiconductor materials | High-purity Si, GaAs, SiC, compound semiconductors |
| 7 | Concrete | Portland cement, geopolymer, sintered-regolith binder |
| 8 | Glass/ceramics | Flat glass, fiber glass, refractory and technical ceramics |
| 9 | Polymers | Thermoplastics, thermosets, elastomers, synthetic fibers |
| 10 | Composites | CFRP, GFRP, metal-matrix composites, aramid laminates |
| 11 | Bulk chemicals | Commodity chemicals: acids, bases, oxidizers, solvents, industrial gases (Cl₂, HF, etc.) |
| 12 | Fine chemicals | Specialty chemicals, catalysts, coatings, adhesives, electronic-grade reagents |
| 13 | Industrial waste | Scrap metal, slag, fly ash, spent catalysts, recyclable mixed waste |
| 14 | Radioactive waste | Spent fission fuel, activated structural material, contaminated waste |

### Manufactured (12)

Fabricated goods — end products or inputs to further assembly, construction, or operations.

| # | Resource | Scope |
|---|----------|-------|
| 1 | Steel structures | Beams, plates, pressure vessels, pipe, prefab modules |
| 2 | Aluminum structures | Sheet, extrusions, pressure shells, lightweight frames |
| 3 | Composite structures | CFRP/GFRP panels, overwrapped tanks, fairings |
| 4 | Machinery | Engines, turbines, pumps, compressors, tooling, mining and process equipment |
| 5 | Electronics | Computers, microcontrollers, sensors, telecom, navigation, displays |
| 6 | Electrical equipment | Motors, generators, wiring, transformers, solar cells, power conditioning |
| 7 | Batteries | Batteries, fuel cells, supercapacitors, other energy storage |
| 8 | Vehicles | Ground, marine, air, and space vehicles (incl. propulsion assemblies) |
| 9 | Robots | Robotic manipulators, autonomous systems, AI-hardware platforms |
| 10 | Instruments | Scientific, medical, metrology, and navigation instruments |
| 11 | Consumer goods | Appliances, tools, furniture, packaging, miscellaneous household items |
| 12 | Textiles | Clothing, technical fabrics, protective/pressure suits |

### Biological (11)

Products of, or primary inputs to, agriculture, aquaculture, forestry, and other biological processes.

| # | Resource | Scope |
|---|----------|-------|
| 1 | Fertilizers | NPK compounds, micronutrient mixes, soil amendments |
| 2 | Crop products | Cereals, legumes, oilseeds, vegetables, fruits, sugar, spices |
| 3 | Animal products | Meat, dairy, eggs, fish, leather, wool, cultured proteins |
| 4 | Algal/microbial products | Algal biomass, single-cell protein, microbial feedstocks |
| 5 | Lumber | Timber, engineered wood products, pulp/paper |
| 6 | Natural fibers | Cotton, hemp, jute, natural rubber |
| 7 | Pharmaceuticals | Drugs, vaccines, biologics, medical compounds |
| 8 | Biochemicals | Industrial enzymes, amino acids, biopolymers, fermentation products |
| 9 | Biomass | Raw plant/microbial mass used as energy or industrial feedstock |
| 10 | Soil/growth media | Terrestrial soil, compost, hydroponic substrates, amended regolith |
| 11 | Biowaste | Organic waste, sewage sludge, crop residue |

---

## Rationale for the Two New Energy Entries

**Refined hydrocarbons** sits between Oil (extractable crude) and downstream consumers. On present-day Earth roughly 85% of refined product mass is burned as fuel and about 15% serves as petrochemical feedstock (naphtha → polymers, solvents, etc.). I placed it in Energy because most mass flows to combustion, but it could equally live in Materials; the cross-class feedstock flow works either way.

**Fusion fuel** covers deuterium-tritium blends and, speculatively, deuterium-He3. The processing chain (deuterium separation from water, tritium breeding in lithium blankets, He-3 extraction from lunar regolith or gas-giant atmospheres) is distinct enough from the hydrogen entry to warrant its own slot. It parallels how LE/HE uranium fuel abstracts the fission fuel cycle. This entry is only relevant to the simulation's future-tech phase.

---

## Borderline Cases

These are the placements I'm least confident about — all workable for now, worth revisiting once process chains are defined.

**Hydrogen, methane, ethane → Energy.** All three are important Volatiles by extraction context (gas-giant atmospheres, Titan lakes, cometary ices) and important chemical feedstocks (Haber process, steam reforming, ethylene cracking). I placed them in Energy because the simulation's Energy class is defined to pull in fuels and propellants, and by mass flow on Earth most H₂ and CH₄ are burned. If you'd prefer to keep the Volatiles list intact and let Energy consist only of the non-extractable entries, that's coherent too — the tradeoff is a smaller Energy class (~5 items) and a larger Volatiles class (~15).

**Refined hydrocarbons → Energy vs Materials.** Discussed above.

**Ethane → Energy.** Ethane is the dominant feedstock for ethylene production and only a minor fuel. It's the weakest of the three Volatile-to-Energy moves. Could stay in Volatiles.

**Composites → Materials.** The material (e.g., prepreg carbon-fiber fabric) lives here; the fabricated product (composite structures) lives in Manufactured. This is clean, but "composites" could arguably merge with "polymers" since the matrix is almost always a polymer.

**Textiles → Manufactured vs Biological.** Textiles are fabricated goods whose inputs come from both Biological (natural fibers, wool) and Materials (polymers / synthetic fibers). Placed in Manufactured as the fabrication step.

**Biochemicals → Biological vs Materials.** Many biochemicals (e.g., industrial enzymes, citric acid) are produced by fermentation but consumed in non-biological industrial processes. Placed in Biological as outputs of biological processes, but "Fine chemicals" in Materials could absorb them.

**Algal/microbial products → Biological.** Negligible present-day mass flow but critical for closed-loop space life support (food, O₂ generation, CO₂ fixation, waste recycling). Included for forward-looking relevance.

---

## Simplifications and Important Omissions

**Within-entry heterogeneity.** Several entries aggregate dozens of distinct commodities with different markets and properties. "Refined metals" merges copper, titanium, zinc, nickel, tungsten, and others that differ in price by two orders of magnitude. "Bulk chemicals" lumps sulfuric acid, sodium hydroxide, chlorine, and ammonia-derived products into one slot. "Consumer goods" is effectively everything not captured elsewhere. This level of aggregation is necessary for a ~70-item resource system, but it means that any process consuming "refined metals" will need to carry implicit assumptions about the sub-mix (or you'll want a composition/sub-allocation system analogous to what you already built for extractable resources).

**No explicit propellant category.** Rocket propulsion draws from multiple classes: hydrogen and methane (Energy), oxygen (Volatiles), heavy noble gases (Volatiles, for ion drives). There is no unified "propellant" entry — propellant demands are composed from existing resources by the propulsion process. This seems fine.

**Agricultural chemicals beyond fertilizers.** Pesticides, herbicides, fungicides, and growth regulators are economically significant on Earth but low mass relative to fertilizers. They're implicitly covered by "fine chemicals" (Materials). In a space context they're largely irrelevant (controlled-environment agriculture). If you later want an integrated agricultural input, "fertilizers" could be broadened to "agricultural chemicals."

**Construction as an activity vs. a resource.** Concrete (Materials) and the three structure types (Manufactured) are construction inputs, but there's no intermediate "prefab habitat module" or "road/rail" resource. I'm assuming the simulation's infrastructure system handles assembly of these inputs into permanent structures. Worth confirming.

**No explicit water-quality distinction.** Potable water, ultra-pure process water, and irrigation water are all "water." Treatment is a process, not a separate resource. This is consistent with how you've handled other processing distinctions.

**Earth-only resources.** Coal, oil, lumber, and natural fibers have no plausible extraterrestrial source. They'll be irrelevant once the simulation moves to a space-only context. Including them is correct for Earth-era completeness; the simulation can zero out their production off-Earth.

**Biofuels.** Ethanol, biodiesel, and biogas are outputs of biological processes used as fuels. They span Biological and Energy. At this abstraction level they'd be produced from biomass (Biological) and either consumed directly or routed through refined hydrocarbons (Energy) or bulk chemicals (Materials). No separate entry seems needed, but the process chain needs to exist.

**Defense and security goods.** Not explicitly represented. Weapons, munitions, and military vehicles would draw from the same upstream resources (steel, electronics, bulk chemicals, etc.) and could be folded into existing Manufactured entries or handled by the services layer.

**Waste heat and entropy.** The simulation presumably tracks energy balances elsewhere. Waste heat is not a resource entry.

---

## Summary Table

| Class | Count | Extractable | Given additions | New |
|-------|-------|-------------|-----------------|-----|
| Energy | 10 | 5 | 3 | 2 |
| Ores | 12 | 12 | 0 | 0 |
| Volatiles | 10 | 10 | 0 | 0 |
| Materials | 14 | 0 | 0 | 14 |
| Manufactured | 12 | 0 | 3 | 9 |
| Biological | 11 | 0 | 0 | 11 |
| **Total** | **69** | **27** | **6** | **36** |

Let me know which borderline calls you'd like to change, whether any entries should be split or merged, and whether the abstraction level feels right before we move on to descriptions or process chains.

---
---

I want to explore potential items that perhaps deserve to be broken out of the catchall categories based on any of the listed criteria (high mass flow, high value, use in multiple application streams, etc.). Does ethanol deserve to be broken out of bulk chemicals? Please take the more heterogeneous catchall items and consider what might be broken out based on various criteria. Don't restate the whole list here.

---
---

*Thinking...*

> The user wants me to analyze the catchall categories and consider what might deserve to be broken out as separate items. Let me think through the most heterogeneous catchalls systematically.
> 
> The main catchall categories are:
> 
> **Materials:**
> - Refined metals (Cu, Ti, Zn, Ni, Sn, Pb, W, Mo, Co, Mn, Cr...)
> - Bulk chemicals (acids, bases, oxidizers, solvents, industrial gases)
> - Fine chemicals (specialty chemicals, catalysts, coatings, adhesives, electronic-grade reagents)
> - Industrial waste (scrap, slag, fly ash, spent catalysts, recyclable mixed waste)
> 
> 
> - Manufactured goods span machinery, electronics, electrical equipment, vehicles, and consumer products, while biological materials include crops and animal-derived products.
> 
> Copper stands out as a breakout category given its massive production volume, critical role across nearly every application stream from electronics to construction, and distinct supply chain from mining through recycling. Titanium is worth considering too for its aerospace and space relevance, though at much lower volumes. For bulk chemicals, sulfuric acid dominates by far at 260 Mt/yr as the largest industrial chemical, while nitrogen-based compounds like nitric acid and urea represent significant downstream products from ammonia, and ethylene/propylene serve as fundamental polymer precursors—though these last two might be implicitly covered under polymers. I'm reconsidering whether ethanol warrants its own category since most of it functions as fuel within existing biomass or energy pathways, and as a solvent it's not uniquely critical compared to alternatives. For breakout candidates, copper stands out as the third-largest metal by mass with universal manufacturing applications and a distinct supply chain, while titanium, though lower in volume, becomes strategically important for space infrastructure and would gain prominence in a space-focused economy.
> 
> On implementation, I realize that breaking out copper as a material doesn't require a separate extractable resource since the industrial metal ores already include elemental compositions that the simulation can track and route appropriately. Solar panels deserve serious consideration as a breakout category from electrical equipment—they're arguably the single most important manufactured good for space infrastructure by both mass and value, and their semiconductor and glass components make them distinct enough to warrant separate analysis.
> 
> For life support equipment, while it's critical in space operations, the mass and value don't justify breaking it out from machinery compared to something like copper, which flows through multiple application streams at massive scale. Reconsidering against the user's criteria of high mass flow and cross-sector use, copper is the strongest candidate for a refined metals breakout at 25 million tons annually and $230 billion in value, while titanium metal and nickel are too niche or already captured through other categories. Sulfuric acid and other bulk chemicals are worth examining similarly for their production volumes and industrial reach. Consumer goods is inherently a catchall for final products, so breaking things out wouldn't add clarity. Machinery has some distinct subcategories like engines and HVAC systems, but they're already captured elsewhere or too integrated into their end uses. Electronics feels fairly cohesive as a category, though telecom equipment could theoretically stand alone. Vehicles are already somewhat non-commodity, and breaking out spacecraft specifically would mainly help a space simulation. Crop products all flow from similar agricultural processes despite their differences. The strongest breakout candidate I'm seeing is copper from electrical equipment, given its massive material flows and distinct supply chain. Paper and packaging are massive on Earth but nearly irrelevant for space applications where digital displays and polymers would replace them. Paints and coatings have moderate relevance due to thermal coatings needed in space, but the mass flow is modest enough to stay within fine chemicals. I'm considering explosives more carefully since they're key mining consumables, though space might use different rock-breaking methods, and I'm ruling out lower-mass items like abrasives, optical fiber infrastructure, magnets as REE byproducts, and additive manufacturing feedstock since they're either too niche or already captured elsewhere.
> 
> For a mature space economy, the dominant mass flows would be propellants, structural materials, radiation shielding, life support consumables, power systems, and thermal management—all of which I've already identified—so there's no compelling space-specific category I'm missing. Ethanol production is substantial at roughly 87 million tons annually, mostly as fuel, with potential as a biofuel in space contexts, though this doesn't seem like a strong independent breakout either.
> 
> Looking across the catchall categories, I should evaluate which ones might contain candidates worth separating out. Refined metals has copper as a standout, but the others—bulk chemicals, fine chemicals, consumer goods, machinery, electronics, electrical equipment, vehicles, crop products, and animal products—don't seem to have obvious breakout candidates that would meaningfully improve the framework.
> 
> I'm also reconsidering whether processed food deserves its own category, since the user distinguishes raw crop and animal products but the actual industrial activity centers on processing and preservation—a massive sector by both mass and value. Though at this abstraction level, that processing might be implicit within the biological sector rather than needing explicit separation.
> 
> Water treatment chemicals like coagulants and disinfectants are critical for life support but low in mass, so they fit adequately within bulk and fine chemicals. Optical components from glass and semiconductors could theoretically be broken out, but they're better left as part of instruments or electronics given the current abstraction.
> 
> My strongest recommendation is to separate copper from the refined metals category, since it's economically and functionally distinct enough to warrant its own line item.
> 
> For moderate consideration: titanium might deserve separation for space relevance, though the case is weaker than copper; processed food represents an implicit gap worth noting; and lithium, despite its strategic importance for batteries, has such tiny mass flows that breaking it out seems premature unless battery chemistry becomes a major simulation focus.
> 
> Silicon in various grades is already covered implicitly through semiconductor materials and raw regolith, so separating it out adds little value. Cement as a distinct category from concrete is worth considering but probably unnecessary at this level of detail.
> 
> My final assessment: copper is the only clear candidate for breaking out from refined metals due to its massive scale and diverse applications across electronics, infrastructure, and manufacturing. Ethanol doesn't warrant separation from bulk chemicals, and the heterogeneity within refined metals—while real—doesn't justify further fragmentation beyond copper. Methanol's 100+ Mt/yr production makes it substantial, though its relevance depends on context—it's a multi-stream chemical with applications in formaldehyde, fuels, and olefins, but less critical without fossil fuel infrastructure. Lime at 400 Mt/yr is massive and genuinely multi-stream across steelmaking, construction, water treatment, and agriculture, though it's just a simple calcination product from limestone, so it's borderline whether to break it out separately. For Machinery, I'm considering what subcategories might warrant their own treatment—engines, turbines, pumps, and specialized equipment like mining or process machinery all have distinct supply chains and applications. Food crops versus industrial crops are worth distinguishing, but the user already separates natural fibers and industrial oils are minor enough to leave in crop products; animal feed is massive in volume but just redirected crop products so it doesn't warrant its own category. For the final answer, I'll confirm ethanol doesn't break out, discuss copper as a clear standout from refined metals, note that bulk and fine chemicals don't have compelling breakout candidates, and explain why cross-cutting items like pipes, wire, and fasteners are already covered by existing categories—though I should mention nuclear reactor components as a specialized manufactured good that straddles energy infrastructure and materials in case the user wants to consider it.

## Ethanol

No, I don't think so. Global production is ~90 Mt/yr, which sounds large, but roughly 70% is fuel ethanol that could be modeled as a biomass → refined hydrocarbons pathway without an explicit intermediate. The remaining industrial-solvent and beverage uses are comfortably absorbed by bulk chemicals and crop products respectively. In a space context ethanol has no special role — it's one of many possible solvents and fuels synthesizable from CO + H₂ or fermentation. It fails the multi-stream test at this abstraction level because its major uses already map cleanly to existing entries.

## Refined Metals

This is the most heterogeneous catchall by far and the one where a breakout is most clearly warranted.

**Copper** is the standout. About 25 Mt/yr production, market value north of $200B/yr, and it feeds into nearly every Manufactured category: wiring and motors (electrical equipment), circuit boards (electronics), heat exchangers (machinery), plumbing and fittings (structures), and coinage/decorative use (consumer goods). No other metal in the "refined metals" bag has that breadth. Most of the rest — zinc, manganese, chromium, lead — feed primarily into a single downstream stream (galvanizing, steelmaking, batteries) and are well-served by the catchall. Nickel and cobalt have grown in importance via batteries, but their mass flows are an order of magnitude below copper and their multi-stream character is weaker. Titanium is interesting for space (lightweight, corrosion-resistant, high melting point) but at ~0.2 Mt/yr of metal it's low-mass even by value and its applications are more specialized than broad.

**Recommendation:** break copper out as its own Materials entry. The rest of "refined metals" stays coherent as a production-weighted basket dominated by Zn, Mn, Cr, Ni, and Pb.

## Bulk Chemicals

I looked hard at this one and nothing clears the bar convincingly.

**Sulfuric acid** (~260 Mt/yr) is the single largest chemical commodity by mass, feeding fertilizer production, ore leaching, and petroleum refining. But roughly 60% goes to phosphate fertilizer — essentially a single stream — and most of the rest is consumed on-site as a process intermediate rather than traded. In a space context it would be synthesized locally from sulfur and water with no distinct supply chain to track.

**Lime** (CaO, ~400 Mt/yr) is enormous by mass and genuinely multi-stream: steelmaking flux, cement precursor, water treatment, soil amendment, chemical feedstock. But it's a one-step calcination product of limestone (already in industrial minerals) and conceptually lives at the same level as "steel" or "concrete" — a simple refined intermediate. The case is moderate; I'd flag it for possible inclusion if you find the simulation needs an explicit calcium-chemistry node, but I wouldn't push for it now.

**Methanol** (~110 Mt/yr) is large and versatile — formaldehyde, fuel blending, olefin feedstock, solvent — but in a space economy it's just one of many small molecules synthesizable from CO + H₂, and its downstream products mostly flow into polymers or refined hydrocarbons.

**Recommendation:** no breakout. Bulk chemicals remains a viable catchall.

## Fine Chemicals

More homogeneous than it looks. The items here (catalysts, coatings, adhesives, electronic-grade reagents, specialty solvents) share the characteristic of being low-mass, high-value, and process-specific. Nothing inside has enough mass flow or breadth of use to justify a separate slot. Pesticides/herbicides are the largest single subgroup (~5 Mt/yr on Earth) but feed a single stream (agriculture) and are largely irrelevant in controlled-environment space farming. This category is fine as-is.

## Machinery

Broad but functionally cohesive — everything here is "a mechanical device that does work." The subgroups (engines/turbines, pumps, mining equipment, HVAC, agricultural equipment, construction equipment) are diverse in application but similar in how they're manufactured and in what inputs they consume (steel, refined metals, electronics, electrical equipment). No subgroup has the multi-stream or mass/value profile to break out. Engines and turbines are the most tempting, but they're primarily consumed by vehicles (already a separate entry) and power-generation infrastructure.

## Electronics and Electrical Equipment

These two categories are already the result of a split (they'd be one category in many classification schemes). Within electronics, semiconductors/chips could theoretically separate, but they're the defining core of the category rather than a distinct commodity stream. Within electrical equipment, **solar panels** are worth flagging: they're the dominant power source for space infrastructure, have a distinct manufacturing process (semiconductor materials + glass + aluminum), and represent a $200B+/yr Earth market growing rapidly. But the user noted that permanent installations and infrastructure are handled elsewhere, and solar panels are almost always installed as infrastructure rather than consumed as inputs. If the simulation needs to explicitly track solar-panel manufacturing as a bottleneck for space development, breaking them out would make sense. Otherwise they sit comfortably in electrical equipment.

## Manufactured Catchalls (Vehicles, Consumer Goods)

Vehicles is heterogeneous (cars vs. cargo ships vs. spacecraft) but all subcategories consume the same upstream inputs and the category is defined by function. Consumer goods is the ultimate sink and deliberately so. Neither benefits from breakout.

## Biological

**Crop products** is broad (cereals, oilseeds, vegetables, fruit, sugar) but the subcategories all come from the same process (agriculture) and feed the same downstream uses (human food, animal feed, food processing). **Animal products** is similar. No breakout needed in either.

One gap worth noting: there's no explicit **processed food** entry. The transformation of raw crop and animal products into shelf-stable, transportable food (milling, canning, freezing, refining sugar and oil) is a ~$4 trillion/yr industry on Earth and represents a real industrial step that consumes energy, bulk chemicals, and packaging. In a space context, food processing and preservation is a critical life-support function. This isn't a breakout from a catchall so much as a missing intermediate — analogous to how steel is an intermediate between iron ores and steel structures. I'd consider adding it to Manufactured or Biological depending on where you want the processing boundary.

## Summary

The one change I'd actually make right now is **copper** out of refined metals. The processed food gap is worth discussing but is a structural question about where processing boundaries sit rather than a catchall-breakout issue. Everything else holds up reasonably well at this abstraction level.

---
---

You made a good argument for Copper. The problem is that Copper Ores is a poor item when considering solar system distribution. So I think we'll keep copper lumped in the catchall to stay parallel with the ores.

The simulation would benefit if we split a few items into high-bulk versus high-value components. I stated these as selection criteria, but I neglected to say that this is a good axis for breaking up catchalls. For example, Machinery has to encompass hydroelectric turbines and EUV lithography machines at the same time. This is a problem in the simulation because "resources" are traded by weight, so it's hard to apply a specific (even if abstract) $/kg value. Also, your food items look good as food production "feedstocks", but don't really cover food "end products". Here are a few possible changes to address these problems:

* Split Machinery into Heavy Machinery and Precision Machinery.
* Add Packaged Foods as a high-bulk end product in the biological class. It captures a space economy and it's kind of the current trend anyway.
* Add 2 high-value consumer items: Specialty Goods and Specialty Foods. These are the high-value counterparts to Consumer Goods and Packaged Foods, respectively. They also can be modeled as nonessential, yet morale-boosting, items.

Here are a few other changes:

* Split Fusion Fuels into Deuterium and Helium-3. This helps in implementation because they involve different sources and fusion technologies.
* Move Ethane to the volatiles class, since it's rarely used as a power-generating or propulsion fuel.
* Rename Refined Metals to Industrial Metals. This makes it more parallel with the ore naming.
* Add Solar Panels. Yes, we're accounting for built infrastructure elsewhere in the simulation, but we do need manufactured items here to build that infrastructure.
* Rename Vehicles to Transport Systems. This sounds a little more general for the space economy.
* Rename Textiles to Technical Textiles. I think it helps to separate the item from Consumer Goods and Natural Fibers. However, does it still deserve a slot if it in narrowed in this way?

Do these changes work with the overall abstraction? I realize that my Machinery spit starts to collide with Electrical Equipment and Instruments. Is there a better division of these manufactured products? Do the changes or my new comments motivate a reanalysis of any other groupings?

---
---

*Thinking...*

> The user is proposing several specific changes and asking me to evaluate them against the overall framework. Let me think through each carefully.
> 
> **Proposed changes:**
> 1. Split Machinery → Heavy Machinery + Precision Machinery
> 2. Add Packaged Foods (biological, high-bulk end product)
> 3. Add Specialty Goods (high-value consumer) and Specialty Foods (high-value food)
> 4. Split Fusion Fuels → Deuterium + Helium-3
> 5. Move Ethane to volatiles class
> 6. Rename Refined Metals → Industrial Metals
> 7. Add Solar Panels
> 8. Rename Vehicles → Transport Systems
> 
> 
> 9. Rename Textiles → Technical Textiles and reconsider whether it warrants its own category
> 
> The core tension is whether splitting Machinery creates redundancy with Electrical Equipment and Instruments, or if the distinction between heavy/precision manufacturing versus electrical/control systems actually clarifies the taxonomy. I need to map out how these categories would interact and whether the proposed changes suggest a deeper reorganization of the manufactured goods tier. Option 2 feels cleaner functionally but loses the weight-based differentiation that matters for trading. Option 3 is what I actually need—categories that work on both axes, where items cluster by both their $/kg value and their consumption patterns, so I can keep Steel Structures and Heavy Machinery separate while grouping high-value items like Electronics and Instruments together. I'm realizing that batteries are already categorized separately as an energy storage item, so electrical equipment should focus on power distribution and conversion rather than energy storage. This means I can reorganize the manufactured goods more cleanly by separating heavy, bulky equipment like generators and transformers from lighter, higher-value precision items like electronics and instruments, with structural materials and transport systems as their own categories.
> 
> The real challenge is that instruments and precision machinery overlap significantly—an MRI machine or robotic surgery system could fit either category—so it makes more sense to combine them into a single "Instruments & Precision Equipment" group rather than trying to force a distinction. This leaves me with a cleaner partition: Heavy Machinery & Electrical Equipment for low $/kg, high-mass items; Instruments & Precision Equipment for medium-high $/kg items; and Electronics as its own category with a distinct value profile.
> 
> But the user specifically mentioned wanting to keep Electrical Equipment separate and is adding Solar Panels, which suggests they're asking me to reconsider how to divide these middle-tier manufactured goods. From the simulation's perspective, what really matters is the material composition, application streams, and value density—so I should organize categories around those factors rather than trying to force arbitrary distinctions.
> 
> Looking at natural clusters: heavy mechanical and electrical equipment like turbines, motors, and transformers consume lots of steel and industrial metals for power generation and mining; electrical distribution items like cables and switchgear use copper and polymers for power transmission; and electronics consume semiconductors and specialized materials for computing and control. Solar panels are distinct enough to warrant their own category given their importance and unique characteristics. I'm reconsidering whether merging "Instruments" with precision machinery makes sense conceptually—a mass spectrometer and a 3D printer are quite different, but in a space economy context they're both high-value, precision-manufactured items with low mass, so the grouping feels justified. I need to decide whether technical textiles should remain as a separate category or be consolidated elsewhere. Technical Textiles sits in an awkward middle ground—low mass flow and overlapping value ranges with Consumer Goods and Precision Equipment—but the functional distinctiveness of space-critical applications like EVA suits and habitat structures, combined with their unique manufacturing processes, probably justifies keeping the category despite being borderline.
> 
> The other proposed changes make sense: splitting Fusion Fuels into Deuterium and Helium-3 captures their fundamentally different extraction sources and technology pathways; moving Ethane to volatiles better reflects its role as a chemical feedstock rather than a propulsion fuel; and renaming Refined Metals to Industrial Metals creates better parallel terminology with Industrial Metal Ores.
> 
> Adding Solar Panels, renaming Vehicles to Transport Systems, and introducing Packaged Foods and Specialty Foods all fill important gaps—the latter two create a natural progression from raw biological products through bulk processing to high-value luxury items that serve both practical and morale functions in a space economy.
> 
> This restructuring suggests I should reconsider the manufactured goods categories more holistically, particularly whether Instruments should merge into Precision Equipment to create a cleaner distinction between heavy machinery, electrical systems, and precision manufacturing equipment.
> 
> The key insight is that generators and large motors blur the line between Heavy Machinery and Electrical Equipment, but modeling them as separate inputs to complex systems like power plants actually reflects reality—both mechanical and electrical components are genuinely needed, so the distinction holds even if individual products straddle the boundary.
> 
> Energy Storage, Polymers/Composites, Glass & Ceramics, and Semiconductor Materials all remain coherent as-is, since they're consumed across multiple manufactured categories rather than being end products themselves.
> 
> Specialty Goods and Specialty Foods work as simulatable categories because their high value-to-weight ratio makes them economically viable for long-distance trade despite extreme transport costs, mirroring how luxury goods historically drove trade routes. Technical Textiles also deserve their slot—they're essential for habitats, spacesuits, life support, and aerobraking, making them distinct from general materials and genuinely space-specific in application. I need to finalize the manufactured goods division by merging Instruments into a broader Precision Equipment category alongside precision machinery and robotics, which resolves the overlap issue and keeps the total around 11 categories. The food items belong in the biological class since they're biologically-derived end products, even though they involve manufacturing. I should note that Technical Textiles primarily derive from synthetic polymers rather than natural fibers, which is fine since the categories don't require one-to-one input relationships. The value hierarchy is clean and useful—Heavy Machinery sits at the low end, Electrical Equipment in the middle with its copper-intensive composition, Precision Equipment higher, and Electronics at the top. I'm considering whether Electronics itself needs splitting between commodity and cutting-edge components, but the distinctive semiconductor manufacturing process gives it enough coherence to keep it unified for now, with the option to refine later if the simulation demands it. I should structure my response to focus on the machinery/electrical/instruments collision since that's where the analytical work matters most.

## Straightforward Changes

**Deuterium / Helium-3 split** — Clean and well-motivated. Different source bodies (any water-bearing body vs. lunar regolith and gas giant atmospheres), different extraction processes, different fusion reactor designs (D-D and D-T vs. D-He3), different technology timelines. One of the better-justified splits in the whole list.

**Ethane → Volatiles** — Agreed. Its primary role is as a cracking feedstock for ethylene, not as a fuel. It behaves like a chemical intermediate, and placing it alongside other light hydrocarbons and feedstock volatiles is more honest about how it actually flows through the economy.

**Industrial Metals** — Better name. It also sidesteps the implication that everything in the category went through a smelting step, which won't be true for all space-sourced metals.

**Transport Systems** — Good generalization. It naturally accommodates spacecraft, cargo tugs, rovers, and autonomous drones alongside conventional vehicles without stretching the label.

**Solar Panels** — Agree these need to be explicit. They're the single most important manufactured input to space power infrastructure, they have a distinctive bill of materials (semiconductor materials, glass, aluminum, trace silver/copper), and their production rate is a genuine bottleneck variable for expansion. They also have an unusual $/kg profile for a manufactured good — relatively low value density (~$0.50–2/kg at the module level on Earth) but absolutely mission-critical, which makes them interesting in the trade model.

## The Manufactured Goods Collision

You're right that splitting Machinery into Heavy and Precision creates a three-way overlap with Electrical Equipment and Instruments. The root issue is that the current categories were drawn along functional lines (mechanical vs. electrical vs. measurement), but the simulation needs them drawn along value-density lines. Those two axes don't align.

I think the cleanest resolution is to let the value-density axis dominate and merge Instruments into a broader **Precision Equipment** category that also absorbs the high-end half of the Machinery split. The resulting partition:

**Heavy Machinery** — Turbines, engines, pumps, compressors, heat exchangers, mining and construction equipment, cranes, large HVAC systems, pressure vessels, industrial furnaces. Primarily steel with some industrial metals. Roughly $5–50/kg. These are the items where mass itself is a dominant design parameter.

**Electrical Equipment** — Generators, transformers, electric motors (as standalone units), power cables and wiring, switchgear, power distribution panels, lighting, power electronics, fuel cells. Copper-heavy, moderate polymer content. Roughly $10–100/kg. This category is the electrical power backbone — it sits between generation (solar panels, fusion reactors built from heavy machinery) and end-use (electronics, precision equipment, life support). Keeping it separate makes sense because its material composition is distinctively copper-intensive and it feeds every application stream.

**Precision Equipment** — This absorbs the old Instruments category plus the high-value end of machinery: scientific instruments, medical devices, robotics, CNC machines, lithography systems, optical systems, metrology equipment, laboratory equipment, navigation and guidance systems. Diverse inputs (steel, industrial metals, glass/ceramics, electronics, polymers). Roughly $100–10,000/kg. The unifying characteristic is that these are complex, precision-manufactured assemblies where performance depends on tight tolerances and sophisticated integration rather than sheer mass.

**Electronics** stays as-is — semiconductors, computing hardware, communication equipment, displays, sensors, circuit boards. Its distinctive manufacturing process (semiconductor fabrication, PCB assembly) and extreme value density ($100–100,000/kg) keep it naturally separate from Precision Equipment even though the boundary is sometimes fuzzy.

This gives you four tiers of manufactured equipment with roughly order-of-magnitude steps in $/kg, each with a distinct material-input signature and a distinct role in the economy. The functional distinctions aren't lost — they're just redistributed along a more simulation-friendly axis. A hydroelectric project consumes Heavy Machinery and Electrical Equipment. A semiconductor fab consumes Precision Equipment and Electronics. A mining operation consumes Heavy Machinery and Transport Systems. The application-stream mapping still works, but now each category has a tighter $/kg range.

## Technical Textiles

It still deserves a slot, though it's genuinely borderline. The case rests on three things. First, the manufacturing process is distinctive — weaving, knitting, or layering high-performance fibers is fundamentally different from assembling mechanical or electronic goods. Second, the inputs are distinctive — it's one of the few manufactured categories that consumes Natural Fibers alongside Polymers and Composites (aramid, UHMWPE, carbon fiber cloth). Third, in a space economy the applications are critical and surprisingly diverse: habitat pressure-containment layers (Vectran, Kevlar), multi-layer insulation blankets, EVA suit materials, filtration media for life support, parachute and aerobraking fabrics, cargo restraint systems, and radiation-shielding curtains. None of these sit comfortably in Consumer Goods, Heavy Machinery, or Precision Equipment.

The risk is that the mass flow is small enough to feel like a rounding error. But at this abstraction level you're already tracking items like Semiconductor Materials that have small mass flows justified by strategic importance. Technical Textiles earns its slot on the same basis.

## Food Items

**Packaged Foods** fills a real gap. Right now Crop Products and Animal Products are agricultural outputs, essentially ingredients. The processing step that turns wheat into shelf-stable bread, or milk into cheese, is a substantial industrial transformation consuming energy, water, bulk chemicals, and packaging materials. In a space context, food preservation and packaging is a critical life-support function — you can't just ship raw grain to a crew of thirty. Packaged Foods captures this as a high-bulk, moderate-value end product.

**Specialty Foods** and **Specialty Goods** are a clever pair. They give the simulation a way to model elastic luxury demand — the kind of trade that becomes economically significant once a settlement matures beyond subsistence. Historically, high-value, low-mass luxury goods (spices, silk, precious metals) drove long-distance trade precisely because transport costs selected for value density. A space economy would exhibit the same pattern: early colonies import only essentials, but maturing ones develop demand for Earth-origin wine, chocolate, artisanal goods, or cultural products. These categories let you model that transition as a morale or quality-of-life variable without trying to enumerate what the luxuries actually are.

One question about placement: you put Packaged Foods and Specialty Foods in the biological class, which makes sense from a supply-chain perspective (they're downstream of Crop/Animal Products). But they're also manufactured products — they consume energy, bulk chemicals, and packaging from the materials/manufactured side. This might create a bookkeeping wrinkle in the simulation if classes are expected to have uniform input profiles. It may not matter depending on how you've structured class boundaries, but it's worth flagging.

## Other Reanalysis

The changes raise one question about **Energy Storage**. You now have Solar Panels as a separate manufactured item, Electrical Equipment handling power distribution, and Energy Storage in the energy class. Batteries are the meeting point of all three — a battery is manufactured like electrical equipment, stores energy like a fuel, and is chemically composed of industrial metals (lithium, cobalt, nickel) and bulk chemicals (electrolyte). The current placement in the energy class makes sense if you think of batteries as "stored energy to be dispatched," but if the simulation needs to model battery manufacturing as a bottleneck (which it probably does for space infrastructure), you may want to ensure the production chain explicitly routes through Electrical Equipment or a manufacturing step rather than treating batteries purely as a stockpiled energy commodity. This isn't a category change — it's an implementation note about how Energy Storage connects to the manufacturing side of the graph.

The rest of the list holds up well under the new changes. The biological class gains a clean two-tier structure (raw agricultural products plus processed food products). The manufactured class gains a coherent value-density ladder. And the energy class gains precision on the fusion side without adding complexity elsewhere.

---
---

Please incorporate the changes as you outlined above.

For clarification, the resource "classes" are solely for organizational and GUI purposes. They have no functional effect at all, although they align with functional differences in some cases. As a reminder, the classes are Energy, Ores, Volatiles, Materials, Manufactured and Biological. ("Extractable" refers to the context file list and is a functional characteristic, not a class.)

Also, items are often composed of other items. Obviously, manufactured items must be composed of materials, but they are also (in most cases) composed of other manufactured items.

I'd like to start to specify the composition of the new resource items. Relist all of the items organized by resource class. Include the following:

- A brief description. For manufactured and many materials, this might be a short list of examples. For items like steel, provide a representative type or blend of types that can stand in for the item globally.
- An elemental composition by weight. Try to generate a sensible representative (abstracted) composition for each.

For resources from the extractable resources file, you can take both of the above directly from the file.

---
---

*Thinking...*

> The user wants me to relist all items organized by resource class, with descriptions and elemental compositions for each. Let me first recall all the items we've been developing, incorporating all the changes discussed:
> 
> **Changes to incorporate:**
> 1. Split Machinery → Heavy Machinery + Precision Equipment (absorbing Instruments)
> 2. Add Packaged Foods (biological class)
> 3. Add Specialty Goods and Specialty Foods (manufactured and biological respectively... wait, let me reconsider placement)
> 4. Split Fusion Fuels → Deuterium + Helium-3
> 5. Move Ethane to volatiles class
> 6. Rename Refined Metals → Industrial Metals
> 
> 
> 7. Add Solar Panels
> 8. Rename Vehicles → Transport Systems
> 9. Rename Textiles → Technical Textiles
> 10. Instruments merged into Precision Equipment
> 
> Now I need to reconstruct the full list by going back through the conversation to identify all resource classes and their items, then apply these changes systematically.
> 
> From the discussion, the six classes are Energy, Ores, Volatiles, Materials, Manufactured, and Biological. I'm working through what was mentioned for each: Ores has 13 items from the file, Volatiles has 14, and I need to verify the exact composition of Materials, Manufactured, Energy, and Biological based on what was referenced throughout. Once I have the complete inventory organized by class with all the renames and merges applied, I can move on to generating descriptions and elemental compositions for each item. Helium-3 is just the ³He isotope, so elementally it's helium at 100%. For energy storage, I'm considering a production-weighted Li-ion battery that blends NMC and LFP cathode chemistries with a graphite anode, accounting for the electrolyte, separator, current collectors, and casing materials to get a representative mass composition. I'm working through a bill of materials approach, calculating the elemental composition of a typical 1 kg lithium-ion cell by breaking down each component's mass and chemical makeup, then simplifying to a production-weighted average since the detailed calculations are getting too granular. I'm realizing the "other" category is too large because I'm not accounting for structural materials like the steel casing and plastic components properly. Let me break down a typical cylindrical lithium-ion cell by its major components—cathode, anode, electrolyte, separator, and the metal can with terminals—and estimate their mass percentages more systematically.
> 
> For the cathode active material, I need to calculate the elemental composition more carefully using the actual molecular weights and stoichiometry of NMC and LFP compounds to get accurate mass fractions for lithium, nickel, manganese, cobalt, and oxygen. The electrolyte consists of lithium hexafluorophosphate dissolved in a mixture of ethylene carbonate and dimethyl carbonate solvents, contributing lithium, phosphorus, and fluorine along with carbon, hydrogen, and oxygen from the organic components. The separator is a polyethylene or polypropylene polymer layer adding carbon and hydrogen, while the polyvinylidene fluoride binder introduces additional carbon, hydrogen, and fluorine. The steel casing and terminal tabs make up the remaining mass as iron. I'm realizing my bottom-up calculation from the BOM isn't quite adding up, so I should just work with typical published composition ranges for lithium-ion cells instead. For a pack-level analysis that includes the housing, structural components, and electronics, I'd need to account for additional aluminum, steel, copper wiring, and circuit board materials beyond just the cell chemistry itself. I'm finalizing the energy storage composition by accounting for structural materials dominating at the pack level, settling on a distribution weighted toward iron, carbon, and aluminum with trace elements making up the remainder. Now I'm moving on to fission fuels, starting with LEU fuel assemblies composed of enriched uranium dioxide with zirconium cladding. I'm calculating the weighted composition of a global steel blend by combining carbon steel, stainless, and alloy steel percentages, then need to finish the weighted average for iron content before moving to the other elements. For aluminum, I'm calculating a production-weighted blend across common alloy series—mostly 6xxx and 3xxx for general use, with some 2xxx and 7xxx for aerospace applications—arriving at a composition of roughly 95% aluminum with small percentages of silicon, magnesium, copper, zinc, and manganese, plus trace amounts of iron, chromium, and titanium.
> 
> For industrial metals, I'm building a basket of refined non-ferrous metals excluding aluminum, weighted by global production volumes where chromium, manganese, zinc, and copper dominate by mass, followed by lead, nickel, titanium, and tin in smaller quantities. I'm realizing that chromium and manganese are primarily consumed as steel additives rather than standalone refined metals, so they shouldn't be weighted the same as copper or zinc in an "industrial metals" category. I should focus on metals that are actually traded and used independently—copper for electronics and wiring, zinc for galvanizing, lead for batteries—and only partially count chromium and manganese for their non-steel applications. I'm calculating the total refined metal production at around 72.4 Mt and converting it to percentage weights for each metal. The key insight is that Industrial Metals should represent the refined metal basket as extracted and processed, regardless of whether those metals eventually feed into steel or other products, so I'm simplifying the composition to the major contributors: copper, zinc, lead, chromium, manganese, nickel, and trace amounts of tin, cobalt, molybdenum, and titanium.
> 
> For Precious Metals, I'm looking at production-weighted annual volumes across silver, gold, and the platinum group metals like palladium, platinum, rhodium, ruthenium, and iridium.
> 
> Silver overwhelmingly dominates by mass at 87%, with gold making up most of the remainder at 11%, and the platinum group metals collectively accounting for less than 2%.
> 
> For Rare Earth Elements, I'm working with separated REE oxides and metals, where cerium is the largest component at around 40%, followed by lanthanum at 25%, neodymium at 15%, and yttrium at 7%, with the remaining rare earths like praseodymium, samarium, gadolinium, and dysprosium making up the rest. For rare earth elements, I'm listing the major oxides by composition: cerium, lanthanum, neodymium, yttrium, and praseodymium make up most of the mass, with smaller contributions from samarium, gadolinium, dysprosium, and trace other rare earths, plus oxygen. For semiconductors, silicon overwhelmingly dominates the market by mass—especially when including solar-grade feedstock—so the composition is almost entirely silicon with negligible amounts of other semiconductor materials like gallium, germanium, and indium. I'm estimating the polymer category at roughly 400 million tons annually, with polyethylene and polypropylene making up the bulk, followed by PVC and PET, so I'll weight the composition accordingly—mostly carbon and hydrogen with trace heteroatoms from additives and reinforcements. Composites like carbon and glass fiber reinforced polymers add small amounts of Si, O, and other elements, so I'm adjusting the polymer composition to C 74, H 11, Cl 9, O 4, N 0.5, S 0.5, and trace amounts of Si and F. For glass and ceramics, I'm focusing on soda-lime glass which dominates global production, along with technical and refractory ceramics, and breaking down the elemental composition of soda-lime glass by its oxide components. I'm calculating the weighted elemental composition across the four glass types by multiplying each component's percentage by its elemental makeup, then summing to get the overall distribution: roughly 47% oxygen, 31% silicon, 8% sodium, 6% calcium, 4% magnesium, 3% aluminum, with trace boron and other elements making up the remainder. For concrete, I'm breaking down the typical mix into its main constituents—aggregate (limestone and sand), cement paste, and water—to estimate the elemental composition, which comes out to approximately 49% oxygen, 17% calcium, 16% silicon, 3% aluminum, 3% carbon, 2% iron, 2% hydrogen, 2% magnesium, and smaller amounts of sodium, potassium, and sulfur. Now I'm calculating the elemental composition of the full concrete mixture by weighting each component—cement, limestone aggregate, silica sand, water, and supplementary materials—then summing their contributions to get the overall percentages of calcium, oxygen, silicon, and other elements. I'm finishing the calculation to get a total of about 100, then adjusting the composition based on aggregate type—limestone shifts the balance toward more calcium and carbon, while granite would increase silicon and aluminum. For a global average assuming limestone is common, I'm settling on approximate percentages: oxygen at 50%, calcium and silicon each around 20%, carbon at 5%, with smaller amounts of hydrogen, aluminum, iron, magnesium, and trace elements. Now I need to tackle bulk chemicals, which is complicated because the category spans such a wide range of industrial products. I'm listing more bulk chemicals like methanol, ethanol, nitric acid, and hydrochloric acid with their production volumes and elemental compositions, but realizing this approach is getting unwieldy—I should just estimate the overall elemental makeup since oxygen, calcium, and sulfur are the dominant elements across these products. I'm summing up the elemental contributions from each major chemical to get totals for oxygen, calcium, sulfur, chlorine, sodium, carbon, hydrogen, nitrogen, and phosphorus across the ~1305 Mt of production.
> 
> Converting to percentages shows oxygen dominates at 42%, followed by calcium at 22%, with carbon, chlorine, and sulfur each contributing 7-9%. I'm verifying the percentages add to 100%.
> 
> Lime is indeed the largest single chemical product by mass despite being somewhat of a borderline case between industrial mineral and chemical. For fine chemicals, I'm breaking down the major categories—paints and coatings dominate at around 40 Mt annually with titanium dioxide pigment and organic polymers as the main components, followed by adhesives, pesticides, and smaller volumes of catalysts and electronic-grade chemicals. Looking at paint composition more carefully—the binder is polymer-heavy carbon, pigments contribute titanium dioxide and iron oxides, and mineral fillers like calcium carbonate and talc round it out—I'm refining my estimate to account for these components while keeping the total at 100.
> 
> For natural fibers, cotton dominates global production at around 80%, so I'm weighting the elemental composition heavily toward cellulose's carbon and oxygen profile, with smaller contributions from wool's nitrogen-rich keratin and plant fibers' lignin content.
> 
> Now I need to consider manufactured fibers, which have entirely different chemical bases.
> 
> For heavy machinery like turbines and industrial equipment, I'm calculating the elemental breakdown by first establishing the material composition (mostly steel with smaller amounts of cast iron, aluminum, copper, polymers, and ceramics), then converting each material's known elemental percentages into their contribution to the overall weight.
> 
> After working through the calculations, I'm getting a composition that feels too heavy on miscellaneous elements, so I'm simplifying to a more realistic distribution: iron dominates at 80%, with chromium, carbon, and aluminum as significant secondary elements, and smaller amounts of copper, nickel, manganese, silicon, oxygen, and hydrogen, leaving about 4.5% for trace metals and other elements.
> 
> For precision equipment, I'm breaking down the material composition differently since these devices use more diverse materials—stainless steel makes up about a third, aluminum alloys another fifth, with meaningful contributions from electronics components, optical glass, polymers, and specialty metals like titanium and tungsten filling out the rest. I'm calculating the elemental composition by working through the mass percentages of each material type, but I'm running into the problem that manufactured goods contain so many trace elements that "other" keeps ballooning beyond what's reasonable. I'm settling on listing elements above 1% and accepting that 10% will remain unaccounted for given the complexity of modern assemblies. For electronics specifically, I'm breaking down the composition by major component types—PCBs, integrated circuits, displays, connectors—and estimating that copper dominates the metallic content while silicon appears in both semiconductors and glass, with polymers and ceramics making up the bulk of the remaining mass. I'm finishing the elemental breakdown by accounting for barium in capacitors and adjusting the percentages to sum to 100, then consolidating the minor elements into an "other" category before cross-checking against e-waste composition studies to validate the distribution. Electrical equipment is dominated by steel, copper, and aluminum—I'm calculating the elemental composition by breaking down the major components (transformer cores, windings, cables, insulation) and their typical mass percentages, which gives me iron around 38%, copper 25%, aluminum 14%, with smaller amounts of carbon, oxygen, silicon, and trace metals like chromium and nickel from steel alloys.
> 
> For solar panels, I'm looking at crystalline silicon modules where glass makes up the bulk of the mass, followed by the aluminum frame, with smaller contributions from the silicon cells themselves, polymers for encapsulation and backing, and tiny amounts of copper and silver for electrical contacts. I'm completing the elemental composition breakdown by tallying the remaining components—junction box and cables contribute copper and polymers, while trace amounts of silver and other metals round out the total. The final elemental inventory shows silicon and oxygen as the dominant elements, followed by aluminum, sodium, and calcium, with smaller quantities of carbon and copper distributed throughout the panel's various layers.
> 
> For transport systems, I'm establishing a production-weighted composition dominated by ground vehicles and steel, then blending in contributions from ships, aircraft, and spacecraft to create a representative fleet average that accounts for iron as the primary element, with significant aluminum in aircraft, and smaller percentages of chromium, nickel, and other alloying elements. Technical textiles are dominated by synthetic polymers like polyester, polypropylene, and nylon, with smaller contributions from aramids, carbon fiber, and glass fiber, so I'm calculating the weighted elemental composition across all these materials to get carbon at 65%, oxygen at 21%, hydrogen at 7%, nitrogen at 3%, with fluorine, silicon, calcium, and aluminum making up the remainder.
> 
> For consumer goods, paper and cardboard dominate at around 400 million tons annually, dwarfing clothing, furniture, and other categories, so I need to reconsider whether this category should be weighted so heavily toward cellulose-based products or if the definition of consumer goods needs adjustment. I'm calculating the weighted elemental composition across all product categories by multiplying each material's percentage by its elemental breakdown, then summing to get the overall proportions of carbon, oxygen, and other elements in this consumer goods category. Magnesium is a minor component, and after totaling all the elemental contributions, I get 95.43 with 4.57 unaccounted for, which I round to create a final composition of roughly 44% carbon, 23% oxygen, 10% iron, 6% hydrogen, and smaller percentages of silicon, chlorine, aluminum, and trace metals. For specialty goods, I'm blending premium electronics, luxury polymers, precious metals, textiles, and glass into a diverse composition that's difficult to pin down precisely, though the precious metal content of around 6.5% seems reasonable for high-end jewelry and luxury items. I'm finalizing the specialty goods composition and moving on to structural steel, which is straightforward—mostly iron with small amounts of chromium, manganese, nickel, and zinc coating. Now I'm starting the biological category with crop products, noting that global production is around 9 gigatons annually, dominated by cereals, sugar cane, and vegetables. Most crops are harvested wet, but traded products are typically dried—I'll use around 12-15% moisture for grain-equivalent forms. Converting to dry mass basis, sugar cane's 2 Gt at 75% moisture only contributes 500 Mt of dry matter, while cereals at 3 Gt with 12% moisture yield about 2.64 Gt dry mass. I'm realizing the moisture content dramatically skews the composition toward oxygen and hydrogen, so I need to reconsider whether the simulation should represent crops as-harvested with water or in a drier state. Since water is tracked separately as its own resource, I'll use a dry basis for crop products, which lets me model any moisture as a mixture of water and dry material. For animal products, I need to establish a production-weighted composition across meat, dairy, eggs, fish, and hides. I'm calculating the elemental composition of packaged foods by blending processed food content with packaging materials—roughly 75% food (dominated by carbohydrates and added salt) and 25% packaging (steel cans, aluminum, glass, and polymers)—to get a weighted average across carbon, oxygen, hydrogen, nitrogen, and trace elements. Finishing the elemental composition calculations for specialty foods by weighting the remaining trace elements, then totaling to 100. Now I'm organizing the full inventory into two main categories: ores covering 13 types of mineral and rock resources, and volatiles covering 14 types of gases and liquids. I'm confirming the categorization: Specialty Goods belongs in Manufactured as a counterpart to Consumer Goods, and Specialty Foods in Biological as a counterpart to Packaged Foods, even though they'll have some cross-category composition. Now I'll organize everything by resource class and provide each item with a brief description and elemental composition by weight, being careful to ensure the percentages sum to 100 and that manufactured items account for their material and packaging components. For the battery cell composition, I'm breaking down each component by mass percentage: the NMC622 cathode material contributes lithium, nickel, manganese, cobalt, and oxygen; graphite makes up the anode; aluminum and copper form the current collectors; and the electrolyte solution adds lithium, phosphorus, fluorine, and organic compounds. Now I'm accounting for the separator material, binder, steel casing, conductive additives, and tabs—calculating their elemental contributions and summing everything up to get the total composition across all battery components. Now I need to account for a production-weighted blend assuming 50% NMC622 and 50% LFP by market share, which means recalculating the material composition for an LFP cell with its lithium iron phosphate cathode and then averaging the two chemistries together.
> 
> Then I'm scaling up to the pack level by factoring in that cells make up about 65% of the total pack weight, with the remaining 35% coming from aluminum housing, steel structures, coolant systems, copper wiring, and battery management electronics, which shifts the elemental composition significantly toward aluminum and iron while diluting the cathode materials. I'm adjusting the composition percentages to sum to exactly 100, accounting for trace elements and pack-level components that introduce miscellaneous materials. For the fission fuel category, I'm settling on uranium as the dominant component with zirconium, oxygen, and iron making up most of the remainder, with minimal trace elements.
> 
> Now I'll compile all the compositions into a clean response following the user's format from the extractable resources file, making sure to distinguish between items where I'm pulling directly from the file versus those where I need to provide approximate compositions for manufactured and biological categories. I'm calculating the weighted average composition by multiplying each material's percentage by its elemental makeup, then summing across the three steel types to get the final blend composition. I'm refining the alloy composition to get cleaner percentages that sum to exactly 100, settling on Fe 96, Cr 2, Ni 1, Mn 0.5, C 0.2, Si 0.2, and other trace elements at 0.1. Now I need to reconsider whether chromium should be counted as a standalone industrial metal since most ferrochrome production goes directly into stainless steel rather than being traded as an independent commodity. I'm refining the contained metal production figures by checking ore production data more carefully—chromite ore yields about 13 Mt of contained chromium, and I'm compiling a complete list of all major industrial metals weighted by their actual production volumes to establish the resource category baseline. I'm working through whether titanium should be categorized as an industrial metal even though much of it becomes titanium dioxide pigment rather than pure metal—ultimately deciding to include it since the elemental content flows through the same supply chain from ore to refined form to downstream uses. Now I'm moving on to finalize the Glass & Ceramics composition.
> 
> Glass production dominates this category at around 250 million tons annually compared to technical ceramics and refractories at much lower volumes, so the composition I have is reasonable. I'm debating whether to include traditional ceramics like bricks and tiles—they're produced at massive scale, roughly 4.5 billion tons yearly—but they're essentially fired clay closer to stone than industrial ceramics, so I'll treat them as implicitly covered under construction materials rather than as a separate Glass & Ceramics component.
> 
> For concrete, I'm using a composition based on limestone aggregate with some granite and basalt mixed in, which should represent a reasonable global average. Now I need to compile the full materials list and double-check whether I should reproduce the ores and volatiles directly from the file or just reference them.
> 
> On the elemental composition front, I need to be careful about isotopes—helium and deuterium are tricky cases where the isotopic distinction matters for the user's categorization, but elementally they're just He and H. The file lists natural helium as He-4 dominant, while the energy class contains separated He-3, so I should treat them as distinct entries even though they're the same element.
> 
> I'm going to compile elemental compositions for all 58 items using a compact format. For the 27 extractable resources, I'll pull directly from the file, but for the remaining 31 items across Energy, Materials, Manufactured, and Biological categories, I need to create realistic compositions. Let me finalize these 31 first before writing everything up, making sure each composition sums to 100 and reflects the actual makeup of each resource type. I'm finalizing the elemental compositions for precision equipment and moving through the remaining manufactured categories—electronics with its mix of semiconductors and conductors, electrical equipment dominated by iron and copper, solar panels heavy in silicon and oxygen, and transport systems with iron as the primary component. Finishing the crop products composition and verifying it sums to 100, then moving on to animal products which has a higher nitrogen and carbon content reflecting its organic nature, also verified to total 100. I'm verifying the packaged foods composition by adjusting iron to 7.5 to reach exactly 100, then confirming the specialty foods total also sums correctly. Now I'm ready to write the response, though I want to reconsider how I represented the rare earth elements composition. Completing the oxygen calculation: 0.70(15) = 10.5%, which gives me a total of 100%. So the average REE product is roughly 90% rare earth elements and 10% oxygen. Now I'm breaking down the individual REE composition based on typical production volumes—cerium and lanthanum dominate at 38% and 24% respectively, with neodymium at 15%, and the remaining elements distributed across smaller percentages.
> 
> For semiconductor materials, I'm reconsidering whether Si 97 with 3% other is too simplified. While silicon overwhelmingly dominates by mass (around 1 Mt/yr versus maybe 1000 tonnes of III-V materials), the category includes polysilicon, wafers with dopants, and various compound semiconductors like GaAs and GaN. I'll specify the major components within that 3% other: gallium, germanium, arsenic, and indium, with the rest as trace elements.
> 
> On bulk chemicals, I'm verifying that the composition I calculated—with oxygen and calcium dominating due to lime production at 400 Mt/yr—is reasonable. Lime is by far the largest contributor by mass, followed by sulfuric acid and methanol, so the heavy weighting toward calcium and oxygen makes sense for this category.
> 
> I'm now questioning whether ammonia belongs in volatiles or bulk chemicals. Since ammonia occurs naturally on outer solar system bodies, it fits in the volatiles class as an extractable resource, whereas synthetic ammonia production would flow through the simulation as a conversion from volatile inputs (N₂ and H₂) to volatile outputs (NH₃). This distinguishes it from purely synthetic chemicals like sulfuric acid that belong in bulk chemicals.
> 
> For precious metals, the composition should reflect just the production-weighted mix of pure elements without oxygen or other non-metals since these are traded as refined metals. For fission fuels, I need to decide whether to specify just the fuel material itself or include the full assembly composition. I'm breaking down the remaining trace elements more explicitly—adding nitrogen, sulfur, and tin—and reducing the catch-all "other" category from 6% to about 4.8% to account for zinc, titanium, boron, and various minor components.
> 
> Let me verify the percentages sum to 100, then I'll write up the response with a note that manufactured and biological items have less precise compositions than raw resources due to their inherent variability.
> 
> I'm confident in the 58-item list now. I'll follow the extractable resources file's format—bold name, dash, brief description, then elemental composition—using the file's exact wording for those items and matching the style for new ones. Since the user asked me to relist all items, I need to include everything with descriptions and compositions, organized by category.
> 
> I'm refining the biological items to ensure they're properly classified. Packaged Foods includes the packaging materials themselves (steel cans, aluminum, glass), which accounts for the Fe, Al, and Si percentages, and Specialty Foods similarly reflects wine bottle glass content. Let me do a final verification of all compositions before writing. Continuing to verify the materials categories—aluminum, industrial metals, precious metals, rare earth elements, semiconductors, polymers, and glass all check out to 100, confirming the distribution across all material types is balanced. I'm verifying the Electrical Equipment category totals to 100, then checking Solar Panels and starting on Transport Systems, which also needs to sum to 100. I'm verifying the calculations for Specialty Goods and Steel Structures both sum to 100, then moving into the Biological category starting with Crop Products. Finishing the verification for the fourth category, then I'm ready to write the response organized by resource class with each item listed alongside its description and composition breakdown. I should present the revised list directly without over-explaining the changes, though a brief note about what's been updated would help with tracking. The response will be substantial at around 175 lines with 58 items and their descriptions, so I need to be efficient with formatting. For items like Precision Equipment that have "other" components, I should check whether to break those down further by comparing how the extractable resources file handles similar cases. I'm reconsidering my approach by setting a threshold—listing all elements above 0.5% explicitly and grouping everything else as "other," which should bring that category down to a more reasonable 2-5% range. Let me restructure the manufactured goods compositions with this strategy in mind, starting with heavy machinery and working through the rest. I'm realizing that even accounting for dozens of trace elements, I still can't explain where 7.4% of precision equipment goes, which suggests the category is too diverse to pin down with a single composition. Let me restructure my approach by increasing polymer and glass content since precision instruments have substantial plastic housings, optical components, and electronic elements that I initially underweighted. I'm finishing the elemental calculations for the remaining materials—copper, silicon, chromium, nickel, and the trace elements like sodium, calcium, nitrogen, and chlorine—to complete the weighted contribution analysis.
> 
> I realize the "Other" category is too large at 14.62%, which suggests my bill of materials isn't accounting for everything consistently, so I'm switching to a more systematic approach by breaking down a representative precision equipment piece into major component categories with their specific alloy compositions and mass percentages, then calculating each element's contribution from steel, aluminum, copper, engineering plastics, elastomers, and glass components.
> 
> After working through the electronic subassemblies, specialty metals, bearings, fasteners, and lubricants with their respective elemental breakdowns, I'm summing up the total contributions for each element—iron dominates at 23.22%, followed by aluminum at 20.09%, carbon at 15.81%, and copper at 7.74%, with the remaining elements distributed across oxygen, chromium, silicon, nickel, hydrogen, nitrogen, titanium, and chlorine. I'm adding up all the elemental contributions and other sources sequentially to get a total around 92.
> 
> 02, then realizing I need to account for the remaining percentage. After rounding and grouping elements by abundance threshold, I'm settling on listing the major elements above 1% individually while consolidating the trace elements into an "other" category, which brings the total to approximately 99% with better readability than listing every single element. I'm adding more specific trace elements to reduce the "other" category, but the percentages keep coming out slightly high, so I'll accept that electronics will have around 8-10% unspecified elements and finalize this composition.
> 
> Now I need to organize all ten resource classes with their descriptions and elemental compositions in a format matching the provided file structure, noting that I'm pulling descriptions and compositions directly from the source material for extractable resources while generating new ones for the non-extractable categories. I should note that Deuterium and Helium-3 are isotopically distinct from their standard forms despite having identical elemental compositions, and I'll clarify this in the descriptions. For the rare earth and precious metals categories, listing individual elements makes sense since each has distinct chemical properties and applications, so I'll maintain that granularity when writing this up.
> 
> Now I'm ready to structure the full list: I'll reproduce the ores and volatiles verbatim from the source file, then create new compositions for energy, materials, manufactured goods, and biological resources, following the same format of name, description with reference standard, and composition line. This gives me 58 total items organized across six categories. I'm verifying the category counts are correct across all resource types, then I'll finalize the list with the proper formatting for "other" elements matching the file's parenthetical style. I'm recalculating the titanium content in fine chemicals more carefully—paint and coatings at roughly 50 Mt annually with about 10% average TiO₂ gives me 3 Mt of titanium, which is 3% of the 100 Mt total, so I'll lower that from 5% and adjust the composition accordingly. The major mass contributors are paints and coatings with polymeric binders and mineral fillers, adhesives with epoxy and polyurethane, and specialty polymers, which should drive the elemental breakdown. Lime is definitely a bulk chemical given its production from limestone and its massive scale, so keeping it in that category makes sense even though it drives up the calcium percentage—that's just an accurate reflection of how dominant it is by mass. For Specialty Goods, since Consumer Goods is classified as Manufactured, Specialty Goods should follow the same categorization. I'm verifying that 16% carbon is reasonable for precision equipment given the polymer content, and 20% aluminum is on the high side but defensible as a category average. My natural fibers composition closely matches cellulose with trace protein, so I'm satisfied with these values and ready to write the final output with usage notes.
> 
> I'll organize the document into eight sections covering ores, volatiles, energy, materials, manufactured goods, biological items, and notes, with roughly 58 items at about 100 words each, following the original file's format of name, description, and elemental composition. Energy to Volatiles, which means updating its class designation even though it's already listed in the Volatiles section of the extractable file. So I need to make sure Ethane is classified as Volatiles class, not Energy class. So Ethane was reclassified from Energy to Volatiles, and Fusion Fuels was split into Deuterium and Helium-3, keeping Energy at 5 items total. My final count of 58 items across all six classes checks out: the 27 extractable resources (13 Ores, 14 Volatiles) plus 31 non-extractable items distributed across Energy, Materials, Manufactured, and Biological classes.
> 
> Now I'm formatting everything according to the user's template—each item as a bullet with bold name, description, and elemental composition listed cleanly. I'm continuing to define the material composition standards, moving through steel as a production-weighted global blend, aluminum alloys across common series, and a basket of refined base metals that captures the diversity of industrial metal production excluding iron and aluminum. Rare earth elements include cerium, lanthanum, neodymium, and yttrium as primary components with smaller amounts of other rare earths. Semiconductor materials are dominated by silicon for solar and electronic applications with minor contributions from gallium, germanium, and other compound semiconductors. Polymers and composites span thermoplastics like polyethylene and polypropylene through thermosets and fiber-reinforced materials, with carbon and hydrogen as the primary elemental constituents.
> 
> Glass and ceramics are primarily composed of silica and oxygen with sodium and calcium from soda-lime glass, along with technical ceramics containing alumina and magnesia. Concrete is a Portland cement-based material with limestone-silica aggregate, where oxygen, calcium, and silicon dominate the elemental composition.
> 
> Bulk chemicals represent a production-weighted mix of commodity inorganic and organic products like lime, sulfuric acid, and methanol, with oxygen, calcium, and carbon as primary elements. Fine chemicals encompass specialty products including coatings, catalysts, and pharmaceuticals, where carbon and oxygen are most abundant, supplemented by titanium, silicon, and nitrogen from various additives and pigments.
> 
> Now I'm verifying the elemental percentages for natural fibers—cotton-dominated textiles are primarily carbon and oxygen with trace amounts of nitrogen and other elements. Moving into manufactured goods, I'm starting with heavy machinery like turbines and engines where iron dominates the material composition, followed by precision equipment like scientific instruments and medical devices that require more diverse materials including aluminum and copper, and then electronics which includes semiconductors and computing hardware.
> 
> For electronics, I'm noting that while the "other" category at 9.5% seems high, it's justified because trace elements like silver, gold, indium, and gallium are functionally essential despite their small mass fractions. Electrical equipment like generators and transformers is iron and copper-heavy, and I'm double-checking my percentages to ensure they sum to exactly 100. Now I'm moving into solar panels, which are primarily crystalline silicon modules. Technical textiles for industrial and aerospace use—including aramid, ultra-high molecular weight polyethylene, carbon fiber, and specialized membranes—are predominantly carbon and oxygen with trace nitrogen and fluorine. Consumer goods like furniture, clothing, and personal care products have a similar carbon-oxygen base but incorporate more iron and chlorine from manufacturing processes and additives. I'm verifying the final count across all categories—thirteen ores, fourteen volatiles, five energy sources, twelve materials, ten manufactured goods, four biological items, totaling fifty-eight entries. Since the user explicitly asked for a complete relist organized by resource class, I'll provide the full inventory with a brief note about the incorporated changes, then add some usage notes about the dry-equivalent basis for biological items and how actual traded compositions may vary. I'm reconsidering the Electrical Equipment composition by breaking down major subcategories—transformers, motors, cables, switchgear, generators, and other components—and weighting them by their typical mass contribution to arrive at more accurate percentages, which pushes iron significantly higher to around 48% and copper to roughly 27%, while aluminum drops to about 3%. I'm realizing aluminum conductors are common in power cables alongside copper, and aluminum also appears in some transformer tanks, so I need to increase its proportion while adjusting iron and copper downward. After several recalculations to hit exactly 100%, I'm settling on a composition with iron at 45%, copper at 24%, aluminum at 6%, and distributing the remainder across carbon, oxygen, chlorine, chromium, silicon, and trace elements—this feels more realistic than my initial estimate which overweighted aluminum and underweighted the primary metals. Now I should revisit the Transport Systems category, which I had estimated at 58% iron.
> 
> For transport, I'm calculating a production-weighted average across cars, ships, aircraft, and rail vehicles by their annual mass output. Ground vehicles dominate at roughly two-thirds of total production, ships account for about a quarter, and the rest is negligible by comparison. Since ships are extremely steel-heavy at around 85% while cars sit at 55% and aircraft lean heavily toward aluminum at 60%, the weighted composition comes out to roughly 62% iron, 10% aluminum, 7% carbon from polymers and composites, and smaller percentages of oxygen, copper, chromium, silicon, and other elements. I'm adjusting the composition to use 60% iron as a rounder number, which shifts oxygen up to 4% and other elements down to 9%, then reconsidering whether I can break down that "other" category further to be more specific about the trace elements. I've settled on a composition that totals 100% with iron as the base at 60%, aluminum and carbon as major components, and a balanced distribution of trace elements including copper, silicon, and various metals, with 8.5% allocated to other rare elements. Now I'm moving on to define the specific compositions for different manufactured goods categories, starting with heavy machinery. I'm verifying the final category compositions sum to 100 and reconsidering whether Heavy Machinery at 80% iron is realistic—for large equipment like excavators and turbines, that proportion seems reasonable given their steel-dominant construction, though I should account for hydraulic fluids, rubber components, and other materials more carefully. I'm verifying the composition adds up to 100% and adjusting the nickel percentage down since most heavy machinery relies on carbon steel rather than nickel alloys, settling on a final distribution that keeps iron dominant at 80% with smaller percentages of copper, aluminum, chromium, and other elements. The composition adds up to 100, so that works. I'm reconsidering whether aluminum at 3% makes sense for heavy machinery since steel dominates that application, so I'm reducing it to 2% and bumping copper to 3% instead. Now I need to format the final answer following the file's structure, which puts the composition on a new line after the description and reference standard. I'm finalizing the resource list with 58 total items across six categories, incorporating the structural changes like splitting Machinery into two types, renaming Refined Metals to Industrial Metals, and adding new categories like Solar Panels and Vehicles, while keeping the "other" entries formatted consistently with parenthetical lists. The Specialty Goods composition with higher copper and silicon from electronics and premium packaging, plus 9% allocated to precious metals in "other," captures the distinction from standard Consumer Goods appropriately. I'm ready to format the response now, deciding between prose or a structured list based on what the user requested. I need to verify that the extractable resources file content remains unchanged since the user's modifications only affected non-extractable items and class assignments, and I should note that Instruments doesn't exist as a separate entry in the current simulation since it was merged into Precision Equipment.

Here is the full 58-item list incorporating all changes: Fusion Fuels split into Deuterium and Helium-3; Ethane reassigned to Volatiles; Machinery split into Heavy Machinery and Precision Equipment (absorbing Instruments); Refined Metals renamed to Industrial Metals; Solar Panels added; Vehicles renamed to Transport Systems; Textiles renamed to Technical Textiles; Packaged Foods, Specialty Foods, and Specialty Goods added.

Compositions for Ores and Volatiles are taken directly from the extractable resources file. Compositions for Materials items represent the refined or blended commodity as traded. Compositions for Manufactured and Biological items are production-weighted estimates across the category and are necessarily more approximate; "other" fractions are larger for complex assemblies.

---

## Ores (13)

- **Iron/Nickel** — Fe-Ni metal alloy; ref. medium octahedrite. Fe 91, Ni 8, Co 0.5, P 0.2, other (S, C, Cr, Ga, Ge, Ir) 0.3.
- **Iron Ores** — Fe oxide/hydroxide; ref. high-grade hematite-goethite DSO. Fe 62, O 33, Si 2, Al 1, H 0.3, other (Mn, Ti, Ca, Mg, P) 1.7.
- **Aluminum Ores** — Al hydroxide; ref. lateritic bauxite (~50 % Al₂O₃). Al 26, O 56, Fe 10, H 3, Si 2, Ti 2, other (Ca, Mg, V, Cr) 1.
- **Industrial Metal Ores** — Production-weighted sulfide-oxide concentrate blend. O 30, Fe 15, S 10, Cr 8, Mn 7, Cu 6, Si 5, Zn 4, Ti 3, Ni 3, Al 2, Pb 1.5, Mg 1.5, Co 0.5, Sn 0.3, W 0.2, Mo 0.2, V 0.2, other (Ca, H, C) 2.6.
- **Precious Metal Ores** — PGM-Au-bearing sulfide concentrate; ref. Bushveld-type + lode gold blend. Fe 38, S 28, Si 10, O 8, Ni 3, Cu 3, Mg 3, Cr 2, Al 2, Ca 1, other 2. Precious metals (ppm): Pd 120, Au 100, Pt 80, Ag 50, Ru 20, Rh 15, Ir 5, Os 5.
- **Rare Earth Ores** — REE fluorocarbonate-phosphate; ref. bastnäsite-monazite concentrate (~60 % TREO). REE 47 (Ce 20, La 12, Nd 6, Pr 3, Y 2, Sm 1, Gd+Dy+HREE 3), O 27, F 5, C 5, Ca 4, Fe 4, Ba 3, Si 2, P 2, Th 1.
- **Radioactive Ores** — U-Th oxide concentrate; ref. ~1:1 U:Th by mass. U 34, Th 35, O 19, Si 6, Fe 3, other (Ca, Al, Ra) 3.
- **Sulfur** — Native S. S 100 (trace Se, Te, As).
- **Industrial Minerals** — Non-metallic mineral deposits; ref. production-weighted blend of limestone, dolomite, apatite, halite, gypsum, sylvite, clay. O 43, Ca 27, C 8, Cl 6, Na 3, Mg 3, Si 2, Al 2, P 2, K 2, S 1, other (F, H) 1.
- **Organics/Tholins** — Complex organic macromolecules (kerogen, tholins, meteoritic IOM). C 60, O 15, N 8, H 7, S 5, other (P, Fe, Cl) 5.
- **Coal** — Solid fossil carbon (Earth only); ref. high-volatile bituminous, as-received. C 75, O 12, H 5, Si 2, S 2, N 1.5, Al 1, Fe 0.5, other (Ca, K, Mg) 1.
- **Stone** — Bulk silicate rock; ref. primitive mantle pyrolite. O 44, Mg 23, Si 21, Fe 6, Ca 2.5, Al 2.3, Na 0.3, Cr 0.3, Ni 0.2, other (Ti, Mn, K, P) 0.4.
- **Regolith** — Surface fines and debris; ref. generic basalt (Moon/Mars/MORB average). O 44, Si 22, Fe 11, Ca 7, Al 7, Mg 5, Na 1.5, Ti 1.5, other (K, Mn, Cr, P) 1.

## Volatiles (14)

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
- **Helium** — He (natural, >99.99 % ⁴He). He 100.
- **Argon** — Ar. Ar 100.
- **Heavy Noble Gases** — Primarily Kr + Xe (ion propulsion, lighting); Ne included as co-product. Ref. Earth atmospheric mass ratio. Ne 77, Kr 20, Xe 3.
- **Oil** — Liquid petroleum (Earth only); ref. medium sour crude. C 85, H 12, S 2, N 0.5, O 0.3, other (V, Ni) 0.2.

## Energy (5)

- **Refined Hydrocarbons** — Liquid fuels refined from petroleum or synthesized from CO + H₂; gasoline, diesel, kerosene, fuel oil, LPG; ref. production-weighted liquid-fuel blend. C 86, H 13, S 0.5, O 0.3, N 0.2.
- **Deuterium** — Heavy hydrogen (²H₂); fusion fuel for D-D and D-T reactions. Extracted from water by isotope separation. Isotopically distinct from Hydrogen; elementally identical. H 100.
- **Helium-3** — Light helium isotope (³He); fuel for aneutronic D-³He fusion. Sources include lunar regolith implantation, gas-giant atmospheres, and tritium decay. Isotopically distinct from Helium; elementally identical. He 100.
- **Energy Storage** — Rechargeable batteries and high-capacity capacitors; ref. production-weighted Li-ion battery pack (NMC/LFP cell blend with aluminum structural housing, BMS, and thermal management). Fe 20, C 19, Al 18, O 14, Cu 10, Ni 4, P 2, F 1.5, H 1.5, Co 1, Mn 1, Li 1, other (Cr, Si, Sn, N, S, Zn) 6.
- **Fission Fuels** — Nuclear fuel assemblies; ref. PWR low-enriched UO₂ fuel rod with Zircaloy-4 cladding and steel/Inconel structural components. U 66, Zr 19, O 9, Fe 4, Cr 1, other (Ni, Sn, Nb, H) 1.

## Materials (12)

- **Steel** — Carbon and alloy steels; ref. production-weighted global blend (~85 % carbon, ~10 % stainless, ~5 % alloy/tool steel). Fe 96, Cr 2, Ni 1, Mn 0.5, C 0.2, Si 0.2, other (Mo, V, W, Cu, Ti, Nb, S, P) 0.1.
- **Aluminum** — Aluminum wrought and cast alloys; ref. production-weighted blend (6xxx, 3xxx, 2xxx, 7xxx series). Al 95, Si 1.2, Mg 1, Cu 0.8, Zn 0.5, Mn 0.5, Fe 0.4, other (Cr, Ti, Ni, V) 0.6.
- **Industrial Metals** — Production-weighted basket of refined base metals (excludes Fe and Al); ref. global primary + secondary metal output. Cu 27, Mn 21, Zn 15, Cr 14, Pb 13, Ti 4, Ni 3.5, other (Sn, Mo, Co, W, V, Nb, Sb, Bi) 2.5.
- **Precious Metals** — Refined precious-metal basket; ref. production-weighted output of Ag, Au, and PGMs. Ag 87, Au 11, Pd 0.7, Pt 0.6, other (Rh, Ru, Ir, Os) 0.7.
- **Rare Earth Elements** — Separated REE products (oxides and metals); ref. production-weighted blend (~70 % oxide form, ~30 % metallic). Ce 34, La 22, Nd 14, O 10, Y 6, Pr 4, Sm 2, Gd 2, Dy 1, other REE (Tb, Ho, Er, Eu, Yb, Lu, Tm, Sc) 5.
- **Semiconductor Materials** — High-purity elemental and compound semiconductors; ref. production-weighted blend dominated by solar- and electronic-grade polysilicon, with minor III-V and II-VI compound contributions. Si 97, other (Ga, Ge, As, In, Cd, Te, Se, N, C, O, P, B) 3.
- **Polymers & Composites** — Thermoplastics, thermosets, elastomers, and fiber-reinforced composites; ref. production-weighted blend (PE 35 %, PP 25 %, PVC 15 %, PET 8 %, PS 5 %, PUR 5 %, other 7 %). C 74, H 11, Cl 9, O 4, N 0.5, S 0.5, other (Si, F, Br) 1.
- **Glass & Ceramics** — Flat glass, container glass, technical ceramics, refractory ceramics; ref. production-weighted blend (~85 % soda-lime glass, balance borosilicate, alumina, and magnesia). O 47, Si 31, Na 8, Ca 6, Mg 4, Al 3, other (B, K, Fe, Ti, Zr, Ba) 1.
- **Concrete** — Portland-cement concrete; ref. structural ready-mix with mixed limestone/silica aggregate, ~12 % cement, ~8 % water. O 50, Ca 20, Si 20, C 5, Al 1, H 1, Fe 1, Mg 0.5, other (Na, K, S, Mn, Ti) 1.5.
- **Bulk Chemicals** — Commodity inorganic and organic chemicals; lime, sulfuric acid, chlor-alkali products, soda ash, methanol, ethanol, nitric acid, phosphoric acid, and other large-volume industrial chemicals; ref. production-weighted blend. O 42, Ca 22, C 9, Cl 7, S 7, Na 5, H 3.5, N 1.5, P 1, other (Mg, K, Fe, Si, Al) 2.
- **Fine Chemicals** — Specialty chemicals, catalysts, coatings, adhesives, pigments, agrochemicals, electronic-grade reagents, pharmaceuticals; ref. production-weighted blend dominated by paints/coatings and specialty polymer additives. C 35, O 26, H 6, Ti 3, Si 4, N 4, Ca 3, S 3, Cl 3, Al 2, Fe 2, Na 2, P 1, other (K, Ba, Cr, Zn, Cu, F, Mg, Sn, B) 6.
- **Natural Fibers** — Plant and animal textile fibers; ref. production-weighted blend (~80 % cotton, balance jute, flax, hemp, wool, silk). C 45, O 48, H 6, N 0.5, other (S, K, Ca, Mg, Fe, Si, P) 0.5.

## Manufactured (10)

- **Heavy Machinery** — Turbines, engines, pumps, compressors, heat exchangers, mining/construction equipment, cranes, pressure vessels, industrial furnaces; ref. sector-averaged bill of materials. Fe 80, Al 2, Cu 3, Cr 2, C 2, O 2, Ni 1, Mn 1, Si 1, H 0.5, Zn 0.5, other (Mo, W, Ti, Co, Sn, V, N, S, P, Cl, Ca) 5.
- **Precision Equipment** — Scientific instruments, medical devices, robotics, CNC machines, lithography systems, optical systems, laboratory equipment, navigation/guidance systems; ref. sector-averaged bill of materials. Fe 23, Al 20, C 16, O 9, Cu 8, Cr 5, Si 4, Ni 3, H 2, Ti 1.5, N 1, Cl 1, other (Co, W, Na, Ca, Mn, Sn, Mg, Mo, Zn, B, Ba, S, P, F, Au, Ag) 6.5.
- **Electronics** — Semiconductors, computing hardware, communication equipment, displays, sensors, circuit boards; ref. production-weighted blend of consumer and industrial electronics. O 20, Cu 15, C 14, Si 12, Fe 7, Al 7, Sn 3, H 3, N 2, Na 2, Ca 2, Ni 1.5, Ba 1, Cl 1, other (Ag, Au, Mg, Cr, Zn, Ti, In, Ga, Ta, F, S, P, K, Mn, W, Pb) 9.5.
- **Electrical Equipment** — Generators, transformers, electric motors, power cables, switchgear, power distribution panels, lighting, power electronics, fuel cells; ref. sector-averaged bill of materials. Fe 45, Cu 24, Al 6, C 6, O 3.5, Cl 2, Cr 1.5, Si 1.5, H 1, Ni 1, Mn 0.5, Zn 0.5, Sn 0.5, other (Mo, N, S, P, Na, Mg, Ca, Ba, Ti, W) 7.
- **Solar Panels** — Photovoltaic modules; ref. crystalline silicon module (~95 % c-Si market share) with tempered glass front, aluminum frame, EVA encapsulant, polymer backsheet, and copper interconnects. O 36, Si 26, Al 10, C 8, Na 7, Ca 5, Cu 3, H 1, Mg 1, other (Ag, Sn, Fe, F, N, B, Cl, Pb) 3.
- **Transport Systems** — Automobiles, trucks, ships, rail vehicles, aircraft, spacecraft, cargo tugs, rovers; ref. production-weighted bill of materials dominated by ground vehicles and ships. Fe 60, Al 10, C 7, O 4, Cu 2, Si 2, H 1.5, Cr 1, Mn 1, Zn 1, Ni 1, Mg 0.5, Cl 0.5, other (Ti, Sn, Mo, V, W, Co, Na, Ca, N, S, P, F, Pb) 8.5.
- **Technical Textiles** — High-performance fabrics for industrial, protective, and aerospace applications; aramid, UHMWPE, carbon fiber cloth, PTFE membranes, glass fiber textiles, multilayer insulation, filtration media; ref. production-weighted blend. C 65, O 21, H 7, N 3, F 1.5, Si 1, other (Ca, Al, B, Mg, Fe, Na, S, Cl) 1.5.
- **Consumer Goods** — Mass-produced everyday goods; furniture, clothing, housewares, paper products, personal care products, building finishes; ref. production-weighted blend. C 44, O 23, Fe 10, H 6, Si 3, Cl 3, Al 1.5, Na 1, Ca 1, Cu 1, N 0.5, Zn 0.5, other (Cr, Ni, Mn, Mg, Ti, S, P, K, Sn) 5.5.
- **Specialty Goods** — High-value, low-mass luxury and specialty items; premium personal electronics, fine craftsmanship, jewelry, art, premium fashion, recreational equipment; ref. estimated blend. C 30, O 18, Fe 10, Al 7, Cu 6, Si 6, H 4, Cr 2, Ni 2, N 1.5, Na 1.5, Cl 1, Ca 1, Zn 1, other (Sn, Ag, Au, Ti, Mo, Mg, S, P, F, Co, Mn, K, Pt, Pd) 9.
- **Steel Structures** — Structural steel products: beams, columns, plate, rebar, sheet, pipe, rail, wire; ref. production-weighted mix of structural shapes, predominantly galvanized carbon steel. Fe 93, Zn 2, Cr 1.5, Mn 1, Ni 0.5, C 0.3, Si 0.3, other (Mo, V, Cu, Al, Ti, S, P, O) 1.4.

## Biological (4)

- **Crop Products** — Raw harvested plant products (dry-equivalent basis); cereals, oilseeds, legumes, sugar crops, vegetables, fruit, roots/tubers; ref. production-weighted global harvest. C 42, O 44, H 6, N 2, K 2, Si 1, P 0.5, S 0.3, Ca 0.3, Mg 0.3, Cl 0.2, Na 0.1, other (Fe, Mn, Zn, B, Cu, Mo) 1.3.
- **Animal Products** — Raw animal-derived products (dry-equivalent basis); meat, dairy, eggs, fish, hides; ref. production-weighted blend. C 50, O 26, H 7, N 8, Ca 2, P 1.5, K 1, S 0.8, Na 0.5, Cl 0.3, Fe 0.3, Mg 0.2, other (Zn, Mn, Se, I, Cu, Co) 2.4.
- **Packaged Foods** — Processed shelf-stable food products including packaging; canned goods, frozen meals, dried foods, baked goods, confectionery, beverages; ref. production-weighted blend (~75 % processed food, ~25 % packaging by mass). C 39, O 32, Fe 7.5, H 6, Al 2.5, N 2, Si 2, Na 2, Cl 1, Ca 1, K 1, P 0.5, S 0.3, Mg 0.2, other (Sn, Cr, Zn, Mn, I, Ti) 3.
- **Specialty Foods** — Premium and artisanal food products with premium packaging; fine wines/spirits, specialty coffee/tea/chocolate, aged cheeses, exotic spices, gourmet meals; ref. estimated blend (~70 % food, ~30 % glass-heavy packaging). C 36, O 33, Si 6, H 5.5, Fe 3, Na 3, N 2, Ca 2, Al 1.5, K 1, Cl 0.5, Mg 0.5, S 0.3, P 0.3, other (Sn, Cr, Mn, Zn, Cu, I, B) 5.4.

---

## Notes

Biological items use dry-equivalent compositions. Actual traded moisture content varies widely (grain ~12 %, fresh produce ~90 %). Any moisture should be modeled as an admixture of the dry product with Water rather than being baked into the composition.

Packaged Foods and Specialty Foods include packaging mass in their compositions. This is deliberate: the packaging is inseparable from the delivered product and contributes substantially to both mass and material demand. It does mean these Biological-class items draw material inputs (Steel, Aluminum, Glass & Ceramics, Polymers & Composites) that are atypical for the class.

Deuterium and Helium-3 are elementally identical to Hydrogen and Helium respectively. Their distinct identities are isotopic. The simulation must track them as separate resource items despite sharing elemental symbols; any elemental-balance bookkeeping should treat ²H and ³He as functionally distinct elements for mass-flow purposes.

Manufactured item compositions carry larger "other" fractions (3–10 %) than extractable resources or refined materials (<3 %). This reflects the genuine complexity of assembled goods containing dozens of elements in sub-percent quantities. Many of these trace constituents (Au, Ag, Ta, In, Ga, Co, W in electronics and precision equipment) are functionally critical despite their small mass fractions.
