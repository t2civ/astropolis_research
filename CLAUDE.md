# Astropolis Research

AI-assisted research for Astropolis simulation content data.

- [Astropolis homepage](https://t2civ.com/)
- [Astropolis public code](https://github.com/t2civ/astropolis_sdk)


## Background

The purpose of this repository is to collect descriptive information, develop abstractions, and generate input data for Astropolis, a simulation that encompasses future industrial and economic development of the solar system. The simulation focuses on resource streams: extraction, conversion, production, commodity trading, etc. It starts at present-day activities on Earth, but runs forward in time to include highly speculative (yet physically plausible) activities in the wider solar system. Abstractions, simplifications, approximations, and speculations are necessary. However, the overall simulation should adhere (at least approximately) to basic physical principles such as mass balance, energy conservation, etc.


## File Organization

- Instructions, table schema, and entity descriptions are in markdown (.md) files.
- Simulation input is formatted as data tables in tab-delimited (.tsv) files in the `tables` directory.
- Table-specific field definitions and table notes live in `*.schema.md` files alongside their tables (e.g., `operations.schema.md` next to `operations.tsv`).
- Table-specific entity descriptions and entity notes live in `*.descriptive.md` files alongside their tables (e.g., `operations.descriptive.md` next to `operations.tsv`). In these files, entity groupings are organizational only and have no functional effect in the simulation.
- The `text/` directory contains translation files. `entities.csv` maps table entity IDs to display names (columns: `id`, plus language codes like `en`).


## General Instructions

1. Provide feedback and ask for clarifications that might help improve the prompt and/or repository files for subsequent analyses.
2. Summarize simplifications and important omissions in your analyses.
3. Use existing file data for cross-calibration when helpful.
4. Entity descriptions in markdown descriptive files should be brief and written in terms of the real-world or speculative real-world, not in terms of model implementation. They should not specifically reference model entities as they appear in descriptive or data table files.
5. When adding or editing entries in any file, follow the conventions and formatting patterns established by existing entries in that file.
6. For inclusion decisions in borderline cases, be minimal when considering present-day Earth activities and expansive when considering future and/or off-Earth activities.


## Data Tables

When creating or modifying TSV data tables, the `/data-tables` skill provides format specifications and conventions. Always read the relevant `*.schema.md` file for table-specific field definitions before working on a table. If understanding of table entities is needed, read the relevant `*.descriptive.md` file.
