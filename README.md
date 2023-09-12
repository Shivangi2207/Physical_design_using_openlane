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

<details><summary>Good flooplan vs Bad Floorplan and introduction to library cells </summary>
<details><summary>Chip floorplanning consideration </summary>
The two most important parameters are:

- Utilisation : Core utilization factor is defined as the ratio of the area of the design (area of the standard cells + area of the macro cells) to the core area.It is better to have a utilization
                Factor of 0.5 to 0.6 to accomodate any extra logic later on.
- Aspect Ratio : Aspect ratio will decide the size and shape of the chip. It is the ratio between horizontal routing resources to vertical routing resources (or) ratio of height and width. Aspect
                 ratio = width/height.Aspect ratio of 1 signifies that the die is of square shape and any other value other than 1 signifies that the die is rectangular shape.

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
<details></details>


</details>
