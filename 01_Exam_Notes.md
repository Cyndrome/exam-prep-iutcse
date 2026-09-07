# CSE 4205 — Exam Notes
## Chapter 5: Sequential Logic & Flip-Flops (Unit 3: Counters and Registers)

> Compiled from: Chapter 5 Slides (Sequential Logic), CSE_4205 Slides 9–16 (JN), and Unit 3 Flip-Flops Notes (M. Raja, CSE/KLU)

---

## TABLE OF CONTENTS
1. Combinational vs Sequential Circuits
2. Clock & Types of Sequential Circuits
3. Latches (SR, Gated SR, D Latch)
4. Flip-Flops vs Latches (Triggering)
5. Master-Slave Flip-Flop
6. Edge-Triggered Flip-Flop
7. Setup Time & Hold Time
8. JK Flip-Flop
9. T (Toggle) Flip-Flop
10. Characteristic Equations & Tables (Summary)
11. Excitation Tables (Summary)
12. Flip-Flop Conversions
13. Direct Inputs (Preset/Clear)
14. Analysis of Clocked Sequential Circuits (D, JK, T)
15. Finite State Machines: Mealy vs Moore
16. State Reduction & State Assignment
17. Design Procedure (Synthesis)
18. Synthesis Examples (D, JK, T flip-flops)
19. Moore Machine Design Example
20. Worked Example: Video Tape Player FSM

---

## 1. COMBINATIONAL vs SEQUENTIAL CIRCUITS

### Combinational Circuits
- Contain **no memory elements**.
- Output depends **only on present inputs**.

### Sequential Circuits
- Contain a **feedback path** from outputs (memory elements) back to the combinational logic.
- Output depends on **present inputs AND present state** (which reflects past inputs).
- Formally: **(inputs, current state) ⇒ (outputs, next state)**

### Comparison Table

| Combinational Circuits | Sequential Circuits |
|---|---|
| Output depends only on present input | Output depends on present input **and** past output (state) |
| No memory unit | Has memory unit to store past output |
| Examples: half adder, full adder, comparator, MUX, DEMUX | Examples: flip-flop, register, counter |
| Faster in speed | Slower compared to combinational circuits |

### Block Diagram Structure
```
Inputs ──► [Combinational Circuit] ──► Outputs
              ▲                │
              └──[Memory Elements]◄──┘  (feedback path)
```

- A sequential circuit is specified by a **time sequence** of inputs, outputs, and internal states.
- **Flip-flops** are the most commonly used memory devices — they "remember" the past history.
- Output is a function of: (1) present inputs, (2) present state of memory elements (i.e., past sequence of inputs).

---

## 2. CLOCK & TYPES OF SEQUENTIAL CIRCUITS

### Clock
- A **clock pulse generator** produces a periodic train of pulses distributed throughout the system.
- Ensures flip-flops change only when clock pulses arrive → synchronization.

### Synchronous Sequential Circuits
- Storage elements affected **only at discrete time instants** (clock pulses used at inputs of storage elements).
- Also called **clocked sequential circuits** — the most popular type.
- No instability problems.
- Memory elements = **flip-flops**: binary cells storing 1 bit; have two outputs Q (normal) and Q′ (complement); maintain state indefinitely until directed to switch by an input signal.

### Asynchronous Sequential Circuits
- Storage elements can change state at **any instant of time** (no clock).
- Output changes immediately as input changes.
- State time depends solely on internal logic circuit delays.

### Comparison

| | Synchronous | Asynchronous |
|---|---|---|
| Timing | Discrete time instants (clock-driven) | Any instant of time |
| Stability | No instability problems | Can have race/instability issues |
| Example | Flip-flops, synchronous counters | Ripple/asynchronous counters |

---

## 3. LATCHES

### 3.1 SR Latch with NOR Gates
- Two cross-coupled NOR gates. Inputs: **S (Set)**, **R (Reset)**.
- **Set → 1, Reset → 0**

**Function Table:**

