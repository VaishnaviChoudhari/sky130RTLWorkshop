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
    * [Introduction iverilog gtkwave Part 1](#Introduction-iverilog-gtkwave-Part-1)
    * [Introduction iverilog gtkwave Part2](#Introduction-iverilog-gtkwave-Part-2)
  * [Introduction to Yosys and Logic Synthesis](#Introduction-to-Yosys-and-Logic-Synthesis)
    * [Introduction to yosys](#Introduction-to-yosys)
    * [Introduction to Logic Synthesis Part 1](#Introduction-to-Logic-Synthesis-Part-1)
    * [Introduction to Logic Synthesis Part 2](#Introduction-to-Logic-Synthesis-Part-2)
  * [Labs using Yosys and Sky130 PDKs](#Labs-using-Yosys-and-Sky130-PDKs)
    * [Yosys 1 good mux Part 1](#Yosys-1-good-mux-Part-1)
    * [Yosys 1 good mux Part 2](#Yosys-1-good-mux-Part-2)
    * [Yosys 1 good mux Part 3](#Yosys-1-good-mux-Part-3)
  ***
  * [Day 2: Timing libs, hierarchical vs flat synthesis and efficient flop coding style](#Day-2-Timing-libs-hierarchical-vs-flat-synthesis-and-efficient-flop-coding-styles)
  * [Introduction to timing .libs](#Introduction-to-timing-libs)
    * [Introduction to dot lib Part 1](#Introduction-to-dot-lib-Part-1)
    * [Introduction to dot lib Part 2](#Introduction-to-dot-lib-Part-2)
    * [Introduction-to-dot-lib-Part-3](#Introduction-to-dot-lib-Part-3)
  * [Hierarchical vs Flat Synthesis](#Hierarchical-vs-Flat-Synthesis)
    * [Hier synthesis flat synthesis Part 1](#Hier-synthesis-flat-synthesis-Part-1)
    * [Hier synthesis flat synthesis Part 2](#Hier-synthesis-flat-synthesis-Part-2)
  * [Various Flop coding styles and optimisation](#Various-Flop-coding-styles-and-optimisation)
    * [Why Flops and Flop Coding style Part 1](#Why-Flops-and-Flop-Coding-style-Part-1)
    * [Why Flops and Flop Coding style Part 2](#Why-Flops-and-Flop-Coding-style-Part-2)
    * [Why Flops and Flop Coding style Part 3](#Why-Flops-and-Flop-Coding-style-Part-3)
    * [Why Flops and Flop Coding style Part 4](#Why-Flops-and-Flop-Coding-style-Part-4)
    * [Interesting optimisations part 1](#Interesting-optimisations-part-1)
    
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
#### Lab 1: Introduction to Lab
 * #### Setting up the environment for all the labs
<p align="center" width="100%">
<img src="https://user-images.githubusercontent.com/68154219/165773236-2c33f8ba-a254-4411-9943-631d0431d890.png">
</p>
 
 #### Lab 2: Introduction iverilog gtkwave Part 1
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
 [!image](https://user-images.githubusercontent.com/68154219/165806522-1e0132c0-6a1e-4954-af4c-9ca09eaf3758.png)
 
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

### Introduction to Logic Synthesis Part 1
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
 
 #### Lab 3: Yosys 1 good mux Part 1
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

 #### Lab 3: Yosys 1 good mux Part 2
 
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
 
 #### Lab 4: Introduction to dot Lib part 1
 
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
 
 #### Lab 4: Introduction to dot Lib part 2
 
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
  
 #### Lab 4: Introduction to dot Lib part 3
 
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
 
 #### Lab 5: Hier synthesis flat synthesis Part 1
 
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
 
      ![image](https://user-images.githubusercontent.com/68154219/166162288-bc2e41bb-f4fb-4d66-9fb5-049c5984d4d7.png)
      
      It is a OR realisation.
    
 #### Lab 5: Hier synthesis flat synthesis Part 2
 
 9. Flatten the netlist 
 
    ![image](https://user-images.githubusercontent.com/68154219/166163509-b2e6f589-b7f0-4d1e-83d0-7835320bbea6.png)
 
    ![image](https://user-images.githubusercontent.com/68154219/166163559-059eb1da-5fc2-4dd1-9623-e9b15bdd8ce5.png)
 
    On comparing the multiple_module_hier.v and  multiple_module_flat.v files, it can be observed that the flatten modules does not preserves the hierarchy.
 
    The multiple_module_hier module instantiates AND, input isolation cells within the respective submodules, whereas multiple_module_flat module directly instantiates the cells within its own module.
 
 10. Graphical representation of flattened multiple_module.v file 
  
     ![image](https://user-images.githubusercontent.com/68154219/166163880-927ee212-1b83-4cb0-8ea8-93ada559f465.png)
 
     The hierarchical graphical representation of multiple_module had sub_module1 and sub_module2, whereas the flattened representation has AND and Input Isolation cells.  

  * Sub module level synthesis can be done for replicating the synthesisof same submodule multiple times if the same module appears multiple times in the design. For eg. In multipler, same operations are executed multiple times.  
 
    ![image](https://user-images.githubusercontent.com/68154219/166164860-5f2c9e35-5531-46cc-99c6-e4912d983a87.png)
 
    ![image](https://user-images.githubusercontent.com/68154219/166164900-ccbe01bf-4d4b-4047-8d43-b2e8e8c45b26.png)
 
    ![image](https://user-images.githubusercontent.com/68154219/166164959-ca65f7db-95d8-41e8-8a4c-8f8765afa672.png)
 
    ![image](https://user-images.githubusercontent.com/68154219/166164980-d5e95784-f2a0-4ab2-b19f-f71d29570d43.png)

    ![image](https://user-images.githubusercontent.com/68154219/166164991-b3f1c1c8-f376-421d-a22f-a9f6638dabcc.png)



## Various Flop coding styles and optimisation
 
 ### Why Flops and Flop coding styles Part 1
 
 * ####Why Flip Flop
 
 In combinational circuits, propagation delay can cause glitch in the output as explained below
 
 For Boolean expression Y = (A & B) | C, for input (A, B, C) = {1, 1, 0}; Y should be 1. But propagation delay of 1 ns causes glitch of 1 ns
 
 ![image](https://user-images.githubusercontent.com/68154219/166165922-217c2c0e-d97b-44cd-a6fb-6227130979f6.png)
 
 * To avoid corruption of output due to glitches, flip flops can be used between two combinational circuits. 
 * Flipflops are triggered only on the edge of the clock applied.
 * So, eventhough the output of combinational circuit is glitching, output will be stable.
 * Flip Flops are initialied by SET/RESET which can be synchronous or asynchronous

 ### Why Flops and Flop coding styles Part 2
 
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
 
 ###  Lab flop synthesis simulations Part 1
 
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

    ###  Lab flop synthesis simulations Part 2
    
    3. Synthesizing all the 4 flipflop verilog files 
       * Follow the same steps as previously explained to Initialize yosys, write liberty, read verilog files and synthesis the design
       * use dfflibmap command used for synthesizing flipflops and then execute abc tool
      
       ![image](https://user-images.githubusercontent.com/68154219/166167988-c817ec12-68e8-4c5a-8086-3e7a4dc5725d.png)

       ![image](https://user-images.githubusercontent.com/68154219/166168013-8d054192-05e3-4c93-8d6a-4e61d57f3e7a.png)
 
       Graphical representation of all 4 flipflops post synthesis is as shown

       * **dff_asyncres**
       
         ![image](https://user-images.githubusercontent.com/68154219/166168079-8f2da380-e048-4352-a31c-ec0380dbbe7c.png)
   
         *sky130_fd_sc_hd_dfrtp_1* is D flipflop reset cell. Since it is a reset, it has inverting clock as input
 
       * **dff_async_set** 
    
         ![image](https://user-images.githubusercontent.com/68154219/166168337-18550979-e570-4682-b6d5-2c9c8e00d5fa.png)
 
       * **dff_asyncres_syncres**
 
        ![image](https://user-images.githubusercontent.com/68154219/166168794-8c347753-83af-478a-a934-bf5f5b5575e2.png)

       * **dff_syncres**
     
         ![image](https://user-images.githubusercontent.com/68154219/166168715-257807b6-b60b-479e-914b-e1d657d15f33.png)

           

         
         
         


 
 
 

 




 
 
 
  
 
 
 
 
 
