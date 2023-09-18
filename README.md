# Physical_design_using_openlane

# Day 1
<details><summary>Installation of required software</summary>

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
Rise Transition : 2.25182 - 2.19362 = 0.0582 ns / 58.20ps
Fall Transitio : 4.10413 - 4.0631 = 0.04103ns/41.03ps
Cell Rise Delay : 2.21701 - 2.15989 = 0.057211ns/ 57.21ps 
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
## Pre-layout Timing Analysis And Importance Of Good Clock Tree

<details><summary>Timing Modelling Using Delay Tables</summary>
To ensure that the CMOS Inverter's A and Y ports, situated on the li1 layer, adhere to the port requirements, it's crucial to confirm that they are precisely located at the intersection of horizontal and vertical tracks. This verification can be accomplished by consulting the "tracks.info" file, which furnishes details regarding track spacing and orientation.

![Screenshot from 2023-09-16 23-53-08](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/313e60c7-c1a6-4a1a-962e-de4eea965b21)



To guarantee that the ports align precisely at the intersection point, it's necessary to synchronize the grid spacing in Magic (tkcon) with the X and Y values of the li1 layer. This grid-track alignment can be established using the following command:
```
grid 0.46um 0.34um 0.23um 0.17um
```
![Screenshot from 2023-09-16 23-47-14](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/5d239215-6bdc-43e9-a0f1-c0090ffe1d56)

# Creating port defination
After completing the layout, the next step involves generating an LEF (Library Exchange Format) file for the cell. During this process, it is crucial to configure properties and definitions for the cell's pins to assist the placer and router tools. In LEF files, a cell containing ports is represented as a macro cell, and these ports are defined as the declared PINs of the macro. The initial step in this procedure is to define the ports and ensure that the correct class and use attributes are set for each port in compliance with the standard format.

To effectively configure the ports, follow these steps in the Magic console:

 Load your design's .mag file, specifically the layout for the inverter.

 Navigate to the "Edit" menu and select "Text." This action will open a dialog box.

   In the dialog box, double-click on the letter 'S' located at the I/O labels on the layout.

   The text field will automatically populate with the correct string name and size for the port.

   To confirm the port definition, ensure that the "Port enable" checkbox is selected, indicating that it functions as a port. Additionally, ensure that the "Default" checkbox remains unchecked.

By following these steps, you can effectively define and configure ports in your layout, facilitating their recognition and utilization in the subsequent LEF file generation process.


![Screenshot from 2023-09-17 00-01-35](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/33e9d867-a92a-4c6d-806f-b183f56c8c2f)


# Standard cell LEF Generation
Before the extraction Of LEF file we have to define the function of each port using the following commands:
```
port A class input
port A use signal

port Y class output
port Y use signal

port VPWR class inout
port VPWR use power

port VGND class inout
port VGND use ground
```
Now to extract file  following commands is used:
```
lef write
```


![Screenshot from 2023-09-17 00-07-37](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/3d7a79f9-cd8c-4607-a535-cf1bcd169719)

# Integrating Custom cell in Openlane
we should copy the extracted LEF file to picorv32a source directory, and also sky130_fd_sc_hd_typical.lib file from vsdcelldesign/libs ditrectory

```
cp sky130_vsdinv.lef /home/shivangi/OpenLane/designs/picorv32a/src/
cp sky130_fd_sc_hd__* /home/shivangi/OpenLane/designs/picorv32a/src/
```
We have to modify config.tcl file also
```

# Design
set ::env(DESIGN_NAME) "picorv32a"

set ::env(VERILOG_FILES) "$::env(DESIGN_DIR)/src/picorv32a.v"

set ::env(CLOCK_PORT) "clk"
set ::env(CLOCK_NET) $::env(CLOCK_PORT)

set ::env(GLB_RESIZER_TIMING_OPTIMIZATIONS) {1}

set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]

set filename $::env(DESIGN_DIR)/$::env(PDK)_$::env(STD_CELL_LIBRARY)_config.tcl
if { [file exists $filename] == 1} {
	source $filename
}
```
To invoke OpenLANE and run synthesis with the new standard cell library, use the following commands:

