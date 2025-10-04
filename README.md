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
    
</details>


---

## Lab on Functional Modelling

<details>

  <summary> LAB </summary>
    
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
  
  ```
  $ sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module
  ```
  
  ```
  $ iverilog -o output/pre_synth_sim/pre_synth_sim.out -DPRE_SYNTH_SIM \
    -I src/include -I src/module \
    src/module/testbench.v src/module/vsdbabysoc.v
  $ cd output/pre_synth_sim
  $ ./pre_synth_sim.out
  $ gtkwave per_synth_sim.vcd
  ```
  
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
