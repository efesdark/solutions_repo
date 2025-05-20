# Circuits

## Problem 1  
### Equivalent Resistance Using Graph Theory

---

## Motivation

Calculating equivalent resistance is a fundamental problem in electrical circuits, essential for understanding and designing efficient systems. While traditional methods involve iteratively applying series and parallel resistor rules, these approaches can become cumbersome for complex circuits with many components. Graph theory offers a powerful alternative, providing a structured and algorithmic way to analyze circuits.

By representing a circuit as a graph—where nodes correspond to junctions and edges represent resistors with weights equal to their resistance values—we can systematically simplify even the most intricate networks. This method not only streamlines calculations but also opens the door to automated analysis, making it particularly useful in modern applications like circuit simulation software, optimization problems, and network design.

Studying equivalent resistance through graph theory is valuable not only for its practical applications but also for the deeper insights it provides into the interplay between electrical and mathematical concepts. This approach highlights the versatility of graph theory, demonstrating its relevance across physics, engineering, and computer science.

---

## Fundamental Concepts and Formulas

### Series Resistors

Resistors are in **series** when they are connected end-to-end, and the same current flows through each resistor. The equivalent resistance, \( R_{eq} \), is the sum of their resistances:

$$
R_{eq} = R_1 + R_2 + \cdots + R_n
$$

### Parallel Resistors

Resistors are in **parallel** when their terminals are connected to the same two nodes, sharing the same voltage across each resistor. The equivalent resistance is found by:

$$
\frac{1}{R_{eq}} = \frac{1}{R_1} + \frac{1}{R_2} + \cdots + \frac{1}{R_n}
$$

Or equivalently:

$$
R_{eq} = \left( \sum_{i=1}^n \frac{1}{R_i} \right)^{-1}
$$

---

## Graph Theory Approach

### Circuit as a Graph

- **Nodes** represent circuit junctions.
- **Edges** represent resistors, weighted by their resistance.

### Simplification Strategy

- **Series Reduction:** If a node connects exactly two edges (resistors in series), combine their resistances by summing and replace with a single edge.
- **Parallel Reduction:** If multiple edges connect the same two nodes (parallel resistors), combine using reciprocal sum formula.

Iterate these steps until only two nodes remain connected by one equivalent resistor.

---

## Algorithm Description (Pseudocode)

```python
def calculate_equivalent_resistance(graph):
    while number_of_nodes(graph) > 2:
        for node in graph.nodes:
            edges = graph.edges_of(node)
            if len(edges) == 2:
                # Series reduction
                r1, r2 = edges[0].weight, edges[1].weight
                new_r = r1 + r2
                merge_edges_in_series(graph, node, new_r)
                break
            elif multiple_edges_between_two_nodes(graph):
                # Parallel reduction
                r_values = get_parallel_resistances(graph)
                new_r = 1 / sum(1/r for r in r_values)
                merge_edges_in_parallel(graph, new_r)
                break
    return graph.final_edge.weight
