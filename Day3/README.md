# DAY3

# Introduction to optimizations

- The fundamental goal of logic optimization during the synthesis stage was introduced. 
- It was understood that this process is crucial for refining a digital design to meet specific Power, Performance, and Area (PPA) targets. 
- The synthesis tools employ a variety of automated techniques to simplify logic, enhance speed, and reduce resource usage. 
- These techniques can be broadly categorized into combinational and sequential optimizations, each with unique strategies.

# Combinational Optimization Techniques

These optimizations focus on logic circuits that do not contain memory elements. One of the most direct and effective methods is Constant Propagation.

  - Constant Propagation: It was learned that this compiler optimization technique identifies variables that have a fixed, constant value.

       - How it works: The synthesis tool analyzes the design code and replaces these variables directly with their constant values (e.g., '1' or '0'). This substitution allows for further simplification, often removing entire sections of logic.

       - Benefits: This process leads to reduced logical complexity, improved performance through faster execution, and optimized resource usage by requiring fewer gates.

# Sequential Optimization Techniques

This category includes more advanced techniques that restructure circuits containing memory elements like flip-flops and finite state machines (FSMs). Key methods that were covered include State Optimization, Retiming, and Cloning.

- State Optimization: For designs involving FSMs, the process of state optimization was explained as a method to improve overall efficiency.

     - How it is done: This is achieved by merging equivalent states to reduce the total state count and applying optimal state encoding schemes (like one-hot or binary).
     - The resulting logic for the state machine is then minimized to create a more compact implementation.

  - Retiming: Retiming was introduced as a powerful sequential optimization that improves a circuit's performance without changing its function.

       - How it is done: The process was described as the repositioning of registers (flip-flops) across combinational logic blocks.
       - By strategically moving these registers, path delays are balanced, which can significantly decrease the minimum clock period and thus increase the maximum operating frequency of the design.

  - Cloning: Cloning was presented as a physically-aware optimization used to solve timing and electrical issues, particularly high fanout.

       - How it is done: This involves identifying a logic cell on a critical path that drives a large load and duplicating it.
       - The connections are then redistributed between the original cell and its "clone." By creating a duplicate, the load is balanced, wire lengths are often reduced, and timing performance is improved.

# Labs on Combinational Logic Optimization

From the provided verilog files , we have different design with different optimization techniques, they are synthesized and the visalization of the netlist for them are as follows
these are synthesized in the same way as shown in day2 of the week1 session

- opt_check2

<img width="3941" height="1013" alt="opt_check2_synthesis_show" src="https://github.com/user-attachments/assets/ec361c7c-1e86-4c6c-9981-39181296b46d" />

- opt_check3

<img width="3943" height="1357" alt="opt_check3_synthesis_show" src="https://github.com/user-attachments/assets/c41bf202-31d0-4de3-8f94-754adf469fcb" />

- opt_check4

<img width="3955" height="1315" alt="opt_check4_synthesis_show" src="https://github.com/user-attachments/assets/6b94a7cd-8092-449c-9a8e-db92dc58ce78" />

- opt_check

<img width="3941" height="1013" alt="opt_check_synthesis_show" src="https://github.com/user-attachments/assets/7f0ae668-f8c2-4b6f-8417-5a46f8cfc579" />

- multiple_module_opt

<img width="3950" height="1981" alt="multiple_module_opt_synthesis_show" src="https://github.com/user-attachments/assets/c3b870ce-52ba-4518-bf54-339d4ab0c7de" />

- multiple_module_opt2

<img width="1724" height="2324" alt="multiple_modules_opt2_synthesis_show" src="https://github.com/user-attachments/assets/682e4847-132f-4d2d-bfd7-c88601ffa6b4" />


# Labs on Sequential Logic Optimization

om the provided verilog files , we have different design with different optimization techniques, they are simulated and synthesized and the visalization of the netlist and waveform for them are as follows
these are synthesized in the same way as shown in day2 of the week1 session 

- dff_const1

<img width="3950" height="1287" alt="dff_const1_gtkwave" src="https://github.com/user-attachments/assets/05cc22e8-84d1-4a38-9fdd-4d1dc1e4cbbf" />

<img width="3964" height="704" alt="dff_const1_synthesis_show" src="https://github.com/user-attachments/assets/0801e860-b39c-48fa-b949-ebb600064867" />

- dff_const2

<img width="3950" height="1287" alt="dff_const2_gtkwave" src="https://github.com/user-attachments/assets/d60c70ec-fb71-4b0b-ad88-67094bb0e073" />

<img width="2885" height="2315" alt="dff_const2_synthesis_show" src="https://github.com/user-attachments/assets/880250b9-78bf-4ec0-895c-06de3fa40b2c" />

- dff_const3

<img width="3960" height="1227" alt="dff_const3_gtkwave" src="https://github.com/user-attachments/assets/0c581c22-f68d-41b6-9e3c-572b438aed14" />

<img width="3939" height="633" alt="dff_const3_synthesis_show" src="https://github.com/user-attachments/assets/a398368d-76bb-44f2-9039-cc6658bbcda4" />

- dff_const4

<img width="3959" height="1254" alt="dff_const4_gtkwave" src="https://github.com/user-attachments/assets/94985c07-ddea-40c9-8988-d2141a18f89d" />

<img width="2254" height="2312" alt="dff_const4_synthesis_show" src="https://github.com/user-attachments/assets/ea956bb1-ce39-4800-bb71-a10cd5c517e5" />

- dff_const5

<img width="3959" height="1254" alt="dff_const5_gtkwave" src="https://github.com/user-attachments/assets/625c9aa2-560c-4305-adf8-5a21fca442af" />

<img width="3939" height="600" alt="dff_const5_synthesis_show" src="https://github.com/user-attachments/assets/30e26f14-6729-49fb-81fc-d485bf31c5f3" />

# Labs on Sequential Logic Optimization for Unused Outputs

Here to demonstrate this optimization, we consider a counter design with the verilog code as follows

<img width="2760" height="797" alt="counter_opt" src="https://github.com/user-attachments/assets/d7d9a0c3-e5d9-4d66-af03-9b26e1762fec" />

This code has unused outputs, where the variable ``q`` is assigned only ``count[0]`` , we can visualize this by synthesising the code and checking the netlist
The visualization of the netlist for the counter is as follows

<img width="3931" height="571" alt="counter_opt_synthesis_show" src="https://github.com/user-attachments/assets/e6b89ea3-05e2-4b73-b4d5-75f774192c14" />

To optimize this , we changed the code as follows

<img width="2609" height="817" alt="counter_opt2_code" src="https://github.com/user-attachments/assets/404627f1-8b15-4731-a481-d444ba31dad2" />

Badically we modified it by assigning 3 bit value to the variable ``q``
We can visualize the changes in the netlist as follows

<img width="3944" height="916" alt="counter_opt2_synthesis_show" src="https://github.com/user-attachments/assets/188f3fec-a670-40b1-9753-13570a16749c" />

# Summry

Learned different types of optimization available in combinational and sequential logci and also optimizised sequential logic with unused outputs

Note: The tools used are the latest version of this date , the synthesis or the netlist visual respresentation might vary based on the version being used.



