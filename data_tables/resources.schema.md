# Schema for resources.tsv

Resources are the physical and intangible items that are extracted, converted, consumed, stored, transported, and/or traded in the simulation.


## Entity Descriptions 

For complex tasks involving resources, it's important to understand what resources are supposed to represent. Read files in the `descriptions` directory as needed:

- physical_resources.md — This file describes all physical resources, excluding service (intangible) resources.
- extractable_resources.md — This file describes extractable resources only, a subset of physical resources. This file is sufficient when working only on the extractable subset (e.g., for strata composition).
- There are no descriptions for service resources yet.


## Fields

- name (1st column) — Resource name per data table instructions.
- resource_class — For GUI only; one of ENERGY, ORES, VOLATILES, MATERIALS, MANUFACTURED, BIOLOGICAL, or SERVICES (all but the latter correspond to groupings in physical_resources.md).
- commodity — BOOL value (default TRUE); specifies whether the resource is traded as a commodity.
- trade_class — One of ELECTRICITY, CRYOGENIC, LIQUID, ICE, BULK, PRECIOUS, or SERVICES. This effects how the resource is handled for trade and transport.
- trade_unit — Resource unit for trade and price display.
- start_price — This column is duplicated from #2015.
- #2015 (non-imported column) — Estimated price of the resource in 2015.
- #2025 (non-imported column) — Estimated price of the resource in 2025.
- #2035 (non-imported column) — Projected price of the resource in 2035.
- is_extraction — TRUE ("x") for extractable resources.
- is_volatile — TRUE ("x") for volatile resources.
