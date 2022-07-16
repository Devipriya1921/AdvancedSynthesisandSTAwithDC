# Advanced Synthesis and STA with DC

# Table of Contents
- [Introduction](#introduction)
- [Introduction to Logic Synthesis](#introduction-to-logic-synthesis)
  - [1.1. Introduction to DC](#11-introduction-to-dc)
  - [1.2. Invoking Basic DC Setup](#12-invoking-basic-dc-setup)
  - [1.3. Introduction to ddc gui with Design vision](#13-introduction-to-ddc-gui-with-design-vision)
  - [1.4. Labs using DC Synopsys DC Setup](#14-labs-using-dc-synopsys-dc-setup)
  - [1.5. TCL Scripting](#15-tcl-scripting)
 - [Basics to STA](#basics-of-sta)
   - [2.1. Introduction to STA](#21-introduction-to-sta)
   - [2.2. Timing dot Libs](#22-timing-dot-libs)
   - [2.3. Exploring dotLib](#23-exploring-dotlib)
- [Advanced Constraints](advanced-constraints)
  - [3.1. Clock Tree Modelling - Uncertainty](#31-clock-tree-modelling---uncertainty)
  - [3.2. Loading Design get_cells, get_ports, get_nets](#32-loading-design-get_cells-get_ports-get_nets)
  - [3.3. Loading Design get_pins, get_clocks, querying_clocks](#33--loading-design-get_pins-get_clocks-querying_clocks)
  - [3.4 Creating Clock Waveforms](#34-creating-clock-waveforms)
  - [3.5 Clock Network Modelling - Uncertainty, report_timing](#35-clock-network-modelling---uncertainty-report_timing)
  - [3.6 IO Delays](#36-io-delays)
  - [3.7 SDC generated_clk](#37-sdc-generated_clk)
  - [3.8 SDC vclk, max_latency, rise_fall IO Delays](#38-sdc-vclk-max_latency-rise_fall-io-delays)
- [Optimizations](#optimizations)
  - [4.1 Combinational  Optimizations](#41-combinational--optimizations)
  - [4.2 Sequential  Optimizations](#42-sequential--optimizations)
  - [4.3 Boundary Optimization](#43-boundary-optimization)
  - [4.4 Register Retiming](#44-register-retiming)
  - [4.5 Isolating output ports](#45-isolating-output-ports)
  - [4.6 Multicycle Paths](#46-multicycle-paths)
  - [4.7 Resource Sharing Optimizations](#47-resource-sharing-optimizations)
- [Quality Checks](#quality-checks)
  - [5.1 Report timing](#51-report-timing)
  - [5.2 Check_timing, Check_design, Set_max_capacitance, HFN](#52-check_timing-check_design-set_max_capacitance-hfn)
* [Acknowledgements](#acknowledgements)
* [Author](#author)

# Introduction

Design Compiler is an Advanced Synthesis Tool used by leading semiconductor companies across world. Design Compiler RTL synthesis solution enables users to meet 
today's design challenges with concurrent optimization of timing, area, power and test. Design Compiler includes innovative topographical technology that enables 
a predictable flow resulting in faster time to results.

Synthesis in VLSI is the process of converting your code (program) into a circuit. In terms of logic gates, synthesis is the process of translating an abstract design 
into a properly implemented chip. Synthesis of logic circuits plays a crucial role in optimizing the logic and achieving the targeted performance, area and power 
goals of an IC.

This workshop includes the following concepts:

- Design fundamentals.
- Setting up DC for synthesis.
- Understanding and Analyzing the STA reports.
- Understanding and writing the Synopsys Design Constraints [SDC].
- Analyzing the quality of netlist synthesized.

The Tools used are:

- Icarus Verilog : For Verilog Compilation, Simulation.
- GTKWave : For viewing the Simulation output.
- Synopsys Design Compiler : For Logic Synthesis
- SAED_PDK 28_32nm Technology Node

Refer : [Link](https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git) for more details

# Introduction to Logic Synthesis

### 1.1. Introduction to DC

* Design Compiler RTL synthesis solution enables users to meet today's design challenges with concurrent optimization of timing, area, power and test. Design Compiler 
includes innovative topographical technology that enables a predictable flow resulting in faster time to results. Topographical technology provides timing and area 
prediction within 10% of the results seen post-layout enabling designers to reduce costly iterations between synthesis and physical implementation. Design Compiler 
also includes a scalable infrastructure that delivers 2X faster runtime on quad-core platforms. 

* Design Compiler is the core of Synopsys' comprehensive RTL synthesis solution, including Power Compiler, DesignWare, PrimeTime and DFTMAX. Design Compiler NXT is 
also available and includes includes best-in-class quality-of-results, congestion prediction and alleviation capabilities, physical viewer, and floorplan exploration. 
Additionally Design Compiler NXT produces physical guidance to IC Compiler, place-and-route solution for tighter correlation to layout and faster placement runtime.

* The industry's most comprehensive synthesis solution:

![dc synthesis-1](https://user-images.githubusercontent.com/83152452/179349313-f7f0947d-61f8-4353-8698-0a9b5df1936e.png)

* Benefits:

1. Concurrent optimization of timing, area, power and test.
2. Results correlate within 10% of physical implementation.
3. Removes timing bottlenecks by creating fast critical paths.
4. Gate-to-gate optimization for smaller area on new or legacy designs while maintaining timing Quality of Results (QoR).
5. Cross-probing between RTL, schematic, and timing reports for fast debug.
6. Offers more flexibility for users to control optimization on specific areas of designs.
7. Enables higher efficiency with integrated static timing analysis, test synthesis and power synthesis.
8. Support for multi voltage and multi supply.
9. 2X faster runtime on quad-core compute servers.


### 1.2. Invoking Basic DC Setup

#### ENVIRONMENT SETUP

``` 
//Git Clone sky130RTLDesignAndSynthesisWorkshop. 
$ git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

```

**sky130RTLDesignAndSynthesisWorkshop** Directory has: My_Lib - which contains all the necessary library files; where lib has the standard cell libraries to be used 
in synthesis and verilog_model with all standard cell verilog models for the standard cells present in the lib. Ther verilog_files folder contains all the experiments 
for lab sessions including both verilog code and test bench codes.

#### Steps Include:

```
read_verilog <file_name>
```
```
link
```
```
compile_ultra (or) compile
```
```
report_power
report_timing
report_area
```

**read_verilog command:**

![lab-1-commands after read_verilog ](https://user-images.githubusercontent.com/83152452/179350836-bae852e8-e1a3-4e0b-953c-25cd66d1078f.png)

**Invoking DC Synthesis:**

![lab-1-invoking dc synthesis-read_verilog cmd](https://user-images.githubusercontent.com/83152452/179350840-3b093e9b-107e-44f0-8904-013aae933561.png)

**gedit:**

![lab-2-sh gedit lab1_net v   - cmd](https://user-images.githubusercontent.com/83152452/179350870-3daa96ad-b7b7-4e14-84c1-def83c71d547.png)

**set_target and set_library commands:**

![lab-4 set target lib and link lib along with echo cmd](https://user-images.githubusercontent.com/83152452/179350880-a59483e1-8b5e-48b5-9a74-e5ec84806101.png)




### 1.3. Introduction to ddc gui with Design vision

**Design Vision Invoking:**

![lab-9-design vision invoking ](https://user-images.githubusercontent.com/83152452/179351032-64f7e73a-9c54-4ee6-a140-b07fd36baa96.png)

**read_ddc command:**

![lab-11-design vision - read_ddc lab1 ddc - cmd](https://user-images.githubusercontent.com/83152452/179351039-187661d4-d793-4afa-940d-d0652997324d.png)

**Schematic View:**

![lab-12- schematic view - lab1_flop_with_en - read_ddc schematic view](https://user-images.githubusercontent.com/83152452/179351044-fb4d4e99-99c5-4d6d-bde6-6d3d0a880b30.png)

>_.lib file is a collection of logical modules which includes all basic logic gates. It may also contain different flavors of the same gate (2 input AND, 3 input AND –
slow, medium and fast version)._

#### Faster cells and Slower Cells

A cell delay in the digital logic circuit depends on the load of the circuit which here is Capacitance.

Faster the charging / discharging of the capacitance --> Lesser is the Cell Delay

Inorder to charge/discharge the capacitance faster, we use wider transistors that can source more current. This will help us reduce the cell delay but at the same 
time, wider transistors consumer more power and area. Similarly, using narrower transistors help in reduced area and power but the circuit will have a higher cell 
delay. Hence, we have to compromise on area and power if we are to design a circuit with low cell delay.

#### Constraints

A Constraint is a guidance file given to a synthesizer inorder to enable an optimum implementation of the logic circuit by selecting the appropriate flavour of cells 
(fast or slow).


### 1.4. Labs using DC Synopsys DC Setup

**Link commands:***

![lab-14- setting link_library to $target_library - commands](https://user-images.githubusercontent.com/83152452/179351244-92174344-c771-4c9d-9b5b-e8eac9242445.png)

**Setting the path to default:**

![lab-15- setting target library default -  synopsys_dc setup](https://user-images.githubusercontent.com/83152452/179351248-a6125ec6-dd12-4ee4-bdcc-c442b8127e2e.png)


### 1.5 TCL Scripting

![lab-16-tcl scripting commands-1](https://user-images.githubusercontent.com/83152452/179351194-a93a8b6d-fb54-44ba-bb9a-09987b489e54.png)



# Basics of STA

### 2.1. Introduction to STA

Static timing analysis (STA) is a method of validating the timing performance of a design by checking all possible paths for timing violations. STA breaks a design 
down into timing paths, calculates the signal propagation delay along each path, and checks for violations of timing constraints inside the design and at the 
input/output interface.

![1  Introduction to STA](https://user-images.githubusercontent.com/83152452/179352233-32dac8cb-55a5-45ff-977e-6171db00ff71.png)


### 2.2. Timing dot Libs

![lab-1-saed32hvt_tt0p85v25c lib - timing dot libs-1](https://user-images.githubusercontent.com/83152452/179352246-ef226027-3dc7-4fcf-b7a0-17e95cee1bc2.png)

![lab-1-saed32hvt_tt0p85v25c lib - timing dot libs-2](https://user-images.githubusercontent.com/83152452/179352255-df5937a1-3c67-465a-bf46-8aa26bbf6bb6.png)


### 2.3. Exploring dotLib 

![lab-2-saed32hvt_tt0p85v25c lib - exploring dot libs-1 - hold_falling](https://user-images.githubusercontent.com/83152452/179352260-f382c16d-09e3-429d-bc3d-a3c15539c143.png)

![lab-2-saed32hvt_tt0p85v25c lib - exploring dot libs-1- hold_rising](https://user-images.githubusercontent.com/83152452/179352268-3ceedaae-4317-4364-95cb-fe93092769df.png)

![lab-3-saed32hvt_tt0p85v25c lib - exploring dot libs-1- area and other constraints](https://user-images.githubusercontent.com/83152452/179352277-a4d3f9d7-982b-4b9d-a77c-ca36df0ae6ce.png)

![lab-3-saed32hvt_tt0p85v25c lib - exploring dot libs-1- cell](https://user-images.githubusercontent.com/83152452/179352297-ad210bd9-f55c-46a8-980f-5ed91561e67a.png)

![lab-3-saed32hvt_tt0p85v25c lib - exploring dot libs-1- receiver capacitance and pin(clk)](https://user-images.githubusercontent.com/83152452/179352301-123f946b-7ea8-4977-9cec-aa805934fbef.png)


# Advanced Constraints

**Clock**
A digital clock is a repeating digital waveform used to step a digital circuit through a sequence of states. A clock signal oscillates between a high and a low state 
and is used like a metronome to coordinate actions of digital circuits. A clock signal is produced by a clock generator. Digital circuits rely on clock signals to know when and how to execute 
the functions that are programmed. If the clock in a design is like the heart of an animal, then clock signals are the heartbeats that keep the system in motion.
In serial communication of digital data, clock recovery is the process of extracting timing information from a serial data stream to allow the receiving circuit to decode 
the transmitted symbols. This is one method of performing a process commonly known as clock and data recovery.

![advanced constraints - intro - 1](https://user-images.githubusercontent.com/83152452/179352587-e98ae362-31c2-448f-99ea-259a98162e8e.png)

### 3.1. Clock Tree Modelling - Uncertainty

Clock uncertainty is the difference between the arrivals of clocks at registers in one clock domain or between domains. it can be classified as static and dynamic 
clock uncertainties. Pre-layout and Post-layout Uncertainty. Pre CTS uncertainty is clock skew, clock Jitter and margin. After CTS skew is calculated from the actual 
propagated value of the clock.

Static clock uncertainty: it does not vary or varies very slowly with time. Process variation induced clock uncertainty. An example of this is clock skew.

Timing Uncertainty of clock period is set by the command set_clock_uncertainty at the synthesis stage to reserve some part of the clock period for uncertain factors 
(like skew, jitter, OCV, CROSS TALK, MARGIN or any other pessimism) which will occur in PNR stage. The uncertainty can be used to model various factors that can 
reduce the clock period. It can define for both setup and hold.

Skew: This phenomenon in synchronous circuits. The Difference in arrival of clock at two consecutive pins of a sequential element.

max insertion delay: delay of the clock signal takes to propagate to the farthest leaf cell in the design.

min insertion delay: delay of the clock signal takes to propagate to the nearest leaf cell in the design.

Latency: The delay difference from the clock generation point to the clock endpoints.

Source latency: Source latency is also called insertion delay. The delay from the clock source to the clock definition points. Source latency could represent either on-chip or off-chip latency.

Network latency: The delay from the clock definition points (create_clock) to the flip-flop clock pins.

Jitter: Jitter is the short term variations of a signal with respect to its ideal position in time. It is the variation of the clock period from edge to edge.it can vary +/- jitter value. From cycle to cycle the period and duty cycle can change slightly due to the clock generation circuitry. This can be modeled by adding uncertainty regions around the rising and falling edge of the clock waveform.

Sources of jitter:

Internal circuitry of the PLL.
Thermal noise in crystal oscillators.
Transmitters and receivers of resonating devices.

![advanced constraints - intro - 2](https://user-images.githubusercontent.com/83152452/179352591-6e00e06d-3ebe-40f0-bca3-702d073ebfbc.png)

### 3.2 Loading design get_cells, get_ports, get_nets

**get_nets**

![lab-6-get_nets   all_connected N1 commands](https://user-images.githubusercontent.com/83152452/179353339-c74dae66-32f4-40bd-a3c2-132e18df41f1.png)

**get_cells**

![lab-4-get_cells commands ](https://user-images.githubusercontent.com/83152452/179353343-4bf02e07-0812-43cf-8a41-2629ee5a7e07.png)

**get_ports**

![lab-3- get_ports commands](https://user-images.githubusercontent.com/83152452/179353347-659a126d-e0dd-4739-9449-72d56b3fee2f.png)


### 3.3  Loading Design get_pins, get_clocks, querying_clocks

**get_pins**

![lab-7-get_pins commands](https://user-images.githubusercontent.com/83152452/179353414-bec36c97-a865-4189-9bdf-873052f29a7b.png)

**list_attributes**

![lab-8- list_attributes - clock](https://user-images.githubusercontent.com/83152452/179353439-eedfb87a-a616-40c9-b96a-d52a9ac4ecda.png)

![lab-8- list_attributes ](https://user-images.githubusercontent.com/83152452/179353556-aed80f55-ec53-42b9-8f15-25455dca087e.png)


### 3.4 Creating Clock Waveforms

![lab-9 - clock waveforms-1](https://user-images.githubusercontent.com/83152452/179353451-67c02da9-2b8f-4842-89fd-b81a0cb15b86.png)

![lab-9 - clock waveforms-2- query_clock_pin tcl](https://user-images.githubusercontent.com/83152452/179353452-f526e44f-4dfa-439c-8bb2-90d46fe957e0.png)


**25% Duty cycle Clock**

![lab-9 - clock waveforms-3- 25 percentage duty cycle clock waveform ](https://user-images.githubusercontent.com/83152452/179353618-0e8e04b9-e302-4f88-bfa5-581deb1fc7fe.png)


### 3.5 Clock Network Modelling - Uncertainty, report_timing

**The process includes:**

![lab-10-set_clock_latency commands](https://user-images.githubusercontent.com/83152452/179353754-44f4154a-ca87-418f-b749-abbbefa26b79.png)

![lab-11-report_timing -to REGC_reg D](https://user-images.githubusercontent.com/83152452/179353766-253884d6-d375-47d2-8bf4-bf9f8daeeac7.png)

![lab-12-set_clock_latency commands -2](https://user-images.githubusercontent.com/83152452/179353760-00c62ebf-3fcc-4fd0-9484-011227f008f1.png)

![lab-13-report_timing -to REGC_reg D -2](https://user-images.githubusercontent.com/83152452/179353774-917c7c86-7866-49b7-9586-b9ccdb3cc941.png)

**Commands:**

```
$  report_timing -to REGC_reg/D -delay min
```

```
$ report_timing -to REGC_reg/D -delay max
```


### 3.6 IO Delays

**Commands:**

```
$ set_input_delay -max 5 -clock [get_clocks myclk] [get_ports IN_A]
$set_input_delay -max 5 -clock [get_clocks myclk] [get_ports IN_B]
$report_port -verbose
```

```
$ set_input_delay -min 1 -clock [get_clocks myclk] [get_ports IN_A]
$set_input_delay -min 1 -clock [get_clocks myclk] [get_ports IN_B]
$report_timing -from IN_A -trans -nosplit
```

```
$ set_input_delay -max 5 -clock [get_clocks myclk] [get_ports OUT_Y]
$set_input_delay -min 1 -clock [get_clocks myclk] [get_ports OUT_Y]
$report_timing -from OUT_Y -trans -nosplit
```

```
$ set_load -max 0.4 [get_ports OUT_Y]
$report_timing -to OUT_Y -cap -trans -nosplit
```

```
$ set_load -min 0.1 [get_ports OUT_Y]
$report_timing -to OUT_Y -cap -trans -nosplit -delay min
```

![lab-14-report_port -verbose](https://user-images.githubusercontent.com/83152452/179354043-69644a35-a2f5-48bf-b651-18f5cd886fc1.png)

![lab-14-report_port -verbose-1](https://user-images.githubusercontent.com/83152452/179354047-97177f23-d82a-4c84-a214-ed095c8f992e.png)

![lab-15- commands - set_input_delay changes -1](https://user-images.githubusercontent.com/83152452/179354051-ecd86a2e-2803-4537-bbf9-53d80beede81.png)

![lab-15- report_port_verbose after changes - set_input_delay changes -1](https://user-images.githubusercontent.com/83152452/179354054-9cddd6d4-57cc-46c2-ac73-93185069ea35.png)

![lab-16- commands for report_timing - gedit a command ](https://user-images.githubusercontent.com/83152452/179354083-cdf09175-4c7e-4809-81b4-fe8ae76314a3.png)

![lab-16-report_timing - gedit a command -1](https://user-images.githubusercontent.com/83152452/179354090-c97030e1-5d20-4cdd-905b-c65837ccf66e.png)

![lab-17- gedit file after changes - set_input_transition commands - report_timing changes commands](https://user-images.githubusercontent.com/83152452/179354103-81363c30-4f27-4b96-a464-3614645e916b.png)

![lab-17- set_input_transition commands - report_timing changes commands](https://user-images.githubusercontent.com/83152452/179354106-9ef18abf-0c6d-4093-9391-5e98f56330ca.png)

![lab-18-set_out_load commands with gedit](https://user-images.githubusercontent.com/83152452/179354121-7345468b-fa16-4376-b4fd-674d3919cabd.png)

![lab-18-set_output_delay -- 1](https://user-images.githubusercontent.com/83152452/179354131-9e51c5d2-74e3-4e1d-adb9-184604f68f30.png)

![lab-18-set_output_delay with capacitance and trans-- 1](https://user-images.githubusercontent.com/83152452/179354139-4fd46d99-edc4-4d31-bb5f-4ce6e1d3f485.png)


### 3.7 SDC generated_clk

**Commands:**

```
$ create_generated_clock -source reference_pin [-divide_by divide_factor] [-multiply_by multiply_factor] [-invert] source
```

![image](https://user-images.githubusercontent.com/55539862/177484874-a14a1b77-ae5d-4d1b-bb5e-04108a4dcc79.png)

Creates a generated clock in the current design at a declared source by defining its frequency with respect to the frequency at the reference pin. 
The static timing analysis tool uses this information to compute and propagate its waveform across the clock network to the clock pins of all sequential elements 
driven by this source. 

The generated clock information is also used to compute the slacks in the specified clock domain that drive optimization tools such as place-and-route.

```
$ create_generated_clock -name MYGEN_CLK -master myclk -source [get_ports clk] -div 1 [get_ports out_clk]
$report_clocks *
```

![lab-19-MYGEN_CLK commands-create gen command - 1](https://user-images.githubusercontent.com/83152452/179354228-0a8431ec-8253-41ef-9d0d-1d4137b173a3.png)

![lab-19-MYGEN_CLK commands-create gen command - 2](https://user-images.githubusercontent.com/83152452/179354231-7470e227-fe3e-4ae3-826b-bea193019d97.png)


### 3.8 SDC vclk, max_latency, rise_fall IO Delays

**Virtual clock - purpose and timing**

A virtual clock is used as a reference to constrain the interface pins by relating the arrivals at input/output ports with respect to it with the help of input and 
output delays.

![image](https://user-images.githubusercontent.com/55539862/177493447-df6fbcb2-7fe7-48e3-9f6b-a55004eeb7ca.png)

```
$ create_clock –name VCLK –period 10
```

**Set driving cells**

It specifies the drive characteristics of input or inout ports that are driven by the cells in the technology library. These commands associate a library pin with input ports so that delay calculation can be accurately modelled.

**VCLK**

```
$ create_clock -name MYCLK -per 10
```

![lab-20- lab8_circuit_modified file - 1](https://user-images.githubusercontent.com/83152452/179355266-63ddedce-f212-4d5f-9bd1-5948ed66dff5.png)

![lab-20- lab8_circuit_modified file - 2](https://user-images.githubusercontent.com/83152452/179355267-3af8a115-f009-489e-8bb5-ec8373773d4a.png)

![lab-20- lab8_circuit_modified file - report_clocks command - 1](https://user-images.githubusercontent.com/83152452/179355272-db9f47ea-c47c-4c03-a130-3a0a4d57a81c.png)

![lab-20- lab8_circuit_modified file - report_port -verbose command - 1](https://user-images.githubusercontent.com/83152452/179355276-a2b515bb-8bf7-4980-92ee-47390c9ad323.png)

![lab-20- lab8_circuit_modified file - report_port -verbose command - 2](https://user-images.githubusercontent.com/83152452/179355279-9adbe224-8b85-4b6a-b259-620816aae630.png)

![lab-21- gedit lab14_circuit file](https://user-images.githubusercontent.com/83152452/179355290-1d6de422-1fb7-4e24-bc8a-3a6ebe3167d7.png)

![lab-22- read_ddc lab14 ddc - 1](https://user-images.githubusercontent.com/83152452/179355293-0862735c-f05a-4f83-b7fb-f4f219cefacb.png)

![lab-23 - create virtual clock - report_clocks](https://user-images.githubusercontent.com/83152452/179355296-123065c2-29e2-438a-9697-a6e29d1f52b8.png)

![lab-23 - create virtual clock - report_timing -to OUT_Z](https://user-images.githubusercontent.com/83152452/179355301-82d15714-818c-4153-8c92-51de9b4ad13d.png)




# Optimizations

### 4.1 Combinational  Optimizations

The Combinational optimization phase transforms the logic-level description of the combinational logic to a gate-level netlist.

Combinational optimization includes:

- Technology-Independent Optimization: This optimization operates at the logic level. Design Compiler represents the gates as a set of Boolean logic equations.
- Mapping: During this process, Design Compiler selects components from the logic library to implement the logic structure.
- Technology-Specific Optimization: This optimization operates at the gate level.
  
![image](https://user-images.githubusercontent.com/55539862/177751853-221a5598-934b-416e-b49b-6d7051275603.png)

![lab1- combinational optimizations - opt_check- schematic view](https://user-images.githubusercontent.com/83152452/179355431-6a6017c4-6d21-4857-8c0d-1531a7c717ed.png)

![lab1- combinational optimizations - opt_check2- schematic view](https://user-images.githubusercontent.com/83152452/179355435-ae02cf79-a71f-41a2-9b2a-67df1e81be18.png)

![lab1- combinational optimizations - opt_check3- schematic view](https://user-images.githubusercontent.com/83152452/179355439-2c111d72-d5aa-414a-98bf-39e7914e1bd6.png)

![lab1- combinational optimizations - opt_check4- report_timing ](https://user-images.githubusercontent.com/83152452/179355440-39e1824e-44a2-49aa-93a0-f60330f2ec6a.png)

![lab1- combinational optimizations - opt_check4- schematic view](https://user-images.githubusercontent.com/83152452/179355445-8df85c5b-c8cd-4282-b11b-65c91bb9c7fd.png)

     
### 4.2 Sequential  Optimizations
  
Sequential optimization includes the initial optimization phase, which maps sequential cells to cells in the library, and the final optimization phase, where Design 
Compiler optimizes timing-critical sequential cells (cells on the critical path):

- Initial Sequential Optimization
- Final Sequential Optimization

![lab3- seq optimizations- gtkwave-dff_const3_vcd](https://user-images.githubusercontent.com/83152452/179355498-bbda3a79-a34a-4cce-b285-a726524f02bb.png)

![lab3- seq optimizations- report_area-dff_const1_v](https://user-images.githubusercontent.com/83152452/179355506-c849cafe-8739-4a11-b3e4-52c39c5592cb.png)

![lab3- seq optimizations- schematic view-dff_const1_vcd](https://user-images.githubusercontent.com/83152452/179355510-c28f075e-0bb3-4830-8744-6af470c70fff.png)

![lab3- seq optimizations- schematic view-dff_const3_v](https://user-images.githubusercontent.com/83152452/179355511-198c115f-8672-4101-8874-3dddbefa7ca9.png)

 
### 4.3 Boundary Optimization
  
Boundary optimization results fastest critical paths and smallest design.

Basically four optimizations collectively called as Boundary optimization in synthesis (with respect to DC):

a) Inversion pushing across hierarchy.
b) propagation of equal and opposite information
c) propagation of unconnected /undriven ports.
d) propagation of constants..

```
$ set_boundary_optimization u_im false
```

![lab-4-boundary optimization-report_area](https://user-images.githubusercontent.com/83152452/179355561-7e0c4e51-198d-45d1-9e8f-3cfd5a0c6d1a.png)

![lab-4-boundary optimization-report_power](https://user-images.githubusercontent.com/83152452/179355566-b31c4229-601a-4d53-afc7-722ca58ab3e2.png)

![lab-4-boundary optimization-schematic](https://user-images.githubusercontent.com/83152452/179355568-6161818a-1e1e-43f9-b082-571b3fe2493f.png) 
 
 
### 4.4 Register Retiming

Register retiming is a circuit optimization technique that moves registers forward or backward across combinational elements in a circuit. The aim of this procedure is
to shorten the clock cycle or reduce circuit area. There are two basic types of register retiming: Forward retiming and backward retiming.

![lab-5-register retiming - report_area - check_reg_retime](https://user-images.githubusercontent.com/83152452/179355619-2125caba-9878-4461-a32d-6e65aa09e736.png)

![lab-5-register retiming - report_power - check_reg_retime](https://user-images.githubusercontent.com/83152452/179355620-4d4feb95-1369-41b1-8b08-48f77b4447f2.png)

![lab-5-register retiming - schematic view- check_reg_retime](https://user-images.githubusercontent.com/83152452/179355626-7b941c6a-b96b-4afd-b8be-6610250fe6b6.png)

 
### 4.5 Isolating output ports
 
![lab-6-isolation output ports - slack time met - report_timing](https://user-images.githubusercontent.com/83152452/179355643-d0386ef9-2fb0-4ea5-8591-abcf5d6c0918.png)

![lab-6-isolation output ports - with buffers](https://user-images.githubusercontent.com/83152452/179355646-385edb6e-35b2-4619-8d93-58bcb3599905.png)

![lab-6-isolation output ports - without buffers](https://user-images.githubusercontent.com/83152452/179355650-32ec22be-0317-4bb9-b897-3dd0bfcf6b2a.png)
 
### 4.6 Multicycle Paths

A Multi-Cycle Path (MCP) is a flop-to-flop path, where the combinational logic delay in between the flops is permissible to take more than one clock cycle. Sometimes 
timing paths with large delays are designed such that they are permitted multiple cycles to propagate from source to destination.
 
```
$set_multicycle_path -setup 2 -to prod_reg[*]/D -from [all_inputs]

$set_multicycle_path -hold 1 -to prod_reg[*]/D -from [all_inputs]
```

![lab-7-mcp_check_v-schematic view](https://user-images.githubusercontent.com/83152452/179355695-b66daef0-2469-4b99-b5f8-5a6a60b6da20.png)

![lab-7-mcp_check_v-slack time met - report_timing](https://user-images.githubusercontent.com/83152452/179355699-b02fc21f-6ebb-4074-9dea-8266513609b1.png)


### 4.7 Resource Sharing Optimizations

![lab2- resource sharing optimizations - resource_sharing_mult_check- report_area](https://user-images.githubusercontent.com/83152452/179355832-5eea58cb-7f02-41d3-a57e-5401c60e8445.png)

![lab2- resource sharing optimizations - resource_sharing_mult_check- report_area-1](https://user-images.githubusercontent.com/83152452/179355835-47d7dd37-d2d1-4818-869b-bc3a90a04dbf.png)

![lab2- resource sharing optimizations - resource_sharing_mult_check- schematic view](https://user-images.githubusercontent.com/83152452/179355837-dcfdaa61-ad11-45af-ae95-766ed0f53a48.png)


# Quality Checks

To start the Physical design these are the files we get input from Synthesis:
- Netlist
- SDC

After receiving database from synthesis team and prior to place and route you can perform some sanity checks. To validate the quality of constraints read in the 
netlist and the sdc file in the primetime and perform check_timing and generate report which will giving inputs like the quality of database like:

how many of the flip flops are getting clocks, how many flops are constrained, how many ports are having constrained or whether there is any violation like that which 
will surely give some idea about the quality of the delivered database.

In order to understand the quality of the database interms of timing , generate timing reports and understand the quality of timing how good or how bad is the database
and how much you can optimize at the backend or at the placement and routing stages or what paths you cannot meet timing even during placement stages.

After analysing bit on the timing reports you can get some idea of what all areas you need to close pack during placement so that you can create regions.

Generate report_area and report_references -hier report in the designcompiler or synthesis stage to better understand the design hierarchy.


### 5.1 Report timing

The Report Timing command allows you to specify options for reporting the timing on any path or clock domain in the design.

In a hold timing report, the tool is checking whether the data is held long enough after the clock arrival at the clock port of the flop. i.e. if the data path is 
faster, the data at the flop edge can change earlier than the stipulated hold time.

Timing path is defined as the path between start point and end point where start point and end point is defined as follows: Start Point: All input ports or clock pins 
of a sequential element are considered as valid start point. End Point: All output port or D pin of sequential element is considered as End point.

The main goal of static timing analysis is to verify that despite these possible variations, all signals will arrive neither too early nor too late, and hence proper 
circuit operation can be assured. Since STA is capable of verifying every path, it can detect other problems like glitches, slow paths and clock skew.

![lab-1- lab8_circuit_modified-report_timing - slack met](https://user-images.githubusercontent.com/83152452/179356215-d74710f9-b895-4900-9a53-01eeba61c870.png)

![lab-1- lab8_circuit_modified-report_timing - slack met-2](https://user-images.githubusercontent.com/83152452/179356216-ff35611c-0026-4e45-b9db-79f925a29b6e.png)

![lab-1- lab8_circuit_modified-report_timing - slack met-3 - for delay min](https://user-images.githubusercontent.com/83152452/179356227-33d3086c-618a-446c-a533-ebe8f153af4d.png)



### 5.2 Check_timing, Check_design, Set_max_capacitance, HFN

**check_timing:** Checks the assertions and structure of the design for potential   timing violations.This   command is used to identify possible problems before generating timing or constraint reports. This command also prints which checks it performs. If a check reveals a violation,   the   command   also prints a message about the violation. By default, the message contains a summary of the violation. To   get   more information about violations, use the -verbose option.

**Check_design:** check_design checks the current design for consistency. The check_design command checks the internal representation of the current design for consistency, and issues error and warning messages as appropriate. 

**Set_max_capacitance:** Specifies a maximum capacitance on pins, ports or design. If maximum capacitance is set on a pin or port, the net connected to that pin 
or port is expected to have a total capacitance less than the specified capacitance_value. If specified on a design, the default maximum capacitance for that design is
set. Library cell pins also can have max_capacitance value specified.

**HFN:** High Fanout Net Synthesis (HFNS) is the process of buffering the High Fanout Nets to balance the load.

![lab-2- en_128- report_area](https://user-images.githubusercontent.com/83152452/179356229-245f51b0-49f2-4ecc-acb3-e26bb1e5feca.png)

![lab-2- en_128- report_power](https://user-images.githubusercontent.com/83152452/179356239-766664cf-ff3c-4e02-89db-c62774f79740.png)

![lab-2- en_128- schematic view](https://user-images.githubusercontent.com/83152452/179356242-7719b023-59d7-40f2-b8c1-bea2e8371a34.png)

![lab-2- feedthrough the design - 1](https://user-images.githubusercontent.com/83152452/179356251-1fd59c51-9053-4076-be78-34df0baefb3f.png)

![lab-2- mux_generate_128_1 - report_power](https://user-images.githubusercontent.com/83152452/179356255-574ce34f-8fe2-4b3e-953e-84ab7f65dcd3.png)

![lab-2- mux_generate_128_1 - report_timing - 1](https://user-images.githubusercontent.com/83152452/179356257-4f4a40cf-a7ae-44b2-98ca-80e20dbdbe9f.png)

![lab-2- mux_generate_128_1 - report_timing - 2](https://user-images.githubusercontent.com/83152452/179356259-7e5a8013-fb5e-4ab2-8ef9-a404c5d2ce90.png)

![lab-2- mux_generate_128_1 - schematic view - 2](https://user-images.githubusercontent.com/83152452/179356267-4f0f4c7c-0024-4a68-92fc-2a09d664e55d.png)

![lab-2- mux_generate_128_1 - schematic view](https://user-images.githubusercontent.com/83152452/179356269-046c6f79-75c3-4415-b4eb-5fc26865423c.png)


# Acknowledgements
- [Kunal Ghosh](https://github.com/kunalg123), Co-founder, VSD Corp. Pvt. Ltd.


## Author
- [A Devipriya](https://github.com/Devipriya1921), B.E (Electronics and Communication Engineering), Bangalore - adevipriya1900@gmail.com



