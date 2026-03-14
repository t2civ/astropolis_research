# Astropolis Research

AI-assisted research for Astropolis simulation content data. We develop the model abstractions and data tables here.

- [Astropolis homepage](https://t2civ.com/)
- [Astropolis public code](https://github.com/t2civ/astropolis_sdk)


## Background

The purpose of this project is to collect descriptive information, develop abstractions, and generate input data for Astropolis. Astropolis is a simulation that focuses on resource streams: extraction, commodity trading, and downstream usage in industrial/economic activities. The simulation starts at present-day technology and operations on Earth, but runs forward in time to include highly speculative (yet physically plausible) future technologies and operations in the wider solar system. Abstractions, simplifications, approximations, and speculations are necessary. However, the overall simulation should adhere (at least approximately) to basic physical principles such as mass balance, energy conservation, etc.


## File Organization

- Instructions, table schema, and entity descriptions are in markdown (.md) files.
- Model input is formatted as data tables in tab-delimited (.tsv) files.
- Table-specific field definitions and notes live in `*.schema.md` files alongside their tables (e.g., `operations.schema.md` next to `operations.tsv`).
- Markdown descriptive groupings (Energy, Extraction, etc.) are for organization and display only; functional effects in the simulation are handled by individual item properties.


## General Instructions

1. Provide feedback and ask for clarifications that might help improve the prompt and/or context files for subsequent analyses.
2. Summarize simplifications and important omissions in your analyses.
3. Use existing file data for cross-calibration when helpful.
4. Item descriptions in markdown context files should be brief and written in terms of the real-world or speculative real-world, not in terms of model implementation. They should not specifically reference model entities as they appear in data tables.
5. For inclusion decisions in borderline cases, be minimal when considering present-day, Earth-only items and expansive when considering future and/or off-Earth items.


## Data Tables

When creating or modifying TSV data tables, the `/data-tables` skill provides format specifications and conventions. Always read the relevant `*.schema.md` file for table-specific field definitions before working on a table.
