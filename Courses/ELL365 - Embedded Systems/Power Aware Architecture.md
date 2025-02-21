

Usually not distributed evenly: hotspots

High temperature  high leakage  high
power  higher temperature

Smart layout that separates the hot units
increases the processor’s power envelope
Power envelope: max power that can be dissipated using cooling tech

Bigger systems cool better and
dissipate more power!

$Power = \frac{energy}{time} = \frac{J}{sec}$
$\text{Power density} = \frac{Power}{area} = \frac{W}{cm^2}$

---
$Energy E = \frac{energy}{instruction} = \frac{Power*sec}{instruction} = \frac{Watt}{Max IPS}$
$Delay D = \frac{sec}{instruction} = \frac{1}{MIPS}$
$Energy Delay Product = E*D = \frac{Watt}{MIPS^2}$
---
## Sources of power consumption

- Dynamic: Charging and discharging caps due to switching
- Short Circuit Currents: Between power rails during switching
- Leakage current: Diodes and transistors

---
## Power vs. Speed
In order to achieve high performance we need to
put all the components that frequently talk to
each other as close as possible

Highly active components produces high power

Close high power components cause hot spots

Hot spots cause high leakage that increase the
temperature.

We need to reduce the frequency and to loose
performance in-order to keep within the thermal
constraints

---

## Power consumption of CMOS

$P =\alpha C_L V^2_{dd} f$

Where 
$\alpha$: switching activity
$C_L$: load capacitance
$V_{dd}$: supply voltage (QUADRATIC RELATION!)
$f$: switching frequency

## Delay of CMOS

$\tau = k C_L \frac{V_{dd}}{(V_{dd}-V_{t})^2}$

Where $V_t$ is threshold voltage (<< $V_{dd}$)

---

## Power saving

Reduce power supply V
Reduce f
Disable function units (control) not in use
Disconnect from power supply when not in use

### Gating

Clock gating: Disable clock to unused registers
Signal gating: Disable signals
Power gating: Deactivate Vdd for unused hardware

### Power-down modes

Controlled by user of software: Idle mode (clock distribution off), nap mode (caching off), sleep mode (power off to unused logic)

When a device becomes idle, it can transition to
lower power usage state.

A fixed amount of additional time and energy are
required to transition back to active state when a
new request for service arrives.

What is the best time threshold to transition to the
sleep state?

Too soon: pay start-up cost too frequently.
Too late: spend too much time in the high-
power state

Generally, transition to sleep state when the cost
of being in active state is at least the cost of
waking up.

### Dynamic V and f scaling

Consume as much as you need

Use lowest frequency that achieves
performance target

Use lowest Vdd that allows desired
frequency

Regulated by software based on
system load

If a variable voltage processor completes a
task before the deadline, the energy
consumption can be reduced.

If a processor uses a single supply voltage
V and completes a task T just at its
deadline, then V is the unique supply
voltage which minimizes the energy
consumption of T.

Simple approach to do voltage
scaling is based on the usage
model(not on the workload). This
approach does not need any OS
support.

Another approach is to build the
power management policy into the
processor’s firmware. This approach
also does not require any OS modification.

### Multicore optimisations

SOC with heterogeneous processor
Match workload to core with best
energy efficiency
According to some objective (energy,
energy-delay,….)

Switching by OS-level scheduler at OS
time slice multiples

Power down unused cores

---
## Optimizing memory and cache

For embedded systems, memory system
can consume upto 90% of the energy

System-on-Chip composed of multiple
memories

Cache energy dissipation:
 Bit lines: precharge, read and write
cycles
 Word lines: whenever a line is accessed
 Address decoders
 Peripheral circuits: comparators, control
logic, etc.

### Techniques

#### Memory bank partitioning
Partition memory into banks and only activate the one being accessed

Pros:
Improves speed and lowers power
Word capacitance reduced
Number of activated bit cells reduced

However, the bank decoding circuit adds
delay and power

Memories are typically divided to 2-8 banks

#### Cache
SRAM power reduction
Cache sub banking
Modifications to CAM cell
Other techniques
![[Pasted image 20250222041731.png]]

#### Low power DRAM
Conventional DRAMs refresh all rows with a fixed
single time interval

Refresh cycles stalls read and write accesses
Consumes power

Selective refresh architecture (SRA): Add a valid bit to each row, refresh only those with valid bit set
Reduces 5%-80% of refreshes

Variable refresh architecture (VRA): Data retention time of each cell is different
Add a refresh period table and refresh counter to each
row and refresh with the appropriate period to each row
Reduces about 75% of refreshes

---
## Optimising bus

![[Pasted image 20250222041919.png]]