```
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
```
![Screenshot from 2023-09-17 01-00-09](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/df24b1cf-a5b8-4a9b-8696-67387eac20ea)




# Introduction to delay table
Delay is a critical factor in chip design, significantly impacting various timing aspects. A cell's delay is influenced by factors like its size and threshold voltages, and it's often represented in the form of a timing table. Importantly, delay is not a fixed value; it varies based on factors such as input transitions and output loads.

Delay tables contain data related to input slew and load capacitance, associated with different buffer sizes. These tables serve as crucial timing models. When algorithms work with these tables, they calculate buffer delays by considering input slew and load capacitance. In cases where precise data is unavailable, interpolation techniques are used to ensure accurate timing analysis and maintain signal integrity.

![Screenshot from 2023-09-17 00-35-06](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/7e5c38a4-e012-4c8f-acb8-1e62f0bfd21b)

Now we will run placement

After placement we will see sky130_vsdinv is in the layout or not:
```
magic -T /home/shivangi/OpenLane/vsdstdcelldesign/libs/sky130A.tech lef read /home/shivangi/OpenLane/designs/picorv32a/runs/RUN_2023.09.18_08.10.36/tmp/merged.nom.lef def read /home/shivangi/OpenLane/designs/picorv32a/runs/RUN_2023.09.18_08.10.36/results/floorplan/picorv32.def

```

![Screenshot from 2023-09-18 16-53-07](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/35ca7b12-b121-44f2-8d3e-f4fbad1670bb)


</details>

<details><summary>Timing Analysis with ideal clocks using openSTA </summary>

## SETUP TIME AND HOLDTIME
In digital circuit design, "setup time" and "hold time" are critical timing parameters that play a pivotal role in determining when valid data needs to be stable concerning the clock signal. These parameters are especially significant in synchronous digital systems where data must be captured accurately on either the rising or falling edge of a clock signal.

Here's a breakdown of setup time and hold time:

- Setup Time (Tsu):
        Setup time specifies the minimum duration before the clock edge (rising or falling) at which the input data must remain stable and valid.
        In simpler terms, it's the time interval leading up to the clock edge during which the data input must stay unchanged to ensure correct data capture by the flip-flop or latch.
        If data changes too close to the clock edge, there may not be sufficient time for the flip-flop to correctly sample the data, potentially leading to errors.

- Hold Time (Th):
Hold time represents the minimum duration after the clock edge during which the input data must remain stable and valid.
It ensures that the data remains unchanged for a specified time following the clock edge to prevent data corruption.
 Data changes occurring too soon after the clock edge can result in a hold time violation, which can disrupt the circuit's reliability.

To sum it up, setup time and hold time are essential timing constraints that guarantee the accurate sampling of data by flip-flops and latches in digital circuits. Violating these constraints can lead to setup and hold time violations, potentially causing errors in the circuit's operation. Designers need to meticulously consider these parameters during the design and timing analysis phases to ensure the dependable and robust functioning of their digital systems.

## Clock jitter

Clock jitter, another critical consideration in digital circuit design, arises from various factors such as clock generator circuitry, noise, power supply fluctuations, and interference from nearby components. In terms of timing closure, accounting for jitter as a significant factor is essential, as it can significantly impact a circuit's performance.

Period jitter is a crucial metric used to assess the stability of a clock signal. It quantifies the variation between the actual cycle time of a clock signal and the ideal period over a substantial number of randomly selected cycles (usually around 10,000 cycles). Period jitter can be expressed either as the average deviation (RMS value) across these cycles or as the difference between the maximum and minimum deviations within the selected group, known as peak-to-peak period jitter. Evaluating period jitter is essential to ensure that the timing of a clock signal remains stable under various operating conditions.

