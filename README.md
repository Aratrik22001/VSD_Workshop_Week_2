# VSD_Workshop_Week_2

In this project I have created a compact, **Open-Source System-on-Chip (SoC)** called **BabySoC**, which I have built upon the lightweight RISC-V processor core **RVMYTH**. I have designed the architecture to handle both digital computation and the integration of analog systems, making it a practical platform for education and experimentation.

The project includes essential components such as a **Phase-Locked Loop (PLL)**, which ensures stable clock signals, and a **10-bit Digital-to-Analog Converter (DAC)** that converts digital data into analog signals. With this feature, BabySoC can connect to consumer electronics like televisions and speakers, enabling audio, video, and other analog applications. This highlights my focus on demonstrating the importance of mixed-signal design in SoC development.

By implementing this design using the **Sky130 open-source technology node**, I aim to promote accessibility and reproducibility, making the project a valuable resource for learners, and researchers alike. In essence, BabySoC reflects my effort to bridge theoretical concepts with practical applications in both **RISC-V architecture** and **open-source semiconductor design**.

## Fundamentals of SoC

<details>
  <summary> THEORY </summary>

  <details>
    
  <summary> 1. What is a System-on-Chip (SoC)  </summary> 

  
  A System-on-Chip (SoC) is an integrated circuit that consolidates all the essential components of a computing system into a single chip. Instead of having       separate ICs for processor, memory, and I/O, an SoC brings them together to form a compact, power-efficient, and high-performance solution.  
  SoCs are ubiquitous in modern electronics, powering devices ranging from smartphones, tablets, and wearables to automotive systems and high-performance          computing solutions.
  
  #### Key characteristics of an SoC include:
  
  - Integration: Combines CPU, memory, communication interfaces, and specialized accelerators.
  - Optimization: Designed for specific applications, balancing performance, area, and power.
  - Scalability: Applicable in both general-purpose and application-specific contexts.
  
  </details>

  <details>
    
  <summary> 2. Components of a Typical SoC </summary>    
  
  A typical SoC consists of the following key blocks:
  
  - #### <ins> CPU (Processor Core) </ins>
  
  The brain of the SoC responsible for executing instructions.
  Often based on architectures like RISC-V, ARM, or x86.
  Can be single-core or multi-core, depending on performance needs.
  
  - #### <ins> Memory Subsystem </ins>
  
    - On-chip memory: SRAM, ROM, cache for high-speed access.
    - External memory interfaces: DDR, LPDDR, Flash for larger storage.
    - Critical for providing low-latency and high-bandwidth access to the CPU and peripherals.
  
  - #### <ins> Peripherals </ins>
  
    - Interfaces such as UART, SPI, I²C, GPIO, and timers.
    - Enable communication with sensors, displays, and other hardware modules.
    - Specialized accelerators (e.g., AI/ML units, DSPs, GPU) may also be part of the peripheral set.
    
  - #### <ins> Interconnect (Bus/Network-on-Chip) </ins>
    
    - Connects CPU, memory, and peripherals.
    - Can be bus-based (e.g., AMBA AXI/AHB) or network-on-chip (NoC) for complex designs.
    - Ensures efficient data transfer and arbitration across components.

  </details>

  <details>
    
  <summary> 3. Why BabySoC is a Simplified Model for Learning SoC Concepts </summary>
  
  Designing a production-grade SoC involves massive complexity. BabySoC is a deliberately simplified model created for educational purposes. It abstracts away     advanced details while preserving the essence of SoC architecture, making it easier for learners to grasp fundamental concepts.
  
  #### <ins>Advantages of BabySoC</ins>
  
  - Clarity in fundamentals: Focuses on core SoC blocks without overwhelming complexity.
  - Stepwise learning: Helps beginners understand the flow from CPU to memory and peripherals.
  - Hands-on approach: Enables early exposure to SoC design methodology, simulation, and functional testing.
  - Foundation for scalability: Once learners understand BabySoC, they can transition to real-world SoCs.

  </details>

  <details>
  <summary> 4. The Role of Functional Modelling Before RTL and Physical Design </summary>
  
  Before diving into RTL (Register Transfer Level) coding and physical implementation, functional modelling is a crucial step in SoC design.
  
  #### 1. <ins> Concept Validation </ins>
  
  - Functional models capture the intended behavior of the SoC at a high level.
  - They allow architects to validate algorithms, data flow, and interactions without worrying about implementation details.
  
  #### 2. <ins> Early Debugging </ins>
  
  - Identifies logical errors and design bottlenecks before expensive RTL synthesis and backend stages.
  - Saves time and effort by catching architectural issues early.
    
  #### 3. <ins> Performance Estimation </ins>
  
  - Helps estimate latency, throughput, and power implications.
  - Provides insights into trade-offs between hardware complexity and system efficiency.
    
  #### 4. <ins> Smooth Transition to RTL </ins>
  
  - Functional models act as golden references for RTL designers.
  - Ensures correctness and consistency when moving toward gate-level and physical design stages.
  
  </details>

  ## BabySoC Components

