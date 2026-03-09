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