Cycle-to-cycle jitter (C2C) measures the variation between two consecutive clock cycles within a randomly chosen set of cycles, typically around 10,000 cycles. Engineers often express C2C jitter as the maximum observed value within this group. This measurement helps capture high-frequency jitter variations that can impact a circuit's performance.

In the frequency domain, phase noise is a phenomenon associated with clock jitter. It represents rapid and short-lived random phase fluctuations within a waveform. Analyzing phase noise in the frequency domain offers valuable insights into the quality and stability of a clock signal. Engineers can convert phase noise data into jitter values suitable for digital design analysis.

Understanding and quantifying clock jitter, whether in terms of period jitter, cycle-to-cycle jitter, or phase noise, is essential for designing reliable digital circuits. By addressing the causes of jitter and incorporating appropriate design margins, engineers can ensure that their designs meet timing specifications and function reliably under various operating conditions.

</details>
<details><summary> Clock tree synthesis TritonCTS and signal integrity </summary> 
	
## Clock Tree Synthesis

Clock Tree Synthesis is a technique for distributing the clock equally among all sequential parts of a VLSI design. The purpose of Clock Tree Synthesis is to reduce skew and delay. Clock Tree Synthesis is provided the placement data as well as the clock tree limitations as input. Clock Tree Synthesis (CTS) is the technique of balancing the clock delay to all clock inputs by inserting buffers/inverters along the clock routes of an ASIC design. 

As a result, CTS is used to balance the skew and reduce insertion latency. Before Clock Tree Synthesis, all clock pins were driven by a single clock source. Clock tree synthesis includes both clock tree construction and clock tree balance.Clock tree inverters may be used to create a clock tree that maintains the correct transition (duty cycle), and clock tree buffers (CTB) can balance the clock tree to fulfill the skew and latency requirements.

The significance of Clock Tree Synthesis (CTS) can be summarized as follows:

- Synchronization in Digital ICs: In digital integrated circuits (ICs), clock signals play a fundamental role in synchronizing the operations of various components. They ensure that data is sampled or altered at precisely the right times, maintaining the integrity of digital operations.

  - Preventing Timing Violations: Properly synchronized clocks are crucial for avoiding setup and hold time violations. These violations can occur if data transitions are not adequately synchronized with clock edges. CTS helps establish precise timing relationships, minimizing the risk of such violations and ensuring correct data processing.

  - Managing Complex ICs: In the context of modern ICs, which can contain millions or even billions of transistors, efficient and reliable clock distribution becomes an increasingly daunting challenge. CTS plays a vital role in managing this complexity by optimizing how the clock signal is routed and distributed throughout the chip, ensuring that it reaches all parts of the design reliably and within specified timing constraints.
 ![Screenshot from 2023-09-17 00-46-12](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/9fa1c0be-caa3-472f-aa50-ed72c8998a7c)

CTS Buffering


![Screenshot from 2023-09-17 00-47-05](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/525652d1-0da6-49e7-b23d-03ddfdad40af)

Cross talk & Cross Net Shielding
- Crosstalk: Unwanted interference between adjacent signal traces or conductors, causing signal degradation or errors.

- Cross Net Shielding: Using shielding layers or materials to physically isolate and protect different signal nets from each other, reducing crosstalk and maintaining signal integrity.

![Screenshot from 2023-09-17 00-49-53](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/4199e949-2683-482e-9312-71c00baebf7b)



  ## LAB
  Commands to run clock tree synthesis

  ```
run_cts
write_verilog ./designs/picorv32a/picorv32a_cts.v

```
![Screenshot from 2023-09-17 18-43-42](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/9daf46fc-5838-422e-9e2f-f78f92ad65f9)


