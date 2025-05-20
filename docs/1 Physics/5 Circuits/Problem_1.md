# Circuits

## Problem 1  
### Equivalent Resistance Using Graph Theory

---

## Motivation

Calculating equivalent resistance is fundamental in circuit analysis. Complex circuits often combine series and parallel resistors in nested configurations. Representing circuits as hierarchical structures allows precise computation of equivalent resistance using recursive formulas.

---

## Formulas

- **Series**: \( R_{eq} = R_1 + R_2 + \cdots + R_n \)  
- **Parallel**: \( \frac{1}{R_{eq}} = \frac{1}{R_1} + \frac{1}{R_2} + \cdots + \frac{1}{R_n} \)

---

## Recursive Calculation

Each group is either:

- A single resistor (leaf), or
- A group of resistors connected in series or parallel.

Equivalent resistance is computed recursively.

---

## Interactive Circuit Simulator (Nested Series & Parallel)

You can:

- Add resistors in **series** or **parallel** to the current selected group,
- Navigate into groups to add nested connections,
- Edit resistor values,
- See the circuit visually on canvas,
- See equivalent resistance updated live.

---

<style>
  body { font-family: Arial, sans-serif; max-width: 720px; margin: auto; padding: 20px;}
  input[type=number] { width: 80px; margin-right: 10px; }
  button { margin: 5px 8px 5px 0; padding: 6px 12px; cursor: pointer; }
  #breadcrumbs { margin-bottom: 10px; }
  #resistor-list { margin-top: 15px; white-space: pre-wrap; font-family: monospace; }
  #canvas-container { text-align: center; margin-top: 20px; }
  #circuitCanvas { border: 1px solid #ccc; background: #f9f9f9; }
</style
