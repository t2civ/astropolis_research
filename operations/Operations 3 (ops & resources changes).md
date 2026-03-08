*Claude-Opus-4.6 (output effort = high). March, 2026.*

---
---

Note: Fungibility in the simulation is handled by having multiple operations that use the same infrastructure (the same "module" in simulation terms). The simulation can switch between operations within a module based on current resource prices. However, each operation has a fixed definition in terms of resource inputs and resource outputs (except extraction operations which depend on location). Co- and by-products are fine. However, distinct operations that focus on a main product will help the simulation switch between operations as resource prices evolve.

Provide the following file changes. All added or modified items need brief descriptions modeled on other items in the respective file.

In physical_resources.md:

- Add Carbon in the Materials group. This represents solid carbon as an output of Methane Pyrolysis or Carbonization, and as input for various manufacturing operations. Define the resource as it exists (or might exist) as an intermediate commodity.

In physical_operations.md

- Split Oil Power (as currently described) into Oil Power and Liquid Hydrocarbons Power.
- Split Ocean Extraction into Desalination and Brine/Ion Processing.
- Add either or both Gasification and Carbonization in the Conversions/Synthesis group with variants for different inputs. Coal Gasification is certainly significant on Earth. I think that Biomass Carbonization could be another source of Carbon with volatiles as co-products. I don't think we need Gasification and Carbonization for every possible input, so use your judgment on which variants to add.
- Rename (and adjust description of) Isotope Separation to Deuterium Purification. (We'll add ³He Purification later if we need it. That's also sourced directly from Volatiles Extraction in relevant strata.) 
- I'm confused about the relationship between Industrial Fermentation and Biofuel Production (and perhaps other related Biological operations). For example, from the names alone I wouldn't be able to guess which has Ethanol as an output. Please sort these out into better abstractions. Consider Cellular Agriculture as an addition. For implementation, each operation needs defined inputs and outputs as noted above (however, see General Instructions note 6 regarding the brief descriptions). We probably want distinct operations that produce as a main product: Methane, Ethanol, Methanol (?), Liquid Hydrocarbons (?), Biofeedstock, Biochemicals, and Algal/Microbial Products.

If you notice any important gaps in either physical_resources.md or physical_operations.md, please do mention them.

---
---

*Thinking...*

