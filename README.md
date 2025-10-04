# VSD_Workshop_Week_2

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

```bash
$ sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module
```

```bash
$ iverilog -o output/pre_synth_sim/pre_synth_sim.out -DPRE_SYNTH_SIM \
  -I src/include -I src/module \
  src/module/testbench.v src/module/vsdbabysoc.v
$ cd output/pre_synth_sim
$ ./pre_synth_sim.out
$ gtkwave per_synth_sim.vcd
```

```bash
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
