# DAY4

# Gate-Level Simulation (GLS)

Gate-Level Simulation (GLS) is understood to be a critical verification step in the design flow. In this process, the synthesized gate-level netlist is simulated, rather than the original RTL code. 
The primary goal is to confirm that the design's functionality and timing are correct after the code has been translated into actual logic gates by the synthesis tool.

   - Why GLS is Performed: The main reason for running GLS is the validation of the synthesis process itself. This serves as a crucial check to ensure:

        - Functional Correctness: The logic synthesized by the tool is confirmed to behave exactly as the original RTL intended.

        - Timing Verification: The design is checked to see if it meets its timing requirements (e.g., setup and hold times). This requires a timing-annotated GLS, where real-world delay information is used in the simulation.

        - Testability: Features added for testing, like scan chains, are verified to be correctly implemented and functional.

   - The GLS Process: The simulation flow is explained as follows: The simulator (like Iverilog) is provided with three main inputs: the gate-level netlist, the corresponding gate-level Verilog models for the standard cells, and the same testbench that is used for RTL verification. The output is typically a waveform file (VCD) which can then be analyzed to verify the results.