> Let me carefully analyze the requested changes and work through them systematically.
> 
> ## Changes Requested:
> 
> ### physical_resources.md:
> 1. Add **Carbon** in the Materials group - solid carbon as intermediate commodity
> 
> ### physical_operations.md:
> 1. Split Oil Power into Oil Power and Liquid Hydrocarbons Power
> 2. Split Ocean Extraction into Desalination and Brine/Ion Processing
> 3. Add Gasification and/or Carbonization variants in Conversions/Synthesis
> 4. Rename Isotope Separation to Deuterium Purification
> 5. Sort out biological operations for better abstractions around fermentation/biofuel
> 
> 
> ---
> 
> Now I'm working through each change. For Carbon, I need to add it as a solid intermediate commodity produced through methane pyrolysis or carbonization—covering carbon black, graphite, carbon fiber precursor, and activated carbon variants.
> 
> On the Oil Power split, I'm distinguishing between crude oil combustion (legacy, Earth-dominated) and refined liquid hydrocarbon combustion (gasoline, diesel, jet fuel, and synthetic fuels from Fischer-Tropsch or ISRU processes). The key difference is that Oil Power handles raw crude or heavy fuel oil, while Liquid Hydrocarbons Power covers refined products and synthetics.
> 
> For Ocean Extraction, I'm splitting this into Desalination for fresh water production via reverse osmosis or thermal distillation, and Brine/Ion Processing for recovering dissolved minerals and salts.
> 
> On Gasification and Carbonization, Coal Gasification is definitely significant on Earth as a major industrial process that converts coal through partial oxidation or steam gasification into synthesis gas. Biomass Carbonization could be another carbon source with volatiles as co-products, though I should use judgment about which variants are actually needed rather than listing every possibility. I'm settling on adding Coal Gasification, Biomass Gasification, and Biomass Carbonization as distinct operations since they represent different resource streams and produce important commodities. Coal Carbonization (coking) is tempting since it's a major industrial process that yields the Carbon commodity, but it's already somewhat captured in the steelmaking operation, so I'll leave it out to avoid redundancy. Coal Gasification, Biomass Gasification, and Biomass Carbonization seem like the right set—they cover the major variants and give distinct input streams. Coal Carbonization is tempting since coking is a massive industry and produces the Carbon resource explicitly, but the user wants restraint and it overlaps somewhat with steelmaking. I'll stick with three for now. Now I'm renaming Isotope Separation to Deuterium Purification and refocusing the description on separating deuterium from hydrogen and water sources. For the biological operations, I'm reviewing what I have—Industrial Fermentation, Biofuel Production, Biogas Production, and Algal Cultivation—and need to figure out which ones are actually necessary and how they fit together.
> 
> The core issue is that these operations overlap in confusing ways. Fermentation is a process that appears in multiple categories, so I need to clarify what each operation's main product actually is: methane from biogas, ethanol from fermentation, liquid hydrocarbons from biodiesel or hydrothermal liquefaction, and biochemicals from industrial fermentation. I'm thinking the cleanest approach is to separate these by their primary output rather than by the process used. I need to map the desired outputs back to specific operations. Methane comes from Biogas Production, ethanol from the new Ethanol Production category, and I should clarify what Industrial Fermentation produces—it should focus on biochemicals rather than fuels. For the remaining outputs like methanol and liquid hydrocarbons, I need to ensure each operation is distinct and clearly tied to its main product. Looking back at what the user actually asked for, they want me to clarify which operations produce specific outputs like ethanol, methane, and methanol as main products, and they're suggesting I consider adding Cellular Agriculture as a distinct operation. So I need to reorganize these biological operations around their primary outputs rather than their processes. I'm realizing that Biofeedstock might already be covered as a co-product through Open Agriculture, Open Forestry, and Algal Cultivation rather than needing its own dedicated operation. For Methanol from biological sources, that's really just a chemical conversion step handled by the existing syngas or biogas reforming pathways, so it doesn't need a separate biological operation. The main question is whether Algal Cultivation should be split into food-grade and industrial biofeedstock variants, or if it can handle both as distinct outputs. I'm realizing the pharmaceutical flow works well as-is: Industrial Fermentation produces biochemical intermediates that feed into Pharmaceuticals Synthesis, which then creates the final pharmaceuticals. So there's no real overlap there. Let me map out which operations to keep unchanged versus which ones need tweaking, starting with the ones that are working fine like Open Agriculture, Controlled-Environment Agriculture, and the fishing and forestry operations, then figure out what adjustments Algal Cultivation and the newer biotech operations actually need. I'm confirming the naming conventions match the existing pattern, so "Ethanol Fermentation" and "Biomass Liquefaction" work well. Biomethanol doesn't need its own operation since the gasification and synthesis pathways already cover that route. Now I'm moving forward with drafting the full list and checking for any gaps in the physical resources documentation. I'm noticing a few other potential gaps — textile manufacturing from biofibers could fit under Consumer Goods Manufacturing, and there's an interesting inconsistency where the energy section mentions heat is handled separately but I don't see a dedicated heat-only operation, which might be intentional or a framework limitation. Let me start drafting the actual file changes, beginning with adding Carbon as a resource.
> 
> Now I'm deciding where to place Carbon in the materials list — it makes sense to slot it among the industrial intermediates, somewhere between the metals and construction materials. Looking at the current ordering, I should probably position it after Semiconductor Materials and before Polymers to keep related industrial materials grouped together.
> 
> For the physical_operations.md changes, I'm refining the Oil Power description to be more specific about the fuel types and applications, and adding context about its declining role in modern energy systems.
> 
> Then I'm splitting Ocean Extraction into separate entries—starting with Desalination as its own operation focused on freshwater production and mineral recovery from seawater. I see the user wanted "Desalination" and "Brine/Ion Processing" as separate categories, so I'll use "Brine Processing" to avoid the slash in the constant name. Now I'm adding gasification and carbonization pathways — coal gasification produces synthesis gas from partial oxidation, biomass gasification does the same from organic feedstock, and I'm about to detail coal carbonization as well. I'm seeing edits to reorganize these industrial processes. The coal coking and biomass carbonization sections stay as-is, but I need to refocus the isotope separation entry on deuterium purification specifically, removing the helium-3 reference and clarifying the feedstock and product. Then I'm replacing the biofuel production section entirely with new biological operations. I'm adding three more fermentation and biomass conversion pathways: ethanol production from various sugar and cellulose sources with co-product streams, liquefaction processes that convert biomass into liquid fuels through hydrothermal and catalytic methods, and cellular agriculture systems that grow animal proteins in bioreactors.
> 
> Now I'm checking whether methane and methanol need their own operations—methane's already covered under biogas production, methanol synthesis is handled through gasification and syngas pathways, and biofeedstock comes from the agricultural operations upstream, so none of these need dedicated entries.
> 
> Let me verify this against what the user actually wants: methane maps to biogas production, ethanol needs its own fermentation operation, methanol can stay chemical-only, liquid hydrocarbons require biomass liquefaction, biofeedstock is a co-product of existing systems, biochemicals fit into industrial fermentation, and I need to figure out where algal and microbial products fit.
> 
> Algal Cultivation already covers that—it produces both the algal products and biofeedstock as outputs, so no modifications needed there. Now I'm thinking through where to position these new operations within the Biological section, starting with the existing order. I'm finishing out the reorganized list and settling on placing Cellular Agriculture right after Controlled-Environment Agriculture since it fits thematically with the other agriculture operations, even though it's more of an industrial bioprocess. Now I need to note the abbreviation conventions for the file—LIQUID becomes LIQ, INDUSTRIAL becomes INDUST, and SYNTHESIS stays as is. I'm noticing a few resource gaps in the simulation—depleted uranium from fuel manufacturing isn't being tracked for storage or reuse, Plutonium-238 production lacks an explicit pathway even though radioisotope power systems consume it, and general water treatment operations beyond desalination aren't explicitly modeled.
> 
> There's also no disposal operation for radioactive waste despite recycling recovering materials, and while oxygen appears in multiple places (extractable volatiles and produced by various operations), that's probably intentional for the simulation. The flows for liquid hydrocarbons and helium extraction look correct, but I should clarify the pathways for Pu-238 and Sr-90 production in reactor operations since those are critical for RTG systems.
> 
> Now I'm finalizing the response with file changes and feedback. The user specified abbreviations for physical_resources.md—INDUSTRIAL becomes INDUST, MATERIALS becomes MAT, PRODUCTS becomes PROD—so Carbon stays as CARBON. For the reference composition, I'm weighing the production mix: metallurgical coke dominates at around 75%, carbon black at 15%, and other sources at 10%, which gives a blend of roughly 92-93% carbon with sulfur as a notable impurity.
> 
> I've settled on a simplified composition: C 93, O 1.5, Si 1.2, Fe 0.8, Al 0.8, S 0.7, Ca 0.5, H 0.4, N 0.3, Mg 0.2, and other trace elements totaling 0.6, which adds up to 100.
> 
> But wait, metallurgical coke is typically lower in carbon—around 87-90%—with ash content between 8-12%. Carbon black is much higher at 97-99% carbon. So I need to recalculate based on the actual blend composition. I'm breaking down the three main components: metallurgical coke making up about 75% of the blend, carbon black at 15%, and biochar or activated carbon at 10%, then weighting their individual compositions to get the final blend.
> 
> Calculating the weighted averages for each element across the three materials...
> 
> I'm realizing 89% carbon is actually lower than expected for this commodity. Since the resource description specifies solid carbon from methane pyrolysis or carbonization meant for manufacturing, I should reference something cleaner than raw coke—methane pyrolysis produces extremely pure carbon, often exceeding 95-99%. Let me adjust to a composition that better reflects a processed, intermediate commodity: around 95% carbon with trace amounts of oxygen, silicon, sulfur, iron, and other minor elements. This represents a blend weighted toward the purer carbon products like pyrolysis carbon and carbon black rather than raw metallurgical coke.
> 
> I've settled on a reference composition of 95% C with the remaining 5% distributed across sulfur, oxygen, hydrogen, silicon, iron, aluminum, calcium, nitrogen, and other trace elements—this sums to 100 and better captures what an intermediate carbon commodity would look like. Now I'm checking whether ethane and methanol are actually listed as extractable resources in the system, which seems worth verifying. I'm overthinking the level of detail here—I should keep the descriptions straightforward. For Biomass Liquefaction, I'll make sure to note that it covers algae-derived oils as well since they convert to liquid hydrocarbons through similar processes. As for where to place solid carbon in the materials list, it makes sense to put it after Thorium and before Concrete since it's a fundamental elemental material used across multiple industries, even though it bridges both inorganic and organic applications.
> 
> Now I'm ready to present the full response with the reorganized Biological section. I'm restructuring the operations to move Biofuel Production out and add Cellular Agriculture and Ethanol Fermentation in their proper places, while also modifying Industrial Fermentation to reflect these changes. I'm organizing the biological operations into clear categories—food production, crafting, material production, processing, and support—and the current ordering looks solid. For the code blocks, I need to show the specific additions and modifications rather than full file rewrites, especially for the Biological section with its multiple changes. I'm also checking the abbreviations note in physical_operations.md to see if any new abbreviations are needed for the new items, and it looks like they all fit within the existing abbreviation guidelines without requiring anything new.
> 
> Now I'm reconsidering the Carbon resource definition. Since the user specified it should be an intermediate industrial commodity that exists or might exist as a tradeable blend, I should adjust the composition to be more forward-looking and less coke-heavy, especially given that methane pyrolysis is explicitly mentioned as a source. I'm settling on a production-weighted blend that balances metallurgical coke, carbon black, and pyrolysis-derived carbons, with specific elemental compositions for each component to create a cleaner, more representative intermediate product. I'm refining the composition to be cleaner and less dominated by coke ash, settling on a simpler breakdown with carbon as the primary component and trace elements distributed more evenly.
> 
> Now I'm moving on to finalize the Brine Processing description, thinking through what specific chemicals and minerals it would extract from the resources available.
> 
> I'm realizing I need to decide whether both Desalination and Brine Processing should stay in the Extraction group, even though Desalination is more of a conversion operation. I think keeping them together makes organizational sense since they're both ocean-based operations.
> 
> I've been overthinking the details here, so I'm going to write out the final response now with a brief summary of changes followed by the updated code.
> 
> One more thing on naming—I'll stick with Biomass Liquefaction since it converts cleanly to BIOMASS_LIQUEFACTION, and I should make sure to note that Biofuel Production is being replaced by this new operation.
> 
> Now let me verify the coverage: Ethanol Fermentation handles the fermentation pathway, Biomass Liquefaction covers both transesterification and hydrothermal liquefaction, and both operations account for co-products. Everything from the old Biofuel Production description maps across, so I'm ready to write out the changes.
> 
> Actually, I need to clarify the abbreviation rules first. Looking at the pattern—SYNTHESIS becomes SYNTH, ENVIRONMENT becomes ENV—abbreviations apply only to specific words in the list. So for my new operations, I'd use CO2_METHANOL_SYNTH format, but something like COAL_GASIFICATION stays as-is since GASIFICATION isn't on the abbreviation list. Let me apply this consistently across all the new operation names.
> 
> Now I'm ready to finalize the response, but I should also think through what Cellular Agriculture would produce—likely cultured animal products and precision-fermented proteins—though I'll keep the descriptions general since the specific resource mappings will be defined elsewhere.
> 
> I notice a couple of gaps worth flagging: Helium-3 purification from volatiles extraction is intentionally deferred as the user mentioned, and there's no pathway for Plutonium-238 or Strontium-90 production, though those might be intentional omissions too. for RTGs is implicit in reactor operations but not tracked as an explicit operation or resource. This could be a gap.
> 
> **No explicit Coking operation** — wait, I'm adding Coal Carbonization which IS coking. Good.
> 
> **DU (Depleted Uranium)** — LEU and HEU Fuel Manufacturing produce depleted UF₆ tails. Currently no resource for this. It's mentioned in the operation descriptions but not tracked.
> 
> **Propellant/reaction mass** for space operations — water, hydrogen, methane are listed as propellants but the actual propulsion operations aren't in this file. The note says "Yards & Transport Manufacturing" covers spacecraft and "fixed infrastructure and large transport vehicles (e.g., heavy rockets) are handled separately." So propulsion is handled elsewhere.
> 
> **No explicit operation for rare-earth magnet production** — this would be part of the rare earths value chain but might be considered covered by Industrial Metals Refining + Electronics Manufacturing / Heavy Machinery Manufacturing.
> 
> Now I'm looking at the carbon composition across existing resources to make sure the new Carbon resource fills a real gap. Coal, organics, polymers, and textiles all contain carbon but none represent pure solid carbon as an industrial commodity, so adding this makes sense. Carbon also appears in composites at a lower concentration, and the new resource would naturally feed into composite production as an input.
> 
> Thinking through the flow: Carbon would be an input to steelmaking operations as a reductant, to electric steelmaking as electrode material, and to composite production as a fiber precursor. It would be produced from methane pyrolysis, coal carbonization, and biomass carbonization. This all tracks logically.
> 
> For naming, I'm settling on "Brine Processing" as the cleaner term for what the user called "Brine/Ion Processing." Now I need to figure out where to place these two new extraction operations in the sequence. The current list has Ocean Extraction at position 4, so I'll replace that with Desalination first (higher-volume output), then Brine Processing right after.
> 
> Now I'm organizing the actual response by file and by the groups that need changes: Energy operations get split, Extraction gets restructured with the new water-related processes, and I'll need to touch Refining, Conversions, and Biological sections as well. I'll present this with clear sections showing the targeted modifications.
> 
> For the biological operations, I'm reconsidering whether "Biomass Liquefaction" properly captures what remains after pulling out Ethanol Fermentation—it should handle hydrothermal liquefaction and transesterification pathways that convert solid biomass into liquid hydrocarbons, which feels like the right abstraction. I'm settling on Biomass Liquefaction as the operation name since it clearly describes the biological conversion process. Now I need to figure out what resource it should produce—looking at the available options, bio-crude from hydrothermal liquefaction would require upgrading to become drop-in fuels, but at the simulation's abstraction level, that upgraded product would be functionally equivalent to Liquid Hydrocarbons. Biodiesel from transesterification also fits as a diesel substitute. So outputting Liquid Hydrocarbons makes sense here.
> 
> I'm satisfied with these decisions and ready to finalize the response. I've got the structure mapped out for the physical_operations.md file with all the sections that need updating. Now I'm drafting the actual response to walk through these changes.