The **VSDBabySoC** is designed as a compact and educational System-on-Chip (SoC) that integrates three essential building blocks: the **RVMYTH RISC-V CPU**, a **Phase-Locked Loop (PLL)** for clock generation, and a **10-bit Digital-to-Analog Converter (DAC)** for analog interfacing. Together, these components demonstrate how digital computation, precise timing, and mixed-signal integration converge within a real-world SoC.

<img width="993" src="https://github.com/Aratrik22001/VSD_Workshop_Week_2/blob/main/Images/BabySoc.jpg">

### 1. <ins> RVMYTH (RISC-V CPU)</ins>

The **RVMYTH processor** is the core computational unit of BabySoC. Based on the open-source **RISC-V instruction set architecture (ISA)**, RVMYTH is designed as a lightweight yet fully functional CPU, making it ideal for educational purposes.

* **Instruction Set**: Implements the base RV32I ISA, which includes essential instructions for arithmetic, logic, memory access, and control flow.
* **Register File**: Contains 32 general-purpose registers, including the **r17 register**, which plays a role in interfacing with the DAC in this design.
* **Pipeline**: Features a simple, single-cycle or multi-cycle pipeline (depending on the implementation), balancing simplicity with clarity for learners.
* **Flexibility**: As an open-source CPU, RVMYTH can be customized, extended, and synthesized on open-source tools, making it an excellent platform for experimenting with CPU architecture and SoC integration.

In BabySoC, the RVMYTH processor is responsible for executing instructions, generating digital data streams, and updating values for conversion by the DAC, effectively acting as the "brain" of the system.


### 2. <ins>Phase-Locked Loop (PLL)</ins>

The **PLL** is a critical subsystem that ensures the BabySoC operates with a stable and synchronized clock signal. In VSDBabySoC, an **8× PLL multiplier** is used to scale a reference clock input to the desired operating frequency for the CPU and peripherals.

* **Components**:

  * *Phase Detector*: Compares the input reference clock with the feedback clock.
  * *Loop Filter*: Smooths the phase error signal to control frequency adjustments.
  * *Voltage-Controlled Oscillator (VCO)*: Generates the output clock frequency, adjusted based on the loop filter’s signal.
  * *Frequency Divider (optional)*: Used in the feedback path for frequency scaling.

* **Functionality**:

  * Locks the output frequency to the input reference, maintaining a stable phase relationship.
  * Provides a synchronized clock across CPU and DAC, ensuring timing integrity.
  * Minimizes clock jitter, delays, and mismatches that could otherwise compromise system performance.

<img width="993" src="https://github.com/Aratrik22001/VSD_Workshop_Week_2/blob/main/Images/PLL.jpg">

* **Why It’s Needed On-Chip**:

  * Off-chip clocks often suffer from distribution delays, jitter, or frequency mismatches.
  * Different SoC blocks may need different clock frequencies, which can be generated internally by the PLL.
  * Crystal oscillator deviations (tolerance, stability, aging) can affect precision; a PLL mitigates these issues by stabilizing the clock.

In short, the PLL is the "heartbeat" of BabySoC, providing precise and reliable timing for digital and mixed-signal operations.


### 3. <ins>Digital-to-Analog Converter (DAC)</ins>

The **10-bit DAC** in BabySoC bridges the gap between the digital domain of the RVMYTH processor and the analog world of real devices.

* **Structure**:

  * Accepts a 10-bit binary input (ranging from 0 to 1023).
  * Produces an analog voltage output proportional to the input digital value.
* **Types of DAC Architectures**:

  * *Weighted Resistor DAC*: Uses resistors of varying weights to produce analog outputs.
  <p align="center">
    <img src="https://github.com/Aratrik22001/VSD_Workshop_Week_2/blob/main/Images/DAC_weighted.jpg">
  </p> 

  
  * *R-2R Ladder DAC*: A scalable, resistor-based design, simpler to implement with consistent accuracy.
  <p align="center">
    <img width="850" src="https://github.com/Aratrik22001/VSD_Workshop_Week_2/blob/main/Images/R-2R_DAC.jpg">
  </p>
  
