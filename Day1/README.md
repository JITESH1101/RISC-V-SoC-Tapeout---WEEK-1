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

        - These files are passed as inputs to the iverilog compiler. This tool compiles the code and generates a simulation executable.

        - During simulation, a Value Change Dump (``bash .vcd ```) file can be generated. This file logs all the signal changes that occurred during the simulation run.

        - The .vcd file is then loaded into a waveform viewer like gtkwave. This tool allows for the visual inspection of the design's signals over time, making it possible to debug and verify the behavior of the circuit.
<img width="3094" height="1494" alt="Screenshot from 2025-09-23 17-01-10" src="https://github.com/user-attachments/assets/04c3863e-2cdc-41c6-b1b5-d16c028fa9e1" />

# Labs on iverilog and gtkwave