Here are the proposed changes, organized by file and section, followed by discussion of gaps and feedback.

---

## physical_resources.md — Materials group

Add the following item between **Thorium** and **Concrete**:

```markdown
- **Carbon** — Solid carbon: metallurgical coke, carbon black, biochar, graphite, activated carbon, and pyrolysis carbon; ref. forward-weighted blend of metallurgical coke (~50%), carbon black (~20%), and pyrolysis/graphite/activated carbon (~30%). C 95, S 0.6, O 1, Si 0.7, H 0.4, Fe 0.4, Al 0.4, Ca 0.3, N 0.3, Mg 0.15, other (K, Na, Ti, Mn, P) 0.75.
```

---

## physical_operations.md — Energy group

Replace the existing **Oil Power** item with two items:

```markdown
- **Oil Power** — Electricity and heat from direct combustion of crude oil and heavy fuel oil in industrial boilers and large-scale generators. Legacy pathway limited to petroleum-surplus regions. Earth-only.
- **Liquid Hydrocarbons Power** — Electricity and heat from combustion of refined liquid fuels (diesel, kerosene, gasoline) and synthetic liquid fuels in reciprocating generators and gas turbines. Applicable wherever stored liquid fuels are available, including off-Earth stations using ISRU-derived Fischer-Tropsch fuels.
```

