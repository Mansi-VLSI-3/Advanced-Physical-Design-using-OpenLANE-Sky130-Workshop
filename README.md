# Advanced-Physical-Design-using-OpenLANE-Sky130-Workshop

![image](https://user-images.githubusercontent.com/79994584/114469850-7ff0e880-9c0b-11eb-9a06-cac7f32717ec.png)


*7 April 2021 - 11 April 2021*

## Contents

### --> Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK

* How to talk to computers?
* SoC design and OpenLANE
* Get familiar to open-source EDA 


### --> Day 2 - Good floorplan vs bad floorplan and introduction to library cells

* Chip Floor planning considerations
* Library Binding and Placement
* Cell design and characterization flows
* General timing characterization parameters


### --> Day 3 - Design library cell using Magic Layout and ngspice characterization

* Labs for CMOS inverter ngspice simulations
* Inception of Layout – CMOS fabrication process
* Sky130 Tech File Labs

### --> Day 4 - Pre-layout timing analysis and importance of good clock tree

* Timing modelling using delay tables
* Timing analysis with ideal clocks using openSTA
* Clock tree synthesis TritonCTS and signal integrity
* Timing analysis with real clocks using openSTA

### --> Day 5 - Final steps for RTL2GDS using tritonRoute and openSTA

* Routing and design rule check (DRC)
* Power Distribution Network and routing
* TritonRoute Features


# Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK

![image](https://user-images.githubusercontent.com/79994584/114463005-76628300-9c01-11eb-9565-266cf5efafa0.png)


![image](https://user-images.githubusercontent.com/79994584/114463028-80848180-9c01-11eb-80ac-73a0c1b78ef1.png)


![image](https://user-images.githubusercontent.com/79994584/114463039-85e1cc00-9c01-11eb-9388-88660e684a84.png)


![image](https://user-images.githubusercontent.com/79994584/114463046-8aa68000-9c01-11eb-8db5-b12aee1e66c6.png)


![image](https://user-images.githubusercontent.com/79994584/114463068-8ed29d80-9c01-11eb-8572-45644a5416dc.png)


![image](https://user-images.githubusercontent.com/79994584/114463089-96924200-9c01-11eb-9f4a-fd3c0508797d.png)


![image](https://user-images.githubusercontent.com/79994584/114463097-9a25c900-9c01-11eb-9668-c93ff1acf268.png)


![image](https://user-images.githubusercontent.com/79994584/114463111-9f831380-9c01-11eb-8ef4-c895e7fa97fa.png)


![image](https://user-images.githubusercontent.com/79994584/114463125-a578f480-9c01-11eb-8c96-8f98ff173431.png)


![image](https://user-images.githubusercontent.com/79994584/114463136-aad63f00-9c01-11eb-8f97-bccd361ef638.png)


![image](https://user-images.githubusercontent.com/79994584/114463155-b0cc2000-9c01-11eb-80a3-be50cc6fa93b.png)


![image](https://user-images.githubusercontent.com/79994584/114463174-b75a9780-9c01-11eb-919a-5e9ab72fd822.png)


![image](https://user-images.githubusercontent.com/79994584/114463193-bde90f00-9c01-11eb-8f57-5c11211d4f2d.png)


![image](https://user-images.githubusercontent.com/79994584/114463219-c5101d00-9c01-11eb-9098-7dbd8837466d.png)


![image](https://user-images.githubusercontent.com/79994584/114463235-c93c3a80-9c01-11eb-8bb2-e332e353c77c.png)


From the synthesis reports, what is the buffer ratio:

![image](https://user-images.githubusercontent.com/79994584/114463376-fa1c6f80-9c01-11eb-972e-4a1e30543e09.png)


More information on OpenLANE can be found on : `https://github.com/efabless/openlane`


# Day 2 - Good floorplan vs bad floorplan and introduction to library cells

**1. Ultilization factor and aspect ratio**

The first step in physical design is to define the height and width of the core and die.

**Die** - It encapsulates the core and it is where the fundamental circuit gets fabricated.

**Core** - It lies inside the die wherein the fundamental logic of design is placed.

Inorder to maximize the throughput we have multiple dies on a single wafer.

***When Netlist occupies 100% of the core area, it means that the utilization of chip is 100%***

**Utilization factor - Area occupied by the Netlist / Total area of the core**

Ideally we do not aim at 100% utilization instead 60-70% utilization is what we look at so that there is room for optimization at later stages.

**Aspect Ratio = Height / Width**

***When the Aspect ratio is 1, it means that the chip is square shaped. While values of aspect ratio other than 1 represent that the chip is rectangular in shape.***

**2. Preplaced Cells**

These are cells which have user defined locations and therefore are already present in the chip before automated placement and routing. Automated placement and routing tools places the remaining logical cells in the design onto chip. Few examples of Pre-placed cells are memories, muxes, etc. They are placed in the core according to the design scenario. For example if the pre-placed cells have more number of inputs then they are placed close to the input side and if they have more number of outputs then they are placed close to the output side.

Arrangement of these IPs in a chip is referred to as Floorplanning.

**3. De-Coupling capacitors** 

When the logic 1 or 0 is not exactly 1 or 0 due to the drop in supply voltage , due to its physical location (far away). Hence if the input and output is within the Noise Margin then the situation is still finebut if it is not then it a huge problem so to fix this we use de - coupling capacitors which store alot of charge and are connected close to the logic circuit physically so that they provide the circuit full supply required and hence de - couples the logic circuit from the supply V, Vdd. When the logic is from 1 to 0, the de-coupling capacitor gets discharged through the Vdd. 
***Therefore de-coupling capacitors are placed close to the input cells.***

The de-coupling capacitors take care of local communication but for global communication **power planning** is used. 


**4. Power Planning** 

***The initial problem was that each circuitry demanded supply at the same time by the de-coupling capacitor.***

Hence all the de - coupling capacitors which where charged to V Volts will have to discharge to 0 through single Gnd, causing a bump in the ground tap point. This problem is known as **Ground Bounce**.

When the de -coupling Capacitors which were at 0 Volts are to be charged to V Volts through a single Vdd, causing lowering of Vdd tap point. This problem is known as **Voltage Droop**.


***So the solution to the above two problems is to use multiple Vdd and Vss***. 

This is called the **Mesh**, and is how the power planning will be done.


**5. Pin Placement**

A netlist gives the connectivity information between gates which is written in the HDL. The pin placement is the used space between die and core. 
The Frontend engineer defines the netlist connections while the backend engineer defines the pin placement. The clock ports are usually bigger in size in comparison to other data pins because the clock is continously drives all the flip flops in the design and to get minimum resistance as R is inversly proportional to A.

***Logical cell placement Block*** - It is placed in the core before the automated placement an routing tools place the other blocks.

**6. Cell Design Flow**

It comprises of three things.

1. Inputs - They include **PDK (Process Design Kit**) *which comes from the foundry* and consists of DRC and LVS rules, SPICE Models, library and user defined specs.
2. Circuit Design Steps -*Cell height* is decided by the distance betwee the power and ground rails, supply voltage, metal layers, pin locations, drawn gate length. 
3. Outputs

![image](https://user-images.githubusercontent.com/79994584/114463910-b118eb00-9c02-11eb-88d5-ced5104f7a2b.png)


![image](https://user-images.githubusercontent.com/79994584/114463930-b70ecc00-9c02-11eb-9d6f-f6eddb7540ae.png)


![image](https://user-images.githubusercontent.com/79994584/114463957-bf670700-9c02-11eb-8987-daac1eac71f4.png)


![image](https://user-images.githubusercontent.com/79994584/114463983-c8f06f00-9c02-11eb-8a7a-6bd8a7a05458.png)


`Which means that we already have a folder inside the runs directory and as the last line says that any changes in the design config will not apply`

Try and find what below command does:

`prep -design picorv32a -tag trial_run1 -overwrite`

*When we use the overwrite flag then whatever we have done in the trial_run1 folder will be erased and the new configuration defined in the designs/ config.tcl will be taken into consideration.*

![image](https://user-images.githubusercontent.com/79994584/114464211-15d44580-9c03-11eb-9d42-d1fe640b5ce6.png)


![image](https://user-images.githubusercontent.com/79994584/114464228-1bca2680-9c03-11eb-8424-720e55d166bc.png)


**To make changes instead of using the overwrite flag use the command**

`Set $env(CLOCK_PERIOD) <time_period>`

And the new  <time_period> will be now set

Since we used the overwrite  flag we will have to perform the synthesis again so; 

repeat --> `Run_synthesis`

![image](https://user-images.githubusercontent.com/79994584/114464419-5e8bfe80-9c03-11eb-9df4-f5c66608d5c8.png)


*Following sysnthesis the next step is floorplan.*

*Standard cells placement take place in placement stage and not floorplan.*

![image](https://user-images.githubusercontent.com/79994584/114464513-7d8a9080-9c03-11eb-842f-969eb116418b.png)


![image](https://user-images.githubusercontent.com/79994584/114464529-83807180-9c03-11eb-9905-e5074ba6f505.png)


![image](https://user-images.githubusercontent.com/79994584/114464542-88452580-9c03-11eb-8e2d-7e669e36b25c.png)

*These are the switches which are to be set.*

![image](https://user-images.githubusercontent.com/79994584/114464602-998e3200-9c03-11eb-9ee9-df09b805b5c8.png)


![image](https://user-images.githubusercontent.com/79994584/114464606-9c892280-9c03-11eb-9fe2-f4955533c805.png)


Now these are the default parameters that are set at the floorplan stage in openlane

For example in the FP_IO_MODE ; if it is 1 then the pins will be equidistant to eachother,
While if it is 0 then they wont be equidistant

The precedence is Lowest priority is for the system default 

Then config.tcl

Then pdk variant.tcl

Now we will run the floorplan 

Using -->  `run_floorplan`

![image](https://user-images.githubusercontent.com/79994584/114464795-db1edd00-9c03-11eb-9d1e-f79930fd9ad6.png)


![image](https://user-images.githubusercontent.com/79994584/114464806-deb26400-9c03-11eb-829b-4de428c2983f.png)


![image](https://user-images.githubusercontent.com/79994584/114464821-e70a9f00-9c03-11eb-94f6-ca549f27fce4.png)


![image](https://user-images.githubusercontent.com/79994584/114464844-ee31ad00-9c03-11eb-8e13-2ce92c2fdd13.png)


![image](https://user-images.githubusercontent.com/79994584/114464860-f558bb00-9c03-11eb-86a5-f58304a36ab7.png)


![image](https://user-images.githubusercontent.com/79994584/114464874-f984d880-9c03-11eb-94de-7bf05a1f0554.png)


![image](https://user-images.githubusercontent.com/79994584/114464894-ff7ab980-9c03-11eb-8a6d-f4d286b4457d.png)


![image](https://user-images.githubusercontent.com/79994584/114464911-06093100-9c04-11eb-8321-67fef781166c.png)


![image](https://user-images.githubusercontent.com/79994584/114464920-0b667b80-9c04-11eb-8388-1cd58e328de7.png)


![image](https://user-images.githubusercontent.com/79994584/114464935-128d8980-9c04-11eb-93d9-2d49d8ee9c39.png)


*If your zoom in you will see that all the standard cells are now placed in the standard cell rows in the placement stage.*


# Day 3 - Design library cell using Magic Layout and ngspice characterization


**1. Spice Deck**

It comprises of the connectivity information of components i.e. Netlist. 

**2. Component values**

The width and length of PMOS and NMOS are defined.

M1: PMOS -> W/L -> 0.375u / 0.25u
M2: NMOS -> W/L -> 0.375u / 0.25u

***Generally the gate Voltage is similar to channel length i.e. if we have channel length L= 0.25u , then the gate voltage = 2.5 V***

**3. Identify 'nodes'** 

A node is required to specify the spice netlist. 

**4. Name nodes**

Every node is represented as a positive number starting from 0 to n. In an increasing order. Nodes can either be represented as gnd, out, in etc.



*** MODEL DESCRIPTION ***

*** NETLIST DESCRIPTION ***

`M1 out in Vdd Vdd pmos W=0.375u L=0.25u`

In the above spice statement a transistor M1 which is a pmos is specified with the order : drain , gate , source, substrate .

`M2 out in 0 0 nmos W=0.375u L=0.25u`

In the above spice statement a transistor M2 which is a nmos is specified with the order : drain , gate , source, substrate .

`cload out 0 10f`

In the above spice statement a load capacitor connected between the out and 0 node having a value of 10fF is specified.

`Vdd vdd 0 2.5`

In the above spice statement a the power supply Vdd is connected between the vdd and 0 nodes and has value 2.5V 

`Vin in 0 2.5`

**Spice Simulation Lab for CMOS Inverter**

*** SIMULATION COMMANDS ***

`.op`

`.dc Vin 0 2.5 0.05`

The above Spice command is to sweep the input voltage 'Vin' from 0 to 2.5V at steps of 0.05 and get the ouput voltage.


There are two cases for pmos and nmos in which there is SPICE Waveform variation :

1. When, Wn = Wp = 0.375u and Ln,p = 0.25u - The (W/L)n = (W/L)p = 1.5 : The VTC Curve is shifted towards the left and the **switching threshold Vm = 0.98 V**
2. When, Wn = 0.375u, Wp= 0.9375 ( observe that Wp = 2.5 * Wn ) : The VTC Curve is exactly at the middle , and the  **switching threshold Vm = 1.2 V**

Hence we observe a difference in transfer characteristics.


**Switching Threshold**

It defines the robustness of CMOS. It is represented as Vm and it lies at the point where Vin = Vout.


**Layout**

Art of layout using Euler's path + Stick diagram

***Layout using stick diagram only***

In this case the conjestion of many metal layers causes alot of DRC rules to come up. However the output remains the same as that in euler's path. 

The improved stick diagram is the combination of Euler's path + Stick diagram.  Stick diagram acts as an interface between final layout and circuit.

After drawing the stick diagram , a rough layout and the dimensions to the metal lines and diffusion layers is specified in accordance with the DRC rules.

***Remember n diffusion contact and p diffusion contact are different.***


**Cell Characterization**

1. Derive actual dimensions for the function
2. Script to create layout in Magic
3. Final layout and input output labelling 
4. Pre-layout and post-layout simulation


git clone `https://github.com/nickson-jose/vsdstdcelldesign.git`

*This will create a folder in your openlane  directory.*

![image](https://user-images.githubusercontent.com/79994584/114465463-c727ab00-9c04-11eb-8842-6665892f7fa8.png)


![image](https://user-images.githubusercontent.com/79994584/114465479-cdb62280-9c04-11eb-87f2-73a7feef45d8.png)


![image](https://user-images.githubusercontent.com/79994584/114465513-d6a6f400-9c04-11eb-8a74-b2ab78166e98.png)


![image](https://user-images.githubusercontent.com/79994584/114465527-dad31180-9c04-11eb-8276-cd61658230a7.png)


![image](https://user-images.githubusercontent.com/79994584/114465541-de669880-9c04-11eb-981f-c28fca06ad2d.png)


![image](https://user-images.githubusercontent.com/79994584/114465563-e58da680-9c04-11eb-8319-c69b33c4a0b2.png)


![image](https://user-images.githubusercontent.com/79994584/114465579-ec1c1e00-9c04-11eb-9895-e02f6d983a3c.png)


`To centre align press s then v`

![image](https://user-images.githubusercontent.com/79994584/114465612-f9d1a380-9c04-11eb-829f-bc20bd0e7649.png)


![image](https://user-images.githubusercontent.com/79994584/114465623-fccc9400-9c04-11eb-8405-3cdf45fb1ed2.png)


![image](https://user-images.githubusercontent.com/79994584/114465638-02c27500-9c05-11eb-8ca6-883e044a2908.png)


![image](https://user-images.githubusercontent.com/79994584/114465657-08b85600-9c05-11eb-92de-692580922535.png)


Edit the spice file using 
`Esc + I`

![image](https://user-images.githubusercontent.com/79994584/114465693-15d54500-9c05-11eb-80b8-15ed75b1969f.png)


To save 
`Esc + :wq!`

![image](https://user-images.githubusercontent.com/79994584/114465739-27b6e800-9c05-11eb-8482-a5edb9fc091d.png)


![image](https://user-images.githubusercontent.com/79994584/114465762-2eddf600-9c05-11eb-8d80-e54bb90affea.png)


![image](https://user-images.githubusercontent.com/79994584/114465790-36050400-9c05-11eb-8583-695a4d55b24e.png)


![image](https://user-images.githubusercontent.com/79994584/114465808-3bfae500-9c05-11eb-965e-f6c79ba8ec34.png)


![image](https://user-images.githubusercontent.com/79994584/114465825-461ce380-9c05-11eb-83cd-5b63565ba55c.png)


![image](https://user-images.githubusercontent.com/79994584/114465836-4917d400-9c05-11eb-828e-e3afdf0bb5b0.png)



# Day 4 - Pre-layout timing analysis and importance of good clock tree

**1. Delay Tables**

A Delay Table is used for representing delay of a particular block while varying the output load and input slew. Every clock with different sizes has a different Delay table and delay tables varies from block to block.

**2. Setup Timing Analysis**

It is the finite time required *before the clock edge* wherein the data stability is maintained. 

**3. Hold Timing Analysis**

It is the finite time required *after the clock edge* wherein data stability is maintained.

**4. Clock Jitter**

It is the temporary variation in clock period, it is modelled by using a single pararmeter **Uncertainity** .


![image](https://user-images.githubusercontent.com/79994584/114465884-5d5bd100-9c05-11eb-86dd-6c13a8103946.png)

![image](https://user-images.githubusercontent.com/79994584/114465922-69e02980-9c05-11eb-9265-0d955cbe1f59.png)


![image](https://user-images.githubusercontent.com/79994584/114465939-6e0c4700-9c05-11eb-8788-2619706bddab.png)


![image](https://user-images.githubusercontent.com/79994584/114465949-71073780-9c05-11eb-91a5-6c1d3d858380.png)


`If u press g then refrence cells used can be seen`.

![image](https://user-images.githubusercontent.com/79994584/114466003-854b3480-9c05-11eb-90b4-5f9229b0c9ec.png)


![image](https://user-images.githubusercontent.com/79994584/114466016-88debb80-9c05-11eb-9364-743e8038c659.png)


![image](https://user-images.githubusercontent.com/79994584/114466023-8d0ad900-9c05-11eb-9a9e-3184895ae055.png)


![image](https://user-images.githubusercontent.com/79994584/114466050-9136f680-9c05-11eb-876c-ce112306b397.png)


![image](https://user-images.githubusercontent.com/79994584/114466074-96944100-9c05-11eb-9446-6939cc43ab56.png)


![image](https://user-images.githubusercontent.com/79994584/114466103-a2800300-9c05-11eb-8c2c-bf1ac5dcc44f.png)


![image](https://user-images.githubusercontent.com/79994584/114466116-a6138a00-9c05-11eb-860f-0f702dcf2c11.png)


![image](https://user-images.githubusercontent.com/79994584/114466123-a9a71100-9c05-11eb-8c3a-4cfb17a7cf0a.png)


![image](https://user-images.githubusercontent.com/79994584/114466171-ba578700-9c05-11eb-8787-fe5d7766dba4.png)

*So the lef file is ready and now we will have to plug this lef file into the picorv32 .*

![image](https://user-images.githubusercontent.com/79994584/114466218-ccd1c080-9c05-11eb-936f-253aa9fdd03a.png)


![image](https://user-images.githubusercontent.com/79994584/114466231-d3603800-9c05-11eb-8c4b-f5fac187dd47.png)


![image](https://user-images.githubusercontent.com/79994584/114466238-d6f3bf00-9c05-11eb-834d-0ca9ba33b38e.png)


![image](https://user-images.githubusercontent.com/79994584/114466249-da874600-9c05-11eb-9471-3868ed31a4f4.png)


![image](https://user-images.githubusercontent.com/79994584/114466261-dfe49080-9c05-11eb-90e2-2aa4d1691350.png)


![image](https://user-images.githubusercontent.com/79994584/114466293-e8d56200-9c05-11eb-928d-ec610e91ee56.png)


![image](https://user-images.githubusercontent.com/79994584/114466309-ef63d980-9c05-11eb-86da-ebddce70c680.png)


![image](https://user-images.githubusercontent.com/79994584/114466317-f2f76080-9c05-11eb-97c3-fbe63908020f.png)


![image](https://user-images.githubusercontent.com/79994584/114466329-f985d800-9c05-11eb-9ba3-8c32744c76eb.png)


![image](https://user-images.githubusercontent.com/79994584/114466340-fd195f00-9c05-11eb-9f1b-77429e06bfb6.png)


![image](https://user-images.githubusercontent.com/79994584/114466352-01457c80-9c06-11eb-8af5-e0003e96cafc.png)


![image](https://user-images.githubusercontent.com/79994584/114466366-06a2c700-9c06-11eb-908c-209050a5cb9c.png)


`set lefs [glob $::env(DESIGN_DIR)/src/*.lef]`

`add_lefs -src $lefs`


![image](https://user-images.githubusercontent.com/79994584/114466495-2e922a80-9c06-11eb-9725-9545175d20dd.png)


![image](https://user-images.githubusercontent.com/79994584/114466509-34880b80-9c06-11eb-920a-3ea846653a69.png)


![image](https://user-images.githubusercontent.com/79994584/114466530-3baf1980-9c06-11eb-8ecc-aff68c37bb2a.png)



# Day 5 - Final steps for RTL2GDS using tritonRoute and openSTA

Routing in simple terms means the best possible physical connectivity between two points.

**Lee’s Algorithm**

There is a Source and a Target which are to be connected (routed). This routing procedure should not take place on the obstruction between the source and Target. The algorithm creates a grid at the backend with certain standard dimensions.
Firstly all the adjacent grids to the source are labelled numerically (horizontal and vertical). 

*Routing with single bend is generally preferred*.

![image](https://user-images.githubusercontent.com/79994584/114466709-7618b680-9c06-11eb-98d6-524c34f19fb5.png)


![image](https://user-images.githubusercontent.com/79994584/114466733-7d3fc480-9c06-11eb-8dcc-5adc8c0c394f.png)    ![image](https://user-images.githubusercontent.com/79994584/114466755-8597ff80-9c06-11eb-955f-84b8afea0877.png)

The best possible solutions should follow:

*	Least number of bends.
*	Typically L shaped (minimal Z shaped)


Three typical DRCs for a pair of wires are :

*	Wire width –  width of a wire (technique used is lithography)
*	Wire pitch – centre to centre distance between two wires (technique used is lithography)
*	Wire spacing – spacing between two wires 


![image](https://user-images.githubusercontent.com/79994584/114466837-9d6f8380-9c06-11eb-971a-ddaaefac4a6c.png)

This is type of a serious DRC violation called the signal short.

*Inorder to fix signal short condition , two different metal layers must be used.*

![image](https://user-images.githubusercontent.com/79994584/114466903-b5470780-9c06-11eb-8e2d-91d95b040888.png)


The other two DRCs to be checked are related to the vias:


1.)	Via width

![image](https://user-images.githubusercontent.com/79994584/114466970-ca239b00-9c06-11eb-8fa4-a74f6a6987fc.png)


2.)	Via Spacing

![image](https://user-images.githubusercontent.com/79994584/114467001-d576c680-9c06-11eb-977d-5810bf01d073.png)


The process of routing comprises of a huge solution space. Hence, in the industry it is broken down into two parts , namely:

1.)	Global Routing  ( done using Fast Route ) – output is a set of routing guides for each of the nets.

2.)	Detailed Routing (done using Triton Route) – we use that global route and do the connectivity.


--------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Acknowledgement

**Kunal Ghosh** , *Co-founder (VSD Corp. Pvt. Ltd).*

**Nickson Jose** , *VSD VLSI Engineer*

**Praharsha Mahurkar** , *VSD TA*

**Akurathi Radhika** , *VSD TA*


























