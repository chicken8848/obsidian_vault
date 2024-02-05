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
