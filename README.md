# Physical_design_using_openlane

# Day 1
<details><summary>Installtion of required software</summary>

## OpenLANE

OpenLane is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, KLayout and a number of custom scripts for design exploration and optimization. It also provides a number of custom scripts for design exploration and optimization.
OpenLane abstracts the underlying open source utilities, and allows users to configure all their behavior with just a single configuration file.


<details>
<summary><strong>Installation of OpenLANE</strong></strong></summary> 

Prior to the installation of the OpenLane install the dependencies and packages using the command shown below :

```
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git
```

## Docker Installation

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world

sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot 


# Check for installation
sudo docker run hello-world
```

## Steps to install OpenLane, PDKs and Tools from github

```
git clone --depth 1 https://github.com/The-OpenROAD-Project/OpenLane.git
cd OpenLane/
make
make test
cd /home/shivangi/OpenLane/designs/ci
cp -r * ../
```
</details>

## OpenSTA


OpenSTA is a distributed software testing architecture designed around CORBA, it was originally developed to be commercial software by CYRANO. The current toolset has the capability of performing scripted HTTP and HTTPS heavy load tests with performance measurements from Win32 platforms. However, the architectural design means it could be capable of much more.


<details>
<summary><strong>Commands to install OpenSTA</strong></summary>

## Steps:
Prior to the installation of the OpenSTA install the dependencies using the command shown below :
```
sudo apt-get install cmake clang gcc tcl swig bison flex 
```

After installing the dependencies use the following command to install OpenSTA:

```
git clone https://github.com/The-OpenROAD-Project/OpenSTA.git
cd OpenSTA
mkdir build
cd build
cmake ..
make
sudo make install
```

  
</details>

## Magic
 

Magic is a venerable VLSI layout tool, written in the 1980's at Berkeley by John Ousterhout, now famous primarily for writing the scripting interpreter language Tcl. Due largely in part to its liberal Berkeley open-source license, magic has remained popular with universities and small companies. The open-source license has allowed VLSI engineers with a bent toward programming to implement clever ideas and help magic stay abreast of fabrication technology. However, it is the well thought-out core algorithms which lend to magic the greatest part of its popularity. Magic is widely cited as being the easiest tool to use for circuit layout, even for people who ultimately rely on commercial tools for their product design flow. 


<details>
<summary><strong> Commands to install Magic</strong></summary>  

```
sudo apt-get install m4
sudo apt-get install tcsh
sudo apt-get install csh
sudo apt-get install libx11-dev
sudo apt-get install tcl-dev tk-dev
sudo apt-get install libcairo2-dev
sudo apt-get install mesa-common-dev libglu1-mesa-dev
sudo apt-get install libncurses-dev
git clone https://github.com/RTimothyEdwards/magic
cd magic
./configure
make
sudo make install
```



</details>

</details>
<details><summary>Soc Design and OpenLANE </summary>
An Application-Specific Integrated Circuit (ASIC) typically consists of three main parts:
 
  - RTL Designs : RTL IPs offer several advantages. They boost productivity, help bring products to market faster, and make designs more reliable. By using RTL IPs, designers can tap into well-                       tested and optimized components, reducing the chances of errors. Plus, they promote the reuse of designs, allowing engineers to mix and match different blocks to create more
                  complex systems. In essence, RTL IPs are like a shortcut to building sophisticated digital circuits.

  
  - EDA Tools : Electronic Design Automation (EDA) tools are software applications used in the design and development of electronic systems, integrated circuits (ICs), and printed circuit boards
                (PCBs). These tools are instrumental in various stages of the design process, from conceptualization and simulation to physical layout and verification. 
    
  - PDK Data : Process Development Kit(PDK) is a collection of files and documentation that describe a specific semiconductor fabrication process. PDKs are provided by semiconductor foundries to their
               customers, typically integrated circuit designers, to enable them to design and simulate chips using the foundry's manufacturing process.

![Screenshot from 2023-09-10 16-46-24](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/64aa6808-2fd6-49f4-88ff-435a18294608)


# Simplified RTL to GSDII Flow

The flow involves of following steps:

- Synthesis : Synthesis is the process of transforming your HDL design into a gate-level netlist, given all the specified constraints and optimization settings. Logic synthesis is the process of
             translating and mapping RTL code written in HDL (such as Verilog or VHDL ) into technology specific gate level representation.

- Floorplanning : Floor Planning involves determining the location, shape, and size of modules in a way that one can avoid congestion. Floor Planning is a quintessential step which decides the
                  layout of the VLSI design. A well-optimized floor planning allows an ASIC design that has higher performance.

- Plcament : Placement is an essential step in physical design flow since it assigns exact locations for various circuit components within the chips core area.OpenLANE uses the detailed placement
             tool RePlAce for this purpose.

- Clock Tree Synthesis (CTS) : Clock Tree Synthesis refers to the process of dispersing the clock and balancing the load. Basically, the clock is delivered to all successive parts. The technique
                               of inserting buffers or inverters along the clock pathways of an ASIC design to achieve zero/minimum skew or balanced skew is known as CTS.

- Routing : The process of creating physical connections based on logical connectivity. Signal pins are connected by routing metal interconnects. Routed metal paths must meet timing, clock skew, max
           trans/cap requirements and also physical DRC requirements.

- Sign-Off GDS2 : Perform a final sign-off on the GDSII file to confirm that it meets all design and manufacturing requirements. This step ensures that the layout is ready for photomask generation
                  and foundry submission.

- GDSII Generation: Generate the GDSII file, which contains the final geometric data for all layers of the chip. This file is used in the fabrication process.

![Screenshot from 2023-09-10 16-49-54](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/90c5b65f-736d-4e32-887d-8ebb9ba368b3)


# OpenLane ASIC flow :
The OpenLANE flow utilizes tools mainly from the Open-ROAD, YosysHQ, and Open Circuit Design projects.

![Screenshot from 2023-09-12 19-00-18](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/96bb1517-adb2-4966-88a9-694f36b81188)


Fig. illustrates the basic default flow; this is what runs in the batch (non-interactive) mode. Most of the steps are configurable and custom flows can be created by the use of interactive scripts. The flow expects the design source HD files as an input as well as the desired PDK source files

- RTL Synthesis and STA : The design is synthesized into a gate-level netlist using yosys and static timing analysis is performed on the resulting netlist using OpenSTA

- Insertion of DFT structures : An open-source Design For Testability (DFT) toolchain, Fault [9], can optionally be used to modify the netlist, inserting scan chains and the necessary IO ports to
                                scan and test thedesign after fabrication.

- Physical Implementation : Advancing with the physical implementation, we note that most of the tools in this stage are used from within the Open- ROAD application in combination with other
                            tools, some of them are custom and based on the OpenDB infrastructure, while others are indpendent., OpenLANE supports two more use cases besides the default one in the                              OpenROAD application; one of them is fully custom I/O pin placement for ases where a user would prefer to have strict control over pin locations. The other custom mode,
                            which is particularly useful during SoC integration to achieve clean routing on the top- level is the so-called contextualized I/O placement; this mode automatically
                            places the I/O pins optimally according to the context of their instantiation at a higher level of hierarchy

- Post-routing Evaluation of Result : DRC and LVS are then performed using magic  and netgen . Antenna checking is performed by either OpenROAD’s ARC (Antenna Rule Checker) or using magic.
                                      Extraction of parasitics from the routed layout is then done using SPEF EXTRACTOR , followed by another round of static timing analysis to have more
                                      accurate timing reports that correspond to the actual physical layout

  # Steps for synthesis in OpenLane:

```
cd ~/OpenLane
make mount
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis

