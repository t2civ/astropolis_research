I want you to develop the context file physical_operations.md. The goal is to include (as abstract simplifications) all industrial activities that involve extraction, refining, production, and/or conversions among resources listed in physical_resources.md, considering both current Earth activities and future solar system development. In short, we need to represent all of the industrial activities that convert our "extracted" resources into all of the other resources.

Consider the problem as a graph with nodes (physical_resources.md items) and edges (physical_operations.md items). However, I don't want to see lists of the resource terms (exactly as they appear in the context file) in the developed physical_operations.md file.

This is a first pass only; we'll improve the list in later passes. I don't have specific number requirements for operations at this time. I expect there to be more operations than resources: the resource graph needs more "edges" than "nodes" for a robust and flexible economy. Focus on broad inclusion now, but use abstractions/simplifications where possible to keep the number manageable.

Please also complete the 3 incomplete descriptions in physical_resources.md for Iron, Uranium and Thorium. Suggest additions for this file only if they fill major gaps in the abstraction (I don't want to grow this list more than necessary).


Notes:

- Add items, complete items, and/or modify items in physical_operations.md as needed. The existing items are intended to help you understand the desired level of abstraction and specificity.
- Items in physical_operations.md need brief descriptions analogous to items in the physical_resources.md context file (without elemental compositions). Include chemical conversions where it might be especially helpful (e.g., Steam Reforming, Haber–Bosch Process, etc.) but not where conversions are basic knowledge and implicit (e.g., combustion in Coal Power). Do mention valuable co- or by-products using regular language with compound or element names as appropriate. Don't list waste products (these are usually obvious).
- Some existing items have notes in brackets. Consider the notes for individual items and then delete. 
- Use existing abstractions and simplifications in physical_resources.md. For example, there is no "Yellowcake" resource, so one refining operation converts Uranium Ores to Uranium, another manufacturing operation converts Uranium plus other resources to LE Uranium Fuel, etc.
- Resource conversions can be more complex than the short name might imply. For example, Industrial Metals Mining produces other ores in addition to Industrial Metal Ores (the name here refers the main target). Rare Earths Refining produces Thorium in addition to Rare Earths. Etc.
- Be general where possible. For example, "Iron Mining" can be implemented to handle any targeted extraction of Iron/Nickle or Iron Ores in any strata that has relevant concentrations. "Volatiles Extraction" can handle extraction of Water or Helium-3 from appropriate strata. Etc.
- Be specific where necessary. An example here might be conversion of Martian regolith and atmosphere to rocket propellants or other useful resources. If an operation exploits a unique stratum potential (and isn't covered by a more generic term), then add it as such. We want these.
- The context file strata.md is included mainly to help when developing extraction operations. Consider different extraction operations to handle asteroids as bulk strata. These may or may not parallel the existing "mining" operations (these were added in consideration of crustal extraction on Earth or similar bodies).
- Energy is obviously a big part of simulation implementation. You can consider it in terms of plausible conversions, but you don't need to reference it for the most part since it is universal. 
- Be creative and speculative! But do note cases where you might be stretching plausibility.
