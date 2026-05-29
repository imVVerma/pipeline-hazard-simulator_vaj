# Pipeline Hazard Simulation Applet

An interactive web-based simulator to visualize how instructions move through a CPU pipeline cycle-by-cycle. The simulator helps understand instruction pipelining, **RAW (Read After Write) hazards**, **stall insertion**, and **data forwarding** through an intuitive execution table.

Built as part of the **Fundamentals of Computer Science (FoCS)** course.

---

## Features

* Supports **4-stage** and **5-stage** pipeline configurations
* Detects **RAW hazards** automatically
* Simulates **stall insertion (baseline behavior)**
* Supports **data forwarding**
* Displays instruction execution **cycle-by-cycle**
* Visual pipeline execution table with:

  * Instruction progression
  * Hazards
  * Stalls (`STALL`)
  * Forwarding (`FWD`)

---

## Pipeline Configurations

### 4-Stage Pipeline

```text
IF → ID → EX → MEM/WB
```

Memory access and write-back are combined into a single stage.

### 5-Stage Pipeline

```text
IF → ID → EX → MEM → WB
```

Standard textbook pipeline model.

---

## Supported Instructions

This simulator supports a strict subset of **MIPS assembly**.

### Arithmetic Instructions

```text
add rd, rs, rt
sub rd, rs, rt
```

Example:

```text
add $t2, $t0, $t3
```

---

### Memory Instructions

```text
lw rt, offset(rs)
sw rt, offset(rs)
```

Examples:

```text
lw $t0, 0($t1)
sw $t2, 4($t3)
```

---

## How to Use

1. Enter instructions in **MIPS assembly format**.
2. Select:

   * **4-stage** or **5-stage** pipeline
   * **Forwarding ON/OFF**
3. Run the simulation.
4. Observe the pipeline execution table to see:

   * instruction progression
   * hazards
   * stalls
   * forwarding behavior

Example input:

```text
lw $t0, 0($t1)
add $t2, $t0, $t3
sub $t4, $t2, $t5
```

---

## Example Output

| Instruction | Cycle 1 | Cycle 2 | Cycle 3 | Cycle 4  | Cycle 5 | Cycle 6 |
| ----------- | ------- | ------- | ------- | -------- | ------- | ------- |
| I1          | IF      | ID      | EX      | MEM      | WB      |         |
| I2          | IF      | STALL   | ID      | EX (FWD) | MEM     | WB      |

---

## Project Structure

```text
pipeline-hazard-simulator/
│── index.html        # Main webpage
│── app.js            # UI interaction + simulator integration
│── simulator.js      # Pipeline simulation engine
│── test.js           # Standalone testing (optional)
│── style.css         # Styling (if applicable)
```

### File Responsibilities

#### `simulator.js`

Core simulation engine.

Handles:

* pipeline execution
* RAW hazard detection
* stall insertion
* forwarding logic
* 4-stage vs 5-stage timing

Main function:

```js
runSimulation(instructions, pipelineType, forwarding)
```

---

#### `app.js`

Acts as the bridge between UI and simulation logic.

Responsible for:

* collecting user input
* calling simulator
* rendering output

---

#### `index.html`

Loads the interface and application scripts.

---

#### `test.js`

Used to independently test pipeline logic without UI.

---

## Assumptions

### Hazard Scope

The simulator currently supports:

✅ RAW (Read After Write)

Not included:

❌ WAR (Write After Read)
❌ WAW (Write After Write)
❌ Control hazards
❌ Structural hazards

---

### Forwarding Rules

When forwarding is enabled:

* **ALU instructions (`add`, `sub`)**

  * value available after **EX**
* **Load instruction (`lw`)**

  * value available after **MEM**

Load-use hazards may still require a stall.

---

## Sample Test Cases

### No Dependency

```text
add $t0, $t1, $t2
sub $t3, $t4, $t5
```

---

### Arithmetic RAW Hazard

```text
add $t0, $t1, $t2
sub $t3, $t0, $t4
```

---

### Load-Use Hazard

```text
lw $t0, 0($t1)
add $t2, $t0, $t3
```

---

### Forwarding Comparison

Run the same sequence with forwarding ON and OFF:

```text
lw $t0, 0($t1)
add $t2, $t0, $t3
```

---

## Running the Project

Clone the repository:

```bash
git clone <repo-url>
```

Navigate into the project:

```bash
cd pipeline-hazard-simulator
```

Open locally using a server (recommended).

### VS Code

Use **Live Server**

OR

### Python

```bash
python -m http.server 8000
```

Open:

```text
http://localhost:8000
```

---

## Contributors

Built by students as part of the **Fundamentals of Computer Science (FoCS)** course.