---

## physical_operations.md — Extraction group

Replace the existing **Ocean Extraction** item with two items (same position in the list):

```markdown
- **Desalination** — Production of fresh water from seawater, brackish water, or brine by reverse osmosis, multi-stage flash distillation, or electrodialysis. Produces concentrated brine reject as a co-product. On ocean worlds, applicable to melt-probe or plume-sourced subsurface water requiring purification.
- **Brine Processing** — Recovery of dissolved minerals and salts from ocean water, concentrated desalination reject, subsurface brines, and evaporite-sourced solutions via selective ion exchange, fractional crystallization, solvent extraction, and solar or thermal evaporation. Target products include magnesium, lithium, potash, bromine, sodium chloride, and other dissolved species. On ocean worlds, targets dissolved solutes accessed via plumes, melt probes, or ice penetrators.
```

---

## physical_operations.md — Refining group

Replace the existing **Isotope Separation** item:

```markdown
- **Deuterium Purification** — Cryogenic distillation, electrolysis cascade, or thermal-cycling separation of deuterium from hydrogen or water. Produces fusion-grade deuterium from natural-abundance hydrogen or water feedstock.
```

---

## physical_operations.md — Conversions/Synthesis group

Add the following four items. Suggested placement: after **Methane Pyrolysis** (since they are thermochemical conversion processes on similar feedstocks), before **CO₂ Methanation**:

