# Grasshopper Common Wiring Workflows

Version: 1.0  
Scope: Four beginner workflows using only the built-in components recorded in KB01.

The RAG assistant should retrieve the closest workflow, check every component and port against KB01, and then answer in numbered port-to-port steps. It should not invent missing steps.

## Workflow 1 Divide a curve and move the points vertically

### User goal

Divide one curve into a chosen number of segments and move the resulting points in the World Z direction.

### Required inputs

- One valid curve referenced from Rhino.
- One integer slider for the segment count.
- One number slider for the Z movement distance.

### Components

- Divide Curve
- Unit Z
- Move

### Wiring

1. Curve parameter output -> Divide Curve `C` input.
2. Integer slider -> Divide Curve `N` input.
3. Divide Curve `P` output -> Move `G` input.
4. Z-distance slider -> Unit Z `F` input.
5. Unit Z `V` output -> Move `T` input.
6. Use Move `G` output as the moved point result.

### Checks

- `N` controls segment count, not the final number of points on an open curve.
- A negative Unit Z factor moves the points downward.
- If each point needs a different height, Unit Z `F` must receive a matching list of numbers rather than one number.

## Workflow 2 Scale repeated geometry with a point attractor

### User goal

Scale a list of shapes according to their distance from an attractor point.

### Required inputs

- A list of base geometries.
- One centre point for every base geometry.
- One attractor point.
- Two number sliders defining the minimum and maximum scale factors.

### Components

- Distance
- Bounds
- Construct Domain
- Remap Numbers
- Scale

### Wiring

1. Geometry centre points -> Distance `A` input.
2. Attractor point -> Distance `B` input.
3. Distance `D` output -> Bounds `N` input.
4. Distance `D` output -> Remap Numbers `V` input.
5. Bounds `I` output -> Remap Numbers `S` input.
6. Minimum scale slider -> Construct Domain `A` input.
7. Maximum scale slider -> Construct Domain `B` input.
8. Construct Domain `I` output -> Remap Numbers `T` input.
9. Base geometries -> Scale `G` input.
10. Geometry centre points -> Scale `C` input.
11. Remap Numbers `R` output -> Scale `F` input.
12. Use Scale `G` output as the scaled geometry.

### Checks

- The geometry list, centre-point list and scale-factor list should correspond item by item.
- If the closest objects should become largest, reverse the target domain, for example from 2.0 to 0.2 instead of 0.2 to 2.0.
- If all distances are identical, Bounds produces a zero-length source domain. The user must change the input condition or provide a fallback factor.
- If branches do not match, inspect the trees before using Flatten or Graft.

## Workflow 3 Sort points by distance and retrieve the nearest items

### User goal

Sort a point list from nearest to farthest relative to one reference point, then retrieve either the nearest point or the first several points.

### Components

- Distance
- Sort List
- List Item or Sub List
- Construct Domain when using Sub List

### Wiring

1. Point list -> Distance `A` input.
2. Reference point -> Distance `B` input.
3. Distance `D` output -> Sort List `K` input.
4. Original point list -> Sort List `A` input.
5. Sort List `A` output now contains points ordered from the smallest distance to the largest distance.

### Retrieve the nearest point

6. Sort List `A` output -> List Item `L` input.
7. Integer value 0 -> List Item `i` input.
8. List Item `i` output is the nearest point.

### Retrieve the nearest five points

6. Sort List `A` output -> Sub List `L` input.
7. Number 0 -> Construct Domain `A` input.
8. Number 4 -> Construct Domain `B` input.
9. Construct Domain `I` output -> Sub List `D` input.
10. Sub List `L` output contains the first five points, provided the source list contains at least five items.

### Checks

- Sort List `K` contains the sorted distances; Sort List `A` contains the corresponding reordered points.
- Do not connect the original unsorted point list directly to List Item if the goal is to find the nearest point.
- Indexing begins at 0.

## Workflow 4 Separate alternate items into two groups

### User goal

Split a list into alternating A and B groups, such as odd and even façade panels.

### Components

- Dispatch
- A repeating Boolean pattern such as `{True, False}`

### Wiring

1. Source list -> Dispatch `L` input.
2. Repeating Boolean pattern `{True, False}` -> Dispatch `P` input.
3. Dispatch `A` output contains items matching True.
4. Dispatch `B` output contains items matching False.

### Checks

- Use a short numbered list to confirm which physical panels belong to A and B.
- Dispatch preserves both groups. Cull Pattern is different because it removes one group and returns only the remaining list.

## Response rule

For every workflow answer, the assistant must output:

1. Goal understanding.
2. Required components.
3. Numbered wiring in `Output -> Input` format.
4. Parameter settings.
5. Data matching and tree warnings.
6. Knowledge-base sources.
7. Unconfirmed information or a clear boundary response.