Since clock tree synthesis has not been performed yet, the analysis is with respect to ideal clocks and only setup time slack is taken into consideration. The slack value is the difference between data required time and data arrival time. The worst slack value must be greater than or equal to zero. If a negative slack is obtained, following steps may be followed:

- Change synthesis strategy, synthesis buffering and synthesis sizing values
- Review maximum fanout of cells and replace cells with high fanout



  Commands

 ```
openroad
read_lef <path of merge.nom.lef>
read_def <path of def>
write_db pico_cts.db
read_db pico_cts.db
read_verilog /home/shivangi/OpenLane/designs/picorv32a/runs/RUN_09-09_11-20/results/synthesis/picorv32a.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
read_sdc /home/shivangi/OpenLane/designs/picorv32a/src/my_base.sdc
set_propagated_clock (all_clocks)
report_checks -path_delay min_max -format full_clock_expanded -digits 4
```
![Screenshot from 2023-09-17 18-27-56](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/75577ece-be41-4a34-b742-fd3baef3a455)

![Screenshot from 2023-09-17 18-28-11](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/d8113da7-25af-4d94-89db-e4d2d1b0df8b)

![Screenshot from 2023-09-17 18-28-22](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/8b21e0e0-ab3b-4b7a-a22e-cdf289275ac5)

Commands to check clock buffers :
```
echo $::env(CTS_CLK_BUFFER_LIST)
set $::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]
echo $::env(CTS_CLK_BUFFER_LIST)
```

  
</details>

# Day 5

## Final steps for RTL2GDS using tritonRoute and openSTA

<details><summary> Final steps in RTL2GDS </summary>

## Maze Routing and Lee's algorithm

The maze-routing algorithm you are referring to, often used in the context of chip multiprocessors (CMPs) and grid-based mazes, is designed to efficiently find routes or paths between two locations while minimizing overhead. This algorithm is essential in the field of integrated circuit design and routing, where the goal is to connect various components on a chip with minimal resource utilization.
There are four steps of routing operations:
Global Routing:

- Establishes a high-level path for each net.
- Focuses on overall routing topology.
- Avoids obstacles and congestion areas.

Track Assignment:

- Divides routing area into tracks or channels.
-  Allocates tracks to specific nets.
-   Considers routing layer constraints.

Detail Routing:

- Determines precise routing paths for each net.
- Minimizes wirelength and avoids conflicts.
- Adheres to design rules and constraints.

Search and Repair:

- Identifies and resolves routing issues.
-   Handles design rule violations and congestion.
-   May require backtracking and iterative adjustments.


The Lee algorithm is a grid-based approach used for routing, particularly in chip design. It begins with designated source and target points and assigns labels to grid cells to find the shortest route between them, often favoring efficient L-shaped paths over zigzags. While valuable for global routing tasks, it can be time-consuming for complex designs with many pins. As a result, alternative algorithms have emerged to address scalability and specific routing challenges. The choice of routing method depends on the design's complexity and resource constraints.


![Screenshot from 2023-09-17 01-29-07](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/128bde00-109f-4e8a-83b7-04b9798ba070)

## DRC
Design Rule Checking (DRC) is a vital step in the physical design process, ensuring that a design adheres to manufacturing constraints dictated by the chosen process technology. Each technology comes with its specific set of rules, which become more numerous and intricate as manufacturing technology advances to smaller nodes. DRC verifies compliance with these predefined process rules provided by foundries, safeguarding against chip failures. It plays a critical role in defining a chip's quality. Key DRCs involve physical wire attributes like minimum width, spacing, and pitch, and they address issues like signal short violations by utilizing additional metal layers while rigorously checking vias, width, and spacing.

![Screenshot from 2023-09-17 01-37-27](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/33fd20f9-05d7-487a-a8c4-b903cae536f4)


</details>

<details><summary>Power distribution Network And Routing </summary>


