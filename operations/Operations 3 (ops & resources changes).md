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
