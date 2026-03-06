# Background

I'm collecting information for a simulation that focuses on resource streams: extraction, commodity trading, and downstream usage in industrial/economic activities. The simulation starts at present-day technology and operations on Earth, but runs forward in time to include highly speculative (yet physically plausible) future technologies and operations in the wider solar system. Abstractions, simplifications, approximations, and speculations are necessary. However, we want the overall simulation to adhere (at least approximately) to basic physical principles such as mass balance, energy conservation, etc.

# General Instructions

1. Report and quit working if context appears to be missing.
2. Provide feedback and ask for clarifications that might help improve the prompt and/or context files for subsequent analyses.
3. Summarize simplifications and important omissions in your analyses.
4. Use provided context files for cross-calibration when helpful. In general, descriptive information is provided in markdown (.md) files and model input data tables are provided in tab-delimited (.tsv) files.
5. Write large numbers (≥1e6) and small numbers (<0.001) using "e" exponent notation (e.g., 5.97e24). Prefer 3 significant digits, but use fewer or more when warranted by actual precision.
6. Prefer the unit tonne (t) for large masses and tonne per hour (t/h) for large mass rates. Otherwise, use SI units as commonly used for the measured property (e.g., g/cm³ for density; km for large distances unless ly or parsec is warranted; etc.).
7. For data table construction, convert entity text to CONSTANT_CASE (capitalize, omit parentheses, omit "&", and replace other non-alphanumeric characters with underscores). Abbreviate INDUSTRIAL as INDUST, MATERIALS as MAT, and PRODUCTS as PROD. Examples: Helium-3 → HELIUM_3, Industrial Metal Ores → INDUST_METAL_ORES, etc.
 