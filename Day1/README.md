# DAY1
# Introduction to Open Source Simulator Icarus Verilog (iverilog)

My learning journey began with an introduction to the fundamentals of digital design verification. The core concepts of what a design is, how it's tested, and the tools used for simulation were covered.

  -  The Digital Design

       - It was understood that the "Design" refers to the actual Verilog code that is written to implement a specific functionality. This code is the embodiment of the required hardware specifications.

   - The Testbench

        - A "Testbench" was introduced as the framework for verifying the design. Its primary role is to apply a stimulus (a set of test vectors) to the design's inputs to check if the outputs are correct.

        - The concept was visualized as a black-box setup. A Stimulus Generator provides the primary inputs to the Design, and a Stimulus Observer checks the primary outputs to confirm the design's functional correctness.
        <img width="2958" height="1494" alt="Screenshot from 2025-09-23 17-00-55" src="https://github.com/user-attachments/assets/69622084-41b2-446e-bf6b-6a41e3f81af0" />

   - The Role of a Simulator

       - A "Simulator" was defined as the software tool used to perform the verification. The RTL design is simulated to ensure it adheres to its specification before proceeding to more complex stages.

       - The working principle of the simulator was explained. It operates on an event-driven basis, meaning it constantly looks for changes in the values of input signals.

        - An output is only re-evaluated when a change on an input signal is detected. If there are no changes to the inputs, the outputs remain unchanged.

  -  Iverilog Simulation Flow

        - A practical simulation flow using open-source tools was presented.

        - The process starts with the Design file (Verilog code) and the Test Bench file.

        - These files are passed as inputs to the ``iverilog `` compiler. This tool compiles the code and generates a simulation executable.

        - During simulation, a Value Change Dump (`` .vcd ``) file can be generated. This file logs all the signal changes that occurred during the simulation run.

        - The ``.vcd `` file is then loaded into a waveform viewer like ``gtkwave ``. This tool allows for the visual inspection of the design's signals over time, making it possible to debug and verify the behavior of the circuit.
<img width="3094" height="1494" alt="Screenshot from 2025-09-23 17-01-10" src="https://github.com/user-attachments/assets/04c3863e-2cdc-41c6-b1b5-d16c028fa9e1" />

# Labs on iverilog and gtkwave

First we need the library and the rtl files to use the tools , so using the linux terminal , enter the following commands to clone a github repository

``
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
``

<img width="3532" height="1906" alt="git_cloned_allfiles_there" src="https://github.com/user-attachments/assets/d709c841-9507-43a3-b78c-b383ec2fc75e" />

Now lets simulate a 2x1 Multiplexer using iverilog and verify its functionality using gtkwave tool. 
To invoke the iverilog tool and compile the source code file and testbench file , use the following command

``
iverilog good_mux.v tb_good_mux.v
``

Next , to run the simulation , run the following command

``
./a.out
``

<img width="3970" height="2110" alt="iverilog gtkwave_good_mux" src="https://github.com/user-attachments/assets/57d57da2-3b22-43a0-a376-68045fc432a1" />

After Simulation , to view the waveform using the gktwave tool, use the following command

``
gtkwave tb_good_mux.vcd
``

<img width="3970" height="2110" alt="gtkwave_good_mux" src="https://github.com/user-attachments/assets/bf8c0eb5-4a34-45a4-8629-7e99c96611fb" />

Now lets check the code and testbench to check its functionality and verify it with the waveform if its the same
We cam open the source and testbench files which are in ``.v`` format using the following command

``
gvim good_mux.v tb_good_mux.v
``

The following picture shows the source and testbench codes

<img width="3970" height="2110" alt="good_mux_sourcecode tb" src="https://github.com/user-attachments/assets/9013f2a6-c7d7-4380-ae36-9241825adec0" />

This Verilog code defines a 2-to-1 multiplexer and a testbench to verify its behavior.

   _ Inputs: `` i0``, `` i1`` (data inputs), `` sel`` (select line)

   _ Output: `` y`` (data output)

   _ Logic: If the `` sel`` line is high (1), the output `` y`` is driven by input `` i1``; otherwise, `` y`` is driven by `` i0``.

The functionality of the code matches with the functionality shown in the waveform

# Introduction to Yosys and Logic Synthesis

