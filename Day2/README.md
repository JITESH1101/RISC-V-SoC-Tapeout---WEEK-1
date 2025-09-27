# DAY2

# Introduction to Timing Libs

In this section, the fundamental concepts of timing libraries and how they account for real-world physical variations are explored.

   - PVT Corners: It is understood that the performance of a silicon chip is not constant. Its behavior is heavily influenced by three factors: Process, Voltage, and Temperature (PVT).

        - Process variations refer to the minor physical differences that occur during the chip fabrication process.

        - Voltage is the operating voltage supplied to the chip, which can fluctuate.

        - Temperature is the ambient operating temperature of the chip.

        - An analogy is used where a CD player might work perfectly in a cool climate like Switzerland (<20∘C) but fail in a hot climate like Dubai (>30∘C). In the same way, a chip's timing behavior must be validated across a range of PVT conditions, known as "corners," to ensure it functions correctly under all specified circumstances.

  - Standard Cell Characterization: The timing libraries (.lib files) contain the timing and power characteristics of basic building blocks called standard cells (like NAND, NOR, Flip-Flops, etc.).

      - It is shown how these basic logic gates are constructed using transistors (PMOS and NMOS).

       - A key design principle is highlighted: stacked PMOS transistors are generally avoided because they have poor performance (lower mobility) compared to NMOS transistors. This is why NAND gates are often preferred over NOR gates in CMOS design.

       - The characteristics of these cell designs across all the different PVT corners are captured and stored in the timing libraries, which are then used by synthesis and timing analysis tools.

Now lets explore the library file and see what it contains , but to open it we need a file editor, so lets install gvim
```
sudo apt install gvim
```

Now to open the library file sky130_fd_sc_hd__tt_025C_1v80.lib , enter the following command

```
gvim sky130_fd_sc_hd__tt_025C_1v80.lib
```
<img width="3960" height="2501" alt="Lib_file" src="https://github.com/user-attachments/assets/e6b69401-0cc4-481c-bd53-9cc07023ed03" />

<img width="1042" height="2100" alt="standard_Cells" src="https://github.com/user-attachments/assets/5e6baecd-0dc1-46f0-8486-8e46a19504e6" />


Also here is the comparision between two different standard cells in the library file

<img width="3955" height="2464" alt="cell_comparision" src="https://github.com/user-attachments/assets/2777f227-eefc-4fd6-b54f-4c0124db9928" />

# Hierarchical vs. Flat Synthesis

The different strategies for synthesizing a large System-on-Chip (SoC) design are covered, with a focus on the advantages of a structured approach.

   - Flat Synthesis: This approach involves compiling the entire design from the top-level down into a single, massive block. While simple for small designs, it becomes very slow and memory-intensive for large, complex SoCs.

   - Hierarchical Synthesis: A more efficient strategy, known as the "divide and conquer" method, is introduced.

        - The design is broken down into smaller, more manageable sub-modules. Each of these modules is synthesized independently.

        - This is particularly powerful when the design contains multiple instances of the same module. The module is synthesized only once, and the resulting netlist and timing information can be reused for all other identical instances.

        - This method significantly reduces the overall synthesis time and computational resources required, making it the preferred approach for modern, "massive" SoC designs.

   - Hierarchical synthesis maintains sub-modules in the design, while flattening produces a netlist from the ground up.

### Key Differences

| Aspect                | Hierarchical Synthesis             | Flattened Synthesis           |
|-----------------------|------------------------------------|------------------------------|
| Hierarchy             | Preserved                          | Collapsed                    |
| Optimization Scope    | Module-level only                  | Whole-design                 |
| Runtime               | Faster for large designs           | Slower for large designs     |
| Debugging             | Easier (traces to RTL)             | Harder                       |
| Output Complexity     | Modular structure                  | Single, complex netlist      |
| Use Case              | Modularity, analysis, reporting    | Maximum optimization         |

---


# Labs on Hierarhial vs Flat Synthesis

Usimg a rtl design with a main module named multiple modules and submodules as submodule1 and submodule2, we can do both hierarchial synthesis and flat synthesis

- For Hierachial Synthesis , the steps to do the synthesis is as follows 
First to invoke the yosys tool , enter the following command in the linux terminal 

```
Yosys
```
Now to read the library file,