```markdown
- **Coal Gasification** — Partial oxidation and steam gasification of coal to produce synthesis gas (CO + H₂) with carbon dioxide and ash as co-products. Major industrial hydrogen and chemical feedstock pathway on Earth; syngas feeds downstream Fischer-Tropsch, methanol, and ammonia synthesis.
- **Biomass Gasification** — Thermal gasification of biofeedstock, crop residues, and organic waste with steam and/or oxygen to produce synthesis gas (CO + H₂). Renewable syngas pathway for downstream fuel and chemical synthesis; applicable wherever biomass or organic waste is available.
- **Coal Carbonization** — Pyrolysis of coal in sealed coke ovens to produce metallurgical coke and co-product volatiles including coal tar, ammonia, and coke-oven gas (hydrogen, methane, carbon monoxide). Primary source of solid carbon for blast-furnace steelmaking and electrode production on Earth.
- **Biomass Carbonization** — Slow pyrolysis of biofeedstock and wood to produce biochar and solid carbon with volatile co-products including methane, carbon monoxide, carbon dioxide, and condensable bio-oil. Provides a renewable solid-carbon source for metallurgical, agricultural (soil amendment), and composite-precursor applications.
```

---

## physical_operations.md — Biological group

This set of changes replaces **Biofuel Production** with two focused operations, refocuses **Industrial Fermentation**, and adds **Cellular Agriculture**. The complete revised Biological section:

