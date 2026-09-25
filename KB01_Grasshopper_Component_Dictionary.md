# Grasshopper Component Dictionary

Version: 1.0  
Scope: Rhino 8 built-in Grasshopper components only. Third-party plug-ins are not included.

This document is a small verified dictionary for a RAG prototype. The assistant must use the full component name and identify the exact output and input ports. If a component is not listed, it must say that the current knowledge base cannot confirm it.

## Divide Curve

- Purpose: Divide a curve into equal-length segments.
- Inputs:
  - Curve `C`: curve to divide; type Curve.
  - Count `N`: number of segments; type Integer.
  - Kinks `K`: whether to split at kinks; type Boolean.
- Outputs:
  - Points `P`: division points; type Point.
  - Tangents `T`: tangent vectors at division points; type Vector.
  - Parameters `t`: curve parameters at division points; type Number.
- Important note: `N` is the number of segments. An open curve normally produces `N + 1` division points.
- Source: https://grasshopperdocs.com/components/grasshoppercurve/divideCurve.html

## Unit Z

- Purpose: Create a vector parallel to the World Z axis.
- Inputs:
  - Factor `F`: multiplication factor and resulting vector length; type Number.
- Outputs:
  - Unit vector `V`: World Z vector; type Vector.
- Important note: a negative `F` produces a vector in the negative Z direction.
- Source: https://grasshopperdocs.com/components/grasshoppervector/unitZ.html

## Move

- Purpose: Translate geometry along a vector.
- Inputs:
  - Geometry `G`: geometry to move; type Geometry.
  - Motion `T`: translation vector; type Vector.
- Outputs:
  - Geometry `G`: translated geometry; type Geometry.
  - Transform `X`: transformation data; type Transform.
- Source: https://grasshopperdocs.com/components/grasshoppertransform/move.html

## Distance

- Purpose: Calculate Euclidean distance between two point coordinates.
- Inputs:
  - Point A `A`: first point; type Point.
  - Point B `B`: second point; type Point.
- Outputs:
  - Distance `D`: distance between A and B; type Number.
- Important note: when one input is a list and the other is one point, Grasshopper commonly repeats the single point to match the list. Data-tree structure must still be checked.
- Source: https://grasshopperdocs.com/components/grasshoppervector/distance.html

## Bounds

- Purpose: Create a numeric domain from the smallest to the largest number in a list.
- Inputs:
  - Numbers `N`: numbers included in the calculation; type Number.
- Outputs:
  - Domain `I`: numeric interval from the lowest to the highest value; type Domain.
- Important note: if all input numbers are identical, the source domain has zero length and remapping may fail or return unusable results.
- Source: https://grasshopperdocs.com/components/grasshoppermaths/bounds.html

## Construct Domain

- Purpose: Create a one-dimensional numeric domain from two limits.
- Inputs:
  - Domain start `A`: lower or starting value; type Number.
  - Domain end `B`: upper or ending value; type Number.
- Outputs:
  - Domain `I`: numeric domain between A and B; type Domain.
- Important note: reversing A and B reverses the domain direction and can reverse an attractor effect.
- Source overview: https://grasshopperdocs.com/addons/grasshopper-maths.html

## Remap Numbers

- Purpose: Map numbers from one numeric domain to another.
- Inputs:
  - Value `V`: numbers to remap; type Number.
  - Source `S`: original numeric domain; type Domain.
  - Target `T`: desired numeric domain; type Domain.
- Outputs:
  - Mapped `R`: remapped numbers; type Number.
  - Clipped `C`: remapped values clipped to the target domain; type Number.
- Important note: the common source domain comes from Bounds. The target domain comes from Construct Domain.
- Source: https://grasshopperdocs.com/components/grasshoppermaths/remapNumbers.html

## Scale

- Purpose: Scale geometry uniformly around a centre point.
- Inputs:
  - Geometry `G`: geometry to scale; type Geometry.
  - Center `C`: centre of scaling; type Point.
  - Factor `F`: scale factor; type Number.
- Outputs:
  - Geometry `G`: scaled geometry; type Geometry.
  - Transform `X`: transformation data; type Transform.
- Important note: `F = 1` keeps the original size, values between 0 and 1 shrink, and values greater than 1 enlarge.
- Source: https://grasshopperdocs.com/components/grasshoppertransform/scale.html

## Sort List

- Purpose: Sort numeric keys and reorder a related list in the same order.
- Inputs:
  - Keys `K`: numeric values used for sorting; type Number.
  - Values A `A`: optional values reordered with the keys; type Generic Data.
- Outputs:
  - Keys `K`: sorted numeric values; type Number.
  - Values A `A`: reordered values; type Generic Data.
- Important note: the Keys list and Values list should correspond item by item.
- Source: https://grasshopperdocs.com/components/grasshoppersets/sortList.html

## List Item

- Purpose: Retrieve one item from a list.
- Inputs:
  - List `L`: source list; type Generic Data.
  - Index `i`: zero-based item index; type Integer.
  - Wrap `W`: whether indices outside the list range wrap around; type Boolean.
- Outputs:
  - Item `i`: selected item; type Generic Data.
- Important note: Grasshopper indices normally begin at 0, so index 0 retrieves the first item.
- Source: https://grasshopperdocs.com/components/grasshoppersets/listItem.html

## Sub List

- Purpose: Extract a consecutive subset from a list.
- Inputs:
  - List `L`: source list; type Generic Data.
  - Domain `D`: domain of indices to copy; type Domain.
  - Wrap `W`: whether indices beyond the list range are remapped; type Boolean.
- Outputs:
  - List `L`: extracted subset; type Generic Data.
  - Index `I`: indices of selected items; type Integer.
- Example: to retrieve the first five sorted items, use a Construct Domain from 0 to 4.
- Source: https://grasshopperdocs.com/components/grasshoppersets/subList.html

## Dispatch

- Purpose: Divide one list into two lists using a repeating Boolean pattern.
- Inputs:
  - List `L`: list to divide; type Generic Data.
  - Dispatch pattern `P`: Boolean pattern; type Boolean.
- Outputs:
  - List A `A`: items corresponding to True values.
  - List B `B`: items corresponding to False values.
- Example: a repeating pattern `{True, False}` sends alternate items to A and B.
- Source: https://grasshopperdocs.com/components/grasshoppersets/dispatch.html

## Cull Pattern

- Purpose: Remove list items using a repeating Boolean mask.
- Inputs:
  - List `L`: source list; type Generic Data.
  - Cull Pattern `P`: Boolean removal pattern; type Boolean.
- Outputs:
  - List `L`: remaining items; type Generic Data.
- Important note: unlike Dispatch, Cull Pattern returns only the items that remain. Test the True and False behaviour with a short numbered list before applying it to geometry.
- Source: https://grasshopperdocs.com/components/grasshoppersets/cullPattern.html

## Knowledge boundary

The database does not currently cover Kangaroo, Ladybug, Honeybee, Karamba3D, Weaverbird, Human UI, Galapagos settings, GhPython code, mesh relaxation, environmental simulation, structural analysis, or automatic creation of `.gh` files. For these topics, the assistant must clearly state that the current knowledge base cannot confirm the required components or ports.
