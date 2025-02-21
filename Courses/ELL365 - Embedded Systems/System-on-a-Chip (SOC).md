# System-on-Chip (SoC)

- A **System-on-Chip (SoC)** integrates processors, caches, memory, and input/output (I/O) devices onto a single piece of silicon.
- This integration reduces product cost and is used whenever possible.
- More complex computers may require multiple chips due to size constraints.
- Smaller computers have shorter wires, enabling faster operation for the same power consumption.

### SoC Composition
- SoC consists of at least two or more complex micro-electronic macro components previously integrated into different single dies.
- Complex functionalities that once required heterogeneous components on a PCB are now integrated into a single silicon chip.

### Evolution of Embedded Systems
- Embedded systems have evolved from microcontrollers and discrete components to fully integrated SoCs.
- **Reason**: Advances in silicon process technology enable complete systems to be designed into one or few integrated devices.
- **Benefits**:
  - Space and power reductions.
  - Increased performance.

---

## Features of SoC

A typical SoC incorporates:
- **Programmable Processor**.
- **On-Chip Memory**.
- **Accelerated Functional Units** (e.g., Digital Encryption Standard block, MPEG2 decoder).
- **Peripheral Devices**.
- **Mixed Technology Designs** integrating:
  - Analog and RF components.
  - Micro-electro-Mechanical Systems (MEMS).
  - Optical input/output.

---

## SoC Design

- **Challenge**: Time and design effort required to integrate different types of components on a chip is a bottleneck for SoC evolution.
- **Solution**: Design reuse to reduce time-to-market.
  - Use parts from previous designs.
  - Utilize parts designed by third parties.
  - Adopt hardware and software component models.
- **Goal**: Proven and tested solutions to avoid re-design and re-verification of real-time hardware and software.

---

## IP-Based Design

- **Intellectual Property (IP) Cores**: Parameterized components with standard interfaces facilitating high-level synthesis.
- **Types of IP Cores**:
  1. **Hard Cores**:
     - Black box in optimized layout form with encrypted simulation models.
     - Example: Microprocessors.
  2. **Firm Cores**:
     - Synthesized netlists that can be simulated and modified if needed.
  3. **Soft Cores**:
     - Register Transfer Level (RTL) HDLs; user is responsible for synthesis and layout.

---

## Platforms

- **Embedded Applications**: Built using common architectural blocks and customized application-specific components.
- **Common Architectures**:
  - Processor, memory, peripherals, bus structures.
- **Platform-Based Design**: Common architectures and supporting technologies (IP libraries and tools).

---

## Platform-Based SoC

Platform-based SoCs are systems that contain:
- **IP Blocks**: Embedded CPU, embedded memory.
- **Real-World Interfaces**: PCI, USB.
- **Mixed-Signal Blocks**.
- **Software Components**: Device drivers, real-time operating systems, and application code.

---

## Classes of Platforms

### 1. Full Application Platform
- Platforms enabling derivative product designers to create complete applications on top of hardware-software architectures.
- **Example**: Complex dual-processor architecture with hierarchical bus system tailored to specific product requirements.
- **Examples**: Philips Nexperia, TI’s OMAP.

### 2. Processor-Centric Platforms
- Centered on specific processors.
- Key software services (e.g., real-time OS kernel) made available through libraries.
- **Examples**: ARM Micropack, ST Microelectronics ST100.

### 3. Communication-Centric Platform
- Communication fabric optimized for specific applications.
- Fabrics often bundled with specific processors.
- **Examples**: ARM AMBA, IBM CoreConnect bus architecture.

### 4. Configurable (Programmable) Platform
- Programmable logic added to the platform allows customization using both hardware and software.
- **Example**: FPGA added to hard-coded processor-centric platforms.
- **Examples**: Altera Excalibur platform with ARM cores, Xilinx VertexII Pro.

---

# Multi-Processor SoC (MPSoC)

- **Full Application Platform**:
  - Multiple processors (CPUs, DSPs, etc.).
  - Hardwired blocks.
  - Mixed-signal components.
  - Custom memory system.
  - Extensive software.

### Examples of MPSoC

1. **Philips Nexperia**:
   - **Applications**: Set-top boxes, multimedia.
   - **Components**: 2 CPUs, 3 buses, several accelerators, I/O devices.

2. **TI OMAP**:
   - **Applications**: Communications, multimedia.
   - **Components**: Multiprocessor with DSP and RISC.

3. **ST Nomadik**:
   - **Applications**: Mobile multimedia.
   - **Components**: Multiprocessor-of-multiprocessors.

---

## Digital Media Processor

### Functionalities in a Portable Media System
- **Live Preview**: Capture, process, display.
- **Live Video Capture**: Compresses video.
- **Live Image Capture**: Compresses images.
- **Live Audio Capture**: Compresses audio.
- **Video Decode/Playback**.
- **Still Image Decode/Display**.
- **Audio Decode/Playback**.
- **Photo Printing**.
- **Concurrent Operation**: Several modes operate simultaneously.

### DM 310 Media Processor
- **Four Subsystems**:
  1. **Imaging/Video System**:
     - CCD controller, preview engine, onscreen display, video encoder.
  2. **DSP**:
     - TMS32054X operating at 72 MHz (max.).
     - Performs bulk of audio/image/video processing.
  3. **Co-Processors**:
     - SIMD engine (8 or 16-bit).
     - Quantization and variable-length coder working concurrently.
  4. **ARM Core**:
     - Manages system-level tasks.
     - Controls all components on-chip except DSP and its co-processors.