A Power Delivery Network (PDN) serves as the foundational infrastructure for ensuring a consistent and reliable supply of electrical power to all components within an integrated circuit (IC) or chip. Establishing a well-designed PDN is critical to guarantee that every device on the chip receives the necessary voltage levels with minimal noise and voltage drops.

The initial step in PDN creation involves meticulous power grid planning. This encompasses determining the chip's overall power requirements, encompassing voltage levels (typically VDD and VSS, or ground) and the current demands of various functional blocks. Designers must also consider the topology of the power delivery network, including the arrangement of power rails, ground lines, power domains, and their interconnections.

To enhance voltage stability and reduce noise, strategically positioned decapacitors (decaps) serve as local energy reserves. They play a vital role in compensating for abrupt changes in current demand, particularly during switching events. The selection and placement of decaps are guided by expected load variations and voltage fluctuations in different chip regions.
 The following command is used to check the last stage the design ran:

 ```
echo $::env(CURRENT_DEF)
```
Now run the following command after the cts:

```
gen_pdn
```


![Screenshot from 2023-09-17 18-36-17](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/2e4cc1aa-1530-4c8b-ad58-530b30135fc5)

![Screenshot from 2023-09-17 18-38-50](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/66a102e5-eb79-4106-a856-89d2ff83791f)

After the Power Distribution Network (PDN) is generated, designers employ various analysis tools to simulate and validate its performance. These analyses encompass assessing voltage drop, IR (voltage drop due to resistance), electromigration, and other power-related concerns.
To enhance the PDN's performance, designers may employ optimization techniques like buffer insertion, voltage islands, and voltage scaling.
Once the chip's layout is finalized, designers initiate post-layout verification to ensure that the actual layout aligns with the PDN plan and design rules. Any disparities or problems discovered during this stage are addressed.
Once the PDN is successfully generated, verified, and all design rules are met, the chip design is deemed ready for "tape-out." This means that the final layout data is sent to a semiconductor foundry for fabrication, marking a significant milestone in the chip manufacturing process.

## ROUTING

Global Routing:
- Purpose: Global routing serves as the first step in the routing process, defining the primary pathways for interconnections.
- Objective: It aims to establish approximate wire locations and high-level connections between components.
- Scope: Global routing focuses on the overall layout, determining the general routing topology.
- Efficiency: It is typically faster and less detailed than detailed routing.
- Use Cases: Global routing is crucial for creating a rough layout of interconnections, aiding in initial floorplanning, and providing an overview of the chip's connectivity.

 Detailed Routing:
        
- Purpose: Detailed routing follows global routing and concentrates on the precise routing paths for individual nets.
- Objective: It involves the selection of exact wires and vias, ensuring a functional and manufacturable layout.
- Scope: Detailed routing deals with the fine-grained routing of each net, considering design constraints and manufacturing rules.
- Precision: It ensures the highest level of precision and adherence to all constraints, including minimum spacing, width, and metal layer utilization.
- Use Cases: Detailed routing is the final step in the physical design process, where each wire's exact path is determined to meet timing closure and comply with design specifications.
       
![Screenshot from 2023-09-17 01-44-23](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/2c1c7cc7-13ce-435b-acff-9506037b6296)

</details>
<details><summary>Triton Routing Features</summary>

## Features of TritonRoute:
	
- Honouring pre-processed route guides: TritonRoute respects pre-processed route guides, allowing designers to guide routing paths based on their specifications.

- Assumes each net satisfies inter-guide connectivity: The tool assumes that each net already adheres to inter-guide connectivity requirements, simplifying the routing process.

- Uses MILP-based panel routing scheme: TritonRoute employs a Mixed-Integer Linear Programming (MILP) approach for panel routing, which can provide optimal routing solutions.

- Intra-layer parallel and inter-layer sequential routing framework: TritonRoute utilizes a combination of intra-layer parallel routing and inter-layer sequential routing to efficiently navigate multiple layers in the chip design, optimizing routing paths across different metal layers.


