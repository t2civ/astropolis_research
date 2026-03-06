Note: Fungibility in the simulation is handled by having multiple operations that use the same infrastructure (the same "module" in simulation terms). The simulation can switch between operations within a module based on current resource prices. However, each operation has a fixed definition in terms of resource inputs and resource outputs (except extraction operations which depend on location). Co- and by-products are fine. However, distinct operations that focus on a main product will help the simulation switch between operations as resource prices evolve.

Provide the following file changes. All added or modified items need brief descriptions modeled on other items in the respective file.

In physical_resources.md:

- Add "Carbon" in the Materials group. This represents solid carbon as an output of Methane Pyrolysis or (possibly) Carbonization, and as input for various manufacturing operations. Define the resource as it exists (or might exist) as an intermediate commodity.

In physical_operations.md

- Split Oil Power (as currently described) into Oil Power and Liquid Hydrocarbons Power.
- Add either or both Carbonization and Gasification in the Conversions/Synthesis group. These generate potentially valuable volatiles from Biomass. The former is also a pathway to the Carbon resource, which seems useful. I'm not sure if both are needed since Carbonization seems to give us everything that Gasification does (?). If you think multiple inputs might be useful, add these as separate operations (e.g., Biomass Carbonization, etc.).
- I'm confused about the relationship between Industrial Fermentation and Biofuel Production (and perhaps other related Biological operations). For example, from the names alone I wouldn't be able to guess which has Ethanol as an output. Please sort these out into better abstractions. For implementation, each operation needs defined inputs and outputs as noted above (however, see General Instructions note 6 regarding the brief descriptions). We probably want operations that produce as a main product: Methane, Ethanol, Methanol (?), Liquid Hydrocarbons (?), Biofeedstock, Biochemicals, and Algal/Microbial Products.  