```

![Screenshot from 2023-09-12 19-17-45-1](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/4ba488f0-9c7c-4ee4-9350-d9b6493f18dd)

![Screenshot from 2023-09-12 19-17-56](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/e0844a7a-2117-41d1-a328-94cc683069aa)

After we run synthesis command, new folder named 'runs' will be created in the picorv32a directory where we find the simulation results, logs etc related to picorv32a synthesis. Netlist of picorv32 can be seen here-

```
cd /home/shivangi/OpenLane/designs/picorv32a/runs/RUN_2023.09.12_13.46.44/results/synthesis
gedit picorv32a.v
```
# Reports can be seen here
```

cd /home/shivangi/OpenLane/designs/picorv32a/runs/RUN_2023.09.12_13.46.44/reports/synthesis
gedit 1-synthesis.AREA_0.stat.rpt
```

# Synthesis report
```
61. Printing statistics.

=== picorv32 ===

   Number of wires:               9824
   Number of wire bits:          10206
   Number of public wires:        1512
   Number of public wire bits:    1894
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:              10104
     sky130_fd_sc_hd__a2111o_2       2
     sky130_fd_sc_hd__a211o_2      101
     sky130_fd_sc_hd__a211oi_2       4
     sky130_fd_sc_hd__a21bo_2       19
     sky130_fd_sc_hd__a21boi_2       7
     sky130_fd_sc_hd__a21o_2       414
     sky130_fd_sc_hd__a21oi_2      127
     sky130_fd_sc_hd__a221o_2       65
     sky130_fd_sc_hd__a221oi_2       1
     sky130_fd_sc_hd__a22o_2       197
     sky130_fd_sc_hd__a22oi_2        2
     sky130_fd_sc_hd__a2bb2o_2      16
     sky130_fd_sc_hd__a311o_2       38
     sky130_fd_sc_hd__a31o_2        90
     sky130_fd_sc_hd__a31oi_2       10
     sky130_fd_sc_hd__a32o_2        89
     sky130_fd_sc_hd__a41o_2         2
     sky130_fd_sc_hd__and2_2       283
     sky130_fd_sc_hd__and2b_2       32
     sky130_fd_sc_hd__and3_2        77
     sky130_fd_sc_hd__and3b_2       76
     sky130_fd_sc_hd__and4_2        46
     sky130_fd_sc_hd__and4b_2        6
     sky130_fd_sc_hd__and4bb_2       3
     sky130_fd_sc_hd__buf_1       2735
     sky130_fd_sc_hd__buf_2         16
     sky130_fd_sc_hd__conb_1       106
     sky130_fd_sc_hd__dfxtp_2     1596
     sky130_fd_sc_hd__inv_2         83
     sky130_fd_sc_hd__mux2_2      1817
     sky130_fd_sc_hd__mux4_2       323
     sky130_fd_sc_hd__nand2_2      250
     sky130_fd_sc_hd__nand2b_2       2
     sky130_fd_sc_hd__nand3_2       18
     sky130_fd_sc_hd__nand3b_2       3
     sky130_fd_sc_hd__nand4_2        2
     sky130_fd_sc_hd__nor2_2       185
     sky130_fd_sc_hd__nor3_2        11
     sky130_fd_sc_hd__nor3b_2        3
     sky130_fd_sc_hd__nor4_2         4
     sky130_fd_sc_hd__nor4b_2        3
     sky130_fd_sc_hd__o2111a_2       1
     sky130_fd_sc_hd__o211a_2      224
     sky130_fd_sc_hd__o211ai_2       6
     sky130_fd_sc_hd__o21a_2       154
     sky130_fd_sc_hd__o21ai_2       94
     sky130_fd_sc_hd__o21ba_2       15
     sky130_fd_sc_hd__o21bai_2       3
     sky130_fd_sc_hd__o221a_2       19
     sky130_fd_sc_hd__o221ai_2       1
     sky130_fd_sc_hd__o22a_2        26
     sky130_fd_sc_hd__o22ai_2        1
     sky130_fd_sc_hd__o2bb2a_2       7
     sky130_fd_sc_hd__o311a_2       31
     sky130_fd_sc_hd__o311ai_2       2
     sky130_fd_sc_hd__o31a_2        21
     sky130_fd_sc_hd__o31ai_2        2
     sky130_fd_sc_hd__o32a_2        14
     sky130_fd_sc_hd__o41a_2         1
     sky130_fd_sc_hd__or2_2        337
     sky130_fd_sc_hd__or2b_2        20
     sky130_fd_sc_hd__or3_2        102
     sky130_fd_sc_hd__or3b_2        17
     sky130_fd_sc_hd__or4_2         29
     sky130_fd_sc_hd__or4b_2         6
     sky130_fd_sc_hd__xnor2_2       78
     sky130_fd_sc_hd__xor2_2        29

   Chip area for module '\picorv32': 102957.494400

