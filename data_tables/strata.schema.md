# Schema for strata.tsv

Strata are volumes of natural or artificial bodies that have defined resource composition, or are templates for resource composition in the case of generic strata. Most strata have the simplified geometry of a sphere, sphere shell, or fraction thereof (where area is defined but not area boundaries).


## Entity Descriptions 

For any complex tasks involving table rows, it's important to understand what the row entities are supposed to represent. Read descriptive information for strata in `descriptions/strata.md`.


## Fields

- name — See strata.md, note 8.
- body — See strata.md, note 8.
- body_radius — Mean radius of the body (to sea level or other standard reference).
- territory — See strata.md, note 8.
- strata_group — See strata.md, note 8.
- depth  — Mean radius minus inner boundary radius of the specific stratum. For bulk strata, mean radius of the body. 0 for generic strata.
- thickness — Thickness of the stratum. For bulk strata, this should be the same as depth. 0 for generic strata.
- spherical_fraction — Fraction of spherical area for strata that include regional boundaries (e.g., Earth territories, Moon terrae versus maria, Mars polar cap, etc.). Use 1 when not applicable (including generic strata).
- area — Surface area of the stratum at its outer radius boundary, treating all bodies as spherical. 0 for generic strata.
- volume — Volume of the stratum. 0 for generic strata.
- density — Mean density of the stratum. Mean expectation density for generic strata.
- mass — Total mass of the stratum. 0 for generic strata.
- survey_level — A value from 0 to 10 (decimals allowed) that indicates our relative knowledge about the stratum composition and deposit locations in it. Construct the scale considering all strata listed in strata.md. 0 for generic strata. For calibration:
   - 0. No knowledge.
   - 0.5 Theoretical models with moderate uncertainty.
   - 2.5. Lunar regol ith (excluding polar).
   - 5. Earth near-surface areas that have been extensively explored for resource extraction.
   - 7. Earth surface that is fully surveyed and explored.
   - 10. Earth atmosphere — near perfect knowledge of a mostly homogeneous composition (no uncertainty about deposits).
- survey_note — A very brief qualitative note that justifies the value provided in survey_level. "NA" for generic strata.


## Notes

1. Internal consistency is required within a stratum given the simplified geometry. Within rounding error: mass = volume × density; volume = spherical_fraction × 4π/3 × (outer_radius^3 - inner_radius^3), where inner_radius = body_radius - depth and outer_radius = inner_radius + depth (or use thin-shell approximation where appropriate).
2. It's not important that adjacent strata "fit" perfectly at boundaries. This is impossible in cases where a boundary is an "area-weighted mean" adjacent to layers of different thickness, e.g., Moon mantle upper boundary adjacent to maria and terrae crust. This is OK.
3. Atmospheres will have tiny mean densities given the volume construction; this is OK.
4. Don't worry too much about getting survey_level exactly right; these will be cross-calibrated after we have all strata.
5. Resource compositions of strata are defined in strata_resources.tsv.