## Pre-processed route guides:
TritonRoute's approach to pre-processed route guides involves several key actions:

- Initial Route Guide Analysis: The tool initially analyzes the directions specified in the preferred route guides. If it encounters non-directional guides, TritonRoute breaks them down into unit widths for routing clarity.

- Guide Splitting: When non-directional routing guides are identified, TritonRoute splits them into unit widths, making them more manageable for the routing process.

- Guide Merging: TritonRoute simplifies routing by merging guides that are orthogonal to the preferred guides, streamlining the routing path.

- Guide Bridging: In cases where guides run parallel to the preferred routing guides, TritonRoute introduces an additional layer to bridge them, ensuring efficient routing within the preprocessed guides.

- Inter Guide Connectivity: TritonRoute assumes that route guides for each net already satisfy inter-guide connectivity. This means guides should be on the same metal layer with touching guides or on neighboring metal layers with non-zero vertical overlap area (utilizing vias for connections). Additionally, each unconnected terminal (e.g., pins of standard cell instances) should have its pin shape overlapped by a routing guide, indicated by a black dot (pin) with a purple box (metal1 layer).

![Screenshot from 2023-09-17 02-05-09](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/9c61c677-f32f-460d-be02-d3b5c4ad0565)


## Inter guide connectivity and intra-inter layer routing:
Inter-Guide Connectivity:

1. Guides are considered connected if they:
        - Share the same metal layer.
        - Touch or intersect along their edges.
        - Exist on neighboring metal layers with a non-zero vertical overlap area (using vias for connections).

2. Intra-Inter Layer Routing:

    - This involves routing signals between different layers of the chip.
    - It ensures connections between different metal layers, often using vias.
![Screenshot from 2023-09-17 02-04-27](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/4fae9a86-9cf9-459e-a02d-6ce90ef69fb2)



## Handling connectivity:
In handling connectivity within Triton Detailed Route, the following components and concepts are essential:

Inputs:

- LEF File: Contains information about library elements, including standard cells and their characteristics.
- DEF File: Provides placement and location data for components in the chip.
- Preprocessed Route Guides: Guides that specify routing directions and paths.
- Constraint Files: These files include:
- Route Guide Honoring: Enforces adherence to preferred routing guides.
- Connectivity Constraints: Specify how components and guides should be interconnected.
- Design Rules: Define rules and constraints for the chip's physical design.

Access Point:

An "Access Point" is an on-grid metal point located on the route guide.Its purpose is to facilitate connections to lower-layer segments, upper-layer pins, or I/O ports.Access Points play a critical role in enabling routing between different layers and components.

Access Point Cluster:

An "Access Point Cluster" refers to a collection of all access points.These access points are derived from various sources, including lower-layer segments, upper-layer guides, pins, or I/O ports.Access Point Clusters help streamline and organize the connectivity options for routing between different layers and components.

These components and concepts are integral to Triton Detailed Route's ability to effectively handle and optimize connectivity in the chip design process. They ensure that routing solutions are both efficient in terms of wire length and via count while adhering to specified constraints and design rules.

![Screenshot from 2023-09-17 02-03-45](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/9f00702f-e17a-46a1-8435-010e98d8116e)


##  Topology Algorithm :

   ![Screenshot from 2023-09-17 02-03-07](https://github.com/Shivangi2207/Physical_design_using_openlane/assets/140998647/3ea7cc0a-fdf9-47bf-9577-29f0fb36135b)



</details>

## References
1. https://www.vsdiat.com
2. https://github.com/Devipriya1921/Physical_Design_Using_OpenLANE_Sky130
3. https://chat.openai.com
4. http://opencircuitdesign.com/magic/
5. https://github.com/nickson-jose/vsdstdcelldesign/
6. https://github.com/The-OpenROAD-Project/OpenLane
7. https://github.com/Pruthvi-Parate/Advanced_Physical_Design_Using_OpenLANE


