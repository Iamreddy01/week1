# Day 2: Timing Libraries, Synthesis Approaches, and Efficient Flip-Flop Coding
## * Welcome to Day 2 of the RTL Workshop. This day covers three crucial topics:

### * Understanding the .lib timing library (sky130_fd_sc_hd__tt_025C_1v80.lib) used in open-source PDKs.
### * Comparing hierarchical vs. flat synthesis methods.
### * Exploring efficient coding styles for flip-flops in RTL design.

## Timing Libraries
#### SKY130 PDK Overview
The SKY130 PDK is an open-source Process Design Kit based on SkyWater Technology's 130nm CMOS technology. It provides essential models and libraries for integrated circuit (IC) design, including timing, power, and process variation information.

### Decoding tt_025C_1v80 in the SKY130 PDK
* tt: Typical process corner.
* 025C: Represents a temperature of 25°C, relevant for temperature-dependent performance.
* 1v80: Indicates a core voltage of 1.8V.
This naming convention clarifies which process, voltage, and temperature conditions the library models.

## Opening and Exploring the .lib File
### To open the sky130_fd_sc_hd__tt_025C_1v80.lib file:
1. Install a text editor : Here i installad the gvim editor
   ```bash
   sudo apt update
   sudo apt install vim-gtk3
   ```
2. open the file :
   ```bash
   gvim sky130_fd_sc_hd__tt_025C_1v80.lib
   ```
   # image

## Hierarchical vs. Flattened Synthesis
### Hierarchical Synthesis
### Definition: 
Retains the module hierarchy as defined in RTL, synthesizing modules separately.
* How it Works: Tools like Yosys process each module independently, using commands such as hierarchy to analyze and set up the design structure.
### Advantages:

* Faster synthesis time for large designs.
* Improved debugging and analysis due to maintained module boundaries.
* Modular approach, aiding integration with other tools.
### Disadvantages:

* Cross-module optimizations are limited.
* Reporting can require additional configuration.

  # image

## Flattened Synthesis
### Definition: Merges all modules into a single flat netlist, eliminating hierarchy.
### How it Works: The flatten command in Yosys collapses the hierarchy, allowing whole-design optimizations.
### Advantages:

* Enables aggressive, cross-module optimizations.
* Results in a unified netlist, sometimes simplifying downstream processes.
### Disadvantages:

* Longer runtime for large designs.
* Loss of hierarchy complicates debugging and reporting.
* Can increase memory usage and netlist complexity.
# example image

## Flip-Flop Coding Styles
#### * Flip-flops are fundamental sequential elements in digital design, used to store binary data. Below are efficient coding styles for different reset/set behaviors.

* Asynchronous Reset D Flip-Flop
```bash
module dff_asyncres (input clk, input async_reset, input d, output reg q);
  always @ (posedge clk, posedge async_reset)
    if (async_reset)
      q <= 1'b0;
    else
      q <= d;
endmodule
```
#### Asynchronous reset: Overrides clock, setting q to 0 immediately.
#### Edge-triggered: Captures d on rising clock edge if reset is low.
* Asynchronous Set D Flip-Flop
```bash
module dff_async_set (input clk, input async_set, input d, output reg q);
  always @ (posedge clk, posedge async_set)
    if (async_set)
      q <= 1'b1;
    else
      q <= d;
endmodule
```
#### Asynchronous set: Overrides clock, setting q to 1 immediately.
* Synchronous Reset D Flip-Flop
```bash
module dff_syncres (input clk, input async_reset, input sync_reset, input d, output reg q);
  always @ (posedge clk)
    if (sync_reset)
      q <= 1'b0;
    else
      q <= d;
endmodule
```
#### Synchronous reset: Takes effect only on the clock edge.


