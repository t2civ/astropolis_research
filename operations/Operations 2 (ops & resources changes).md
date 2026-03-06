## Changes to physical_resources.md

- Add Spent Uranium Fuel and Spent Thorium Fuel to the Materials group. Modify Radioactive Waste to account for these additions.

I'm trying to limit growth of physical resources as much as possible. However, if you discover important gaps or flaws in physical resources (related to changes and analyses below), do suggest additions or other changes.


## Changes to physical_operations.md

- Split Nuclear Fuel Fabrication into 3 separate operations: LEU Fuel Fabrication, HEU Fuel Fabrication, and Thorium Fuel Fabrication. Absorb Uranium Enrichment into the uranium fuel fabrications (this is necessary because we don't have enriched uranium as an intermediate).
- Split Spent Fuel Reprocessing into Uranium Fuel Reprocessing (abstracts both LE and HE) and Thorium Fuel Reprocessing.
- Split Chemical Synthesis into Industrial Chemicals Synthesis and Fine Chemicals Synthesis.
- Remove Coal Processing and absorb it into Coal Power and Steelmaking (basic oxygen). We don't have grades of coal or a coke intermediate resource, so Coal has to be a direct input to the two operations that use it (however, we can keep rare-earth elements or other by-products in either of these).
- Split Methane Power into combustion and fuel cell variants (this is necessary for simulation handling of heat). I think it's clear enough to have these named Methane Power and Methane Fuel Cells, respectively. Hydrogen needs a similar treatment. However, I do wonder if hydrogen combustion power is really needed in the simulation; if you think that's not likely to be significant, then just add Hydrogen Fuel Cells and drop Hydrogen Power. Add combustion and fuel cell variants as likely to be relevant for ethanol and methanol.
- Add Construction & Infrastructure to the Manufacturing group. This single operation encompasses production of all fixed structures.
- Add Commercial Fishing to the Biological group. This covers marine harvest not covered by Open Aquaculture.
- Split Artisanal Production into Artisanal Farming and Artisanal Crafting (corresponding to production of the 2 "artisanal" physical resources).

Please look over physical operations for additional gaps. We need at least one operation that produces (or mainly produces) each physical resource that is either not extractable or very constrained in its extraction availability (e.g., hydrogen and methanol). The converse is not necessarily a problem: we can have resources produced only as co- or by-products, although it's desirable to have alternative sourcing where such is possible. Conversions, Synthesis, and Biological groups are each a little sparse, so I wouldn't mind expanding these with additional items or splitting existing items. (I think Bioprocessing might be a good candidate for splitting.)