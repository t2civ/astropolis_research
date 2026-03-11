# Schema and Instructions for operations.tsv

## Fields

- name (implicit 1st column) — Operation name per table instructions.
- electricity — Electricity produced (as a positive value) or used (as a negative value) by the operation in MW.
- efficiency — Efficiency of the operation, such that 1 minus efficiency equals proportional waste heat. Select an efficiency value that is somewhat forward looking but within the range of current Earth operations.
- input_resources — A semicolon-delimited list of input resources for the operation.
- input_rates — A semicolon-delimited list of input resource rates (in t/h) for the operation.
- output_resources — A semicolon-delimited list of output resources for the operation.
- output_rates — A semicolon-delimited list of output resource rates (in t/h) for the operation.
- mass_flow — Equals the sum of input_rates equals the sum of output_rates, within rounding precision.

## Notes

- For Extraction group operations, leave all fields blank except the name field (these are handled very differently).
- Renewable power operations and RADIOISOTOPE_POWER have no input or output resources.
- For Energy group operations, normalize the operation so that `electricity` is 1 MW. For others, normalize the operation so that `mass_flow` is either 1 t/h or 1 kg/h (use the latter for very high value-to-weight or high energy-to-weight products such as PRECIOUS_METALS and TENSOR_PROCESSORS).
- Use resources as they are defined in physical_resources.md. E.g., input rate for LE_URANIUM_FUEL should consider the assembled fuel weight, not just the uranium content.
- Use catchall resources as needed. E.g., if an operation in the real-world requires 1 kg/h of lithium, use 1 kg/h of INDUSTRIAL_METALS.
- For chemical conversions, consider only stoichiometric quantities for inputs and outputs. For fabricated/manufactured items, consider product composition and subtractive processes (the latter producing INDUSTRIAL_WASTE, REGOLITH, etc.).
- Don't consider plant maintenance or repair parts used to run the operation; that will be handled separately.
- Consider inputs and outputs as a net reaction. I.e., don't have the same resource as input and output.
- Don't worry about water use in industrial processes, except as a stoichiometric input or output.
- Don't worry about the source of input resources (e.g., OXYGEN from atmosphere versus purified supply) or the fate of output resources (e.g., collected versus exhaust). An amendment is that you can consider alternative compositions and how they might be more useful or less harmful. E.g., you could consider outputting SULFER rather than SULFUR_DIOXIDE from COAL_POWER.
- Be forward looking if the particular problem is not well constrained (as stated above for efficiency).
- Report if you find major gaps in the operations list or associated lists.
