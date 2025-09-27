# DAY4

# Gate-Level Simulation (GLS)

Gate-Level Simulation (GLS) is understood to be a critical verification step in the design flow. In this process, the synthesized gate-level netlist is simulated, rather than the original RTL code. 
The primary goal is to confirm that the design's functionality and timing are correct after the code has been translated into actual logic gates by the synthesis tool.

   - Why GLS is Performed: The main reason for running GLS is the validation of the synthesis process itself. This serves as a crucial check to ensure:

        - Functional Correctness: The logic synthesized by the tool is confirmed to behave exactly as the original RTL intended.

        - Timing Verification: The design is checked to see if it meets its timing requirements (e.g., setup and hold times). This requires a timing-annotated GLS, where real-world delay information is used in the simulation.

        - Testability: Features added for testing, like scan chains, are verified to be correctly implemented and functional.

   - The GLS Process: The simulation flow is explained as follows: The simulator (like Iverilog) is provided with three main inputs: the gate-level netlist, the corresponding gate-level Verilog models for the standard cells, and the same testbench that is used for RTL verification. The output is typically a waveform file (VCD) which can then be analyzed to verify the results.


# Synthesis-Simulation Mismatch

A synthesis-simulation mismatch is defined as a critical issue where the behavior observed during RTL simulation does not match the behavior seen in the post-synthesis gate-level simulation. This indicates that the synthesis tool's interpretation of the Verilog code is different from the simulator's, which can lead to incorrect hardware.

It is explained that these mismatches often arise from ambiguous or improper coding practices. The most common causes are identified as:

   - Missing Sensitivity List: This is shown to be a frequent source of mismatches. For combinational logic, if an ``always`` block's sensitivity list is incomplete (e.g., ``always @(sel)`` for a MUX), the simulator only updates the output when ``sel`` changes. The synthesis tool, however, correctly infers a MUX that is sensitive to changes in all its inputs (``sel``, ``i0``, ``i1``). The use of ``always @(*)`` is identified as the correct practice to prevent this.

   - Blocking vs. Non-Blocking Assignments: The improper use of these assignments is another major cause of mismatches, as detailed in the next section.

   - Non-Standard Verilog: The use of non-synthesizable constructs like ``#delays`` or ``initial`` blocks is known to cause issues, as they may simulate correctly but are ignored or misinterpreted by synthesis tools.

# Blocking vs. Non-Blocking Assignments

The two types of procedural assignments in Verilog are explained, along with the pitfalls of their incorrect usage.

   - Blocking Assignments (``=``):
   
       - These statements are executed sequentially, in the exact order they appear in the code. One assignment is fully completed before the next one begins.

       - It is advised that blocking assignments be used for modeling combinational logic inside an ``always @(*)`` block.

         ```
         always @(*) y = a & b;
         ```

   - Non-Blocking Assignments (``<=``):

        - These statements are scheduled to occur concurrently. The right-hand side of all assignments within the block is evaluated first, and then all the left-hand side variables are updated at the end of the time step. This behavior correctly mimics how parallel hardware works.

        - It is emphasized that non-blocking assignments must be used for modeling sequential logic (e.g., flip-flops) inside an ``always @(posedge clk)`` block.

          ```
          always @(posedge clk) q <= d;
          ```

# Caveats and Mismatches

The misuse of these assignments is a primary source of synthesis-simulation mismatches.

   - Mismatch in Combinational Logic: It is demonstrated that the order of blocking statements within an ``always @(*)`` block is critical for simulation but can be ambiguous for synthesis. One line might use the "old" value of a variable while the next line updates it, leading to simulation results that do not match the parallel nature of the synthesized hardware.

   - Mismatch in Sequential Logic: A critical error is highlighted: the use of blocking assignments (``=``) to model sequential logic. In simulation, this creates a race condition where the output of one flip-flop is updated and immediately passed to the next in the very same clock cycle. The synthesis tool, however, correctly infers a series of flip-flops that pass data from one cycle to the next. This results in a severe functional mismatch between what is simulated and the actual hardware that gets built.

# Labs on GLS and Synthesis Simulation mismatch

1) First lets do do simulation for the following code 