| S | R | Q | Q′ | Comment |
|---|---|---|----|---------|
| 1 | 0 | 1 | 0 | Set state |
| 0 | 0 | 1 | 0 | (after S=1,R=0) — remains |
| 0 | 1 | 0 | 1 | Reset state |
| 0 | 0 | 0 | 1 | (after S=0,R=1) — remains |
| 1 | 1 | 0 | 0 | **Forbidden/Invalid** (Q = Q′, oscillates if next SR=00) |

**Characteristic equation:** Q(t+1) = S + R′·Q(t)  (with S·R = 0 restriction)

**Behavior Summary (asynchronous circuit):**
- (S,R) = (0,0): no operation (memory/hold)
- (S,R) = (0,1): reset (Q=0)
- (S,R) = (1,0): set (Q=1)
- (S,R) = (1,1): **indeterminate/invalid** state (Q=Q′=0) — avoid; if it occurs, transition to (0,0) causes unpredictable oscillation.

### 3.2 SR Latch with NAND Gates (S′R′ Latch)
- Two cross-coupled NAND gates. Behaves with **active-low** inputs.

| S | R | Q | Q′ | Comment |
|---|---|---|----|---------|
| 1 | 0 | 0 | 1 | Reset |
| 1 | 1 | 0 | 1 | (after S=1,R=0) |
| 0 | 1 | 1 | 0 | Set |
| 1 | 1 | 1 | 0 | (after S=0,R=1) |
| 0 | 0 | 1 | 1 | **Forbidden** |

- Note: For NAND-based, the invalid/forbidden state is **S=0, R=0** (opposite of NOR-based).

### 3.3 SR Latch with Control (Enable) Input
- Adds an **En** (Enable/Clock) input to gate the S and R signals.
- **En = 0** → No change (latch holds state) — this is the "no operation" / disabled condition.
- **En = 1** → Latch is enabled; behaves like basic SR latch.

**Function Table:**

| En | S | R | Next state of Q |
|----|---|---|------------------|
| 0 | X | X | No change |
| 1 | 0 | 0 | No change |
| 1 | 0 | 1 | Q = 0; reset state |
| 1 | 1 | 0 | Q = 1; set state |
| 1 | 1 | 1 | Indeterminate |

### 3.4 D Latch (Transparent Latch)
- Eliminates the indeterminate state of SR latch by using a **single data input D**, with D′ automatically fed to the R side (via inverter).
- **Gated D-latch:** D ⇒ Q when En=1 (transparent); no change when En=0.
- Called "transparent" because output follows input exactly whenever En=1.
- This is **level-triggered** (level-sensitive).

**Function Table:**

| En | D | Next State of Q |
|----|---|------------------|
| 0 | X | No change |
| 1 | 0 | Q = 0 (reset) |
| 1 | 1 | Q = 1 (set) |

**Characteristic equation:** Q(t+1) = D (when En = 1)

---

## 4. FLIP-FLOPS vs LATCHES (Triggering)

### Trigger
- The state of a latch/flip-flop switches due to a change of the control (clock) input.

| Type | Triggering | Sensitivity |
|------|------------|-------------|
| **Latch** | Level triggered | Responds throughout the entire clock pulse width (level-sensitive) |
| **Flip-Flop** | Edge triggered | Responds only at the instant of clock transition (rising or falling edge) |

### Problem with Latches (Level-Triggered) in Sequential Circuits
- If a level-triggered device is used with feedback, the **feedback path can cause instability** because the time interval of logic-1 (pulse width) is too long.
- **Multiple transitions** might occur during the logic-1 level → unreliable operation.

### Solution: Edge-Triggered Flip-Flops
- State transition happens **only at the clock edge** (instantaneous, ideally zero-width).
- Eliminates the multiple-transition problem.
- Two design approaches: (a) **Master-Slave** flip-flop, (b) **True edge-triggered** flip-flop.

### Four Types of Pulse-Triggering (from Unit 3 notes)
1. **High Level Triggering** — responds while clock is HIGH (straight lead symbol, no bubble/triangle).
2. **Low Level Triggering** — responds while clock is LOW (bubble on clock lead, no triangle).
3. **Positive Edge Triggering** — responds at LOW→HIGH transition (triangle symbol, no bubble).
4. **Negative Edge Triggering** — responds at HIGH→LOW transition (triangle + bubble).

