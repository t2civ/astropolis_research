---
name: data-tables
description: >
  Use when creating or modifying simulation data tables (.tsv files).
  Triggers on: TSV table creation, adding rows/columns, modifying table data,
  working with operations.tsv, resources.tsv, strata.tsv, or similar data tables.
---

# Data Table Format Specifications

All simulation data is imported via tab-delimited tables in .tsv files. Tables are either DB-style (database-like with entity rows and field columns) or Entity x Entity data matrices.

**Important:** Before creating or modifying any table, always read the relevant `*.schema.md` file alongside the table for table-specific field definitions, notes, and constraints.

Full format documentation is in the I, Voyager Tables plugin README, found locally in Godot projects at `addons/ivoyager_tables` or at https://github.com/ivoyager/ivoyager_tables.


## Entity Naming

Data table entities use CONSTANT_CASE: capitalized letters, digits, and underscores (omitting &, +, and other non-alphanumeric characters). Convert markdown entity names as needed.

Standard abbreviations:
- INDUSTRIAL -> INDUST
- MATERIALS -> MAT
- PRODUCTS -> PROD
- LIQUID -> LIQ
- SYNTHESIS -> SYNTH
- RECYCLING -> RECYC
- MANUFACTURING -> MANUF
- FABRICATION -> FAB
- ENVIRONMENT -> ENV
- ATMOSPHERE -> ATMOS

Examples: Helium-3 -> HELIUM_3, 3He-3He Fusion Power -> 3HE_3HE_FUSION_POWER, Industrial Recycling & Incineration -> INDUST_RECYC_INCINERATION. During table import, entity names are prefixed with type identifiers such as "RESOURCE_", "OPERATION_", etc.


## Data Types

Types are specified per field (DB-style) or per table (Entity x Entity):
- Built-in: BOOL, FLOAT, INT, STRING, STRING_NAME, VECTOR2, VECTOR3, VECTOR4, COLOR, VARIANT
- TABLE: reference to another table entity
- ARRAY[<type>]: array of a type (e.g., ARRAY[STRING], ARRAY[FLOAT])
- VECTORx types expect comma-delimited float values in cells
- ARRAY types expect semicolon-delimited values in cells


## Units and Numbers

- The importer handles most SI and many non-SI unit abbreviations including compound units (space = multiplication). Examples: "km", "g/cm^3", "t/d", "$/t" (short for "USD/t"), "10^24 kg".
- Unit can be overridden in any cell by direct specification (e.g., "10 g" is 10 grams regardless of column unit).
- Write large numbers (>=1e6) and small numbers (<0.001) using "e" exponent notation (e.g., 5.97e24).
- Prefer 3 significant digits, but use fewer or more when warranted by actual precision.
- Prefer tonne (t) for large masses and tonne per hour (t/h) for large mass rates. Otherwise, use SI units as commonly used for the measured property (e.g., g/cm^3 for density, km for large distances).
- Follow existing patterns in tables that already exist.


## Skipping Rows and Columns

- Rows starting with # are skipped by the importer.
- Columns where the 1st row item starts with # are skipped.


## DB-Style Table Format

- Top-left cell: "name\Field". Top row cells contain field names. The 1st column is always the `name` field.
- Below the top row, 1-4 header rows follow (in any order), identified by their first cell:
  - `Type` (required): data type for the column
  - `Unit` (optional): default unit for FLOAT/VECTORx/ARRAY[FLOAT] columns (overridable per cell)
  - `Default` (optional): default value for empty cells
  - `Prefix` or `Prefix/<1st column prefix>` (optional): string prefix for values; the latter form prefixes 1st column entity names
- Data rows follow, each starting with the row entity name.


## Entity x Entity Table Format

- Top-left cell: empty or `<row prefix>\<column prefix>` for entity name prefixing.
- First row and first column specify row and column entities.
- After data, rows starting with @ are table directives:
  - `@ENTITY_X_ENTITY` (required): marks this as an Entity x Entity table
  - `@DATA_TYPE=<type>` (required): data type for the whole table
  - `@DATA_UNIT=<unit>` (optional): default unit for the whole table (overridable per cell)
  - `@DATA_DEFAULT=<default>` (optional): default value for empty cells or missing rows/columns


## General Table Instructions

1. For columns without instructions (not described in prompt or schema), leave empty when creating new items or copy existing values when modifying.
2. When adding to or modifying an existing table, follow patterns and conventions already used in the table.
