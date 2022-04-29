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
 ### Design and Testbench setup
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
 
 ### iVerilog based Simulation Flow
 
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
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/165779132-571f8736-442f-4241-ad91-6cea5867cecb.png">
 </p>
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
***
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
 
 ![image](https://user-images.githubusercontent.com/68154219/165987430-19c59679-2506-4cb3-b4c4-0fb0bfd8f68d.png)

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
 ![image](https://user-images.githubusercontent.com/68154219/165997897-ca7baa52-32ef-4d24-bf6b-c2e64af51b3b.png)

***
 ## Labs using Yosys and SKY130 [PDKs](https://www.synopsys.com/glossary/what-is-a-process-design-kit.html)
 
 #### Lab 3: Yosys 1 good mux Part 1
 #### * What was learnt
      * How to invoke Yosys
      * How to Synthesize the Design
 #### *Labs*
 * Invoke yosys
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166007116-fe97b3c2-0107-4297-b7d6-f7aba642cf0e.png">
 </p>

 * Read the liberty .lib file
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166008066-ed700735-418c-455a-ad68-3f487edc88cc.png">
 </p>
 
 * Read verilog design good_mux.v file
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166008559-8ce5ad80-eede-48e9-bf6b-e63afbb894ef.png">
 </p>
 
 * Synthesize the module good_mux
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166009446-c2ecbdb5-d21e-4e1a-a599-fb422a43c380.png">
 </p>
 
 * Generate netlist
 <p align="center" width="100%">
 <img src="https://user-images.githubusercontent.com/68154219/166013365-f2700774-ebb8-4e11-ac77-741da1739150.png">
 </p>
 
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
 
 * The graphical representation in above part consists of a 2 : 1 mux. 
 * It has 3 inputs i0, i1, sel loaded to mux through buffer.
 * The output y also has a buffer.

  #### Lab 3: Yosys 1 good mux Part 2
 
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
 * Opening .lib file sky130_fd_sc_hd__tt_025C_1v80
 
 ![image](https://user-images.githubusercontent.com/68154219/166068006-807e6379-272c-49df-bfbe-75d8139f8b0e.png)

 ![image](https://user-images.githubusercontent.com/68154219/166068151-ee5e99f1-2b4c-4b5b-af5a-e8f675395963.png)

 * Factors causing variation in fabrication are
  * Process
  * Voltage
  * Temperature
 In .lib file name  *sky130_fd_sc_hd__tt_025C_1v80*
 * *130* represents 130nm fabrication
 * *tt* represents typical process
 * *025C* represents 25 degrees celcius temperature
 * *1v80* represents 1.8 volt
 
 #### Cell Definitions
 
 ![image](https://user-images.githubusercontent.com/68154219/166070000-e2508230-e777-4cdc-b4ab-8df2b3e1392c.png)

 For Example, The first cell name "sky130_fd_sc_hd_a2111o_1"
 * Leakage power value is given
 * a2111o means it will and a0, a1 then or it with b1,c1,d1
