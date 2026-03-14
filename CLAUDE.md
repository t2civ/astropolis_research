# Astropolis Research

AI-assisted research for Astropolis simulation content data.

- [Astropolis homepage](https://t2civ.com/)
- [Astropolis public code](https://github.com/t2civ/astropolis_sdk)


## Background

The purpose of this repository is to collect descriptive information, develop abstractions, and generate input data for a simulation that focuses on resource streams: extraction, commodity trading, and downstream usage in industrial/economic activities. The simulation starts at present-day technology and operations on Earth, but runs forward in time to include highly speculative (yet physically plausible) future technologies and operations in the wider solar system. Abstractions, simplifications, approximations, and speculations are necessary. However, the overall simulation should adhere (at least approximately) to basic physical principles such as mass balance, energy conservation, etc.


## File Organization

- Instructions, table schema, and entity descriptions are in markdown (.md) files.
- Model input is formatted as data tables in tab-delimited (.tsv) files.
- Table-specific field definitions and notes live in `*.schema.md` files alongside their tables (e.g., `operations.schema.md` next to `operations.tsv`).
- Descriptive information for table entities are in the `descriptions` directory, sometimes organized as subsets. E.g., resources are described in physical_resources.md and extractable_resources.md (the latter is a subset of the former; both are subsets of all resources). Groupings in descriptive files are organizational only.


## General Instructions

1. Provide feedback and ask for clarifications that might help improve the prompt and/or repository files for subsequent analyses.
2. Summarize simplifications and important omissions in your analyses.
3. Use existing file data for cross-calibration when helpful.
4. Entity descriptions in markdown descriptive files should be brief and written in terms of the real-world or speculative real-world, not in terms of model implementation. They should not specifically reference model entities as they appear in descriptive or data table files.
5. For inclusion decisions in borderline cases, be minimal when considering present-day Earth and expansive when considering future and/or off-Earth.
6. When adding or editing entries in any file, follow the conventions and formatting patterns established by existing entries in that file.


## Data Tables

When creating or modifying TSV data tables, the `/data-tables` skill provides format specifications and conventions. Always read the relevant `*.schema.md` file for table-specific field definitions before working on a table.
