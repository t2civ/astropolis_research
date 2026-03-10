Construct a tab-delimited table with physical operations as row entities. This will become model input data so follow provided table instructions. Include the following column fields:

- name — Operation name per table instructions.
- electricity — Electricity produced (as positive value) or used (as negative value) by the operation in MW.
- efficiency — Efficiency of the operation, such that 1 minus efficiency equals proportional waste heat. Select an efficiency value that is somewhat forward looking but within the range of current Earth operations.
- input_resources — A semicolon-delimited list of input resources for the operation (resource names per table instructions).
- input_rates — A semicolon-delimited list of input resource rates (in t/h) for the operation (each corresponds to a resource above).
- output_resources — A semicolon-delimited list of output resources for the operation.
- output_rates — A semicolon-delimited list of output resource rates (in t/h) for the operation.
- mass_flow — This should equal the sum of input_rates which should equal the sum of output_rates.

Notes:

- For Extraction group operations, leave all fields blank except the name field (these are handled very differently).
- SOLAR_POWER and RADIOISOTOPE_POWER have no input or output resources.
- For Energy group operations, normalize the operation so that `electricity` is 1 MW. For others, normalize the operation so that `mass_flow` is either 1 t/h or 1 kg/h (use the latter for very high value to weight products such as PRECIOUS_METALS and TENSOR_PROCESSORS).
- Use resources as they are defined in physical_resources.md. E.g., input rate for LE_URANIUM_FUEL should consider the assembled fuel weight, not just the uranium content.
- For chemical conversions, consider only stoichiometric quantities for inputs and outputs. For fabricated/manufactured items, consider product composition and subtractive processes (the latter producing INDUSTRIAL_WASTE, etc.).
- Don't consider plant maintenance or repair parts used to run the operation; that will be handled separately.
- Don't have the same resource as input and output. E.g., there should be no "excess oxygen" in combustion.
- Don't worry about water use in industrial processes, except as a stoichiometric input or output.
- Don't worry about the source of input resources (e.g., OXYGEN from atmosphere versus purified supply) or the fate of output resources (e.g., collected versus exhaust). An amendment to the latter is that you can consider different potential output compositions and how they might be more useful or less harmful. For example, you could consider outputting SULFER rather than SULFUR_DIOXIDE from COAL_POWER.
- As for efficiency, be forward looking if the particular operation or consideration is not well constrained.
- Report if you find major gaps in either the resources or operations lists.

For now, I only want you to construct table rows for the Energy and Extraction groups. I don't want you to analyze other operation groups at this time, but tell me if you have suggestions that might improve results when moving on to the remaining operations.