```
read_liberty -lib /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Now to read the verilog file 

```
read_verilog /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/multiple_modules.v
```

Now to synthesise the top module

```
synth -top multiple_modules
```

<img width="2889" height="2492" alt="synth_top_multiple_modules" src="https://github.com/user-attachments/assets/75063738-8d60-4478-b6cc-7bd1930504da" />


Now to map the technology file to the design and complete the synthesis, 

```
abc -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Now , to generate a netlist file 

```
write_verilog -noattr multiple_modules_net.v
```

Now, to visualize the gatelevel netlist ,

```
show
```

Now the following is the visualization of the netlist
<img width="3982" height="899" alt="show_multiple_modules" src="https://github.com/user-attachments/assets/96f0b1c4-6e4c-45e6-bf80-b396945eb1b2" />


- For Flat Synthesis , the synthesis can be done by using the following commands

First to invoke the yosys tool , enter the following command in the linux terminal 

```
Yosys
```
Now to read the library file,

```
read_liberty -lib /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Now to read the verilog file 

```
read_verilog /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/multiple_modules.v
```

Now to synthesise the top module

```
synth -top multiple_modules
```

Now to map the technology file to the design and complete the synthesis, 

```
abc -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Now to do flat synthesis, enter the following command

```
flatten
```

Now , to generate a netlist file 

```
write_verilog -noattr multiple_modules_net.v
```

Now, to visualize the gatelevel netlist ,

```
show
```

Now the visualization of the netlist is as follows

<img width="3982" height="899" alt="show_multiple_modules_flatten" src="https://github.com/user-attachments/assets/afbd3ab3-fcc9-4a1b-815b-a6af827bd116" />


Now to compare both the netlist for better understanding,
<img width="3982" height="2397" alt="hiervsflat_netlist" src="https://github.com/user-attachments/assets/a52c2550-85cb-4932-b65d-ca8ea38cafed" />

Also after trying to synthesize a submodule from the design, here is the netlist visualization

<img width="3957" height="925" alt="show_submodule" src="https://github.com/user-attachments/assets/fe6374df-9f22-4d47-af35-3b2d429d0e75" />


# Various Flop Coding Styles and Optimization

This section delves into the importance of flip-flops in digital design, different ways their reset logic is implemented, and how synthesis tools optimize arithmetic logic.

   - The Need for Flip-Flops: It is explained why flip-flops are critical for creating reliable synchronous systems.

        - Purely combinational logic circuits can produce unwanted, temporary signal changes known as glitches or hazards. These happen because different signal paths through the logic can have different propagation delays.

        - Flip-flops are used to sample the output of combinational logic at a precise moment (the active clock edge). By the time the clock edge arrives, the glitches have usually subsided, and the logic's output has stabilized. This ensures that only the correct, stable data is passed to the next stage of the design.

   - Asynchronous vs. Synchronous Resets: Two primary styles of reset logic for flip-flops are examined.

        - Asynchronous Reset: This type of reset is independent of the clock. As seen in the timing diagrams, when the asynchronous reset signal (`` ares``) is asserted, the flip-flop's output (`` Q``) is immediately forced to its reset state (e.g., '0'), regardless of any clock activity.

        - Synchronous Reset: This reset is synchronized with the clock. The reset signal is only acted upon at the next active clock edge. It is essentially treated as another piece of synchronous input data that determines the next state of the flip-flop. This style can help prevent issues where a reset signal is removed too close to a clock edge.

   - Synthesis Optimizations: It is demonstrated that synthesis tools are highly sophisticated and can convert high-level Verilog code into optimized hardware.

        - Multiplication by Powers of Two: A simple operation like `` y = a * 2`` is not implemented as a complex multiplier. Instead, the tool recognizes this as a left bit-shift. For a 3-bit input `` a[2:0]``, the result is achieved by simply concatenating a '0' to the end, effectively shifting all bits to the left by one position to produce the 4-bit output `` y[3:0]``.

        - Multiplication by a Constant: A more complex example,`` y = a * 9``, is also shown to be optimized. The tool breaks the operation down using mathematical properties: ``a * 9`` is the same as ``a * (8 + 1)``, which equals (``a * 8``) + (``a * 1``). In hardware, this is implemented far more efficiently as a 3-bit left shift (``a << 3``) added to the original a, completely avoiding the need for a dedicated multiplier circuit.

Below are efficient coding styles for different reset/set behaviors.

- Asynchronous Reset D Flip-Flop

