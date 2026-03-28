# Schema for modules.tsv

Modules are described in `modules.descriptive.md`.


## Fields

- name (1st column) — Module name per data table instructions.
- operations — Operations that the module provides capacity for.
- reconfig_time — Required time in days (d) to fully reconfigure a module from one operation (100%) to another (100%).
- reconfig_cost — Required cost in USD millions ($M) to fully reconfigure a module from one operation (100%) to another (100%).


## Notes

1. A module's size is arbitrary. One module provides capacity for one operation at 100% run rate, or multiple operations at fractional run rates that add to 100%.
2. A module's capacity can be shifted among its enabled operations (i.e., "reconfigured"). The speed and cost of this reconfiguration are fixed properties of each module and can represent anything from a major refitting to a minor operational adjustment. For mining operations at a particular stratum, this may represent an entire relocation of the mines (although portable equipment may be reused).
3. When implementing modules that support multiple operations, it may be necessary to re-normalize operation rows in `operations.tsv`. For example, COMBUSTION_POWER_PLANTS can be reconfigured from 100% METHANE_POWER to 100% ETHANOL_POWER; if the same base infrastructure can't support the same power output, then re-normalize operation rows as needed. Prefer to keep one row (the "best") as currently normalized.
