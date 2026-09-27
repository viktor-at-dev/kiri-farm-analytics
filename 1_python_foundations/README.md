# Agricultural Digital Twin Architecture

**Author:** Victor Njoroge   
**Tech Stack:** Core Python (Zero External Dependencies)

---

## Business Context
The farm is a large enterprise comprising of numerous fields, multiple staff and multiple machinery that need to be accounted for, the data for daily operations ,Analog data logging has created isolated silos, high error rates, and zero real-time querying capabilities.tractors are wasteful of  resources as uncoordinated machinery navigation leads to spatial overlap, inflated fuel usage, and premature equipment wear. the solution write well factored zero dependancy  code to handle the data handling, and machinery oversight via  zero-library python programming 

## Technical Stack & Architectural Constraints

The project relies exclusively on native Python data structures, set operations, and custom control flow with **zero third-party dependencies**. 

By deliberately avoiding abstraction-heavy frameworks like Pandas or NumPy, every spatial algorithm and state mutation explicitly demonstrates raw algorithmic complexity ($O(1)$ set lookups vs. linear scans), explicit boundary defenses, and deterministic memory mutation without external package overhead.

### Scale & Setting
he system models a large-scale agricultural enterprise operating across multiple field plots, field personnel, and heavy machinery assets.

### Core System Bottlenecks
1. **Data Integrity & Visibility:** Daily field operations relied on analog data logging, creating isolated data silos, high human error rates, and zero real-time querying capabilities for operational ledger audits.
2. **Spatial Traversal & Resource Management:** Uncoordinated machinery navigation led to spatial overlap—frequently re-ploughing previously worked ground—inflating fuel consumption and causing premature equipment wear.

### Engineering Mandate
Design and build a zero-dependency Digital Twin in Python to enforce defensive state management, automate cross-farm crop comparisons, and optimize fleet movement via algorithmic path planning.

---

## Technical Stack & Architectural Constraints

The project relies exclusively on native Python data structures, set operations, and custom control flow with **zero third-party dependencies**. 

By deliberately avoiding abstraction-heavy frameworks like Pandas or NumPy, every spatial algorithm and state mutation explicitly demonstrates raw algorithmic complexity ($O(1)$ set lookups vs. linear scans), explicit boundary defenses, and deterministic memory mutation without external package overhead.

![set_applications in plant comparison](images/compare_plants.png)
---

## Architectural Deep Dives & Code Proofs

### 1. Defensive State Management & Ledger Integrity
* **Problem:** Simultaneous harvest entries across different farm locations risked throwing runtime `KeyError` exceptions or overwriting existing yields when keys were uninitialized.
* **Solution (`record_harvest`):** Dynamically inspects nested dictionary paths and initializes missing branches on the fly before aggregating numerical values.

![record_harvest function implementation and execution output](images/record_harvest.png)

## Poor resource management
* **Problem:** The tractors ended up wasting resources mainly fuel due to poor ploughing techniques
* **solution** Implement Boustrophedon sweep in ploughing by continuos iteration to find the most suitable
  <table>
  <tr>
    <td width="50%">
      <img src="images/standard_movement.png" alt="tractor_movement_standard implementation and execution output">
      <p align="center"><b>Standard Traversal</b></p>
    </td>
    <td width="50%">
      <img src="images/realistic_movement.png" alt="tractor_movement_realistic implementation and execution output">
      <p align="center"><b>Optimized Boustrophedon Traversal</b></p>
    </td>
  </tr>
</table>

```python
def record_harvest(registry, farm_id, crop, kg):
    if farm_id not in registry:
        registry[farm_id] = {}
    if crop not in registry[farm_id]:
        registry[farm_id][crop] = 0