After understanding design verification, the focus shifted to logic synthesis, which is the process of converting the abstract hardware description into a physical gate-level implementation.

  -  Register Transfer Level (RTL) Design

        - RTL was understood as a behavioral representation of a digital circuit. It describes how data is transferred and transformed between registers.

        - An example of a synchronous Verilog module was shown, using an `` always @ (posedge clk)`` block, which is a common way to write synthesizable RTL code.

   - Logic Synthesis

        - "Synthesis" was defined as the process of translating the high-level RTL description into a gate-level netlist.

        - This process effectively converts the behavioral code into a structural description, detailing which logic gates (like AND, OR, etc.) are required and how they are interconnected.

        - An illustration was provided showing how Verilog code for a multiplexer (`` assign int = sel ? A : B;``) and a flip-flop (`` Q <= int;``) is mapped directly to their corresponding hardware gate symbols.

     - The Synthesizer and `` .lib`` Files

         - The "Synthesizer" is the tool that performs synthesis. For this course, `` Yosys`` was introduced as the chosen open-source synthesis tool.

         - For the synthesizer to create a netlist, it needs a library of available logic gates. This is provided in a `` .lib`` file, which is a collection of standard cells.

         - These `` .lib`` files contain basic logical modules like AND, OR, NOT, flip-flops, etc.
           <img width="3094" height="1494" alt="Screenshot from 2025-09-23 17-02-20" src="https://github.com/user-attachments/assets/bf3d009d-062b-4b03-ba7c-ce209f4bc595" />


     - Different "Flavors" of Standard Cells

         - It was explained that a `` .lib`` file doesn't just contain one version of each gate. It contains multiple "flavors."

         - This includes variations in the number of inputs (e.g., 2-input AND, 3-input AND, 4-input AND gates).

         - Crucially, it also includes different performance versions of the same gate, often categorized as slow, medium, or fast.

     - The Need for Fast Cells (Setup Time)

         - The reason for having different speed flavors was explained in the context of timing paths between two flip-flops (DFF A and DFF B).

         - The maximum operating speed of a circuit is determined by the longest combinational delay path. To ensure data is ready before the next clock edge arrives at DFF B, the following equation must be met: TCLK​>TCQ_A​+TCOMBI​+TSETUP_B​.

         - To make the circuit run at a higher frequency (i.e., have a smaller TCLK​), the combinational delay (TCOMBI​) must be reduced. This is achieved by using faster cells in that logic path.

     - Trade-offs of Faster vs. Slower Cells

         - The physical difference between fast and slow cells was clarified. The load in a digital circuit is capacitive.

         - To charge/discharge this capacitance faster (leading to lower cell delay), transistors need to source more current. This is achieved by making the transistors wider.

         - However, wider transistors result in low delay at the cost of more area and power.

         - Conversely, narrower transistors have more delay but consume less area and power. Faster cells, therefore, come with a penalty.

     - The Need for Slow Cells (Hold Time)

         - While fast cells are needed for performance, slow cells are also essential for fixing "hold" violations.

         - A hold violation occurs when data from DFF A changes too quickly and arrives at DFF B before the previous data has been properly latched. The condition to avoid this is: THOLD_B​<TCQ_A​+TCOMBI​.

         - To fix a hold violation, the combinational path delay (TCOMBI​) needs to be increased. This is where slower cells are intentionally used to add delay and ensure the signal arrives later.

      - Guiding the Synthesizer with Constraints

         - The final concept tied everything together. The synthesizer must be guided to make intelligent choices.

         - Using too many fast cells leads to a circuit with bad power and area characteristics, and can even cause hold time violations.

         - Using too many slow cells results in a sluggish circuit that cannot meet its performance targets.

         - This guidance is provided to the synthesizer through a set of rules called "Constraints" (e.g., specifying the target clock frequency). The tool then uses these constraints to select the optimal mix of cell flavors to balance performance, power, and area.

# Labs on Yosys and SKY130 PDKs

Now lets synthesize the the previously simulated 2x1 multiplexer design. 
First to invoke the yosys tool , enter the following command in the linux terminal 

``
Yosys
``
Now to read the library file,

``
read_liberty -lib /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
``

Now to read the verilog file 

``
read_verilog /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/good_mux.v
``

Now to synthesise the top module

``
synth -top good_mux
``

Now to map the technology file to the design and complete the synthesis, 

``
abc -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
``

Now , to generate a netlist file 

``
write_verilog -noattr good_mux_net.v
``

Now, to visualize the gatelevel netlist ,

``
show
``

<img width="2761" height="1103" alt="show_schematic" src="https://github.com/user-attachments/assets/cd983552-efb7-457c-9d6b-434ba7103b39" />

We can open the netlist file using gvim command and check the netlist

<img width="3172" height="1736" alt="simplified_netlist" src="https://github.com/user-attachments/assets/ec17f720-e3a9-4194-9d01-808461be87c6" />

# Summary

Learnt about simulators, designs, and testbenches, ran the  first Verilog simulation with iverilog and visualized waveforms, analyzed the 2-to-1 mux code and explored Yosys and learned why gate libraries have various flavors.
















































