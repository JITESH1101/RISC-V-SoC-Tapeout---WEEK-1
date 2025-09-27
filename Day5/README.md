# DAY5

# IF CASE Constructs

The fundamental ways to describe conditional logic in Verilog were explored. It was understood how ``if`` and ``case`` statements are synthesized into hardware and what common pitfalls need to be avoided.

  - ``if-else-if`` for Priority Logic

       - An ``if-else-if`` structure is synthesized into a priority encoder. The conditions are checked in sequence, creating a chain of logic.

       - The hardware implementation resembles a cascade of multiplexers, where the condition of the first ``if`` has the highest priority. This is a key behavioral insight to remember during design.

   - The Danger of "Inferred Latches"

        - A major caution with if statements in combinational logic (e.g., inside an ``always @(*)`` block) was highlighted. If all possible conditions are not covered by an ``if`` or ``else`` branch, the synthesizer must assume the output should hold its previous value for the unspecified conditions.

        - To achieve this "memory" behavior, a latch is inferred. Latches are generally undesirable in synchronous designs because they can be transparent and lead to timing issues. This is considered a bad coding style. 

   - How ``case`` Statements are Used

        - ``case`` statements are generally preferred for comparing one expression against many possible constant values.

        - They are typically synthesized into a single, large multiplexer, which represents parallel logic rather than the chained priority logic of an ``if-else-if`` structure. This can often lead to more optimal and faster hardware.

   - Common Pitfalls with ``case`` Statements

        - Similar to ``if`` statements, an incomplete ``case`` statement (where not all possible values of the select signal are covered) will also infer latches. The standard practice to prevent this is to include a ``default`` statement.

        - Another issue, partial assignment, was noted. If an output variable is not assigned a value in every single branch of the case statement, a latch will be Dinferred for that variable to hold its value in the undefined branches. The rule of thumb is to ensure all outputs are assigned in all segments of the ``case``, including the ``default`` block.
    

# for Loops and for-generate

The distinction between the two types of ``for`` loops in Verilog was made clear. They serve very different purposes and are not interchangeable.

  - The Core Difference

       - A regular ``for`` loop is used inside a procedural block (like ``always``). It's a behavioral construct used for evaluation and modeling, essentially "unrolling" repetitive operations to describe complex combinational logic. It does not create multiple copies of hardware.

       - A ``for-generate`` loop is used outside procedural blocks. Its sole purpose is structural: to replicate or instantiate hardware modules and logic multiple times. This is perfect for creating regular structures like arrays of gates, adders, or memory cells.

   - Using for Loops ``for`` Wide Logic

        - It was shown how a ``for`` loop is incredibly useful for describing wide multiplexers or demultiplexers.

        - Instead of writing a massive ``case`` statement with dozens or hundreds of entries, a ``for`` loop can describe the same behavior in just a few lines of code. The loop iterates to find which input line matches the select signal and assigns it to the output. This is a powerful modeling technique.

   - Using ``for-generate`` to Replicate Hardware

        - The concept of ``for-generate`` was demonstrated by building a Ripple Carry Adder (RCA).

        - An RCA is built by chaining together multiple Full Adder (FA) modules. The ``carry-out`` of one FA becomes the ``carry-in`` of the next.

        - A ``for-generate`` loop was used to create multiple instances of a single ``full_adder`` module, automatically handling the wiring between each instance. This is a clean, scalable, and efficient way to describe such repetitive hardware structures.

# Labs on Incomplete If case

For the Incomplete If case the following are the codes provided


<img width="3967" height="2443" alt="all_incomp_codes" src="https://github.com/user-attachments/assets/8c8bb32c-b95f-452e-aa9f-af67d8833300" />

Now lets see the simulation waveform and synthesis netlist visualization for the following

- incomp_if


<img width="3967" height="1359" alt="incomplete_if_gtkwave" src="https://github.com/user-attachments/assets/d878b0cd-5b37-40ad-9c18-054653315fce" />


<img width="3967" height="1905" alt="incomp_if_synthesis_show" src="https://github.com/user-attachments/assets/093198e2-fc44-423d-86a1-fbdfb2bac7e3" />

- incomp_if2


<img width="3967" height="1299" alt="incomp_if2_gtkwave" src="https://github.com/user-attachments/assets/c2d046ca-6386-4b19-bfcd-f885aca01ad4" />


<img width="3967" height="1021" alt="incomp_if2_Synthesis_show" src="https://github.com/user-attachments/assets/76ecb74d-a48f-47d5-86dc-d9cc04216033" />

