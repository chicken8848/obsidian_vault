A finite state machine has:
- $k$ states: $S_1, S_2, \cdots, S_k$ (one is 'initial' state)
- $m$ inputs: $i_1, i_2, \cdots, i_m$
- $n$ outputs: $o_1, o_2, \cdots, o_n$
- Transition rules: $S(s,i)$ for each state $s$, input $i$
- Output rules: $O(s)$ for each state $s$
# FSM Specifications
All state transition diagrams can be described by truth tables. Binary encodings are assigned to each state. The truth table can then be simplified using the reduction techniques we learned for combinational logic.

| IN | Current State |  | Next state |  | Unlock |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 0 | SX | 000 | S0 | 001 | 0 |
| 1 | SX | 000 | SX | 000 | 0 |
| 0 | S0 | 001 | S0 | 001 | 0 |
| 1 | S0 | 001 | S01 | 011 | 0 |
| 0 | S01 | 011 | S0 | 001 | 0 |
| 1 | S01 | 011 | S011 | 010 | 0 |
| 0 | S011 | 010 | S0110 | 100 | 0 |
| 1 | S011 | 010 | SX | 000 | 0 |
| 0 | S0110 | 100 | S0 | 001 | 1 |
| 1 | S0110 | 100 | S01 | 011 | 1 |