```
module dff_asyncres (input clk, input async_reset, input d, output reg q);
  always @ (posedge clk, posedge async_reset)
    if (async_reset)
      q <= 1'b0;
    else
      q <= d;
endmodule
```

Asynchronous reset: Overrides clock, setting q to 0 immediately.
Edge-triggered: Captures d on rising clock edge if reset is low.

- Asynchronous Set D Flip-Flop

```
module dff_async_set (input clk, input async_set, input d, output reg q);
  always @ (posedge clk, posedge async_set)
    if (async_set)
      q <= 1'b1;
    else
      q <= d;
endmodule
```
Asynchronous set: Overrides clock, setting q to 1 immediately.

- Synchronous Reset D Flip-Flop

```
module dff_syncres (input clk, input async_reset, input sync_reset, input d, output reg q);
  always @ (posedge clk)
    if (sync_reset)
      q <= 1'b0;
    else
      q <= d;
endmodule
```

Synchronous reset: Takes effect only on the clock edge.


# Labs on Flop Coding and Optimizations

To Simulate the above Flop Coding Styles, its the same process as mentioned as we did on day 1 , the following is the results for the simulation of the three flop coding styles mentioned above

- Async_Reset

  <img width="3965" height="1361" alt="asyncreset_simulation_waveform" src="https://github.com/user-attachments/assets/cdfb6e61-8ae2-4ee1-9192-275c486e644e" />

- Async_set

  <img width="3965" height="1361" alt="async_Set_gtwave_simulation" src="https://github.com/user-attachments/assets/23768c64-47e7-4ab4-81b1-78031af982f4" />

- Sync_Reset

 <img width="3965" height="1361" alt="sync_Reset_gtkwave_simulation" src="https://github.com/user-attachments/assets/fcc792d8-5aca-4b92-9e3d-e9b0bf8b62b3" />

Now to synthesise and check the netlist visualization  of the above flop coding styles, the following commands are used

First to invoke the yosys tool , enter the following command in the linux terminal 

```
Yosys
```
Now to read the library file,

```
read_liberty -lib /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Now to read the verilog file 

```
read_verilog /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/dff_asyncres.v
```

Now to synthesise the top module

```
synth -top dff_asyncres
```

Now to map the flip flops,

```
dfflibmap -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Now to map the technology file to the design and complete the synthesis, 

```
abc -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Now , to generate a netlist file 

```
write_verilog -noattr dff_asyncres_net.v
```

Now, to visualize the gatelevel netlist ,

```
show
```

Now using the above process all flop coding styles are synthesized and the results are as follows

- Asyncres

<img width="3951" height="701" alt="asyncres_synthesis_show" src="https://github.com/user-attachments/assets/fc7dcc7b-49e7-4d44-bd83-a17bf62108a3" />

- Async_set

<img width="3951" height="701" alt="async_Set_synthesis_show" src="https://github.com/user-attachments/assets/144d081a-56c8-40d6-b349-24d04e6c1804" />

- Sync_reset

<img width="3951" height="739" alt="sync_reset_synthesis_show" src="https://github.com/user-attachments/assets/549fcb28-11fb-428c-a53b-8a26aa6aa1f1" />


Now as for the optimizations mentioned above , lets visualize the multiplication with 2 and also with 8 by synthesising the codes ``mult2`` and ``mult8``
Here is the netlist and its visualization of ``mult2`` after synthesis using the steps mentioned above,

<img width="2379" height="661" alt="netlist_mult2" src="https://github.com/user-attachments/assets/72042865-6035-4c16-b9e8-6aa0fc910955" />

<img width="3940" height="1260" alt="interesting_opt_mul2_synthesis" src="https://github.com/user-attachments/assets/9bfab8b3-910b-4f69-bbec-9767a85a1b2e" />

Here is the netlist and its visualization of ``mult8`` after synthesis using the steps mentioned above,

<img width="2700" height="824" alt="mult_8_netlist" src="https://github.com/user-attachments/assets/a249ffa6-ca74-4f56-bbfd-24ffb152ab7c" />

<img width="3943" height="995" alt="mult8_show_synthesis" src="https://github.com/user-attachments/assets/eba2d301-0cee-4040-a26e-7d519616bd99" />
    

# Summary

Learned about timing libs , what each library file consists , observed standard cells , leaned about hirearchial vs flat synthesis , different flop coding styles and some interesting optimization techniques.

Note: The tools used are the latest version of this date , the synthesis or the netlist visual respresentation might vary based on the version being used.





