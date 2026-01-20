# DPoA_in_PRISM_games

**DPoA_in_PRISM_games** is a PRISM games modelling of a digital power of
attorney protocol. (Submission #213 for IJCAI 2026)

---

## Requirements

- Java 9 or higher
- Compatible OS: Windows, Linux, or macOS
- PRISM-games installed. See [https://www.prismmodelchecker.org/games/download.php](https://www.prismmodelchecker.org/games/download.php).

---

## Usage with PRISM-games GUI

### 1. Launch the PRISM-games GUI

* **Linux/macOS**: Run `bin/xprism` from the PRISM-games directory.
* **Windows**: Double-click the PRISM-games shortcut.

---

### 2. Load the Model File

* In the GUI, go to the **Model** menu → **Open model**.
* Select the file: `modelSimple.prism`.
* The model should appear in the right pane. If parsed successfully, a checkmark ✅ will appear next to the model name in the left pane.
* Confirm that the model type is shown as **csg** (concurrent stochastic game).

---

### 3. Load the Property File

* In the GUI, go to the **Properties** menu → **Open properties list**.
* Select the file: `goalsModelSimple.props`.

---

### 4. Verify a Property

* Right-click on any listed property and select **Verify**.
* Use the bottom tabs to switch between: **Model**, **Properties**, **Simulator**, and **Log**

---


### 5. Results



We report the results and running times, for each of the two steps PRISM-games perform: 1) Model building 2) Model checking/analysis

- We increased the memory limits: cudd=16g, java(heap)=32g
- We set PRISM to stop the computation it reaches the time-out limit of 20 min

#### Size and Construction times for Different Models
We used the command-line version of PRISM-games to iterate the results of building different
models. Below is a typical command that we used, which 
```
bin/prism -cuddmaxmem 4g -javamaxmem 8g -javastack 1g modelSynchroMalicious.prism \ 
-const max_phases=1:3,A_malicious=false,V_malicious=false -timeout 120
```

| model | malicious | max rounds | size of S | size of T | time(s) |
|-------|-----------|------------|----------:|----------:|--------:|
| AS    | none      | 1          |       239 |       522 |    0.10 |
| AS    | none      | 2          |       807 |      2032 |    0.10 |
| AS    | none      | 3          |      1661 |      4426 |    0.12 |
| AS    | none      | 20         |     59937 |    180376 |    3.36 |
| AS    | A\*,V\*   | 1          |     16554 |     92517 |    2.70 |
| AS    | A\*,V\*   | 2          |    222612 |   1765965 |    22.0 |
| AS    | A\*,V\*   | 3          |   1087646 |  10141041 |  102.00 |
| AS    | A\*,V\*   | 4          |   3403512 |  34256223 |  336.00 |
| SJA   | none      | 1          |     14644 |     63999 |    2.10 |
| SJA   | none      | 2          |   1444772 |   9969235 |  115.00 |
| SJA   | none      | 3          |       n/a |       n/a |     T.O |
| SJA   | A\*,V\*   | 1          |     23916 |    107361 |    2.40 |
| SJA   | A\*,V\*   | 2          |   2242838 |  15352850 |  182.00 |
| SJA   | A\*,V\*   | 3          |       n/a |       n/a |     T.O |

#### Model Checking Times  


We used a model with all honest agents to model check the completeness
formulas P1, P2, and P3. We took  max_rounds=2, and we call this model AS2
```
 bin/prism -cuddmaxmem 4g -javamaxmem 8g -javastack 1g modelAS.prism goals.props -prop 1,2,3 -const max_rounds=2,A_malicious=false,V_malicious=false -timeout 600
```

We used the model with possibly dishonest A* and V* to to model check the
soundness and safety of revocation formulas P1, P2, and P3
We took  max_rounds=2
```
 bin/prism -cuddmaxmem 4g -javamaxmem 8g -javastack 1g modelAS.prism goals.props -prop 4,5 -const max_rounds=2,A_malicious=true,V_malicious=true -timeout 600
```

SJA2* = Synchronous Joint Action with max_rounds=2 with malicious A* V*

| property | model | result | time |
|----------|-------|--------|-----:|
| p1       | SJA2  | true   |  138 |
| p2       | SJA2  | true   |  107 |
| p3       | SJA2  | true   |  129 |
| p4       | SJA2  | n/a    |  n/a |
| p5       | SJA2* | true   |   84 |
| p6       | SJA2* | true   |   77 |
| p7       | SJA2* | true   |   164 |

AS2* = Asynchronous with max_rounds=2 with malicious A* V*

| property | model | result | time |
|----------|-------|--------|-----:|
| p1       | AS2   | true   | 0.15 |
| p2       | AS2   | true   | 0.10 |
| p3       | AS2   | true   | 0.09 |
| p4       | AS2   | true   |   64 |
| p5       | AS2*  | true   |   55 |
| p6       | AS2*  | true   |   60 |
| p7       | AS2*  | true   | 43 |






- 
