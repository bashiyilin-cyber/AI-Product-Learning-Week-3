# Grasshopper Data Types and Common Errors

Version: 1.0

This document helps the assistant explain why a logically correct component sequence can still fail because the data type, list length or tree structure does not match.

## Core data types

- Number: decimal numeric value used for distances, factors and parameters.
- Integer: whole number used for counts and indices.
- Boolean: True or False value used for switches and patterns.
- Point: XYZ coordinate.
- Vector: direction and magnitude; it is not a position.
- Curve: line, polyline, arc, NURBS curve or another curve-like object.
- Geometry: a generic geometry input that may accept points, curves, surfaces or Breps depending on the component.
- Domain: one-dimensional numeric interval such as 0 to 1.
- Generic Data: accepts multiple data types but does not guarantee that the next component can use them.

## Common type mistakes

### Point and vector are not interchangeable

Move `T` requires a Vector. A point coordinate should not be presented as a confirmed translation vector. Use Unit X, Unit Y, Unit Z or Vector 2Pt when a vector must be created.

### Number and integer are different

Divide Curve `N` and List Item `i` require integers. A decimal slider may be rounded or converted, which can cause unexpected counts or indices. Use an integer slider for these inputs.

### Domain is not a list of two independent numbers

Remap Numbers `S` and `T` require Domain data. Two sliders must first enter Construct Domain `A` and `B`; then its Domain `I` output connects to Remap Numbers.

## Lists and matching

Grasshopper components may operate on single items, lists or data trees. When two inputs contain lists, the items are matched according to Grasshopper's data-matching rules. A workflow can be conceptually correct but still produce wrong results when list lengths or branch paths do not correspond.

Before changing the tree, check:

1. How many items are in each input?
2. Do corresponding items have the same order?
3. Are the items stored in the same branch paths?
4. Is the component expecting a single item, list or tree?

## Flatten

Flatten removes branch structure and places all items into one list.

Use Flatten only when the operation should ignore the original groups. For example, global sorting across all points may require one flat list. Do not recommend Flatten automatically because it can destroy floor, panel or façade-group relationships.

## Graft

Graft creates a separate branch for each item. It can be useful when each geometry must be processed independently against another list or tree. Unnecessary Graft can create many branches and unexpected cross-matching.

## Simplify

Simplify removes shared, redundant path information but preserves the branching relationships. It is different from Flatten.

## Common error states

### Orange component

An orange component usually indicates a warning. Common causes include missing optional data, invalid values or partial conversion. Read the runtime message before changing the wiring.

### Red component

A red component indicates an error. Common causes include required input missing, incorrect type, invalid geometry, impossible calculation or plug-in failure. The assistant should ask for the exact runtime error text when the database cannot identify the cause.

### Null result

Null means that a valid output was not produced. Check whether the referenced Rhino object still exists, whether the geometry is valid and whether an index is outside the list range.

### Empty list

An empty list contains no items. Downstream components may appear valid but produce no geometry. Check filters, culling patterns and input references.

### Mismatched list lengths

In the attractor workflow, the base geometries, centre points and scale factors should correspond. If one list is shorter or has different branches, Scale may reuse items or produce an unexpected result.

### Zero-length remapping domain

If all values sent to Bounds are identical, the Source domain for Remap Numbers has zero length. The assistant should identify this condition rather than recommending random rewiring.

## Safe troubleshooting sequence

1. Confirm the exact user goal and input geometry type.
2. Check that every required input contains data.
3. Add temporary Panels to inspect numbers, counts and paths.
4. Test the workflow with one item before using a full list or tree.
5. Check Component runtime messages.
6. Compare list lengths and branch paths.
7. Use Flatten, Graft or Simplify only after identifying the intended matching relationship.
8. Reconnect the workflow one step at a time.

## Knowledge boundary and refusal rule

If the question depends on a component, port, plug-in or workflow absent from KB01 and KB02, the assistant must not fill the gap from memory. It should answer:

`当前知识库没有足够资料确认这个 Component 或 Wiring。请提供官方组件说明，或将该组件加入知识库后再继续。`

If the goal is vague, such as `帮我做一个好看的参数化塔`, the assistant should first ask about the base geometry, floor generation logic, controllable parameters and whether third-party plug-ins are allowed.

## Reference material

- McNeel Grasshopper list components guide: https://developer.rhino3d.com/guides/grasshopper/list-components/
- McNeel guide to data trees: https://developer.rhino3d.com/guides/grasshopper/the-why-and-how-of-data-trees/
- McNeel advanced data structures guide: https://developer.rhino3d.com/guides/grasshopper/gh-algorithms-and-data-structures/advanced-data-structures/