# Labs on Incomplete Overlapping Case 

Now , for Incomplete ``Case`` situation, the following are the provided codes

<img width="3973" height="2316" alt="all_case_codes" src="https://github.com/user-attachments/assets/758b7571-4ea3-4e8b-a617-e7bf60c93383" />

Now lets see the simulation waveform and the synthesis netlist visualization for the following

- comp_case


<img width="3961" height="1394" alt="comp_case_gtkwave" src="https://github.com/user-attachments/assets/c06e9a34-295d-434a-9af7-abfdf77dc5ae" />


<img width="3961" height="771" alt="comp_case_synthesis_show" src="https://github.com/user-attachments/assets/3b44f5dc-15de-4a8b-a1f3-1ad7fc0d3d43" />

- incomp_case

<img width="3983" height="1862" alt="incomp_case_gtkwave" src="https://github.com/user-attachments/assets/06ec3381-9336-4b23-a155-85a927453fb5" />


<img width="3961" height="776" alt="incomp_Case_synthesis_show" src="https://github.com/user-attachments/assets/09e5dd42-b991-47bb-99e1-b2c567bab5c7" />

- partial_case_assign

<img width="3961" height="1262" alt="partial_Case_Assign_synthesis_Show" src="https://github.com/user-attachments/assets/3eec8e42-ae5a-473b-96d3-61aa32e5c7ad" />

- bad_case


<img width="3961" height="1442" alt="bad_case_gtkwave" src="https://github.com/user-attachments/assets/89338e9f-4c63-4400-bec5-467b7f29268f" />


<img width="3961" height="1936" alt="bad_case_synthesis_show" src="https://github.com/user-attachments/assets/c9a2c925-77e9-42c9-b3bd-2ab16e1ff22a" />

Now after doing GLS, we get the following waveform

<img width="3961" height="1463" alt="bad_Case_gls_gtkwave'" src="https://github.com/user-attachments/assets/447fd589-76ed-4a8c-b870-04dc08b71082" />

Having overlapping cases shows the bad way of coding

# Labs on For loop and For generate

Now , to demonstrate ``for loop`` and ``for generate``, the following are the simulation and synthesis netlist visulatizations

- mux_generate


<img width="3974" height="1614" alt="mux_generate_gtkwave" src="https://github.com/user-attachments/assets/4bbf8f6c-0274-4830-8168-badaa7d75bf5" />


<img width="3974" height="2109" alt="mux_generate_synthesis_show" src="https://github.com/user-attachments/assets/a4e8f93e-88ba-498f-be13-90fa2eafa548" />

- demux_generate

<img width="3974" height="1748" alt="demux_generate_gtkwave" src="https://github.com/user-attachments/assets/786c16a3-15dd-4aae-98d3-26692b45968d" />


<img width="2471" height="2343" alt="dmux_Generate_synthesis_show" src="https://github.com/user-attachments/assets/29856bd2-2771-41d3-a1e6-9b23c55b73e1" />

- demux_Case


<img width="3974" height="1748" alt="demux_case_gtkwave" src="https://github.com/user-attachments/assets/87d310f7-171a-41c3-afd4-1489e9e88ae3" />


<img width="2348" height="2343" alt="demux_Case_synthesis_show" src="https://github.com/user-attachments/assets/f119f4eb-2d22-4514-be16-102731b81295" />

# Ripple Carry Adder

The following is the code for the Ripple carry adder


<img width="3973" height="1042" alt="rca_Code" src="https://github.com/user-attachments/assets/51bb1874-3212-48c4-acd2-54954d906455" />

Now , after doing Simulation we get

<img width="3950" height="1515" alt="rca_gtkwave" src="https://github.com/user-attachments/assets/858b1ac1-f493-4805-8049-0d53d6d7d51c" />

After doing synthesis, Netlist visualization is as follows

<img width="2701" height="2332" alt="rca_synthesis_show" src="https://github.com/user-attachments/assets/798abff2-0fb8-4445-8b1e-8cae710d80db" />

Now by doing GLS, we get the following waveform

<img width="3946" height="1301" alt="rca_gls_gtkwave" src="https://github.com/user-attachments/assets/af377755-1b24-434a-89d5-afed79b52fce" />

By comparing both the waveforms for the rippe carry adder we see that it is working correctly.

# Summary

Learned IF CASE constructs , for loops , for generate and performed lab exercises on them and verified a Ripple Carry adder at the end


Note: The tools used are the latest version of this date , the synthesis or the netlist visual respresentation might vary based on the version being used.
