```

# Flop ratio
```
Flop ratio = (No.of D flipflops)/(Total no.of cells) =1596/10104 = 0.1579
```


</details>

## Day 2

## Good flooplan vs Bad Floorplan and introduction to library cells 
<details><summary>Chip floorplanning consideration </summary>
The two most important parameters are:

- Utilisation : Core utilization factor is defined as the ratio of the area of the design (area of the standard cells + area of the macro cells) to the core area.It is better to have a utilization
                Factor of 0.5 to 0.6 to accomodate any extra logic later on.
- Aspect Ratio : Aspect ratio will decide the size and shape of the chip. It is the ratio between horizontal routing resources to vertical routing resources (or) ratio of height and width. Aspect
                 ratio = width/height.Aspect ratio of 1 signifies that the die is of square shape and any other value other than 1 signifies that the die is rectangular shape.

```

Utilisation Factor =  Area occupied by netlist
                     __________________________
                         Total area of core
                         

Aspect Ratio =  Height
               ________
                Width
                
  ```

# Floor planning

Pre-placed Cells : Pre-placed cells (or pre-placed blocks) in ASIC (Application-Specific Integrated Circuit) design refer to predefined and fixed blocks of logic or circuitry that are manually 
                   placed in specific locations on the semiconductor chip's layout before the automated placement and routing process.These cells are placed manually by the chip designer or through                    automated tools. Since these IP's are placed before automated Placement and Routing, these are reffered to as Pre-placed cells.
  
  ![Screenshot from 2023-09-10 21-51-53](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/b03cf105-c40d-4c12-b9a1-7a6c3b48d55c)

                   
Decoupling capacitors: Pre-placed cells must then be surrounded with decoupling capacitors (decaps). The resistances and capacitances associated with long wire lengths can cause the power supply  
                       voltage to drop significantly before reaching the logic circuits.Their role is to decouple the circuit from power supply by supplying the necessary amount of current to the                          circuit. They pervent crosstalk and enable local communication.

![Screenshot from 2023-09-10 22-22-04](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/1caeeebb-5a49-4f2b-83bc-b44365ed4898)

Power Planning:Let us suppose that there are multiple macros in a chip and output changes from '1' to '0', then it discharged into ground line because of which we can see ground bumpp. Similarly  
              when it is charged from 0 to 1 we can see voltage drop in power supply.Hence to resolve this we can have multiple supply line for vdd as well as ground as shown below:


![Screenshot from 2023-09-10 22-28-53](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/c4702946-dc09-4e1d-b727-87cb66dc3295)

Pin Placement : The netlist defines connectivity between logic gates. The place between the core and die is utilised for placing pins. The connectivity information coded in either VHDL or Verilog                  is used to determine the position of I/O pads of various pins. The input, output and Clock pins are placed optimally such that there is less complication in routing or optimised                     delay.
![Screenshot from 2023-09-10 22-39-26](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/6defb5df-c7ce-4112-be5f-c90029002da8)

The Clock port are bigger than the normal I/O pins because of it's continuous use and larger area offers less resistance.
Final design:
![Screenshot from 2023-09-11 01-32-10](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/8d8c13a0-78ab-4664-b9f3-d5a945aa7cb9)





</details>
<details><summary>Library Binding and Placement</summary>

# To run the picorv32a floorplan in openLANE:
```
run_floorplan
```

To view the floorplan, Magic is invoked after moving to the results/floorplan directory:
![Screenshot from 2023-09-15 23-33-03](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/2514b799-694c-46e3-88fb-4cf1e1af9446)

To view the floorplan, Magic is invoked after moving to the results/floorplan directory:

```
magic  /home/shivangi/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.min.lef def read picorv32a.def 
```
![Screenshot from 2023-09-15 23-32-03](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/8afd4cec-b21a-4c0f-8b03-4d3b5e4efb09)


![Screenshot from 2023-09-15 23-08-12](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/614db525-5ff8-4215-ba78-6912f2071e92)

We can zoom into the magic layout pressing z key. 
the standard cell can be found at the bottom left corner.



  # Placement Optimization

  The next step in the OpenLANE ASIC flow is placement. The synthesized netlist is to be placed on the floorplan. Placement is perfomed in 2 stages:

  
  - Global placement, also known as initial placement or coarse placement, aims to establish a rough placement of logical elements (cells) on the chip's layout canvas. The primary objective of global placement is to get an approximate positioning of cells before fine-tuning them in the detailed placement stage.

  - Detailed placement, often referred to as legalization and optimization, is the stage where the rough placement from global placement is refined to meet specific design objectives and constraints more accurately.

Placement run on OpenLANE & view in Magic
```
run_placement
```
![Screenshot from 2023-09-15 23-23-01](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/e8130639-c689-4ce1-9aa9-fb5615b8c35b)

 The design can be viewed on magic within results/placement directory:

```
magic  /home/shivangi/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.min.lef def read picorv32a.def 
```
![Screenshot from 2023-09-15 23-22-46](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/bb736a34-5ca0-4e25-a464-323339fec16e)



</details>
<details><summary>Cell Design And Characterization Flow</summary>
The standard cell design process is like building a customized digital circuit. It involves several important steps, starting with what you need and ending with the final results you want to achieve.

![Screenshot from 2023-09-16 20-01-22](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/47b91240-e46a-4a14-9d24-66e498de03b8)



# Input:
![Screenshot from 2023-09-16 00-07-44](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/7f0631cc-d375-486e-bec2-579ea47a309a)
![Screenshot from 2023-09-16 00-17-33](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/555b0f07-ff23-4a2c-a4e6-a848d3a0384c)


 - PDKs :A Process Design Kit (PDK) is a library of basic photonic components generated by the foundry to give open access to their generic process for fabrication.
   
 - DRC & LVS Rules : DRC only verifies that the given layout satisfies the design rules provided by the fabrication unit. It does not ensure the functionality of layout. Because of this, idea of LVS is originated. As LVS performs comparison between 2 Netlist, it does not compare the functionalities of both the Netlist.
   
 - SPICE Models: A SPICE model is a text-description of a circuit component used by the SPICE Simulator to mathematically predict the behavior of that part under varying conditions.
   
 - Libraries: Standard cell libraries with pre-designed logic gates and flip-flops are crucial building blocks for the design.
   
 - User-Defined Specifications:     Design requirements and constraints set by the designer, such as performance targets, power budget, and functionality.

# Design step:
![Screenshot from 2023-09-16 00-23-47](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/3c1c6997-b170-4e27-b48b-f52d2f38e7e7)


- Circuit Design:The step of the design cycle which outputs the schematics of the integrated circuit.
  
- Layout design is the process of arranging visual and textual elements on-screen or on-paper in order to grab a reader's attention and communicate information in a visually appealing way
  
- Extraction of Parasitics: Extracting parasitic elements (such as capacitance and resistance) from the layout to refine the circuit's performance simulation.
  
- Characterization: Characterize the cells by measuring their performance under various conditions, such as different input vectors and operating voltages.

# Output:
- Circuit Description Language (CDL):A human-readable or machine-readable representation of the circuit, often used for simulation and documentation.
- Library Exchange Format (LEF):Library Exchange Format (LEF) is a specification for representing the physical layout of an integrated circuit in an ASCII format that defines the physical properties of standard cells and facilitates the integration of these cells into the chip's layout.
- GDSII : t is a binary file format representing planar geometric shapes, text labels, and other information about the layout in hierarchical form.
- Extracted SPICE Netlist (.cir):A netlist that includes parasitic elements extracted from the layout, used for more accurate electrical simulations.

- Timing, Noise, and Power .lib Files: Libraries containing information on the timing characteristics, noise margins, and power consumption of the designed cells, essential for further chip-level analysis and integration.

  ## Charactersation flow

  ![Screenshot from 2023-09-16 20-10-09](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/551a79c1-ed7b-45a3-bff1-65a05b2882e4)

A typical standard cell characterization flow includes the following steps:

  - Read in the models and tech files
  - Read extracted spice netlist
  - Recognise behaviour of the cell
  - Read the subcircuits
  - Attach power sources
  - Apply stimulus to characterization setup
  - Provide necessary output capacitance loads
  - Provide necessary simulation commands
  - GUNA Software Integration:Feed the data from steps 1 through 8 into the GUNA software.

Use the GUNA software to generate comprehensive models for the standard cell, including timing models (setup time, hold time, propagation delay), noise models (noise margins, sensitivity to noise), and power models (static power, dynamic power).
Verify and validate the generated models against simulation results to ensure accuracy and reliability.
Document the generated models and their characteristics for future use in ASIC design. - Create reports summarizing the characterization results and models.



  


</details>
<details><summary>Timing Characterization Parameters</summary>
  
#### Timing threshold definitions 
Timing defintion |	Value
-------------- | --------------
slew_low_rise_thr	| 20% value
slew_high_rise_thr | 80% value
slew_low_fall_thr |	20% value
slew_high_fall_thr |	80% value
in_rise_thr	| 50% value
in_fall_thr |	50% value
out_rise_thr |	50% value
out_fall_thr | 50% value

# Propagation Delay and Transistion Time
- Propagation Delay : the time difference between when the transitional input reaches 50% of its final value and when the output reaches 50% of its final value
![Screenshot from 2023-09-16 12-21-36](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/ebe956fd-4bcd-470b-8159-8f0b96198475)

- Transition Time : Transition time is known as time needed to a signal to rise from 10% to 90% or to fall from 90% to 10%. The former is called rise time and later is known as fall time

![Screenshot from 2023-09-16 12-20-57](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/ead56b17-a2e7-4612-a307-b914103941d0)

```
rise delay =  time(out_fall_thr) - time(in_rise_thr)

