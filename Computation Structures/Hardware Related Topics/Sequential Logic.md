# Needed: Storage
Comabinational logic is stateless, valid outputs always reflect current inputs.

One way to do that is to use capacitors,
- Pros:
	- Compact - low cost / bit 
- Cons:
	- complex interface
	- stable?
	- it leaks
Another way to do it is to use positive feedback to maintain storage indefinitely.

# Settable Storage Element
![[Pasted image 20240205105609.png]]

| G | D | Q' | \| | Q |
| ---- | ---- | ---- | ---- | ---- |
| 0 | -- | 0 | \| | 0 |
| 0 | -- | 1 | \| | 1 |
| 1 | 0 | -- | \| | 0 |
| 1 | 1 | -- | \| | 1 |
## Dynamic Discipline
- $T_{setup}$ = $2T_{PD}$: Interval prior to G transition for which D must be stable and valid
- $T_{HOLD}$ = $T_{PD}$: interval following G transition for which D must be stable and valid
## Edge-triggered flip flop
To address the presence of unstable/invalid output during transition of input, we need to create a DFF by putting two D-Latches in series
![[Pasted image 20240205111903.png]]
Observations:
- Only one latch "transparent " at any time:
	- master closed when slave is open
	- slave closed when master is open
	- no combinational path through flip flop
- Q only changes shortly after 0 -> 1 transition of CLK, so flip flop appears to be "triggered" by rising edge of CLK

Considering hold time requirement for slave:
- Negative (1 -> 0) clock transition => slave freezes data:
	- should be no output glitch, since master held constant data
	- BUT master output contaminated by change in G output
- HOLD TIME of slave not met, unless we assume sufficient contamination delay in the path to its D input
Accumulated $t_{CD}$ through inverter, G -> Q path of master must cover slave $t_{HOLD}$ for this design to work
- $t_{\text{CD,Master}} > t_{\text{HOLD,Slave}}$


### Flip-Flop Timing
$t_{PD}$: maximum propagation delay, CLK -> Q
$t_{CD}$: minimum contamination delay, CLK -> Q
$t_{SETUP}$: setup time
- guarantee that D has propagated through feedback path before master closes
$t_{HOLD}$: hold time
- guarantee master is closed and data is stable before allowing D to change

# Single Clock Synchronous Circuits
## Single-clock Syrchronous Discipline
- No combinational cycles
- Single periodic clock signal shared among all clocked devices
- Only care about value of register data inputs just before rising edge of clock
- Period greater than every combinational delay + setup time
- Change saved state after noise-inducing logic transitions have stopped
# Sequential Logic Device Timing Constraint
We can now use a Flip-Flop in our circuit as a 'memory' device that we can put in series.
![[Pasted image 20240206131611.png]]
![[Pasted image 20240206131625.png]]
From the diagram above, we can define two timing constraints for this particular scenario where $t_{CLK}$ is the CLK period
- $t_1$:$t_{CD}R_1 + t_{CD}CL \geq t_HR_2$
- $t_2$:$t_{PD}R_1 + t_{PD}CL + t_{SETUP}R_2 \leq t_{CLK}$
## $t_1$ for old value
The $t_1$ constraint ensures that the $t_H$ requirement of the downstream register, $R_2$, is fulfilled by the devices that's put upstream.
1. When the CLK rises at $t_i$, both $R_1$ and $R_2$ are "capturing" different values simultaneously
2. $R_1$ is receiving current input value at $t_i$, while $R_2$ is receiving the computed old input value that was produced by $R_1$ at $t_{i-1}$.
3. The devices upstream of $R_2$ has to help to hold on to this old $t_{i-1}$ values for the $t_H$ of $R_2$ to be fulfilled before responding to the rising edge of the clock and producing new values
## $t_2$ for new value
The $t_2$ constraint ensures that the clock period is long enough for three things
- valid signal to be produced at the output of $R_1$
- signal to propagate through CL in between
- signal to be set-up at the downstream register $R_2$ (for memory mode)

## $t_{pd}$ of CL
The $t_{pd}CL$ as the time taken to do actual work or logic computation.

# Timing specifications of sequential circuits
Effective propagation delay $t_{pd}$ of the circuit:
- $t_{pd} = t_{pd,R_n} + t_{pd,CL}$
Effective contamination delay $t_{CD}$ of the circuit:
- $t_{CD} = t_{CD,R_n} + t_{CD,CL}$
Where $R_n$ is the last register in the circuit
