# RTL Design using Verilog with SKY130 Technology  
[![sky130RTLWorkshopLOGO](https://user-images.githubusercontent.com/68154219/165596584-9e7f0e31-4797-4b04-960a-a5a5e63f0ef3.PNG)](https://www.vlsisystemdesign.com/rtl-design-using-verilog-with-sky130-technology/?awt_a=5L_6&awt_l=MhARG&awt_m=3Zzzz39ugcA8._6)

## Brief Description of the Workshop
Workshop intends to teach the verilog coding guidelines that results in predictable logic in Silicon. it is important to note that every verilog code is not synthesizable and even if it is , it may result in different logic depending on the coding styles used. The course details all these aspects of the Verilog HDL with theory and backed with lot of practical examples. Workshop introduces to the digital logic design using Verilog HDL . Validating the functionality of the design using Functional Simulation. Writing Test Benches to validate the functionality of the RTL design. Logic synthesis of the Functional RTL Code. Gate Level Simulation of the Synthesized Netlist.

# *Index*
***
* [Day 1: Introduction to Verilog RTL Design and Synthesis](#Day-1-Introduction-to-Verilog-RTL-Design-and-Synthesis)
  * [Introduction to open-source simulator iverilog](#Introduction-to-open-source-simulator-iverilog)
    * [Introduction to iverilog Design Testbench](#Introduction-to-iverilog-Design-Testbench)
  * [Labs using iverilog and gtkwave](#Labs-using-iverilog-and-gtkwave)
    * [Introduction to Lab](#Introduction-to-Lab)
    * [Introduction iverilog gtkwave](#Introduction-iverilog-gtkwave)
  * [Introduction to Yosys and Logic Synthesis](#Introduction-to-Yosys-and-Logic-Synthesis)
    * [Introduction to yosys](#Introduction-to-yosys)
    * [Introduction to Logic Synthesis](#Introduction-to-Logic-Synthesis)
  * [Labs using Yosys and Sky130 PDKs](#Labs-using-Yosys-and-Sky130-PDKs)
    * [Yosys 1 good mux](#Yosys-1-good-mux)
***
* [Day 2: Timing libs, hierarchical vs flat synthesis and efficient flop coding style](#Day-2-Timing-libs-hierarchical-vs-flat-synthesis-and-efficient-flop-coding-styles)
  * [Introduction to timing .libs](#Introduction-to-timing-libs)
    * [Introduction to dot lib](#Introduction-to-dot-lib)
  * [Hierarchical vs Flat Synthesis](#Hierarchical-vs-Flat-Synthesis)
    * [Hier synthesis flat synthesis](#Hier-synthesis-flat-synthesis)
  * [Various Flop coding styles and optimisation](#Various-Flop-coding-styles-and-optimisation)
    * [Why Flops and Flop Coding style](#Why-Flops-and-Flop-Coding-style)
    * [Interesting optimisations](#Interesting-optimisations)
 ***
* [Day 3: Combinational and Sequential Optimisations](#Day-3-Combinational-and-Sequential-Optimisations)
  * [Introduction to optimisations](#Introduction-to-optimisations)
  * [Combinational Logic Optimisations](#Combinational-Logic-Optimisations)
  * [Sequential Logic Optimisations](#Sequential-Logic-Optimisation)
  * [Sequential Optimisations for Unused Outputs](#Sequential-Optimisations-for-Unused-Outputs)
  ***
  * [Day 4: GLS, Blocking vs Non-Blocking Synthesis-Simulation Mismatch](#Day-2-GLS-Blocking-vs-Non-Blocking-Synthesis-Simulation-Mismatch)
  * [GLS, Synthesis-Simulation Mismatch and Blocking/Non-Blocking statements](#GLS-Synthesis-Simulation-Mismatch-and-Blocking/Non-Blocking-statements)
    * [GLS Concepts And Flow Using Iverilog](#GLS-Concepts-And-Flow-Using-Iverilog)
    * [Synthesis Simulation Mismatch](#Synthesis-Simulation-Mismatch)
    * [Blocking And Non Blocking Statements In Verilog](#Blocking-And-Non-Blocking-Statements-In-Verilog)
    * [Caveats with Blocking Statements](#Caveats-with-Blocking-Statements)
  * [Labs on GLS, Synthesis-Simulation Mismatch](#Labs-on-GLS-Synthesis-Simulation-Mismatch)
  * [Lab Synth sim mismatch blocking statement ](#Lab-Synth-sim-mismatch-blocking-statement)
 ***
* [Day 5: If, case, for loop and for generate](#Day-5-If-case-for-loop-and-for-generate)
  * [If Case Constructs](#If-Case-Constructs)
  * [Labs on Incomplete IF Case](#Labs-on-Incomplete-IF-Case)
  * [Labs on Incomplete Overlappin Case](#Labs-on-Incomplete-IF-Case)
 ***
    
 # Day 1: Introduction to Verilog RTL Design and Synthesis
 ***
 ## Introduction to open-source simulator iverilog

 #### *What was learnt*
 #### Terms such as
 * ## Design
    * Design is an actual Verilog code or set of Verilog codes which has the intended functionality
      to meet with the required specifications.
 * ## Testbench 
    * Testbench is the setup to apply stimulus (test_vectors) to the design to check its functionality. 
    * Verification is required to ensure the design meets the timing and functionality requirements.
    * Verilog Testbenches are used to simulate and analyze designs without the need for any physical hardware or any hardware device
 * ## Simulator and how the simulator works
    * RTL design is checked for adherence to the spec by simulating the design
    * Simulator is the tool for simulating the design 
    * Simulator looks for the changes on the input signals
    * Upon change to the input, output is evaluated. That is, if there is no change to input, there is no change to output
 * ## iVerilog
    * We are using iVerilog as simulating tool for labs
    * [Icarus Verilog](http://iverilog.icarus.com/) or iVerilog is a Verilog simulation and synthesis tool.
    * It operates as a compiler, compiling source code written in Verilog (IEEE-1364) into some target format.
 * ## Value Change Dump Format
    * A Value Change Dump File (.vcd file) is an ASCII file which contains header information, variable definitions, and the value changes for specified variables, or all variables, in a given design. 
    * The value changes for a variable are given in scalar or vector format, based on the nature of the variable. 
    * Verilog VCD file is generated by EDA Logic Simulation Tools
 * ## GTKWave 
    *  [GTKWave](http://gtkwave.sourceforge.net/) is a VCD waveform viewer based on the GTK library. This viewer support VCD and LXT formats for signal dumps.
    *  Waveform dumps are written by the Icarus Verilog runtime program [vvp](https://iverilog.fandom.com/wiki/Vvp_Flags)
    *  The user uses $dumpfile and $dumpvars system tasks to enable waveform dumping, then the vvp runtime takes care of the rest.
    *   The output is written into the file specified by the $dumpfile system task. 
    *   The example below dumps everything in and below the tb_good_mux module.
    <p align="center" width="100%">
    <img src="https://user-images.githubusercontent.com/68154219/165763562-4f431070-a810-4a8b-95fc-2228fe0bc754.png"> 
    </p>
    
    * The $dumpvars is used to dump the changes in the values of nets and registers in a file that is named as its argument
    * The $dumpvars is used to specify which variables are to be dumped ( in the file mentioned by $dumpfile)
    * The general syntax of the $dumpvars include two arguments as in
      $dumpvars(<levels> , <module_or_variable> );
    * When level is set to 0, and only the module name is specified, it dumps ALL the variables of that module and all the variables in ALL lower level modules instantiated by this top module. If any module in not instantiated by this top module, then its variable will not be covered.

### Introduction to iverilog Design Testbench
 ###  Design and Testbench setup
 1. Design Setup :
 
<p align="center" width="100%">
    <img src="https://user-images.githubusercontent.com/68154219/165711560-36ce90db-1b59-4405-b330-6f2d066e1ed5.PNG"> 
</p>

 A Design has more than one primary inputs and primary outputs.

2. Testbench Setup :
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/165723917-0018cc34-3451-4f8a-883a-594a6b85c030.png">
 </p>
 All the primary input are given stimulus generator and all primary outputs are given to stimulated observer in a testbench. So, testbench does not have a primary input or primary output.
 
 ###  iVerilog based Simulation Flow
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/165758468-745eb0c9-855e-4409-9179-8098d6bdd707.png">
 </p>
 
* Design and Testbench are applied to the iVerilog simulator tool
* Output of the simulator is a vcd file
* vcd file is viewed using gtkwave viewer

 ## Labs using iverilog and gtkwave
#### *Labs*

 * #### Setting up the environment for all the labs
<p align="center" width="100%">
<img src="https://user-images.githubusercontent.com/68154219/165773236-2c33f8ba-a254-4411-9943-631d0431d890.png">
</p>
 
 * #### Loading the design (good_mux.v file) and testbench file (tb_good_mux.v) to iverilog to generate a.out output file
<p align="center" width="100%">
<img src="https://user-images.githubusercontent.com/68154219/165775238-8e39ccdf-fc51-4bda-8172-543b628a256f.png">
</p>
 
* #### Opening the vcd file (tb_good_mux.vcd) and checking its functionality on gtkwave viewer
 
  ![image](https://user-images.githubusercontent.com/68154219/165779132-571f8736-442f-4241-ad91-6cea5867cecb.png)
 
  The waveform demonstrates that the output triggers on positive clock edge. The logic of the design can be understand from the waveform as the output follows input i0 when sel signal is LOW and follows input i1 when sel signal is HIGH.
 
 
 #### GTKWAVE VIEWER
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/165780807-53e3f295-a452-4e4f-8f4b-0f1f76cf166f.PNG">
 </p>

 #### Lab 3: Introduction iverilog gtkwave Part 2
 * #### Looking into file structure
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/165806522-1e0132c0-6a1e-4954-af4c-9ca09eaf3758.png">
 </p>
 
 * #### Design good_mux verilog file 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/165809109-1c65c82b-468f-419b-921e-e76f3fbd863b.png">
 </p>
 
 * #### Testbench tb_good_mux verilog file
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/165813337-6ecaf95b-38de-421a-806c-8d9612c1b1dc.png">
 </p>
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/165813505-59f026d0-80d7-4ab1-b42e-8a6c3fff284e.png">
 </p>

In testbench, stimulus generator sets sel, i0, i1 to zero. The simulation runs for 300 nano seconds. 
  - The sel input is toggled after every 75 nano seconds
  - The i0 input is toggled after every 10 nano seconds
  - The i1 input is toggled after every 55 nano seconds

 ## Introduction to Yosys and Logic Synthesis
  #### *What was learnt*
 #### Terms such as
 * ## RTL
   * Stands for [Register Transfer Level](https://inst.eecs.berkeley.edu/~ee290c/sp18/lec/Lecture12A.pdf)
   * An abstraction for digital circuits, consisting of 
     * Combinational logic
     * Registers (state elements)
     * Modules (hierarchical and “blackbox” - e.g. analog macros, SRAM macros, etc) and ports/nets
   * Behavioural representation of the required specification.
   * Described in terms of a hardware description language (HDL)
 * ## Netlist 
   * A netlist is textual description of a circuit made of components in VLSI design. 
   * Components are: gates, resistors, capacitors or transistors. Connection of these components are called netlist.
   * Netlist contains the information regarding logical connectivity of all standard cells and macros.
   * Physical design will convert the gate level netlist in complete physical geometric representation. The file produced at the output of the layout is the GDSII (GDS2) FILE or oas which is the file used by the foundry to fabricate the silicons.
 * ## Synthesizer
   * Tool used for converting RTL to netlist.
   * [Yosys](https://www.yosyshq.com/open-source) is the synthesizer used in this course.
   * Yosys is the pure open source software and is a framework for RTL synthesis and more
   * Yosys currently has extensive Verilog-2005 support and provides a basic set of synthesis algorithms for various application domains. It is the core component of most our implementation and verification flows.
 * ## .lib 
   * Collection of logical modules
   * Includes basic logic gates like AND, OR, NOT, etc.
   * Consists of different flavours of same gate like 
     * 2 input AND gate with Slow, Medium and Fast versions,
     * 3 input AND gate with Slow, Medium and Fast versions,
     * 4 input AND gate with Slow, Medium and Fast versions, etc.
 
 ### Introduction to yosys
 ### Yosys Setup
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/165822673-82cec922-2270-4837-a3e2-ef15ecac8fb5.png">
 </p>
 
 Design and .lib ([verilog liberty](https://www.teamvlsi.com/2020/05/lib-and-lef-file-in-asic-design.html#:~:text=LIB%20file%20is%20an%20ASCII,time%20requirement%20of%20the%20cell.)) files are loaded in the yosys synthesizer. The output is the netlist file.
 The netlist is the representation of the design in form of the cells present in the .lib file
 *  *read_verilog* command is used for reading design file.
 *  *read_liberty* command is used for reading lib file.
 *  *write_verilog* command is used for writing design file.
 
 ### Verify the Synthesis
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/165824923-1191db78-9085-4905-975f-4ffcde968b7d.png">
 </p>
 
* Netlist and Testbench files are loaded in iverilog simulator. 
* The output vcd file is observed in the gtkwave viewer. 
* If this output and the RTL simulation output are same then the netlist output of the synthesier is verified to be correct.
* This concludes that the primary inputs and outputs between RTL design and Synthesized netlist will remain the same. So, the same testbench can be used for both. 

### Introduction to Logic Synthesis
#### Different flavours of gate
 * Combinational delay in logic path determines maximum speed of operation of Digital Logic Circuit.
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/165930756-13521cb5-f229-48e4-ac90-ebc6a027fc5e.png">
 </p>

### Need of Fast working cells
Flipflop A and Flipflop B are connected through combinational circuit. To find maximum clock rate that can be applied to the flipflop

 ## *T<sub>clk</sub> > T<sub>CQ_A</sub> +  T<sub>COMBI</sub> +  T<sub>SETUP_B</sub>*
 
 *  T<sub>clk</sub>     - Period of the clock applied
 *  T<sub>CQ_A</sub>    - Propagation Delay of Flipflop A
 *  T<sub>COMBI</sub>   - Propagation Delay of Combinational circuit
 *  T<sub>SETUP_B</sub> - Minimum time duration before the clock arrives for data to reach Flipflop B (SETUP time of FLF B)

 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/165944209-94898d02-8866-4016-aea8-0447715dfacc.png">
 </p>
 
 * #### [SETUP](https://vlsiuniverse.blogspot.com/2013/06/setup-and-hold-basics-of-timing-analysis.html) time is defined as the minimum amount of time before the clock's active edge that the data must be stable for it to be latched correctly.
 * #### Here, T<sub>clk</sub> is the minimum clock period. So, f<sub>clk</sub> is the maximum clock rate or maximum clock Frequency.
 * #### Maximum is the speed of operation, better is the performance of the system.
 * #### For maximum performance, the T<sub>COMBI</sub> delay should be minimum
 * #### So, for high performance we need Fast working cells.
 
 ### Need of Slow working cells
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/165986581-afc4190d-6159-4b6e-a80b-fc591461537d.png">
 </p>

 * If Flipflop A captures the data in first clock cycle then, from the circuit above, Flipflop B stores the previous value of Flipflop A in the next clock cycle. 
  <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/165987430-19c59679-2506-4cb3-b4c4-0fb0bfd8f68d.png">
 </p>
 
*  If the circuit operates at very fast speed, Flipflop A and Flipflop B both will capture data at same clock pulse. This will lead to loss of data, as Flipflop B will not store previous value of Flipflop A. 
 
 * So, we need a minimum delay from Flipflop A to Flipflop B, which should be greater than the HOLD time. The fastest change to data in Flipflop B should happen after HOLD time window.

 ## *T<sub>HOLD_B</sub> < T<sub>CQ_A</sub> +  T<sub>COMBI</sub>*
 
 *  T<sub>HOLD_B</sub>  - HOLD Time of Flipflop B
 *  T<sub>CQ_A</sub>    - Propagation Delay of Flipflop A
 *  T<sub>COMBI</sub>   - Propagation Delay of Combinational circuit
 
* #### [HOLD](https://vlsiuniverse.blogspot.com/2013/06/setup-and-hold-basics-of-timing-analysis.html) Time is defined as the minimum amount of time after the clock's active edge during which data must be stable.
* #### The data that was launched at the current edge should not travel to the capturing flop before hold time has passed after the clock edge. 
* #### Adherence to hold time ensures that the data launched at current clock edge does not get captured at the same edge.
* #### In other words, hold time adherence ensures that system does not deviate from the current state and go into an invalid state. 
* #### So, we need slow cells for preventing data corruption.
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/165992608-6404e5df-5888-4fd7-b8b4-7c44108e75ae.png">
 </p>
 
* #### As shown in the figure, the data at the input of flip-flop can change anywhere except within the seTup time hold time window for achieving optimal speed.
 
### Faster Cells v/s Slower cells
 
 * Load in the Digital circuit is the capacitor.
 * Faster the charging and discharging of capacitor, less in the cell propagation delay.
 * For Faster charging and discharging of capacitor,  we need transistors with more current sourcing capacity.
 * From the equation, 
 
   ![image](https://user-images.githubusercontent.com/68154219/165995076-4b50324e-a856-4b2c-b21a-d8aba1d0b30b.png)
   
   ![image](https://user-images.githubusercontent.com/68154219/165995793-fb1c9080-cc46-4603-af56-275ffdd07b26.png)

  * #### Therefore, Wider the transistor, low is the delay, more Area and Power is consumed as well.
  * #### Narrow the transistor, high is the delay, less Area and Power is consumed.
 
 ### Selection of Cells
 
 * Need to guide the synthesizer to select the flavour of cells that is optimum for the implementation of logic circuit.
 * More Faster cell causes
   * Bad circuit in terms of Area and Power
   * HOLD time variation
 * More use of slower cells causes
   * Sluggish circuit
   * May not meet the performance needed
 * The guidance to select appropriate flavours of cells is given by [CONSTRAINTS](http://www.vlsi-expert.com/2012/02/design-constraint-maximum-transition.html#:~:text=So%20Constraints%20are%20the%20instructions,or%20how%20the%20tool%20behaves.).
 
 ### Synthesizer Illustration
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/165997897-ca7baa52-32ef-4d24-bf6b-c2e64af51b3b.png">
 </p>
 
 
 ## Labs using Yosys and SKY130 [PDKs](https://www.synopsys.com/glossary/what-is-a-process-design-kit.html)
 
 #### Yosys 1 good mux 
 #### *What was learned*
 * How to invoke Yosys
 * How to Synthesize the Design
 #### *Labs*
 * Invoke yosys
 
   ![image](https://user-images.githubusercontent.com/68154219/166007116-fe97b3c2-0107-4297-b7d6-f7aba642cf0e.png)

 * Read the liberty .lib file
 
   ![image](https://user-images.githubusercontent.com/68154219/166008066-ed700735-418c-455a-ad68-3f487edc88cc.png)
 
 * Read verilog design good_mux.v file
 
   ![image](https://user-images.githubusercontent.com/68154219/166008559-8ce5ad80-eede-48e9-bf6b-e63afbb894ef.png)
 
 * Synthesize the module good_mux
 
   ![image](https://user-images.githubusercontent.com/68154219/166009446-c2ecbdb5-d21e-4e1a-a599-fb422a43c380.png)
 
 * Generate netlist using [ABC](http://people.eecs.berkeley.edu/~alanmi/abc/#:~:text=Berkeley%20Logic%20Synthesis%20and%20Verification,appearing%20in%20synchronous%20hardware%20designs.) tool

   ![image](https://user-images.githubusercontent.com/68154219/166013365-f2700774-ebb8-4e11-ac77-741da1739150.png)
 
   ![image](https://user-images.githubusercontent.com/68154219/166021172-e9eb10a3-bf45-4f5b-aafb-053c13468169.png)

 It has
 * 3 inputs i0, i1, sel
 * 1 output y
 * Zero internal signal
 
 * Graphical Representation of design
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166053553-e04bdca1-b2af-42a7-9b0b-c826e2eb625a.png">
 </p>
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166054967-6ae57303-927c-4085-a92c-83531722ca2c.png">
 </p>
 
 * The graphical representation above consists of a 2 : 1 mux ($82 sky130_fd_sc_hd__mux2_1). 
 * It has 3 inputs i0, i1, sel loaded to mux through buffer.
 * The output y also has a buffer.
 * Write a Netlist 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166059167-4127f8cb-f984-4351-8d19-67a14109e0f5.png">
 </p>
 
![image](https://user-images.githubusercontent.com/68154219/166059645-4546cf1e-ea2e-4b08-86a3-a9956c4ea81d.png)

![image](https://user-images.githubusercontent.com/68154219/166059770-b71534c0-542b-4f34-996a-d08dc64efc9c.png)

* Modify the switch for obtaining netlist in the simple way
 
![image](https://user-images.githubusercontent.com/68154219/166060935-e8c4a2c9-6507-479e-86ea-3dfa388b961b.png)

![image](https://user-images.githubusercontent.com/68154219/166060773-b03c3248-877b-4389-8f66-dbcb8b083585.png)

 ***
 # Day 2: Timing libs, hierarchical vs flat synthesis and efficient flop coding style
 ***
 ## Introduction to timing .libs

 #### *What was learnt*
  * Contents of .lib file
  * understanding .lib file name
 #### *Labs*
 
 #### Introduction to dot Lib 
 
 * Opening .lib file sky130_fd_sc_hd__tt_025C_1v80
 
 ![image](https://user-images.githubusercontent.com/68154219/166068006-807e6379-272c-49df-bfbe-75d8139f8b0e.png)

 ![image](https://user-images.githubusercontent.com/68154219/166068151-ee5e99f1-2b4c-4b5b-af5a-e8f675395963.png)

 The .lib file tells us about :
 
 * #### Factors causing variation in fabrication are
   * Process
   * Voltage
   * Temperature
 
 * In .lib file named  **sky130_fd_sc_hd__tt_025C_1v80**
   * **130** represents 130nm fabrication
   * **tt** represents typical process
   * **025C** represents 25 degrees celcius temperature
   * **1v80** represents 1.8 volt
 
 * CMOS Technology is used
 * Delay Model is Look up Table
 * Time unit is 1 nano sec
 * Voltage unit is 1V
 * Current unit is 1mA
 * Load resistance unit is 1K Ohms
 * Load capacitance unit is 1pf
 
 * #### Cell Definitions
 
 ![image](https://user-images.githubusercontent.com/68154219/166070000-e2508230-e777-4cdc-b4ab-8df2b3e1392c.png)

 For Example, The first cell  **sky130_fd_sc_hd_a2111o_1**
 * Leakage power value is given
 * **a2111o** means it will AND a0, a1 then OR it with b1,c1,d1
 * cell consists of all input combinations where !(variable) represents logic zero of a variable.
 
 The logic equation of the cell *sky130_fd_sc_hd_a2111o_1* is given in the .behavioral file
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166155199-bf8026f1-95f3-4b67-9fef-5f1003f8a514.png">
 </p>
 
 * Comparison between 3 different flavours of the same cell
 
 ![image](https://user-images.githubusercontent.com/68154219/166155530-aa014058-b69d-4160-980b-7866913c6935.png)

 ![image](https://user-images.githubusercontent.com/68154219/166155681-2a59fa14-3bcc-46d3-9d3e-fad84c044b6c.png)
 
  *  Flavour 1 has smallest area so has less wide transistor and more delay. So flavour 1 is slow flavour cell but has least power leakage.
  *  Flavour 4 has largest area so has wider transistor and less delay. So flavour 2 is fast flavour cell but has maximum power leakage.
  *  As compared to Flavour 1 and 4, Flavour 2 has moderately large area and width of the transistor.  So flavour 4 is moderate flavour cell and has moderate power leakage.
  *  The cell footprint is same for all the flavours of a cell
 
 ![image](https://user-images.githubusercontent.com/68154219/166156289-0225ada3-b556-47ac-b256-94a4299ae389.png)

  *  The maximum transition is same for all the flavours of a cell.
  *  The rise capacitance increases with increase in the flavour number. So, flavour 1 has high delay and flavour 4 has low delay.
 
 For less number of input gate, eg. 2 input AND gate, the .behavioral file is as given below
 
 ![image](https://user-images.githubusercontent.com/68154219/166157424-ce14139c-f34d-44ef-a852-ed5fad135b57.png)
 ![image](https://user-images.githubusercontent.com/68154219/166157357-f39d2a78-9f49-45b4-84f2-444a9a333078.png)

 * Comparison between 3 different flavours of the 2 input AND gate cell is as shown below
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166157525-0cc578c8-1b5a-4440-b416-cae37e9f1d99.png">
 </p>
 
  * It has four combinations of leakage power for 4 different input combinations such as (A, B) = {00, 01, 11, 10}.
 
    Thus, **No. of leakage power combination = No. of input combinations**
 
 
 ## Hierarchical vs Flat Synthesis
 
 #### Hier synthesis flat synthesis
 
 * *Opening multiple_modules.v file*
 
 ![image](https://user-images.githubusercontent.com/68154219/166159071-2a1deedc-431e-47cc-9ecb-8e0aa9b55361.png)
 
 * It consists for 2 submodules - sub_module1 and sub_module2
 * sub_module2 is a OR gate : **y = a | b**
 * sub_module1 is a AND gate : **y = a & b**
 * multiple_modules is the combination of sub_module1 and sub_module2 as shown
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166159619-2e51f810-0bca-461c-a6fc-019fb0a28ad5.png">
 </p>
 
* #### Synthesis of multiple_modules.v
 
 Steps for hierarchical synthesis:
 
 1. Launch yosys 
 2. read liberty file
 3. read multiple_modules.v file
 4. synthesize the multiple_modules
 
    ![image](https://user-images.githubusercontent.com/68154219/166160190-747d50ad-8165-4add-9d14-f0a908d6fac9.png)
    ![image](https://user-images.githubusercontent.com/68154219/166160134-c76b1b90-782b-4ef6-a8f0-de0e24ab4191.png)
 
    ![image](https://user-images.githubusercontent.com/68154219/166160282-98727824-4123-4f82-85fb-41a008fdfdf9.png)
 
    ![image](https://user-images.githubusercontent.com/68154219/166160417-f46642c7-fa2f-449c-ad91-0d9eff5ea586.png)

 * The statistics of sub_module1 and sub_module2 shows the number of wires, memories, processes and cells.
 * The hierarchical design shows there are 2 cells - AND, OR
 
 5. Generating netlist and Mapping .lib file with design multiple_modules.v file using [ABC](http://people.eecs.berkeley.edu/~alanmi/abc/#:~:text=Berkeley%20Logic%20Synthesis%20and%20Verification,appearing%20in%20synchronous%20hardware%20designs.) tool
 
    ![image](https://user-images.githubusercontent.com/68154219/166160879-0856df54-095f-47db-8185-bf4a634700f1.png)
    ![image](https://user-images.githubusercontent.com/68154219/166160927-59c3e4e2-314c-44ed-8723-e788f62f256b.png)
 
 6. Graphical representation of multiple_module design 
 
    <p align="center" width="100%">
    <img src="https://user-images.githubusercontent.com/68154219/166161028-b648bcfe-a051-4490-85db-fcc6d809dbcf.png">
    </p>
 
    This is the hierarchical design where we can see the hierarchy.
 
 7. Write netlist in multiple_modules_hier.v file without attributes
 8. Opening the netlist multiple_modules_hier.v file
 
    ![image](https://user-images.githubusercontent.com/68154219/166161327-4cccc15b-0f8c-4f5f-9d41-af019641c7df.png)
 
    ![image](https://user-images.githubusercontent.com/68154219/166161829-360852bf-6a77-46c3-92d2-ef05f3868f1e.png)
    
    * multiple_modules preserves the hierarchy with instantiation of sub_module1 and sub_module2
    * sub_module1 has instantiation of 2 input AND cell named *sky_fd_sc_hd__and2_0*
    * sub_module2 has instantiation of input isolation cell  named *sky130_fd_sc_hd__lpflow_inputiso1p*
      
      The schematic of [sky130_fd_sc_hd__lpflow_inputiso1p](https://antmicro-skywater-pdk-docs.readthedocs.io/en/test-submodules-in-rtd/contents/libraries/sky130_fd_sc_hd/cells/lpflow_inputiso1p/README.html) cell is
 
      <p align="center" width="100%">
      <img src="https://user-images.githubusercontent.com/68154219/166162288-bc2e41bb-f4fb-4d66-9fb5-049c5984d4d7.png">
      </p>
      
      It is a OR realisation.
 
 9. Flatten the netlist 
 
    ![image](https://user-images.githubusercontent.com/68154219/166163509-b2e6f589-b7f0-4d1e-83d0-7835320bbea6.png)
 
    ![image](https://user-images.githubusercontent.com/68154219/166163559-059eb1da-5fc2-4dd1-9623-e9b15bdd8ce5.png)
 
    On comparing the multiple_module_hier.v and  multiple_module_flat.v files, it can be observed that the flatten modules does not preserves the hierarchy.
 
    The multiple_module_hier module instantiates AND, input isolation cells within the respective submodules, whereas multiple_module_flat module directly instantiates the cells within its own module.
 
 10. Graphical representation of flattened multiple_module.v file 
 
     <p align="center" width="100%">
     <img src="https://user-images.githubusercontent.com/68154219/166163880-927ee212-1b83-4cb0-8ea8-93ada559f465.png">
     </p>
    
     The hierarchical graphical representation of multiple_module had sub_module1 and sub_module2, whereas the flattened representation has AND and Input Isolation cells.  

  * Sub module level synthesis can be done for replicating the synthesisof same submodule multiple times if the same module appears multiple times in the design. For eg. In multipler, same operations are executed multiple times.  
 
    ![image](https://user-images.githubusercontent.com/68154219/166164860-5f2c9e35-5531-46cc-99c6-e4912d983a87.png)
 
    ![image](https://user-images.githubusercontent.com/68154219/166164900-ccbe01bf-4d4b-4047-8d43-b2e8e8c45b26.png)
 
    ![image](https://user-images.githubusercontent.com/68154219/166164959-ca65f7db-95d8-41e8-8a4c-8f8765afa672.png)
 
    ![image](https://user-images.githubusercontent.com/68154219/166164980-d5e95784-f2a0-4ab2-b19f-f71d29570d43.png)
 
    <p align="center" width="100%">
    <img src="https://user-images.githubusercontent.com/68154219/166164991-b3f1c1c8-f376-421d-a22f-a9f6638dabcc.png">
    </p>
    
## Various Flop coding styles and optimisation
 
 ### Why Flops and Flop coding styles 
 
 * #### Why Flip Flop
 
 In combinational circuits, propagation delay can cause glitch in the output as explained below
 
 For Boolean expression Y = (A & B) | C, for input (A, B, C) = {1, 1, 0}; Y should be 1. But propagation delay of 1 ns causes glitch of 1 ns
 
  <p align="center" width="100%">
  <img src="https://user-images.githubusercontent.com/68154219/166165922-217c2c0e-d97b-44cd-a6fb-6227130979f6.png">
  </p>
 
 * To avoid corruption of output due to glitches, flip flops can be used between two combinational circuits. 
 * Flipflops are triggered only on the edge of the clock applied.
 * So, eventhough the output of combinational circuit is glitching, output will be stable.
 * Flip Flops are initialied by SET/RESET which can be synchronous or asynchronous

 
 1. Open the verilog files of 4 types of flip flops - dff_asyncres.v, dff_async_set.v, dff_asyncres_syncres.v, dff_syncres.v
 
    ![image](https://user-images.githubusercontent.com/68154219/166165321-40293df2-8ca5-45ed-8fbd-a8e14360dc65.png)
    
    * **dff_asyncres - Asynchronous Reset D Flip Flop**
        * The logic is executed only on positive edge of clock or positive edge of async_reset
        * If async_reset = HIGH, output is LOW; else output follows input at d.
    
    * **dff_async_set - Asynchronous Set D Flip Flop**
        * The logic is executed only on positive edge of clock or positive edge of async_set
        * If async_reset = HIGH, output is HIGH; else output follows input at d.
 
    * **dff_asyncres_syncres - Asynchronous Reset Synchronous Reset D Flip Flop**
        * The logic is executed only on positive edge of clock 
        * If async_reset = HIGH, output is LOW; else if syncres = HIGH output is LOW, else output follows input at d.
 
    * **dff_syncres - Asynchronous Reset Synchronous Reset D Flip Flop**
        * The logic is executed only on positive edge of clock 
        * If sync_reset = HIGH, output is LOW; else if syncres = HIGH output is LOW, else output follows input at d.
 
 ### Lab flop synthesis simulations
 
 2. Simulating all 4 flipflop verilog designs
 
    *  **dff_asyncres**
 
       ![image](https://user-images.githubusercontent.com/68154219/166167029-5390b775-fb18-41d3-bd1a-161b0f211dce.png)
 
       Waveform :
       
       ![image](https://user-images.githubusercontent.com/68154219/166167060-295001d2-46d2-486f-9885-d7e3aecc4cfd.png)
 
     *  **dff_async_set**
 
       Following similar simulation steps
 
       Waveform :
       
       ![image](https://user-images.githubusercontent.com/68154219/166167344-5f150901-2a81-48e5-8680-7c469e8bb521.png)

    *  **dff_asyncres_syncres**
 
       Following similar simulation steps
 
       Waveform :
       
       ![image](https://user-images.githubusercontent.com/68154219/166167328-63902f06-aa79-4d05-8291-a11f0dd5d0e4.png)
 
    *  **dff_syncres**
 
       Following similar simulation steps
 
       Waveform :
       
       ![image](https://user-images.githubusercontent.com/68154219/166167510-ad4066be-e8c5-4125-aab7-b483ebc3e675.png)
    
 3. Synthesizing all the 4 flipflop verilog files 
    * Follow the same steps as previously explained to Initialize yosys, write liberty, read verilog files and synthesis the design
    * use dfflibmap command used for synthesizing flipflops and then execute abc tool

    ![image](https://user-images.githubusercontent.com/68154219/166167988-c817ec12-68e8-4c5a-8086-3e7a4dc5725d.png)

    ![image](https://user-images.githubusercontent.com/68154219/166168013-8d054192-05e3-4c93-8d6a-4e61d57f3e7a.png)

    Graphical representation of all 4 flipflops post synthesis is as shown

    * **dff_asyncres**

      <p align="center" width="100%">
      <img src="https://user-images.githubusercontent.com/68154219/166168079-8f2da380-e048-4352-a31c-ec0380dbbe7c.png">
      </p>
      
      *sky130_fd_sc_hd_dfrtp_1* is D flipflop reset cell. Since it is a reset, it has inverting clock as input

    * **dff_async_set** 

      <p align="center" width="100%">
      <img src="https://user-images.githubusercontent.com/68154219/166168337-18550979-e570-4682-b6d5-2c9c8e00d5fa.png">
      </p>
     
    * **dff_asyncres_syncres**

     <p align="center" width="100%">
     <img src="https://user-images.githubusercontent.com/68154219/166168794-8c347753-83af-478a-a934-bf5f5b5575e2.png">
     </p>

    * **dff_syncres**
      
      <p align="center" width="100%">
      <img src="https://user-images.githubusercontent.com/68154219/166168715-257807b6-b60b-479e-914b-e1d657d15f33.png">
      </p>

 ### Interesting Optimisations

 For example, optimising verilog designs mult_2.v and mult_8.v

 1. Opening the verilog files

    <p align="center" width="100%">
    <img src="https://user-images.githubusercontent.com/68154219/166191702-d1ba0923-273a-4dd1-a594-2e9bae66b243.png">
    </p>  

    * **mult_2.v** 

      After simulation and synthesis 

      * The Boolean equation is y = a * 2
      * No of inputs   = 3 (a[2 : 0])
      * No. of outputs = 4 (y[3 : 0])
      * Logic Table 

        <p align="center" width="100%">
        <img src="https://user-images.githubusercontent.com/68154219/166195492-c5c10357-b0b4-41dc-8941-900eee0d012e.png">
        </p>

      * The output of the multiplier is the left shifted input with zero padded at LSB. 
      * So, there are zero memories, processes and cells. 

       ![image](https://user-images.githubusercontent.com/68154219/166197143-b13b9499-7295-4058-aa7b-a615082b1140.png)

      * Graphical Representation of Multiplier of 2 is as shown

        <p align="center" width="100%">
        <img src="https://user-images.githubusercontent.com/68154219/166201854-71d3f633-76e5-48f3-b91e-85ceb3485dc0.png">
        </p>

      * Generating Netlist file mul2_net.v

        ![image](https://user-images.githubusercontent.com/68154219/166202442-325b3a0c-8930-42b5-ad4e-ff6088557647.png)

        <p align="center" width="100%">
        <img src="https://user-images.githubusercontent.com/68154219/166202209-dcec3c9c-9f9b-4355-af2b-a23ae66638d1.png">
        </p>

    * **mult_8.v**

       After simulation and synthesis 

      * The Boolean equation is y = a * 9 = a * 8 + a = y(mul_2) + a 
      * No of inputs   = 3 (a[2 : 0])
      * No. of outputs = 6 (y[5 : 0])
      * Logic Table 

        <p align="center" width="100%">
        <img src="https://user-images.githubusercontent.com/68154219/166200805-c7d692e8-959f-49f6-8614-907900d20e71.png">
        </p>


      * The output of the multiplier is the double replication of input, that is, aa. 
      * So, there are zero memories, processes and cells. 

       ![image](https://user-images.githubusercontent.com/68154219/166201772-c16beb07-5686-47d5-87b5-e0626059e2a9.png)

      * Graphical Representation of Multiplier of 8 is as shown

        <p align="center" width="100%">
        <img src="https://user-images.githubusercontent.com/68154219/166201347-8699cb2c-e187-4d9e-b347-81879475c14b.png">
        </p>

      * Generating Netlist file mul8_net.v

        ![image](https://user-images.githubusercontent.com/68154219/166201443-69c5c68f-5646-4eca-86d5-18eb787ecb65.png)

        <p align="center" width="100%">
        <img src="https://user-images.githubusercontent.com/68154219/166201513-4c7066af-a63c-4ed6-9c63-4d1f08ed9a2a.png">
        </p>

 ***
 # Day 3: Combinational and Sequential Optimisations
 *** 
 ## Introduction to optimisations
 
 #### *What was learned*
 
 * **Combinational Logic Optimisation** 
   *  It is squeezing the combinational circuit logic to get the most optimised design which is efficient in terms of *area* and *power*.
   * Techniques for Combinational Logic Optimisation are - 
     1. Constant Propagation also called as Direct Optimisation
     2. Boolean Logic Optimisation using K-Maps and Quine McKluskey
 
 1. #### Constant Propagation Optimisation
 
 For example, consider the combinational circuit shown below
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166206822-7590348c-df49-44cd-ae54-d7bc9f7b8330.png">
 </p>
 
 The AND and NOR gates circuit is optimised to single NOT gate circuit with input C for input A = 0.
 
 The combinational circuit requires Total 6 MOS transitors  which is optimised to only 2 MOS transistors. 
 
 2. #### Boolean Logic Optimisation
 
 For example, consider the boolean equation shown below
 
 ![image](https://user-images.githubusercontent.com/68154219/166207963-7e95ce8f-2ec5-4882-b8f2-a115dd923f86.png)
 
 The logic is
 * If C = 1, Y1 = A; else Y1 = 0
 * If B = 1, Y2 = C; else Y2 = Y1
 * If A = 1, Y = Y1; else Y = C'
 
   <p align="center" width="100%">
   <img src="https://user-images.githubusercontent.com/68154219/166210687-abb9125c-10cc-4470-9a74-8d30340ad800.png">
   </p>
 
   Y1 = AC
   Y2 = B’Y1 + BC = AB’C + BC = AC + BC
   Y = A’C’ + AY2 = A’C’ + AC + ABC = A’C’ + AC = A XNOR C
   #### Optimised equation is Y = A XNOR C

   * **Sequential Logic Optimisation**
   *  It is squeezing the sequential circuit logic to get the most optimised design which is efficient in terms of *area* and *power*.
   * Techniques for Sequential Logic Optimisation are - 
     1. Basic Sequential Constant Propagation Optimisation
     2. Advanced Sequential Constant Propagation Optimisation
        * State Optimisation 
        * Retiming
        * Sequential Logic Cloning (Floor Plan Aware Synthesis) [*Not covered in this workshop*]
 
 1. #### Basic Sequential Constant Propagation Optimisation
 
    Consider the below examples
 
    <p align="center" width="100%">
    <img src="https://user-images.githubusercontent.com/68154219/166214605-2c1ad223-0ea4-46c2-8daf-1a03e59bbc60.png">
    </p>
   
    * Q is always 0 as D = 0 irrespective of whether or not RESET is HIGH or LOW
    * Y = (0 A)' = 1
    
    But when SET is appiled in place of RESET, 
    * Q is not constantly zero. 
    * For SET = 1, Q is HIGH irrespective of clock. Thus it is Asynchronous.
    * For SET = 0, Q waits for clock edge and then goes LOW. Thus it is Synchronous.
    * Thus, this design cannot be optimised and has to be retained. 
    * For Sequential circuit to be optimised, Q pin should be constant.
 
    #### *Every flipflop with D input pin tied cannot be optimised*
 
 2. #### Advanced Sequential Constant Propagation Optimisation
 
    *  State Optimisation - Optimisation of Unused States
    *  Retiming - If different part of same sequential circuit has different frequencies, the output of the sequential circuit has optimised frequency equal to the minimum frequency. 
    *  Sequential Logic Cloning (Floor Plan Aware Synthesis) - To avoid long routing, D flipflops are used to make copies of the logic in the path. Thus retaining the logic throughout the long path.
 
#### *Labs*
 
## Combinational Logic Optimisations

1. Opening and simulating opt_check.v files
 
   <p align="center" width="100%">
   <img src="https://user-images.githubusercontent.com/68154219/166219168-f97fb08a-bcfa-4095-98b6-4047875f977c.png">
   </p>
 
   <p align="center" width="100%">
   <img src="https://user-images.githubusercontent.com/68154219/166219371-ead08bbf-cc2f-4637-b764-112292f0d9d8.png">
   </p>

 2. Synthesing opt_check.v, opt_check2.v, opt_check3.v, opt_check4.v files
 
    * **opt_check.v** 
    
      ![image](https://user-images.githubusercontent.com/68154219/166221306-aa3a8daa-4c21-4a74-ae8b-fbcff3253ca2.png)
 
      ![image](https://user-images.githubusercontent.com/68154219/166221415-77ec87d6-5840-4fd0-9f3b-24dfeb333cf4.png)
    
      There is one cell present in the design
 
 
      ![image](https://user-images.githubusercontent.com/68154219/166221518-2731f00f-810d-43e4-9c07-88985f98843e.png)

       opt_clean removes unused cells and wires, purge command clears named left over or unconnected nets which is passed implicitly to opt_clean.
 
 
      ![image](https://user-images.githubusercontent.com/68154219/166221591-f068817f-2bfc-4ad6-9c26-6ab75e96806e.png)
 
      ![image](https://user-images.githubusercontent.com/68154219/166221761-a276ba11-5c46-4dcc-b3d5-f85931ba3644.png)
 
    * **opt_check2.v** 
    
      ![image](https://user-images.githubusercontent.com/68154219/166223057-b3f3ed42-7361-4d7c-aa94-f59849180c2e.png)
 
      ![image](https://user-images.githubusercontent.com/68154219/166223158-7e3e3d40-1da6-4d40-aa06-be8e78e396d4.png)
    
      There is one cell present in the design
 
 
      ![image](https://user-images.githubusercontent.com/68154219/166221518-2731f00f-810d-43e4-9c07-88985f98843e.png)

      opt_clean removes unused cells and wires, purge command clears named left over or unconnected nets which is passed implicitly to opt_clean.
 
 
      ![image](https://user-images.githubusercontent.com/68154219/166221591-f068817f-2bfc-4ad6-9c26-6ab75e96806e.png)
 
      ![image](https://user-images.githubusercontent.com/68154219/166223426-8dc60f5f-6430-4060-b516-8f4293d628fb.png)
 
    * **opt_check3.v** 

      ![image](https://user-images.githubusercontent.com/68154219/166223864-42ae603c-bdd2-4227-b339-8aaa2622705c.png)

      ![image](https://user-images.githubusercontent.com/68154219/166223991-1c341458-fde0-4512-b830-2122eb90ba65.png)

      There are 2 cells present in the design

 
      ![image](https://user-images.githubusercontent.com/68154219/166224069-8fc0c406-7371-4e0b-87d9-e33a003800dd.png)
 
    * **opt_check4.v** 

      ![image](https://user-images.githubusercontent.com/68154219/166224452-84445774-1729-428e-9b95-8cc64829574b.png)

      ![image](https://user-images.githubusercontent.com/68154219/166224522-6b7eb5f8-dce0-421f-b42f-4a33b62d4426.png)

      There are 4 cells present in the design


      ![image](https://user-images.githubusercontent.com/68154219/166224587-28f162af-c6ff-45ee-9a5e-0016e7cfa73d.png)


 3. Graphical Representation
 
    <p align="center" width="100%">
    <img src="https://user-images.githubusercontent.com/68154219/166221815-c7b5e074-fac6-413b-b401-230f640b1f66.png">
    </p>

    <p align="center" width="100%">
    <img src="https://user-images.githubusercontent.com/68154219/166223659-23e214f3-f309-4d6f-a390-6d8017fb1df8.png">
    </p>
 
    <p align="center" width="100%">
    <img src="https://user-images.githubusercontent.com/68154219/166224275-82a86a52-ae9d-4602-a77b-7d37187bf225.png">
    </p>

    <p align="center" width="100%">
    <img src="https://user-images.githubusercontent.com/68154219/166225222-ab485cdd-c322-4872-8bfd-b231a56b6b5d.png">
    </p>
 
 **[ASSIGNMENT]** Similarly multiple_module_opt.v is optimised and then flatten which increases efficiency of the design
 
 1.  Opening and simulating verilog files
 
 ![image](https://user-images.githubusercontent.com/68154219/166226559-db152f33-6ea4-4e3b-8b36-6d13dbd47552.png)
 
 ![image](https://user-images.githubusercontent.com/68154219/166226667-be0e5dc0-8401-440e-880b-07346e3e3975.png)

 2. Synthesizing the design 
 
 ![image](https://user-images.githubusercontent.com/68154219/166227638-5256d3ea-0f3a-4415-a794-9f394ed82008.png)

 3. Hierarchical statistics
 
 ![image](https://user-images.githubusercontent.com/68154219/166227792-a62426b5-8ba9-4be9-89cf-b15417c995f9.png)

 ![image](https://user-images.githubusercontent.com/68154219/166227872-44a082b6-727d-4c01-b693-10c12ff0a22d.png)

 4. Graphical Representation
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166227973-e4a35128-3f51-484e-9db2-df5608311fc2.png">
 </p>
 
 5. Write hierarchical netlist file
 
 ![image](https://user-images.githubusercontent.com/68154219/166228064-10d78185-ee2f-42cd-8913-e6bba4326dab.png)

 6. Flattening the design
 
![image](https://user-images.githubusercontent.com/68154219/166228129-8bfbf085-c83f-41d1-8afd-8edcb5192452.png)

 7. Comparing hierarchical and flat modules
 
 ![image](https://user-images.githubusercontent.com/68154219/166227135-8bd408c6-c6f5-4a58-9fd1-6da6e9b6acdb.png)

 The submodules in hierarchical design is removed in flattened design
 
 8. For sub_module1
 
    ![image](https://user-images.githubusercontent.com/68154219/166228374-8b1c2d51-0f64-44e9-9a39-3d802f29d556.png)
 
    <p align="center" width="100%">
    <img src="https://user-images.githubusercontent.com/68154219/166228427-47ee16cd-caab-4cc2-b4b2-1582c4d6192c.png">
    </p>
  


## Sequential Logic Optimisations
 
We will simulate and synthesize 4 types of D flipflop constant files to optimise the design. 

* **dff_const1.v**
 
#### Simulating the design
 
![image](https://user-images.githubusercontent.com/68154219/166230336-905ffa0e-2de7-4852-ab12-4722e5c3053a.png)
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166229861-409b6639-6e6b-44f1-872c-954f2782e8a5.png">
 </p>
  
  <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166229905-c83531ec-66de-40b9-a1dd-ecb5ce6d31fe.png">
 </p>

 Q is constantly zero when RESET is HIGH

 #### Synthesizing design
 
 ![image](https://user-images.githubusercontent.com/68154219/166230429-a58cdb61-3a9f-4d2b-9ccc-6b88b83d9716.png)
 
 ![image](https://user-images.githubusercontent.com/68154219/166230681-dbbb1e1c-835f-4388-8126-2b6481d9125c.png)

 ![image](https://user-images.githubusercontent.com/68154219/166230592-9cfc6a45-3d69-4da5-b20a-f6a4f97c5fbe.png)

 ![image](https://user-images.githubusercontent.com/68154219/166230624-7a0cc3ae-20bf-45e9-8426-4985ea2588cb.png)

 ![image](https://user-images.githubusercontent.com/68154219/166230721-5fb4393f-d31f-4088-9c18-bf0bf8ffde6c.png)

 #### Graphical Representation
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166230781-8eeb8e0f-1b78-4cf0-8f35-866cf0224140.png">
 </p>
 
 Similar steps are followed for simulation and synthesis of other D flipflop constant design
 
 Following are the results of their simulation and synthesis
 
 
 * **dff_const2.v**
 #### Simulating the design
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166231424-09e05b55-9055-4c96-8d14-234f5b92ffcf.png">
 </p>
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166231585-4e4d63d5-6624-4410-a49c-b8408fdcba50.png">
 </p>
 
#### Synthesizing design
 
 ![image](https://user-images.githubusercontent.com/68154219/166231720-50df183d-5db7-43eb-a402-7599b4b4142b.png)
 
#### Graphical Represenntation

 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166231889-0e471781-cddd-47b5-a2a7-627e40ce970d.png">
 </p>
 
 * **dff_const3.v** 

 #### Simulating the design

 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166233590-fc2ec18e-8461-400f-bc69-bb2625fe3120.png">
 </p>
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166232303-6691eef2-f4fe-4b8f-8a25-dfad2c514eda.png">
 </p>
 
#### Synthesizing design
 
 ![image](https://user-images.githubusercontent.com/68154219/166233389-287a2caa-1021-4ee2-b401-d74b70817fec.png)
 
#### Graphical Representation 
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166233210-8da9d23a-2333-4aba-a038-c73e1b30899e.png">
 </p>

## Sequential optimisation unused outputs
 
 In this module, we will simulate and synthesis and optimizing counter design
 
 #### Opening counter_opt files
 
 ![image](https://user-images.githubusercontent.com/68154219/166234794-e09666d5-dc8a-49e3-be0c-17af49a8cb10.png)

 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166235072-44b55d3e-b51e-49e0-8c34-5c619dd3e406.png">
 </p>
 
 
 #### Simulation waveforms 
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166235280-ce9d756b-8b68-452d-9153-83da20de92b8.png">
 </p>
 
#### Synthesis result
 
 ![image](https://user-images.githubusercontent.com/68154219/166235457-53ff9816-72d3-452d-9556-a09bb93cd1da.png)

 ![image](https://user-images.githubusercontent.com/68154219/166235516-4ad3abc3-9108-48e3-bf1a-1201734389ca.png)

 #### Graphical Representation
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166235615-400c623d-28af-4d9f-a373-f6f0e94f53e5.png">
 </p>
 
         
 #### Now, we will copy counter_opt.v file and change the count bits to count[2:0] to create 2 unused bits.       
         
![image](https://user-images.githubusercontent.com/68154219/166237751-3d6dfc77-c15e-40ac-98d1-7a55a0a1d5cc.png)

 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166242377-53ef06e6-66c4-4762-a3cc-f7dbed838f2a.png">
 </p>
 
#### Synthesis result
 
 ![image](https://user-images.githubusercontent.com/68154219/166238121-581cba51-66b4-4051-a7d9-38e48be6cf58.png)

 ![image](https://user-images.githubusercontent.com/68154219/166238204-5f067673-0f59-4def-8064-c11939d90621.png)
 
 ![image](https://user-images.githubusercontent.com/68154219/166238320-e573bb62-444b-4982-8282-7f91ee3f3962.png)

 #### Graphical Representation
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166238640-7639de64-c164-480c-b711-8ff9317f3358.png">
 </p>
 
***
# Day 4: GLS, Blocking vs Non-Blocking Synthesis-Simulation Mismatch
*** 
## GLS, Synthesis-Simulation Mismatch and Blocking/Non-Blocking statements
 
### GLS Concepts And Flow Using Iverilog
 
#### *What was learned*

 * GLS stands for **Gate Level Simulation**
 * It means running the Testbench with Netlist as [Design Under Test](http://www.verifsudha.com/2016/07/22/dut-verification-engineer/) (DUT).
 * Netlist is logically same as RTL Code
 * We use GLS is verify the correctness of design after synthesis.
 * It ensures timing of the design is met. For this GLS needs to be run with delay annotation.
 
 #### GLS using iVerilog
 
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166262916-72e01a27-d005-43a8-9ff6-1edc2aef919e.png">
 </p>
 
### Synthesis and Simulation Mismatch
 
 It occurs due to
 
 * #### Missing Sensitivity List - 
        A Simulator works only when there is change in input signal. In the verilog code, the input variable written in always@(sensitivity) is the Missing sensitivity. Only after change in the  sensitivity variable, the output is loaded with the value.  
 * #### Blocking vs Non Blocking Assignments
 * #### Non Standard Verilog Coding
 
 ### Blocking And Non Blocking Statements In Verilog
 
 Always inside a block
 
 * '=' refer to **Blocking**
 * Blocking executes the statements in order it is written.
 * So the first statement is evaluated before second statement
 
 * '<=' refer to **Non Blocking**
 * Executes all the RHS when ALWAYS block is entered and assigns to RHS
 * Parallel execution
  
 ### Caveats with Blocking statements

 ![image](https://user-images.githubusercontent.com/68154219/166425883-16d90692-de1e-480d-903c-401ad925c610.png)

 * In the first case, q gets the value of q0, then q0 gets the value of input d. So, here there are 2 D flipflops executed in series. q has the previous value of q0.
 * In the second case, q0 gets the value of d, then q gers the value of q0. So, here only one D flipflop implemented as the q gets the current value of q0.

 ![image](https://user-images.githubusercontent.com/68154219/166427080-f5be6d03-122f-4674-bd36-5ee35ba074a1.png)

 ## Labs on GLS, Synthesis-Simulation Mismatch
  
  For gate level simulation 
  * Open the ternary_operator_mux.v,bad_mux.v, good_mux.v files
   
   <p align="center" width="100%">
   <img src="https://user-images.githubusercontent.com/68154219/166427746-aed997f5-ba7b-4e5a-ae02-92a9dc1655b3.png">
   </p>
    
  *  Simulation results
    
   RTL OUTPUT
    
   <p align="center" width="100%">
   <img src="https://user-images.githubusercontent.com/68154219/166432527-65001873-0932-4b0a-993c-b1a9a3527e82.png">
   </p>
    
   GLS OUTPUT
    
   <p align="center" width="100%">
   <img src="https://user-images.githubusercontent.com/68154219/166432798-75f08078-eca8-4bfb-9288-46bc05baa82e.png">
   </p>

    The design is within uut (unit under test). It shows wires alomg with other elements of the design
 
  * Synthesis Result
       
   ![image](https://user-images.githubusercontent.com/68154219/166429120-29f4b3ff-b0c5-4ff5-a422-c82e800666be.png)

   <p align="center" width="100%">
   <img src="https://user-images.githubusercontent.com/68154219/166429510-6412c67e-126a-4596-8045-bb08bd75e296.png">
   </p>
   
  ## Lab Synth Simulation Mismatch Blocking statement
   
  The RTL and GLS simulation waveform of bad_mux is  as shown below
    
  ![image](https://user-images.githubusercontent.com/68154219/166434098-f8f57be6-a41c-4a2a-a729-9ca3e2be7ff7.png)
  
   The upper waveforms are for RTL Simulation and lower is for GLS simulation
    
     

   
   


 
  
  
 
 
 
 
 