Propagation delay = time(out_thr) - time(in_thr)

Fall transition time: time(slew_high_fall_thr) - time(slew_low_fall_thr)

Rise transition time: time(slew_high_rise_thr) - time(slew_low_rise_thr)
```


</details>
</details>

## Day 3

## Design Library Cell using magic and ngspice
<details><summary>CMOS inverter ngspice simulations </summary>

##  Cmos Inverter

![Screenshot from 2023-09-16 21-22-38](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/c8be5e7a-6c73-4376-9402-72150ad6c293)


CMOS inverter definition is a device that is used to generate logic functions is known as CMOS inverter and is the essential component in all integrated circuits. A CMOS inverter is a FET (field effect transistor), composed of a metal gate that lies on top of oxygen's insulating layer on top of a semiconductor.

The input signal is applied to the gate terminals of both the NMOS and PMOS transistors. The output is taken from the connection point (the drain of NMOS and the source of PMOS) between these two transistors.
    When Vin is high and equal to VDD, the n-MOS transistor is ON while P-MOS is off. We get the following equivalent circuit where a direct path exists between Vout and the ground node, resulting in a steady-state value of 0V

  On the other hand, when the input voltage is 0V, n-MOS and p-MOS transistors are OFF and ON respectively. The following equivalent shows that a path exists between VDD and Vout, yielding a high output Voltage.

  

# Spice Deck

![Screenshot from 2023-09-16 15-08-32](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/8a76b1c6-85de-4119-994b-bd954b222dd8)

Spice deck for the above:

```
*** MODEL Descriptions ***