```markdown
## Biological

- **Open Agriculture** — Conventional cultivation of crops and raising of livestock on open land under ambient atmospheric conditions. Produces plant-based foods, animal-based foods, fiber crops, and biomass feedstock. Applicable on Earth and in any sufficiently terraformed open environment, including O'Neill cylinder interiors.
- **Controlled-Environment Agriculture** — Sealed, controlled-environment crop cultivation and animal husbandry in pressurized greenhouses, space station grow modules, or planetary surface habitats. Requires artificial or filtered lighting, active atmosphere management (CO₂ supply, humidity control), water recycling, and nutrient delivery. Co-produces oxygen via photosynthesis.
- **Cellular Agriculture** — Production of animal-equivalent proteins and food products via mammalian cell culture and precision fermentation in bioreactors: cultured meat, dairy proteins, egg proteins, and structured fats. Consumes water, sugars, amino acids, and growth-factor nutrients in sterile controlled-environment facilities. Potentially significant for off-Earth habitats where traditional animal husbandry is impractical.
- **Artisanal Farming** — Small-scale family-farm and handcraft food production: artisan cheeses, charcuterie, preserves, craft beverages, baked goods, and other specialty food products. Represents culturally significant, high-value-per-mass food production distinct from industrial agriculture and food processing.
- **Artisanal Crafting** — Small-scale handcraft production of non-food goods: pottery, handmade textiles, woodcraft, leatherwork, artisanal soap, candles, and decorative items. Represents culturally significant, high-value-per-mass goods production distinct from industrial manufacturing.
- **Commercial Fishing** — Wild-capture marine and freshwater harvest of fish, shellfish, and other aquatic organisms. Produces animal products for food processing and direct consumption. Distinct from aquaculture (farmed production). Primarily Earth-based.
- **Open Aquaculture** — Marine and freshwater farming of fish, shellfish, seaweed, and microalgae in open water bodies.
- **Controlled-Environment Aquaculture** — Recirculating sealed-system aquatic farming for fish, shellfish, and algae. Essential protein and micronutrient source for isolated off-Earth habitats.
- **Open Forestry** — Managed cultivation and harvest of trees for timber, pulp, and biomass in open conditions.
- **Food Processing** — Cooking, preservation (retort, freeze-drying, dehydration, thermostabilization), and packaging of agricultural and aquacultural products into shelf-stable prepared meals. Critical for space operations where fresh-food supply is intermittent.
- **Algal Cultivation** — Photobioreactor and open-pond cultivation of microalgae (Spirulina, Chlorella, Nannochloropsis) and cyanobacteria. Produces food-grade algal biomass and industrial biofeedstock from water, CO₂, nitrogen, and trace mineral nutrients. Co-produces oxygen via photosynthesis. Key food, feedstock, and life-support pathway for off-Earth habitats.
- **Industrial Fermentation** — Aerobic and anaerobic bioreactor fermentation (bacterial, yeast, fungal) and enzymatic processing to produce biochemicals: industrial enzymes, amino acids, organic acids, biopolymers, and pharmaceutical intermediates. Consumes organic carbon substrates, water, nitrogen, and trace nutrients. Core pathway for industrial biotechnology distinct from fuel-producing fermentation.
- **Ethanol Fermentation** — Yeast or bacterial fermentation of sugars, starches, and cellulosic biomass to produce ethanol. Consumes crop-derived feedstock, biofeedstock, or algal sugars with water and nutrients. Co-produces carbon dioxide and organic residues (distillers grains, vinasse) usable as animal feed or soil amendments.
- **Biogas Production** — Anaerobic digestion of organic waste: biowaste, crop residues, and sewage sludge decomposed by microbial consortia to produce methane-rich biogas and nutrient-dense digestate. Biogas is upgraded to pipeline-grade methane or used directly for power generation; digestate is recoverable as fertilizer. Critical waste-processing and energy-recovery pathway for closed-loop off-Earth habitats.
- **Biomass Liquefaction** — Hydrothermal liquefaction, catalytic pyrolysis-upgrading, and transesterification of biological lipids and wet biomass to produce liquid hydrocarbons (renewable diesel, bio-crude, biodiesel). Consumes biofeedstock, algal biomass, or crop-derived oils. Co-produces organic residues and water.
- **Pharmaceuticals Synthesis** — Chemical synthesis, fermentation, purification, and formulation of drugs, vaccines, biologics, and medical compounds. Consumes biochemical intermediates, fine chemicals, water, and packaging materials (glass vials, polymer closures, foil laminates).
- **Soil Conditioning** — Preparation of growth media from raw regolith, mineral amendments, organic waste, water, and microbial inoculants. Includes composting, pH adjustment, nutrient loading, and toxin remediation (e.g., perchlorate removal for Mars regolith). Critical ISRU step for establishing agriculture on extraterrestrial surfaces.
```

