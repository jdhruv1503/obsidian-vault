# Power and Energy in Processors

- **Hotspots**: Power consumption is usually not distributed evenly, leading to hotspots.
- **High Temperature**: High temperature increases leakage, which increases power consumption, further raising temperature.
- **Smart Layout**: Separating hot units increases the processor’s power envelope (maximum power that can be dissipated using cooling technology).
- **Bigger Systems**: Larger systems cool better and dissipate more power.

### Power Formulas:
- **Power**:  
  $$Power = \frac{Energy}{Time} = \frac{J}{sec}$$
- **Power Density**:  
  $$Power\ Density = \frac{Power}{Area} = \frac{W}{cm^2}$$

### Energy and Delay:
- **Energy**:  
  $$Energy\ (E) = \frac{Energy}{Instruction} = \frac{Power \times sec}{Instruction} = \frac{Watt}{Max\ IPS}$$
- **Delay**:  
  $$Delay\ (D) = \frac{sec}{Instruction} = \frac{1}{MIPS}$$
- **Energy Delay Product**:  
  $$Energy\ Delay\ Product = E \times D = \frac{Watt}{MIPS^2}$$

---

## Sources of Power Consumption

1. **Dynamic Power**: Charging and discharging capacitors due to switching.
2. **Short Circuit Currents**: Current flowing between power rails during switching.
3. **Leakage Current**: Current through diodes and transistors even when inactive.

---

## Power vs. Speed

- **High Performance**: Requires placing frequently communicating components close together.
- **High Power Components**: Highly active components produce high power, leading to hotspots.
- **Hotspots**: Cause high leakage, increasing temperature.
- **Trade-off**: Reducing frequency to manage thermal constraints may result in performance loss.

---

## Power Consumption of CMOS

$$P = \alpha C_L V^2_{dd} f$$

Where:
- $\alpha$: Switching activity
- $C_L$: Load capacitance
- $V_{dd}$: Supply voltage (quadratic relationship!)
- $f$: Switching frequency

---

## Delay of CMOS

$$\tau = k C_L \frac{V_{dd}}{(V_{dd} - V_t)^2}$$

Where:
- $V_t$: Threshold voltage ($V_t << V_{dd}$)

---

## Power Saving Techniques

1. **Reduce Power Supply ($V$)**.
2. **Reduce Frequency ($f$)**.
3. **Disable Unused Function Units**.
4. **Disconnect from Power Supply When Not in Use**.

### Gating Techniques:
- **Clock Gating**: Disable clock to unused registers.
- **Signal Gating**: Disable signals.
- **Power Gating**: Deactivate $V_{dd}$ for unused hardware.

### Power-Down Modes:
- **Idle Mode**: Clock distribution off.
- **Nap Mode**: Caching off.
- **Sleep Mode**: Power off to unused logic.

When a device becomes idle, it transitions to a lower power state. Transitioning back to the active state requires additional time and energy. The optimal time threshold to transition to the sleep state balances:
- **Too Soon**: Frequent start-up costs.
- **Too Late**: Excessive time in high-power state.

Generally, transition to sleep state when the cost of being active exceeds the cost of waking up.

---

## Dynamic Voltage and Frequency Scaling

- **Consume Only What You Need**.
- Use the lowest frequency that meets performance targets.
- Use the lowest $V_{dd}$ that allows the desired frequency.
- Regulated by software based on system load.

If a variable voltage processor completes a task before the deadline, energy consumption can be reduced. For a single supply voltage $V$, completing a task $T$ just at its deadline minimizes energy consumption.

### Approaches:
1. **Usage Model-Based**: Simple approach without OS support.
2. **Firmware-Based**: Power management policy built into the processor’s firmware, also without OS modification.

---

## Multicore Optimizations

- **Heterogeneous Processors**: Match workloads to the most energy-efficient core.
- **Objective**: Optimize for energy, energy-delay, etc.
- **Switching**: OS-level scheduler switches at OS time slice multiples.
- **Power Down Unused Cores**.

---

## Optimizing Memory and Cache

- **Memory System**: Can consume up to 90% of energy in embedded systems.
- **System-on-Chip (SoC)**: Composed of multiple memories.

### Cache Energy Dissipation:
- **Bit Lines**: Precharge, read, and write cycles.
- **Word Lines**: Accessed whenever a line is accessed.
- **Address Decoders**.
- **Peripheral Circuits**: Comparators, control logic, etc.

### Techniques:

#### Memory Bank Partitioning
- Partition memory into banks and activate only the one being accessed.
- **Pros**:
  - Improves speed and lowers power.
  - Reduces word capacitance and number of activated bit cells.
- **Cons**:
  - Bank decoding circuit adds delay and power.
- **Typical Division**: 2-8 banks.

#### Cache Optimization
- **SRAM Power Reduction**:
  - Cache sub-banking.
  - Modifications to CAM cell.
  - Other techniques.

![[Pasted image 20250222041731.png]]

#### Low Power DRAM
- **Conventional DRAM**: Refreshes all rows with a fixed interval, stalling read/write accesses and consuming power.
- **Selective Refresh Architecture (SRA)**:
  - Adds a valid bit to each row; refreshes only rows with the valid bit set.
  - Reduces 5%-80% of refreshes.
- **Variable Refresh Architecture (VRA)**:
  - Adds a refresh period table and refresh counter to each row; refreshes rows with appropriate periods.
  - Reduces about 75% of refreshes.

---

## Optimizing Bus

![[Pasted image 20250222041919.png]]