*** NETLIST Description ***

M1 out in vdd vdd pmos W=0.37u L=0.25u
M2 out in 0 0 nmos W=0.375u L=0.25u

cload out 0 10f

vdd vdd 0 2.5

Vin in 0 2.5

*** SIMULATION Commands ***

.op

.dc Vin 0 2.5 0.05

***.include tsmc_025um_model.mod ***
.LIB "tsmc_025um_model.mod" cmos_models
.end

```

Spice Simulation

![Screenshot from 2023-09-16 21-19-18](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/1dce89ff-01af-450f-a005-3ead96961b52)


Model File:
```
* SPICE 3f5 Level 8, Star-HSPICE Level 49, UTMOST Level 8

.lib cmos_models 
* DATE: Feb 23/01
* LOT: T0BM                  WAF: 07
* Temperature_parameters=Default
.MODEL nmos  NMOS (                                LEVEL   = 49
+VERSION = 3.1            TNOM    = 27             TOX     = 5.8E-9
+XJ      = 1E-7           NCH     = 2.3549E17      VTH0    = 0.3907535
+K1      = 0.4376003      K2      = 8.265151E-3    K3      = 4.214601E-3
+K3B     = -3.7220937     W0      = 2.517345E-6    NLX     = 2.310668E-7
+DVT0W   = 0              DVT1W   = 0              DVT2W   = 0
+DVT0    = 0.2411602      DVT1    = 0.3707226      DVT2    = -0.5
+U0      = 316.5922683    UA      = -9.89493E-10   UB      = 2.154013E-18
+UC      = 2.474632E-11   VSAT    = 1.254499E5     A0      = 1.2735648
+AGS     = 0.2428704      B0      = 2.579719E-8    B1      = -1E-7
+KETA    = 4.87168E-4     A1      = 0              A2      = 0.5196633
+RDSW    = 120            PRWG    = 0.5            PRWB    = -0.2
+WR      = 1              WINT    = 2.357855E-8    LINT    = 1.210018E-9
+DWG     = 2.292632E-9
+DWB     = -9.94921E-10   VOFF    = -0.1039771     NFACTOR = 1.3905578
+CIT     = 0              CDSC    = 2.4E-4         CDSCD   = 0
+CDSCB   = 0              ETA0    = 3.894977E-3    ETAB    = 7.800632E-4
+DSUB    = 0.0307944      PCLM    = 1.7312397      PDIBLC1 = 0.999135
+PDIBLC2 = 4.850036E-3    PDIBLCB = -0.0866866     DROUT   = 0.8612131
+PSCBE1  = 7.995844E10    PSCBE2  = 1.457011E-8    PVAG    = 0.0099984
+DELTA   = 0.01           RSH     = 5              MOBMOD  = 1
+PRT     = 0              UTE     = -1.5           KT1     = -0.11
+KT1L    = 0              KT2     = 0.022          UA1     = 4.31E-9
+UB1     = -7.61E-18      UC1     = -5.6E-11       AT      = 3.3E4
+WL      = 0              WLN     = 1              WW      = -1.22182E-16
+WWN     = 1.2127         WWL     = 0              LL      = 0
+LLN     = 1              LW      = 0              LWN     = 1
+LWL     = 0              CAPMOD  = 2              XPART   = 0.4
+CGDO    = 3.11E-10       CGSO    = 3.11E-10       CGBO    = 1E-12
+CJ      = 1.741905E-3    PB      = 0.9876681      MJ      = 0.4679558
+CJSW    = 3.653429E-10   PBSW    = 0.99           MJSW    = 0.2943558
+CF      = 0              PVTH0   = -0.01          PRDSW   = 0
+PK2     = 2.589681E-3    WKETA   = -1.866069E-3   LKETA   = -0.0166961      )
*
.MODEL pmos  PMOS (                                LEVEL   = 49
+VERSION = 3.1            TNOM    = 27             TOX     = 5.8E-9
+XJ      = 1E-7           NCH     = 4.1589E17      VTH0    = -0.583228
+K1      = 0.5999865      K2      = 6.150203E-3    K3      = 0
+K3B     = 3.6314079      W0      = 1E-6           NLX     = 1E-9
+DVT0W   = 0              DVT1W   = 0              DVT2W   = 0
+DVT0    = 2.8749516      DVT1    = 0.7488605      DVT2    = -0.0917408
+U0      = 136.076212     UA      = 2.023988E-9    UB      = 1E-21
+UC      = -9.26638E-11   VSAT    = 2E5            A0      = 0.951197
+AGS     = 0.20963        B0      = 1.345599E-6    B1      = 5E-6
+KETA    = 0.0114727      A1      = 3.851541E-4    A2      = 0.614676
+RDSW    = 1.496983E3     PRWG    = -0.0440632     PRWB    = -0.2945454
+WR      = 1              WINT    = 7.879211E-9    LINT    = 2.894523E-8
+DWG     = -1.112097E-8
+DWB     = 9.815716E-9    VOFF    = -0.1204623     NFACTOR = 1.2259401
+CIT     = 0              CDSC    = 2.4E-4         CDSCD   = 0
+CDSCB   = 0              ETA0    = 0.3325261      ETAB    = -0.0623452
+DSUB    = 0.9206875      PCLM    = 0.833903       PDIBLC1 = 9.948506E-4
+PDIBLC2 = 0.0191187      PDIBLCB = -1E-3          DROUT   = 0.9938581
+PSCBE1  = 2.887413E10    PSCBE2  = 8.325891E-9    PVAG    = 0.8478443
+DELTA   = 0.01           RSH     = 3.6            MOBMOD  = 1
+PRT     = 0              UTE     = -1.5           KT1     = -0.11
+KT1L    = 0              KT2     = 0.022          UA1     = 4.31E-9
+UB1     = -7.61E-18      UC1     = -5.6E-11       AT      = 3.3E4
+WL      = 0              WLN     = 1              WW      = 0
+WWN     = 1              WWL     = 0              LL      = 0
+LLN     = 1              LW      = 0              LWN     = 1
+LWL     = 0              CAPMOD  = 2              XPART   = 0.4
+CGDO    = 2.68E-10       CGSO    = 2.68E-10       CGBO    = 1E-12
+CJ      = 1.864957E-3    PB      = 0.976468       MJ      = 0.4614408
+CJSW    = 3.118281E-10   PBSW    = 0.6870843      MJSW    = 0.3021929
+CF      = 0              PVTH0   = 6.397941E-3    PRDSW   = 30.410214
+PK2     = 2.100359E-3    WKETA   = 5.428923E-3    LKETA   = -0.0111599      )
*
.endl
```
Commands to open ngspice and run the simulation:
```
ngspice
source Cmos.cir
```
To execute it:
```
run
setplot
display
```
we can set the dc plot
![Screenshot from 2023-09-16 21-26-07](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/e9e57607-c897-4943-ab66-d385b43c6c84)

# Switching threshold
The point where Vin = Vout (both PMOS and. NMOS in saturation since VDS = VGS) • If VM = VDD/2, then this implies symmetric rise/fall behavior for the CMOS gate.This specific threshold results in both the PMOS and NMOS transistors being in an active state, which can lead to the generation of a leakage current.
![Screenshot from 2023-09-16 16-14-04](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/919d9ff1-75d1-4a4e-bf30-4927591eadc0)

Below shown switching threshold representation where Wp/Lp and xWn/Ln relation and calculation shown:

![Screenshot from 2023-09-16 16-32-47](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/1d5fcf3b-3795-48e0-a4e2-215efd91f1c8)

Modified Cmos file:

```
*** MODEL Descriptions ***

