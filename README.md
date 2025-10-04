# VSD_Workshop_Week_2

```
$ sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module
```

```
$ iverilog -o output/pre_synth_sim/pre_synth_sim.out -DPRE_SYNTH_SIM \
  -I src/include -I src/module \
  src/module/testbench.v src/module/vsdbabysoc.v
$ cd output/pre_synth_sim
$ ./pre_synth_sim.out
```