**Clock Pulse Transition:** A pulse moves 0→1 (**positive/rising edge**) then 1→0 (**negative/falling edge**).

---

## 5. MASTER-SLAVE FLIP-FLOP

- Composed of **two separate latches in series**: a **Master latch** (positive-level triggered, i.e., enabled when CLK=1) and a **Slave latch** (negative-level triggered, i.e., enabled when CLK=0), connected via an inverter on the clock line.

### Operation
- **CLK = 1:** Master is enabled — (S,R) ⇒ (Y,Y′). Slave is disabled — (Q,Q′) holds.
- **CLK = 0:** Master is disabled — (Y,Y′) holds. Slave is enabled — (Y,Y′) ⇒ (Q,Q′).
- (S,R) **cannot** affect (Q,Q′) directly — they must pass through the master first.
- **State changes coincide with the negative-edge transition of CLK** (i.e., output changes when clock goes from 1 to 0).
- This **isolates** the output from being affected while the input is still changing — solving the instability problem.

### Working Summary (from Unit 3 Notes)
- When CLK = 1: Master is **gated** (accepts input), Slave is in **hold**.
- When CLK = 0: Master goes to **hold**, Slave becomes **gated** (passes master's locked value to output).
- Data is accepted on HIGH clock, but output changes only on the **falling edge** — making it a synchronous device.

---

## 6. EDGE-TRIGGERED FLIP-FLOP (D-type, Positive-Edge)

- Built from **three (or four) NAND-based SR latches**.
- The circuit ensures state change occurs **only during the clock-pulse transition** (edge), not throughout the pulse width.

### Behavior Summary
- (S,R) = (0,1): Q = 1
- (S,R) = (1,0): Q = 0
- (S,R) = (1,1): no operation (hold)
- (S,R) = (0,0): should be avoided (invalid for internal gates)

### Positive-Edge-Triggered FF Summary
- **CLK = 0:** (S,R) = (1,1) → no state change (internally held)
- **CLK = ↑ (rising edge):** state changes **once**
- **CLK = 1:** state **holds** (no further changes even if D changes)
- This **eliminates feedback/instability problems** in sequential circuits.
- **All flip-flops must make their transition at the same time** (synchronized to the same clock edge).

---

## 7. SETUP TIME AND HOLD TIME

### Setup Time (t_setup)
- The D input **must be maintained at a constant value prior to** the application of the active clock edge.
- Equals the **propagation delay through the input gates** feeding the internal latches (e.g., gates 4 and 1 in the example circuit).
- If violated, the flip-flop may not correctly capture the new data.

### Hold Time (t_hold)
- The D input **must NOT change after** the application of the active clock edge (for a short duration after).
- Equals the **propagation delay of the internal reset/feedback gate** (e.g., gate 3).
- If violated, the flip-flop output may become unpredictable (metastability).

**Example values (from timing diagram):** setup time = 2.8 ns, hold time = 1.4 ns (illustrative, from Fig 5's timing diagram with 1.4 ns gate delays).

**Mnemonic:** *Setup* = time BEFORE clock edge data must be stable. *Hold* = time AFTER clock edge data must remain stable.

---

## 8. JK FLIP-FLOP

- Derived conceptually as **D flip-flop + external combinational logic**, or as an SR flip-flop with the invalid state eliminated.
- Inputs: **J** (like Set) and **K** (like Clear/Reset).
- When **J = K = 1**, output **toggles** (complements) — this resolves the SR "forbidden state" problem.

### Characteristic/Function Table

| J | K | D (internal) | Q(t+1) | Function |
|---|---|---|--------|----------|
| 0 | 0 | Q(t) | Q(t) | No change (memory) |
| 0 | 1 | 0 | 0 | Reset FF to 0 |
| 1 | 0 | 1 | 1 | Set FF to 1 |
| 1 | 1 | Q′(t) | Q′(t) | Complement output (toggle) |

### Characteristic Equation
**Q(t+1) = JQ′ + K′Q**

### Internal Realization
- D = JQ′ + K′Q (JK flip-flop can be built from a D flip-flop plus this combinational logic, with feedback of Q, Q′).
- **All operations must complete within the clock interval** — i.e., feedback settling must finish before the next active edge (important for the "unclocked" NAND-based JK to avoid race).

### Clk-based Characteristic Table (Unit 3 Notes version)

| Clk | J | K | Q(t+1) |
|-----|---|---|--------|
| 0 | X | X | Memory |
| 1 | 0 | 0 | Memory |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 (Set) |
| 1 | 1 | 1 | Toggle |

---

## 9. T (TOGGLE) FLIP-FLOP

- **"Complementing FF"**: When T = 1, a clock edge **complements** the output. When T = 0, no change.
- Useful for designing **binary counters**.

### Characteristic Table

| T | Q(t+1) | Function |
|---|--------|----------|
| 0 | Q(t) | No change |
| 1 | Q′(t) | Complement |

### Characteristic Equation
**Q(t+1) = T ⊕ Q = TQ′ + T′Q**

### Two Realizations
**(a) From JK flip-flop:** Tie J and K together (J = K = T).
**(b) From D flip-flop:** D = T ⊕ Q = TQ′ + T′Q (XOR gate feeding D, with Q fed back).

---

## 10. CHARACTERISTIC EQUATIONS/TABLES — MASTER SUMMARY TABLE

| Flip-Flop | Characteristic Equation | Characteristic Table |
|-----------|--------------------------|------------------------|
| **SR** | Q(t+1) = S + R′Q(t)  (S·R=0) | S=0,R=0→No change; S=0,R=1→0; S=1,R=0→1; S=1,R=1→invalid |
| **D** | Q(t+1) = D | D=0→Reset(0); D=1→Set(1) |
| **JK** | Q(t+1) = JQ′ + K′Q | 00→No change; 01→Reset(0); 10→Set(1); 11→Complement |
| **T** | Q(t+1) = TQ′ + T′Q = T⊕Q | T=0→No change; T=1→Complement |

- Characteristic equations define next state **algebraically** as a function of inputs and present state.
- Characteristic tables define next state in **tabular form**.

---

## 11. EXCITATION TABLES — MASTER SUMMARY TABLE

Excitation tables specify **what flip-flop inputs are required** to cause a specific present-state → next-state transition. Essential for **synthesis (design)**.

| Q(t) | Q(t+1) | S | R | J | K | D | T |
|------|--------|---|---|---|---|---|---|
| 0 | 0 | 0 | X | 0 | X | 0 | 0 |
| 0 | 1 | 1 | 0 | 1 | X | 1 | 1 |
| 1 | 0 | 0 | 1 | X | 1 | 0 | 1 |
| 1 | 1 | X | 0 | X | 0 | 1 | 0 |

**How to read:** "X" = don't care. E.g., for JK: if Q(t)=0 and Q(t+1)=1, J must be 1 but K can be X (0 or 1) since K only matters when trying to reset.

**Memory Trick for JK Excitation:**
- 0→0: J=0, K=X
- 0→1: J=1, K=X
- 1→0: J=X, K=1
- 1→1: J=X, K=0

---

## 12. FLIP-FLOP CONVERSIONS (General Procedure)

**Steps to convert Flip-Flop A (available) to Flip-Flop B (required):**
1. Write the truth table listing Qp (present), Qp+1 (desired next state), inputs of B (external/given), and derive required inputs of A (actual FF) for every combination.
2. Use excitation table of FF-A to find its required inputs for each Qp→Qp+1 transition.
3. Simplify the FF-A input expressions using K-maps (in terms of B's inputs and Qp).
4. Draw the combinational logic + FF-A to realize FF-B.

### 12.1 SR → JK
- S = J·Qp′ ; R = K·Qp

### 12.2 JK → SR
- J = S ; K = R (with S,R=1,1 treated as don't care for J,K since invalid for SR)

### 12.3 SR → D
- S = D ; R = D′

### 12.4 D → SR
- D = S + R′·Qn (i.e., D = S + R̄Qn)

### 12.5 JK → T
- J = T ; K = T

### 12.6 JK → D
- J = D ; K = D′

### 12.7 D → JK
- D = J·Qp′ + K′·Qp

**Quick Reference Table:**

| Conversion | Equation(s) |
|------------|-------------|
| SR → JK | S = J·Q′, R = K·Q |
| JK → SR | J = S, K = R |
| SR → D | S = D, R = D′ |
| D → SR | D = S + R′Q |
| JK → T | J = K = T |
| JK → D | J = D, K = D′ |
| D → JK | D = JQ′ + K′Q |

---

## 13. DIRECT INPUTS (Preset / Clear)

- **Preset (PRE)** — asynchronous input that **sets** the FF ("direct set").
- **Clear (CLR)** — asynchronous input that **clears** the FF ("direct reset").
- **Purpose:** Bring all FFs in a system to a known state **before** clocked operation begins (initialization).
- **Asynchronous set:** Set occurs **as soon as** preset = 1 (independent of clock).
- **Synchronous set:** Set occurs only when preset = 1 **AND** CLK↑ (i.e., on next clock edge).
- In circuit diagrams, active-low Reset/Preset is denoted with a bubble at the input.

**Example (D FF with Asynchronous Reset):**

| R | Clk | D | Q | Q′ |
|---|-----|---|---|----| 
| 0 | X | X | 0 | 1 |
| 1 | ↑ | 0 | 0 | 1 |
| 1 | ↑ | 1 | 1 | 0 |

(Reset is active LOW here: R=0 forces Q=0 regardless of clock; FF triggers on positive edge of CLK only when R=1.)

---

## 14. ANALYSIS OF CLOCKED SEQUENTIAL CIRCUITS

**Goal:** Given a circuit, derive: state (transition) equations, output equation(s), state table, and state diagram.

**General procedure:**
1. Identify flip-flop input equations (excitation equations) from the circuit — i.e., D_A, D_B, J_A, K_A, etc. in terms of present state and external inputs.
2. Substitute into the flip-flop's characteristic equation to get state equations A(t+1), B(t+1).
3. Derive the output equation y = f(present state, inputs).
4. Build the state table (present state, input → next state, output).
5. Draw the state diagram: circles = states; directed edges labeled "input/output".

### 14.1 Analysis with D Flip-Flops
- Since D flip-flop's characteristic equation is Q(t+1) = D, the **state equation = the input equation directly**.
- Example: A(t+1) = Ax + Bx; B(t+1) = A′x; y = (A+B)x′

### 14.2 Analysis with JK Flip-Flops
1. Determine J and K input equations from circuit (function of present state + inputs).
2. Use JK characteristic table (or equation Q(t+1)=JQ′+K′Q) to determine next state for every present-state/input combination.
3. Build state table with columns: Present State | Input | Next State | Flip-Flop Inputs (J_A,K_A,J_B,K_B).

**Example (from slides):**
- J_A = B, K_A = Bx′
- J_B = x′, K_B = A′x + Ax′
- State equations (derived): A(t+1) = A′B + AB′ + Ax ; B(t+1) = B′x + ABx + A′Bx′

### 14.3 Analysis with T Flip-Flops
- Characteristic equation: Q(t+1) = TQ′ + T′Q = T⊕Q
- Determine T input equations, apply to get next state.

---

## 15. FINITE STATE MACHINE (FSM): MEALY vs MOORE

Two models for describing sequential circuit behavior:

### Mealy Machine
- **Outputs are functions of BOTH present state AND inputs.**
- Output can change asynchronously with input changes (within a clock period), since it's combinationally derived from input+state.
- Block diagram: Inputs feed both the "Next State Logic" AND the "Output Logic" directly.

### Moore Machine
- **Outputs are functions of the PRESENT STATE ONLY.**
- Output changes synchronously — only when the state changes (i.e., at clock edges).
- Block diagram: Inputs feed only "Next State Logic"; output logic depends only on the State Register output.

### Comparison Table

| Feature | Mealy | Moore |
|---------|-------|-------|
| Output depends on | State + Input | State only |
| Output changes | Can change mid-cycle (with input) | Changes only at clock edge |
| Number of states | Often fewer | Often more (needs extra states to "hold" output) |
| Response speed | Faster (reacts immediately to input) | Slower (waits for next state) |
| Circuit complexity | Output logic more complex | Output logic simpler |

**Block diagrams:**
```
Mealy:  Inputs ──┬──► [Next State Comb. Logic] ──► [State Register] ──┬──► [Output Comb. Logic] ──► Outputs (Mealy-type)
                 └───────────────────────────────────────────────────┘
                 (inputs also feed output logic directly)

Moore:  Inputs ──► [Next State Comb. Logic] ──► [State Register] ──► [Output Comb. Logic] ──► Outputs (Moore-type)
                 (inputs do NOT feed output logic directly)
```

---

## 16. STATE REDUCTION & STATE ASSIGNMENT

### 16.1 State Reduction
- **Goal:** Reduce number of states → potentially reduce number of flip-flops and gates (not guaranteed, but often helps).
- **Equivalent States:** Two states are equivalent if, for **every input**, they produce **exactly the same output** AND go to the **same next state** (or to equivalent next states).
- If two states are equivalent, **one can be eliminated** (merged) — one of the duplicate rows is removed from the state table, and references to the removed state are redirected to its equivalent.
- **Method:** Check each pair of states systematically for equivalence; unused/don't-care states can be exploited for further minimization.
- **Important:** State reduction does **NOT guarantee** a saving in flip-flops or gates — but often does.

**Worked Example concept:** A 7-state machine reduced to a 5-state machine by finding states with identical output sequences behavior (e.g., states e, f, g in the example collapse due to identical next-state/output behavior).

### 16.2 State Assignment
- **Goal:** Assign unique binary codes to each state to **minimize the cost of the combinational circuits** (not necessarily straightforward — "not easy certainly").
- **Rule:** Any binary assignment is satisfactory as long as each state gets a **unique** code.

**Three Common Assignment Methods:**

| Method | Bits Needed | Notes |
|--------|-------------|-------|
| **Binary Code** | n-bit for m states, where 2ⁿ ≥ m | Simple, uses fewest FFs |
| **Gray Code** | n-bit for m states, where 2ⁿ ≥ m | Adjacent codes differ by 1 bit → more suitable for K-map simplification, potentially lower power (fewer bit transitions) |
| **One-Hot** | m-bit for m states | Uses m flip-flops (one per state, only one is "1" at a time); often used in control/FSM design for speed & simplicity of decode logic |

**Example (5 states a,b,c,d,e):**

| State | Binary | Gray | One-Hot |
|-------|--------|------|---------|
| a | 000 | 000 | 00001 |
| b | 001 | 001 | 00010 |
| c | 010 | 011 | 00100 |
| d | 011 | 010 | 01000 |
| e | 100 | 110 | 10000 |

---

## 17. DESIGN PROCEDURE (SYNTHESIS)

**"Synthesis"** = the part of design that follows a well-defined procedure, starting from a spec and ending in a working circuit.

### Steps:
1. **Specification → State Diagram** (most challenging step — requires understanding of the problem).
2. **State reduction** if necessary (eliminate equivalent states).
3. **Assign binary values** to the states (state assignment).
4. **Obtain the binary-coded state table.**
5. **Choose the type of flip-flop** (D, JK, T, SR).
6. **Derive the simplified flip-flop input equations and output equations** (using K-maps).
7. **Draw the logic diagram.**

---

## 18. SYNTHESIS EXAMPLES

### 18.1 Example Problem
**"Design a circuit that detects one to three or more consecutive 1's in an input string."**

**State Diagram (4 states S0,S1,S2,S3):**
- S0 (output 0) --0--> S0 (self loop); --1--> S1
- S1 (output 0) --0--> S0; --1--> S2
- S2 (output 0) --0--> S0; --1--> S3
- S3 (output 1) --0--> S0; --1--> S3 (self loop, stays outputting 1)

**State Table (using state variables A,B):**

| A | B | x | A(next) | B(next) | y |
|---|---|---|---------|---------|---|
| 0 | 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 | 1 | 0 |
| 0 | 1 | 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 1 | 0 | 0 |
| 1 | 0 | 0 | 0 | 0 | 0 |
| 1 | 0 | 1 | 1 | 1 | 0 |
| 1 | 1 | 0 | 0 | 0 | 1 |
| 1 | 1 | 1 | 1 | 1 | 1 |

### 18.2 Synthesis Using D Flip-Flops
Since D(t+1) = Q(t+1) directly:
- **D_A(A,B,x) = Σm(3,5,7) = Ax + Bx**
- **D_B(A,B,x) = Σm(1,5,7) = Ax + B′x**
- **y(A,B,x) = Σm(6,7) = AB**

### 18.3 Synthesis Using JK Flip-Flops
Using excitation table to determine J_A, K_A, J_B, K_B from the state table, then K-map simplification:
- **J_A = Bx′**
- **K_A = Bx**
- **J_B = x**
- **K_B = (A ⊕ x)′**

*(Compare: JK design needs 4 excitation equations vs D design's 2 — but JK circuits may need fewer gates overall in some cases since J/K don't-cares allow more simplification.)*

### 18.4 Synthesis Using T Flip-Flops — 3-bit Binary Counter Example
**No inputs except clock** — this is a self-incrementing counter (000→001→010→...→111→000).

**State Table (3-bit counter):**

| A2 A1 A0 (present) | A2 A1 A0 (next) | T_A2 T_A1 T_A0 |
|---|---|---|
| 000 | 001 | 0 0 1 |
| 001 | 010 | 0 1 1 |
| 010 | 011 | 0 0 1 |
| 011 | 100 | 1 1 1 |
| 100 | 101 | 0 0 1 |
| 101 | 110 | 0 1 1 |
| 110 | 111 | 0 1 1 |
| 111 | 000 | 1 1 1 |

**Simplified equations (via K-map):**
- **T_A0 = 1** (LSB always toggles every clock)
- **T_A1 = A0** (toggles when A0=1)
- **T_A2 = A1·A0** (toggles when both A1 AND A0 = 1)

**Logic diagram:** Three T flip-flops in cascade; T_A0 tied to 1 (always toggle); T_A1 = A0; T_A2 = AND(A1,A0). This is the classic **ripple/synchronous binary counter** pattern (generalizes: T_An = AND of all lower bits).

---

## 19. MOORE MACHINE — DETAILED DESIGN EXAMPLE

**Given state diagram** (4 states S0,S1,S2,S3) with a Moore-type next-state/output table:

| Present State | Next State (I=0) | Next State (I=1) | Output (I=0) | Output (I=1) |
|---|---|---|---|---|
| S0 | S0 | S2 | 0 | 0 |
| S1 | S0 | S2 | 1 | 1 |
| S2 | S2 | S3 | 1 | 1 |
| S3 | S3 | S1 | 0 | 0 |

*(Note: In true Moore form, output depends only on state — the table above shows the same output for both I=0 and I=1 columns per row, confirming Moore behavior: e.g., S0 always outputs 0, S1 and S2 always output 1, S3 always outputs 0.)*

### Design steps:
1. **4 states → need 2 flip-flops** (call them M, N). Using JK flip-flops.
2. Build **combined table**: Present State (M,N) | Input I | Next State (M,N) | JK inputs for M and N | Output.
3. Use K-maps to extract:
   - **M_J = I**
   - **M_K = N·I**
   - **N_J = M·I**
   - **N_K = M′**
   - **Output = M′N + MN′** (i.e., output = M ⊕ N)
4. Draw circuit: Next-state logic (AND/OR gates) → State register (2 JK FFs) → Output logic (XOR-equivalent via M′N+MN′).

### Comparison: D flip-flop implementation
Using D flip-flops for the same Moore machine gives:
- D_A and D_B derived via K-map from present state/input → next state.
- **Output remains the same:** M′N + MN′ (independent of which FF type is used internally — output logic depends only on state encoding, not FF type).
- **Which implementation is better?** Depends on gate count after minimization — generally compared by counting literals/gates in the derived equations for each FF type (JK often gives simpler equations due to more don't-cares in excitation table, but requires 2 gate networks per FF instead of 1).

---

## 20. WORKED EXAMPLE: VIDEO TAPE PLAYER (FSM Design Case Study)

**Inputs:** Stop_Button, Pause_Button, Forward_Button, Rewind_Button, Play_Button, Record_Button, Reset, clk
**Outputs:** Stop_Tape, Pause_Tape, Forward_Tape, Rewind_Tape, Play_Tape, Record_Tape

**States and Transitions:**
- **Stop** (Stop_Tape=1): entered via Reset=1 or Stop_Button=1
- **Will_Record** → **Record** (Record_Tape=1): entered via Record_Button=1 AND Play_Button=1 (simultaneously)
- **Will_Forward** → **Forward** (Forward_Tape=1): entered via Forward_Button=1
- **Will_Rewind** → **Rewind** (Rewind_Tape=1): entered via Rewind_Button=1
- **Will_Play** → **Play** (Play_Tape=1) ⇄ **Pause** (Pause_Tape=1): Play_Button=1 enters Will_Play→Play; Pause_Button=1 toggles between Play and Pause.
- All "Will_X" states output Stop_Tape=1 momentarily (transition/preparation states) before settling into the actual action state.

**Key design insight:** This demonstrates a real-world **Moore-style FSM** where each state has one dedicated output active, and "Will_X" intermediate states are used to model realistic mechanical delay (e.g., a VCR must stop the tape motion before it can rewind/forward/play).

---

## QUICK-REVISION FORMULA SHEET

| Concept | Formula/Fact |
|---------|--------------|
| SR characteristic eq. | Q(t+1) = S + R′Q, restriction S·R=0 |
| D characteristic eq. | Q(t+1) = D |
| JK characteristic eq. | Q(t+1) = JQ′ + K′Q |
| T characteristic eq. | Q(t+1) = TQ′ + T′Q = T⊕Q |
| SR→JK | S=JQ′, R=KQ |
| JK→SR | J=S, K=R |
| SR→D | S=D, R=D′ |
| D→SR | D = S + R′Q |
| JK→T | J=K=T |
| JK→D | J=D, K=D′ |
| D→JK | D = JQ′+K′Q |
| Mealy output | f(state, input) |
| Moore output | f(state) only |
| n-bit binary/gray code | covers up to 2ⁿ states |
| One-hot code | needs m FFs for m states |
| Setup time | Data stable BEFORE clock edge |
| Hold time | Data stable AFTER clock edge |
| Master-Slave FF trigger | Output changes on negative (falling) edge of CLK |
| Latch vs FF | Latch = level-triggered; FF = edge-triggered |

---

## COMMON PITFALLS / EXAM TRAP AREAS
1. **Confusing SR latch's NOR-based invalid state (S=R=1) with NAND-based invalid state (S=R=0).** These are opposite!
2. **Forgetting the excitation table "don't care" (X) entries** — students often try to force a 0 or 1 when X is correct, losing minimization opportunities.
3. **Mixing up Mealy and Moore outputs** — remember: Moore = state only (output written INSIDE the state circle); Mealy = state/input pair (output written on the ARROW/transition).
4. **State reduction does NOT always reduce flip-flop count** — a common trick question.
5. **T flip-flop counter pattern:** T_A0 = 1 always; T_A1 = A0; T_A2 = A1A0; generalizes to T_An = AND of all bits below it — a very common numerical question.
6. **Setup time vs Hold time direction** — Setup is BEFORE the edge, Hold is AFTER the edge — frequently swapped by students.
7. **Master-Slave changes on negative edge, not positive** (even though data is accepted while CLK=1) — a classic exam trick.