*** NETLIST Description ***

M1 out in vdd vdd pmos W=0.375u L=0.25u
M2 out in 0 0 nmos W=0.375u L=0.25u

cload out 0 10f

vdd vdd 0 2.5

Vin in 0 0 pulse 0 2.5 0 10p 10p 1n 2n

*** SIMULATION Commands ***


.tran 10p 4n


***.include tsmc_025um_model.mod ***
.LIB "tsmc_025um_model.mod" cmos_models
.end
```




</details>
<details><summary>Inception Of Layout </summary>

## CMOS Fabrication

- Substrate Selection: Choose the chip's body or substrate material.The substrate is the foundational material upon which the entire IC will be built.

- Active Region Creation: Isolate active regions for transistors using SiO2 and Si3N4 layers, achieved through deposition, photolithography, and etching.

- N-Well and P-Well Formation: Use ion implantation with Boron for P-well and Phosphorous for N-well creation to form N-type and P-type regions.

- Gate Terminal Formation: Create NMOS and PMOS gate terminals through photolithography techniques.

- LDD (Lightly Doped Drain) Formation: Develop LDD regions with light doping to prevent the hot electron effect.

- Source and Drain Formation: Prepare source and drain regions with screen oxide, Arsenic implantation, and annealing.

- Local Interconnect Formation: Remove screen oxide using HF etching and deposit low-resistance Titanium (Ti) for contacts.

- Higher-Level Metal Formation: CMP for planarization followed by TiN and Tungsten deposition. Top SiN layer for chip protection.

Final representation of the fabrication process

![Screenshot from 2023-09-16 21-49-54](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/a037ca12-de94-4a86-a1c4-6aba1118c47a) 

# Inverter Standard cell Layout & SPICE extraction
To see the magic layout of the CMOS inverter we'll get the magic file from  [vsdstdcelldesign](https://github.com/nickson-jose/vsdstdcelldesign)  by cloning it within Openlane directory

```
git clone https://github.com/nickson-jose/vsdstdcelldesign
```
It will create a folder named vsdstdcelldesign in Openlane directory.
now we will view the sky130_inv.mag file using following command. Before that we have to make sure sky130A.tech file is also in the same directory.

```
magic -T sky130A.tech sky130_inv.mag &
```

![Screenshot from 2023-09-16 16-49-56](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/ef4db438-d222-485c-b812-8171dec8c913)

#  Identification of NMOS and PMOS:

![Screenshot from 2023-09-16 22-03-35](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/59d75f50-6718-42cb-81c4-4878838c21bd)

![Screenshot from 2023-09-16 22-03-52](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/22725ce5-962c-4a09-894e-e1252bd3e8cd)

# Connectivity of Source and Drain:

![Screenshot from 2023-09-16 22-05-38](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/1dee9189-f158-45fd-a7b5-2e957601358f)

- P-Diffusion and N-Diffusion Regions: Examine the layout to identify P-diffusion and N-diffusion regions in relation to the polysilicon layers. These regions represent the active areas of PMOS and NMOS transistors in the CMOS inverter.

- Drain and Source Connections: Ensure that the drains of both PMOS and NMOS transistors are connected to the output port (designated as Y), and the sources of both transistors are connected to the power supply VDD (often represented as VPWR).

- LEF (Library Exchange Format): LEF is a format used in electronic design automation (EDA) that provides information about cell boundaries, VDD and GND (ground) lines, pin placements, and other physical details of integrated circuit libraries. It does not contain information about the logic or functionality of the circuit and is often used to protect intellectual property (IP).

- SPICE Extraction: SPICE (Simulation Program with Integrated Circuit Emphasis) extraction is a process that involves extracting electrical parameters from a physical layout (such as .mag format) to create a SPICE netlist. This netlist is used for circuit simulation and analysis. 


## Steps To Create Standard Cell and Extract Spice Netlist
# Commands
```
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```

Following spice file is created:

![Screenshot from 2023-09-16 22-09-17](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/81e548a6-9d36-4b62-81cb-d220bce9a587)

```
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=10000u

