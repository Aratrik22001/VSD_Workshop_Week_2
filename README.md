# VSD_Workshop_Week_2

In this project I have created a compact, **Open-Source System-on-Chip (SoC)** called **BabySoC**, which I have built upon the lightweight RISC-V processor core **RVMYTH**. I have designed the architecture to handle both digital computation and the integration of analog systems, making it a practical platform for education and experimentation.

The project includes essential components such as a **Phase-Locked Loop (PLL)**, which ensures stable clock signals, and a **10-bit Digital-to-Analog Converter (DAC)** that converts digital data into analog signals. With this feature, BabySoC can connect to consumer electronics like televisions and speakers, enabling audio, video, and other analog applications. This highlights my focus on demonstrating the importance of mixed-signal design in SoC development.

By implementing this design using the **Sky130 open-source technology node**, I aim to promote accessibility and reproducibility, making the project a valuable resource for learners, and researchers alike. In essence, BabySoC reflects my effort to bridge theoretical concepts with practical applications in both **RISC-V architecture** and **open-source semiconductor design**.

## Fundamentals of SoC

<details>
  <summary> THEORY </summary>
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