Changes summarized: **Biofuel Production** is removed and replaced by **Ethanol Fermentation** (main product: ethanol) and **Biomass Liquefaction** (main product: liquid hydrocarbons). **Industrial Fermentation** is refocused on biochemicals with a clarified description. **Cellular Agriculture** is added after Controlled-Environment Agriculture.

Regarding methanol and biofeedstock as main products: biological methanol production is marginal — methanol is better served by the existing CO₂ Methanol Synthesis and CO Methanol Synthesis operations downstream of Biomass Gasification. Biofeedstock is already a co-product of Open Agriculture, Open Forestry, and Algal Cultivation, so a dedicated operation isn't needed.

---

## Rationale for gasification/carbonization variant selection

I included four variants covering two feedstocks × two process types:

**Coal Gasification** and **Biomass Gasification** are distinct because their inputs (Coal vs. Biofeedstock) have very different availability profiles and the simulation needs separate operations per resource stream (per note 3). Coal gasification is the larger current industrial pathway; biomass gasification provides the renewable-syngas route.

**Coal Carbonization** (coking) is included because it is the dominant real-world source of the new Carbon resource, producing ~1.1 Gt/yr of metallurgical coke globally, and its volatile co-products (H₂, CH₄, NH₃, coal tar) are industrially significant. It's currently embedded implicitly in the Basic O₂ Steelmaking description but deserves an explicit operation now that Carbon is a tracked resource.