```
module ternary_operator_mux (input i0, input i1, input sel, output y);
  assign y = sel ? i1 : i0;
endmodule
```

its functions as , if select input is 1 then the output will be i1 , else i0

Now the simulation waveform is as follows

<img width="3938" height="1275" alt="ternary_operation_mux_Gtkwave" src="https://github.com/user-attachments/assets/538c0b65-ef53-4f5b-b884-afbe9505737c" />

Now that we have the simulation waveform for the code , lets synthesise the code,

The following is the visualization of the netlist of the above code after synthesis

<img width="3959" height="1346" alt="ternary_operator_mux_synthesis_show" src="https://github.com/user-attachments/assets/66d2b7eb-471a-4f0c-b6c1-045cc5e94454" />

after writing netlist file in ``ternary_operator_mux_net.v`` file, now do the gate level synthesis we need to again invoke the iverilog tool

To do GLS, enter the following command after exiting Yosys tool ,

```
iverilog ../my_files/verilog_model/primitives.v  ../my_files/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
```

After dumping into vcd , invoke gtkwave tool and the waveform obtained is as follows

<img width="3959" height="1477" alt="ternary_operator_mux_gls_gtkwave" src="https://github.com/user-attachments/assets/b6404a05-986d-4e59-bc76-4738c2acbf59" />

By comparing the waveform, of simulation and after GLS waveform , we see that both are same , so there is no Synthesis Simulation Mismatch


2) Now lets try the same with another code

The following code is bad_mux to demonstrate the Synthesis Simulation Mismatch

```
module bad_mux (input i0, input i1, input sel, output reg y);
  always @ (sel) begin
    if (sel)
      y <= i1;
    else 
      y <= i0;
  end
endmodule
```

Now the simulation waveform is as follows
<img width="3959" height="1477" alt="bad_mux_Gtkwave" src="https://github.com/user-attachments/assets/d95094cd-931c-4290-ad60-d615d59f4bdd" />

After doing synthesis, the visualization of netlist is as follows

<img width="3959" height="1301" alt="bad_mux_synthesis_show" src="https://github.com/user-attachments/assets/85d4abe0-cadb-40d0-83ac-897cc70284ca" />

Now by doing GLS as shown above , we see the following waveform

<img width="3188" height="726" alt="bad_mux_gls_gtkwave" src="https://github.com/user-attachments/assets/583c3a94-e32f-4de6-9ce3-9e83cdf8d04f" />

Now by comparing the waveforms after simulation and GLS , we see some differences,

- When ``sel`` input is low then the output ``y`` should reflect the activity of ``i0`` but it isnt doing so.

This difference in the waveform is synthesis simulation Mismatch

To fix this issue , we can change the ``always`` in the following way

```
always @ (*) begin
  if (sel)
    y = i1;
  else
    y = i0;
end
```


# Lab on Synthesis Simulation Mismatch on Blocking Statement

For this case , the following is the given verilog code 

```
module blocking_caveat (input a, input b, input c, output reg d);
  reg x;
  always @ (*) begin
    d = x & c;
    x = a | b;
  end
endmodule
```

Now by doing simulation , we get the following waveform

<img width="3983" height="1272" alt="blocking_Caveat_gtkwave" src="https://github.com/user-attachments/assets/251acd74-0df8-4feb-ab9e-ad6c315ae85d" />

Now  after doing synthesis , we get the following representation of netlist

<img width="3951" height="1320" alt="blocking_caveat_synthesis_show" src="https://github.com/user-attachments/assets/4bf252c5-6659-4d36-956a-0e6cd7d47d69" />

After this , by doing GLS as we did before , we get the following waveform

<img width="3944" height="1280" alt="blocking_caveat_gls_gtkwave" src="https://github.com/user-attachments/assets/1cf127f1-cc9d-4642-878a-2db5459044a0" />

By comparing the two waveforms , we see that the value of d being shown is not the present value but its the previous value , which is wrong.

To fix this issue , we change the always block as follows

```
always @ (*) begin
  x = a | b;
  d = x & c;
end
```

# Summary

Learned Gate Level Synthesis , Synthesis Simulation Mismatch , Blocking and Non blocking statements

Note: The tools used are the latest version of this date , the synthesis or the netlist visual respresentation might vary based on the version being used.






































