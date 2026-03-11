# Table Background

All simulation data is imported via tab-delimited tables in .tsv files. Tables are either DB-style (database-like with entity rows and field columns) or Entity × Entity data matrices. These tables use bespoke formats as described below.

## General Table Notes

1. Data table entities are specified in CONSTANT_CASE consisting of capitalized letters, digits, and underscores (omitting &, +, and other non-alphanumeric characters). Convert markdown file entity names as needed. Specific abbreviations to use are INDUSTRIAL → INDUST, MATERIALS → MAT, PRODUCTS → PROD, LIQUID → LIQ, INDUSTRIAL → INDUST, SYNTHESIS → SYNTH, RECYCLING → RECYC, MANUFACTURING → MANUF, FABRICATION → FAB, ENVIRONMENT → ENV, ATMOSPHERE → ATMOS. Examples: Helium-3 → HELIUM_3, ³He-³He Fusion Power → 3HE_3HE_FUSION_POWER, Industrial Recycling & Incineration → INDUST_RECYC_INCINERATION. During table import, these entity names are prefixed with type identifiers such as "RESOURCE_", "OPERATION_", etc., as specified in table formats below.
2. Data type is specified per field (DB-style) or per table (Entity × Entity) as indicated in table formats below. These types can be built-in types (BOOL, FLOAT, INT, STRING, STRING_NAME, VECTOR2, VECTOR3, VECTOR4, COLOR, VARIANT, etc.), references to other table entities (TABLE), references to internal enums (various forms), or arrays of a type (e.g., ARRAY[STRING]). Defined element types such as VECTORx expect table cells to contain comma-delimited data (floats in the case of VECTORx). ARRAY[<type>] expects cells to contain semicolon-delimited data.
3. For float values, the table importer can handle most SI and many non-SI unit abbreviations including compound units (where space is treated as a multiplication operator). Examples: "km", "g/cm^3", "t/d", "$/t" (short form of "USD/t"), "10^24 kg".
4. Unit can be overridden in any cell by direct specification in the cell. E.g., "10 g" is correctly interpreted as 10 grams regardless of column or table unit specification. This is used sometimes to improve readability of values far outside the range of other cells in the column or table.
5. The table importer skips any row starting with # and any column in which the 1st row item starts with #.
6. Write large numbers (≥1e6) and small numbers (<0.001) using "e" exponent notation (e.g., 5.97e24). Prefer 3 significant digits, but use fewer or more when warranted by actual precision.
7. Prefer the unit tonne (t) for large masses and tonne per hour (t/h) for large mass rates. Otherwise, use SI units as commonly used for the measured property (e.g., g/cm³ for density; km for large distances unless ly or parsec is warranted; etc.). Follow existing patterns in tables that already exist.
8. In the case of columns for which you don't have instructions (e.g., a field not described in the prompt or schema), leave empty when creating new items or copy existing values when modifying an existing item.


## DB-Style Table Format

- The top-left cell is always empty. Top row cells to the right contain field names. The 1st column (after header rows) is always implicitly the `name` field.
- Below the top row, 1 to 4 "header" rows follow in any order. The first cell indicates the header row content:
   - `Type` (required) — Specifies data type for the column (see note 2 above). 
   - `Unit` (optional for FLOAT, VECTORx, ARRAY[FLOAT], etc.) — Specifies default unit for the column. This can be overridden in a cell per note 4 above.
   - `Default` (optional) — If present, specifies column value to use if the cell is empty.
   - `Prefix` or `Prefix/<1st column prefix>` (optional) — If present, specifies a prefix string to prepend to string values in each column. The latter construction is used to prefix the 1st column with row entity names.
- Data rows follow, each starting with the row entity name. Some column types require or allow delimited data within the column cells. Examples: VECTOR4 type requires 4 comma-delimited float values. ARRAY[FLOAT] expects any number of semicolon-delimited float values.

## Entity × Entity Table Format

- The top-left cell may be empty or may contain prefix specification for rows and columns in the form `<row prefix>\<column prefix>`.
- After the top-left cell, the first row and the first column specify row and column entities.
- After table data, rows starting with @ are table directives. These include the following:
   - `@ENTITY_X_ENTITY` (required) — Tells the importer that this is an Entity × Entity table.
   - `@DATA_TYPE=<type>` (required) — Specifies data type for the whole table (see note 2 above). E.g., `@DATA_TYPE=VECTOR4` specifies VECTOR4.
   - `@DATA_UNIT=<unit>` (optional) — Specifies default unit for the whole table. E.g., `@DATA_UNIT=kg` specifies kg. This can be overridden in a cell per note 4 above.
   - `@DATA_DEFAULT=<default>` (optional) — Specifies default value imputed for empty cells or missing columns or rows.

