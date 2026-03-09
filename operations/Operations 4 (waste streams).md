Please consider waste streams broadly and suggest additions or changes for physical_resources.md and physical_operations.md.

Some items that I think are currently missing:

- There is no operation to handle Biowaste.
- There is no resource that represents household waste (I don't think Industrial Waste captures this).

I don't want to grow resources or operations more than necessary. However, I do want our abstractions to capture distinctly different kinds of mass flows. Also, we can't have "orphans" in our implementation. All resources need to go somewhere (except see below) and all operations need inputs and outputs that are defined in our resources list.  

FYI: The simulation includes some sources, sinks, and conversions that aren't represented in the files:

- Infrastructure and manufactured items do go out of service. These can either sit in place and do nothing (e.g., an abandoned station or a garbage heap on the Moon), or flow back into the resource model as Industrial Waste or similar.
- Populations consume food and goods and generate garbage (whatever resource that is) and Biowaste.
- There is an "ecosystem services" process that can handle some amount of biological, household, and industrial waste, mainly on Earth (but possibly in other large ecosystems in the future).
- In much of the solar system, waste can just be dumped outside. However, I don't think that will be the best option economically in most cases.