

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