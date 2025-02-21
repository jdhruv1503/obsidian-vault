

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