.subckt sky130_inv A Y VPWR VGND
M1000 Y A VPWR VPWR pshort w=37 l=23
+  ad=1443 pd=152 as=1517 ps=156
M1001 Y A VGND VGND nshort w=35 l=23
+  ad=1435 pd=152 as=1365 ps=148
C0 A Y 0.05fF
C1 VPWR Y 0.11fF
C2 A VPWR 0.07fF
C3 Y 0 0.24fF
C4 VPWR 0 0.59fF
.ends
```




</details>

<details><summary>Sky130 Tech File Labs</summary>
After Extracting the spice netlist, modify the netlist by adding the mentioned below :

  ```
VDD VPWR 0 3.3V
VSS VGND 0 0
Va A VGND PUSLE(0V 3.3V 0 0.1ns 0.1 ns 2ns 4ns)
.tran 1n 20n
.control
run 
.endc
.end
```

After creating the "sky130_in.spice" file, it undergoes editing to incorporate the "pshort.lib" and "nshort.lib" libraries, specifically designed for PMOS and NMOS components. Additionally, the minimum grid size of the inverter is calculated based on the Magic layout and incorporated into the deck using the command ".option scale=0.01u". To maintain consistency, the model names within the MOSFET definitions are adjusted to "pshort.model.0" for PMOS and "nshort.model.0" for NMOS.

The final sky130A_inv.spice file modified to:

```
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=0.01u
.include ./libs/pshort.lib
.include ./libs/nshort.lib
//.subckt sky130_inv A Y VPWR VGND
M1000 Y A VPWR VPWR pshort_model.0 w=37 l=23 
+  ad=1.44n pd=0 as=1.51n ps=0.156m
M1001 Y A VGND VGND nshort_model.0 w=35 l=23 
+  ad=1.44n pd=0.152m as=1.37n ps=0.148m

