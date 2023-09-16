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

- Transition Time : Transition time is known as time needed to a signal to rise from 10% to 90% or to fall from 90% to 10%. The former is called rise time and later is known as fall time

```
rise delay =  time(out_fall_thr) - time(in_rise_thr)

Propagation delay = time(out_thr) - time(in_thr)

Fall transition time: time(slew_high_fall_thr) - time(slew_low_fall_thr)

Rise transition time: time(slew_high_rise_thr) - time(slew_low_rise_thr)
```


</details>
</details>

## Day 3

<details><summary>Design Library Cell using magic and ngspice</summary>
<details><summary>CMOS inverter ngspice simulations </summary>


</details>

</details>