**Biomass Carbonization** is included per your suggestion as a renewable Carbon source with distinct volatile co-products.

I did not add variants for organics/tholins, waste, or other feedstocks — those can be added later if the simulation needs finer resolution.

---

## Gaps and feedback

**Gaps I notice in the current files:**

The most significant gap is **depleted uranium**. Both LEU and HEU Fuel Manufacturing explicitly produce depleted UF₆ tails as a by-product, and DU has real uses (radiation shielding, kinetic penetrators, fertile blanket material for breeder reactors), but there is no resource entry to track it. It could be added as a Materials item or folded into the existing Uranium resource with a note, depending on whether the simulation needs to track the enrichment tails stream separately.

**Plutonium-238 and Strontium-90 for RTGs** are mentioned as inputs to Radioisotope Power but have no production pathway — they are bred in reactors (and extracted via reprocessing for Pu-238, or from fission-product waste for Sr-90). This is a sourcing gap unless RTG fuel production is considered implicit in Uranium Fuel Reprocessing.

**Wastewater treatment / general water purification** (as distinct from desalination) is absent. For Earth and for closed-loop habitats, municipal and industrial water treatment is a massive operation. It might be handled as part of life-support infrastructure, but it's worth noting.

**3D printing / additive manufacturing** is not listed and could be significant for off-Earth operations where subtractive machining wastes scarce material. It might be considered implicit in existing manufacturing operations, but dedicated operations for powder-bed fusion, fused deposition, etc., could be added if the simulation needs to distinguish additive from subtractive manufacturing.

**Helium purification and liquefaction** — Helium is listed as an extractable resource (co-product of Gas Drilling and Atmosphere Separation on gas giants), but there's no explicit refining/purification operation. This is less critical than the others since the simulation may handle it implicitly in the extraction step.

**Prompt/context file feedback:**

Note 4 in physical_resources.md skips from 3 to 5 — there is no note 4. This appears to be a numbering typo.

The relationship between Coal and Carbon could use a brief cross-reference now that Carbon exists as a separate resource. Currently, Basic O₂ Steelmaking says it "consumes coal via integrated coking" — if Coal Carbonization is a separate operation producing Carbon, the steelmaking description may eventually need updating to consume Carbon (coke) rather than Coal directly, or both, depending on how you want the simulation to route coking.