VDD VPWR 0 3.3V
VSS VGND 0 0V
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns  2ns 4ns)

C0 A Y 0.05fF
C1 VPWR Y 0.11fF
C2 A VPWR 0.07fF
C3 Y 0 0.24fF
C4 VPWR 0 0.59fF
C5 VPWR VGND 0.781f
//.ends
.tran 1n 20n
.control
run 
.endc
.end
```

For simulation, ngspice is invoked in the terminal:

```
ngspice sky130_inv.spice
```
The output "y" is to be plotted with "time" and swept over the input "a":
```
plot y vs time a
```
![Screenshot from 2023-09-16 22-37-03](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/63ed3246-10b9-4069-84eb-78a9655c8919)

# Output Waveform:
![Screenshot from 2023-09-16 22-36-07](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/20e93ed7-e6d0-4332-acaf-997bb07d5d83)


Timing parameters for characterizing the inverter standard cell include:

- Rise Transition: This measures the time it takes for the output to transition from 20% of its maximum value to 80% of its maximum value.

- Fall Transition: This parameter quantifies the time it takes for the output to change from 80% of its maximum value to 20% of its maximum value.

- Cell Rise Delay: It's calculated as the time when the output reaches 50% of its rise from its minimum value minus the time when the input falls by 50%.

- Cell Fall Delay: This is determined as the time when the output drops to 50% of its fall from its maximum value minus the time when the input rises by 50%.

These parameters are crucial for understanding the timing behavior of the inverter standard cell in digital circuit design.

The above timing parameters can be computed by noting down various values from the ngspice waveform.


```
Rise Transition : 2.25421 - 2.18636 = 0.006785 ns / 67.85ps
Fall Transitio : 4.09605 - 4.05554 = 0.04051ns/40.51ps
Cell Rise Delay : 2.21701 - 2.14989 = 0.06689ns/66.89ps 
Cell Fall Delay : 4.07816 - 4.05011 = 0.02805ns/28.05ps 

```
# MAGIC DRC
 Commands to download the package from the web and extract it:

 ```
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
tar xfz drc_tests.tgz
```
Now, when we run the "met3.mag" file in Magic, we can observe an instance where a group of rules is not met in the Metal 1 layer. This failure could be due to issues with the patterning of the metal layer, including the presence of shorts or opens. These issues have the potential to disrupt electrical connections within an integrated circuit design.

```
magic -d XR met3.mag
```

![Screenshot from 2023-09-16 23-10-21](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/9bdfd8fb-ef8f-4e01-be13-82ce8759d531)

Commands to see metal cuts:
```
cif see VIA2

```
![Screenshot from 2023-09-16 23-12-30](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/867225be-b4eb-410a-ab1a-7ae1ea156bf5)

# Lab To Fix poly.9 error in SKY130 Tech File
Command to load poly file
```
load poly.mag
```
following screen will appear

![Screenshot from 2023-09-16 23-14-40](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/219600b1-73f9-4de0-8ebd-600732bfd0d8)

As we can see there are some error . Now to rectify it we need to make some adjustment in SKY130 technology file 

In line 
```
spacing npres *nsd 480 touching_illegal \
	"poly.resistor spacing to N-tap < %d (poly.9)"
```
![Screenshot from 2023-09-16 23-22-50](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/c307ac67-04c3-46fe-b308-a21bbe2704a7)

Change to
```
spacing npres allpolynonres 480 touching_illegal \
	"poly.resistor spacing to N-tap < %d (poly.9)"
```


also 
```
spacing xhrpoly,uhrpoly,xpc alldiff 480 touching_illegal \

	"xhrpoly/uhrpoly resistor spacing to diffusion < %d (poly.9)"
```
![Screenshot from 2023-09-16 23-24-25](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/7333f35e-c355-49b0-94c7-c67038628050)


change to
```
spacing xhrpoly,uhrpoly,xpc allpolynonres 480 touching_illegal \

	"xhrpoly/uhrpoly resistor spacing to diffusion < %d (poly.9)"

```
Modified layout
![Screenshot from 2023-09-16 23-28-02](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/01295c7c-a166-4fda-9acc-d15b51be9a7c)


</details>

# Day 4
# Pre-layout Timing Analysis And Importance Of Good Clock Tree
