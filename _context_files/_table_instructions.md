# Table Background

All simulation data is imported via tab-delimited tables in .tsv files. All tables are either "db-style", with entities specified in the 1st column and properties (fields) specified as headers in the 1st row, or "entity × entity" data matrices where the 1st row and 1st column specify entities. The table importer can handle most SI and many non-SI unit abbreviations including compound units (where space is treated as a multiplication operator). Examples: "km", "g/cm^3", "t/d", "$/t" (short form of "USD/t"), "10^24 kg".

# Table Details & Instructions

1. For db-style tables, column unit (if applicable) is specified in a single row that starts with the row label "Unit" (if present, this is usually the 2nd row after property field headers). For entity × entity tables, the whole-table unit (if applicable) is specified by a line at the end of the file in the form "@DATA_UNIT=kg" (insert applicable unit in place of "kg").
2. In either table type, unit can be overridden in any cell by direct specification in the cell. E.g., "10 g" is correctly interpreted as 10 grams regardless of column or table unit specification. Use this for cell values that are far outside the range of other cells in the column or table.
3. Write large numbers (≥1e6) and small numbers (<0.001) using "e" exponent notation (e.g., 5.97e24). Prefer 3 significant digits, but use fewer or more when warranted by actual precision.
4. Prefer the unit tonne (t) for large masses and tonne per hour (t/h) for large mass rates. Otherwise, use SI units as commonly used for the measured property (e.g., g/cm³ for density; km for large distances unless ly or parsec is warranted; etc.). Follow existing patterns in tables that already exist.
5. Data table entities are specified in CONSTANT_CASE consisting of capitalized letters, digits, and underscores (omitting "&" and other non-alphanumeric characters). Convert markdown file entity names as needed. Specific abbreviations to use are INDUSTRIAL → INDUST, MATERIALS → MAT, PRODUCTS → PROD, LIQUID → LIQ, INDUSTRIAL → INDUST, SYNTHESIS → SYNTH, RECYCLING → RECYC, MANUFACTURING → MANUF, FABRICATION → FAB, ENVIRONMENT → ENV. Examples: ³He-³He Fusion Power → 3HE_3HE_FUSION_POWER, Industrial Recycling & Incineration → INDUST_RECYC_INCINERATION.

 