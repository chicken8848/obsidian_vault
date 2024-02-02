# MOSFET

- Devices that are used to 'switch' 1s to 0s and vice versa, so that we can implement functionalities
- It has 4 terminals. Input is supplied at the gate, output is obtained at the drain
- The current flow between source and drain $I_{DS} \propto \frac{W}{L}$ (The width and length of the MOSFET)
- Source and drain is physically symmetrical, we name them depending on the type of the MOSFET
## NFET 
The majority of the charge carrier for the bulk are holes. The majority charge carrier for the source and drain are electrons. Typically, the bulk is connected to GND to keep the PN junction reverse biased.

### How NFET Operates
![[Pasted image 20240129105413.png]]
1. Connections:
	- Bulk is connected to ```GND``` to keep the PN junction reverse biased, meaning that no current should flow or leak between source and bulk and between drain and bulk.
	- S (and also bulk) is connected to ```GND``` for NFET. Current from D is therefore drained to ```GND``` connected to S.
2. It is "ON" when $V_{GS} = V_G - V_S$ is high enough. Since source terminal is connected to GND for NFET,
	- $V_{GS} = V_G - 0 = V_G$
	- the NFET is "ON" whenever $V_G > V_{TH}$
	- When $V_G > V_{TH}$, it draws electrons towards the gate.

## PFET
The majority of the charge carrier for the bulk are electrons. The majority of the charge carrier for the source and drain are holes. Typically, the bulk is connected to VDD to keep the PN junction reverse biased.

### Terms
1. VDD: voltage source
2. $V_{TH}$: threshold voltage
3. GND: ground
4. Reverse-biased: a state whereby D is insulated from S, where current cannot flow from D to S in the presence of applied voltage
5. "ON" refers to a state whereby there exists a connection between D and S, so that current can flow through them
6. "OFF" refers to a state whereby there exists a connection between D and S, so that current can flow through them
![[Pasted image 20240129105233.png]]
# Complementary MOS circuitry
## The pull-up and pull-down circuit in CMOS 
![[Pasted image 20240129105913.png]]
To form a fully functional combinational logic device, these PFETs and NFETs can be connected together to form a CMOS circuit.

Contents of the pull-up circuit:
1. All FETs in the pull-up circuit are PFETs.
2. Hence all of their bulks are connected to the VDD, and so are all of their sources.
3. It is called 'pull-up' because when there is any connection from the source VDD to the drain output, then the output of the overall CMOS circuit is 1.
4. We call the pull-up circuit to be ON if there exists any direct path for current to flow from any source of the PFETs in the pull-up circuit to the logic output drain.

Contents of the pull-down circuit:
1. All FETs in the pull-down circuit are NFETs
2. Hence all of their bulks are connected to GND, and so are all of their sources.
3. It is called 'pull-down' because when there is connection to from the Source (GND) to the drain output, then the output of the overall CMOS circuit is 0
4. We call the pull-down circuit to be ON if there exists any direct path for electrons to flow from any source of the NFETs in the pull-down circuit to the logic output drain.

## The CMOS complementary recipe

If both PFET and NFET are both "ON", then there exists a path from VDD to GND, resulting in a short circuit. 

Hence it is important for a CMOS circuit to contain complementary pull-ups and pull-downs. Only one component on at one time.

You can also create logic circuits with this stuff, example, NAND gate:
![[Pasted image 20240129111946.png]]
# Timing Specifications
## Propagation delay $t_{PD}$
An UPPER BOUND on the delay from valid inputs to valid outputs.
## Contamination Delay $t_{CD}$
A LOWER BOUND on the delay from any invalid input to an invalid output.
In other words, how long the original valid output remains valid and stable after the original input becomes invalid.

# Power Specifications
$$
\begin{gather}
\text{Energy dissipated } = CV_{DD}^2 \text{ per cycle} \\
\text{Power consumed } = fnCV_{DD}^2 \text{ per chip}
\end{gather}
$$
where,
$$
\begin{gather}
f = \text{frequency of charge/discharge} \\
n = \text{number of gates/chip}
\end{gather}
$$

