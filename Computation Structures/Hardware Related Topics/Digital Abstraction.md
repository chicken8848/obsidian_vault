# Combinational Devices
A combinational device is a circuit element that has
- one or more digital inputs
- one or more digital outputs
- a functional specification that details the value of each output for every possible combination of valid input values
- a timing specification consisting (at minimum) of an upper bound $t_{pd}$ on the required time for the device to compute the specified output values from an arbitrary set of stable, valid input values
A set of interconnected elements is a combinational device if 
- each circuit element is combinational
- every input is connected tot exactly one output or to some vast supply of constant 0's and 1's
- the circuit contains no directed cycles
Given an acyclic circuit meeting the above constraints, we can derive functional and timing specs for the input/output behaviour from the specs of its components

# Noise
Difference in voltage from:
- IR drop 
	- between gates: 30mV
	- within module 50mV
	- across chip: 350mV
- $L\frac{dI}{dt}$ drop
	- use extra pins and bypass caps to keep within 250mV
- LC ringing triggered by current "steps"

## Crosstalk
Crosstalk is when the true voltage is lower than the expected when there are many overlapping wiring layers in an integrated circuit. 
In a modern integrated circuit $\Delta V_a$ might be 2.5V. Designers often try to avoid these really bad cases by careful routing of signals, but some crosstalk is unavoidable.

# Sequential Interference
$\Delta V$ from energy storage left over from earlier signalling on the wire
- Transmission line discontinuities (reflections off of impedance mismatches and terminations)
- charge storage in RC circuit (narrow pulses are lost due to incomplete transitions)
- RLC ringing (triggered by voltage "steps")
Fix: slower operation, limiting voltage swings and slew rates

# Noise Margins
Because a wire does not obey the static discipline, a combinational device must restore marginally valid signals. It must accept marginal inputs and provide unquestionable outputs (to leave room for noise)

## A buffer
![[Pasted image 20240123134629.png]]
## Voltage Transfer Characteristic (VTC)
Plot of $V_{out} \text{ vs } V_{in}$ where each measurement is taken after any transients have died out.

Note: VTC does not tell you anything about how fast a device is - it measures static behaviour not dynamic behaviour

![[Pasted image 20240123134803.png]]

Static Discipline requires that the VTC avoid the shaded regions (AKA "forbidden zones"), which correspond to valid inputs but invalid outputs.

Net result: combinational devices must have GAIN > 1 and be NONLINEAR

### Finding a combinational device
Find the set of ${V_{IH}, V_{IL}, V_{OH}, V_{OL}}$ where the forbidden regions do not intersect the VTC plot.

$|V_{IH} - V_{IL}| < |V_{OH} - V_{OL}|$ to ensure that GAIN > 1 and NONLINEAR