* **Function in BabySoC**:

  * Receives processed values from RVMYTH’s registers.
  * Converts these values into analog signals saved in an output file (`OUT`) or driven to external devices.
  * Enables interfacing with consumer electronics like TVs, speakers, or mobile phones, demonstrating how digital data streams can generate real-world multimedia outputs.

The DAC, therefore, acts as the "voice" of BabySoC, translating raw binary values into meaningful analog waveforms that can be interpreted by external systems.

---

### Summary

* **RVMYTH (CPU):** The computational engine, executing instructions and preparing data.
* **PLL:** The clock manager, ensuring synchronized, stable operation.
* **DAC:** The interface to the analog world, converting digital outputs into real signals.

Together, these components embody the essence of a modern SoC—**digital processing tightly coupled with precise timing and analog connectivity**—all implemented on the **Sky130 open-source technology node** to promote accessibility and hands-on learning.
   
</details>

---

## Lab on Functional Modelling

<details>

  <summary> LAB </summary>

  ### 1. <ins>Cloning</ins>
  
  Clone the top-level module, that integrates the rvmyth, pll, and dac modules, using this link.
  [VSDBabySoC](https://github.com/manili/VSDBabySoC.git)

  The Directory Structure of the Project is as follows:    
  ```txt
  VSDBabySoC/
  ├── src/
  │   ├── include/
  │   │   ├── sandpiper.vh
  │   │   └── other header files...
  │   ├── gls_model/
  │   │   ├── primitives.v
  │   │   └── sky130_fd_sc_hd.v
  │   ├── module/
  │   │   ├── vsdbabysoc.v      # Top-level module integrating all components
  │   │   ├── rvmyth.v          # RISC-V core module
  │   │   ├── avsdpll.v         # PLL module 
  │   │   ├── avsddac.v         # DAC module
  │   │   └── testbench.v       # Testbench for simulation
  │   ├── script/
  │   │   └── yosys.ys          # For Synthesis
  └── output/
  │   ├── synth
  │   ├── pre_synth_sim
  │   └── post_synth_sim
  ```
  ### 2. <ins>Convertion from .tlv to .v</ins>
  In the given files we have rvmyth.tlv present, but the verilog version of that file is required. Hence conversion is done in the        following way. 
  ```
  $ sudo apt install pipx -y
  $ pipx install sandpiper-saas
  $ sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module
  ```
  <p align="center">
    <img src="https://github.com/Aratrik22001/VSD_Workshop_Week_2/blob/main/Images/rvmyth.jpg">
  </p> 
  Now we have all the necessary files required for simulation.

  ### 3. <ins>Pre-Synthesis Simulation</ins>
  ```
  $ iverilog -o output/pre_synth_sim/pre_synth_sim.out -DPRE_SYNTH_SIM \
    -I src/include -I src/module \
    src/module/testbench.v src/module/vsdbabysoc.v
  $ cd output/pre_synth_sim
  $ ./pre_synth_sim.out
  $ gtkwave per_synth_sim.vcd
  ```
  #### Results and Waveform
  <p align="center">
    <img src="https://github.com/Aratrik22001/VSD_Workshop_Week_2/blob/main/Images/pre_synth.jpg">
  </p> 
  
  - **DPRE_SYNTH_SIM**: Defines the PRE_SYNTH_SIM macro for conditional compilation within the testbench environment. This allows for simulation-specific behavior to be enabled or disabled without affecting the synthesizable RTL.
  
  - **clk, reset**: Global clock and reset signals distributed across the SoC. These signals ensure synchronous operation and deterministic initialization of all digital logic.
  
  - **RV_TO_DAC**: A 10-bit digital output bus generated by the RVMyth core. This signal represents the processed digital data intended for digital-to-analog conversion.
  
  - **OUT**: The analog output of the DAC. It provides a continuous-time, analog representation of the RV_TO_DAC digital signal, enabling direct interfacing with mixed-signal or analog subsystems.
  
  ```
  $ yosys .src/script/yosys.ys
  ```
  
  ```
  $ iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 -o output/post_synth_sim/post_synth_sim.out -DPOST_SYNTH_SIM \
    -I src/include -I src/module -I output/synth -I src/gls_module \
    src/module/testbench.v
  $ cd output/post_synth_sim
  $ ./post_synth_sim.out
  $ gtkwave post_synth_sim.vcd
  ```
</details>
