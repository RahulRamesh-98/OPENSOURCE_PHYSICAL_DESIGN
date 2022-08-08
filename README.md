# VSDIAT_Advanced_physical_design_Using_OPENLANE_skywater130pdk Workshop

The repository contains all the studies done during the Advanced Physical Design Using OpenLANE / SKY130 workshop. It is primarily foucused on a complete RTL2GDS flow using the open-soucre flow named OpenLANE. PICORV32A RISC-V core design is used for the purpose.

## Tools used during the workshop 
All the tools used are open source tools.

Yosys                  Synthesis of RTL Design
ABC                    Mapping of Netlist
OpenSTA                Static Timing Analysis
OpenROAD               Floorplanning, Placement, CTS, Optimization, Routing
TritonRoute            Detailed Routing
Magic VLSI             Layout Tool
NGSPICE                SPICE Extraction and Simulation
SPEF_EXTRACTOR         Generation of SPEF file from DEF file

## Day 1: Inception of Open-Source EDA, OpenLane and Sky130 pdk
### Basic IC Design Terminologies

1.Package: It is a case that surrounds the circuit material to protect it from physical damage or corrosion and allow mounting of the electrical contacts connecting it to the printed circuit board (PCB). The below snippet shows an IC with 48 pins and Quad Flat No-Leads(QFN) package.
2.Die: A die is a small block of semiconducting material on which a given functional circuit is fabricated.
3.Core: It is the actual area of the IC where the logic resides.
4.Pads: These are the interfaces between the internal signals of a chip and the external pins
  ![Screenshot from 2022-08-08 22-58-44](https://user-images.githubusercontent.com/67609171/183478180-6e22454b-07ff-46a8-a83b-e32f116bb434.png)

### OpenLANE Initialisation
For invoking OpenLANE in Linux Ubuntu, we should first run the docker everytime we use OpenLANE. This is done by using the following script:
![Screenshot from 2022-08-05 13-08-26](https://user-images.githubusercontent.com/67609171/183480535-4a792810-76a7-4494-80a8-86ab24cc6c4b.png)
![Screenshot from 2022-08-05 13-32-18](https://user-images.githubusercontent.com/67609171/183480936-f0a37c8f-968d-4199-9eff-864a82f9c779.png)
To invoke OpenLANE run the (./flow.tcl)  script.
OpenLANE supports two modes of operation: interactive and autonomous.
To use interactive mode use -interactive flag with (./flow.tcl)
![Screenshot from 2022-08-05 13-34-48](https://user-images.githubusercontent.com/67609171/183481409-293f0255-d2d9-42c8-ac7e-4b532fd14505.png)
Next is to prepare our design for the OpenLANE flow. This is done using command: (prep -design <design-name)
![Screenshot from 2022-08-05 14-02-54](https://user-images.githubusercontent.com/67609171/183483863-9d790c97-ef1c-4b67-b1d4-21b6bc6520f2.png)

### Synthesis
The first step in OpenLANE flow is RTL Synthesis of the design loaded
![Screenshot from 2022-08-05 14-21-19](https://user-images.githubusercontent.com/67609171/183484739-3e22ae3e-6557-4bbc-af06-6a142ad3d120.png)
![Screenshot from 2022-08-05 14-22-35](https://user-images.githubusercontent.com/67609171/183484930-a5b3c3fe-8386-440f-9251-6b8abbfb4f32.png)
 
  Flop Ratio is calculated using formula : Flop Ratio= (Total no.of Flops )/(Total no.of cells)
  ![Screenshot from 2022-08-05 14-24-36](https://user-images.githubusercontent.com/67609171/183485549-812629a6-6449-47d5-a440-875f2f846cb6.png)

## Day 2 - Good floorplan vs bad floorplan and introduction to library cells

### Floorplan using OpenLANE
run_floorplan
 
 ![Screenshot from 2022-08-05 15-22-47](https://user-images.githubusercontent.com/67609171/183486352-96fdb9aa-af2d-4649-af40-8a8f2cbea325.png)
 
 Successful floorplanning gives a def file as output. This file contains the die area and placement of standard cells.
 
 ![Screenshot from 2022-08-05 16-07-17](https://user-images.githubusercontent.com/67609171/183486609-3bd339ab-006b-420a-a2fb-90774258509d.png)

### Review Floorplan Layout in Magic
Magic Layout Tool is used for visualizing the layout after floorplan.
Technology File (sky130A.tech), Merged LEF file (merged.lef), DEF File are required to view the floorplan in Magic.

![Screenshot from 2022-08-05 16-17-37](https://user-images.githubusercontent.com/67609171/183487157-be2b6a20-184d-4f0f-8f9d-66a553e18671.png)

![Screenshot from 2022-08-05 16-22-02](https://user-images.githubusercontent.com/67609171/183487372-c80c3645-6e36-4eca-8182-b4b1d1509261.png)

The tkcon window can be used to get the required details from the layout. All we have to do is move the cursor on to the specific field required to be known, click "S" on keyboard, and on tkcon window, type "what". The details will be displayed.

![Screenshot from 2022-08-05 16-26-04](https://user-images.githubusercontent.com/67609171/183487747-e616aacc-6f8c-438c-bdeb-9ff304005c8a.png)

### Placement

(run_placement)
The DEF file created during floorplan is used as an input to placement.

![Screenshot from 2022-08-05 22-18-35](https://user-images.githubusercontent.com/67609171/183488372-072c320a-165b-461e-b0c0-bc7ad9aad4cf.png)
![Screenshot from 2022-08-05 22-21-27](https://user-images.githubusercontent.com/67609171/183488629-296860b0-6e0a-4878-acf5-69826df74d89.png)


## Day 3: Design library cell using magic layout and NgSpice characterization
### CMOS Inverter Design using Magic
The snippet below shows a layout for CMOS Inverter

![Screenshot from 2022-08-07 01-59-30](https://user-images.githubusercontent.com/67609171/183489312-13e1d202-f671-4a16-9568-43a36a6720c6.png)

### Extract SPICE Netlist from Standard Cell Layout
Extract the circuit from the layout design.
(extract all)

Convert the extracted circuit to SPICE model.
(ext2spice cthresh 0 rthresh 0)
(ext2spice)

![Screenshot from 2022-08-07 13-24-52](https://user-images.githubusercontent.com/67609171/183490169-f0f0c099-c7fe-4c56-aa1c-314cffaa2f8b.png)

Some modification are done to the SPICE netlist for the purpose of simulations

![Screenshot from 2022-08-07 14-03-29](https://user-images.githubusercontent.com/67609171/183490763-2351ed96-498a-4b27-894f-16f36b4a416a.png)


### Transient Analysis using NGSPICE
Command used to invoke NGSPICE (ngspice <name-of-SPICE-netlist-file>)
  
  ![Screenshot from 2022-08-07 16-31-17](https://user-images.githubusercontent.com/67609171/183491562-741f0e43-63f4-43ef-9506-8ece012e5da2.png)

  
Command used to plot waveform in ngspice tool (ngspice 1 -> plot y vs time a)
  
Waveform of Inverter output vs input with respect to time.  
  
  ![Screenshot from 2022-08-07 16-38-15](https://user-images.githubusercontent.com/67609171/183491944-b6196e2e-5e50-4a5f-957e-082ecc4b30bc.png)

 
## Day 4 - Pre-layout timing analysis and importance of good clock tree

### Magic Layout to Standard Cell LEF
Before creating the LEF file we require some details about the layers in the designs. This details are available in (tracks.info) in the following format:  [<layer_name> <x or y axis> <track_offset/origin> <track_pitch/spacing>
  
  ![Screenshot from 2022-08-07 17-02-50](https://user-images.githubusercontent.com/67609171/183493018-09a86726-f9cc-480f-9cb4-cf0006729418.png)

  ![Screenshot from 2022-08-07 17-12-04](https://user-images.githubusercontent.com/67609171/183493317-ac7e9479-c2c4-4722-bb3a-a2509c519dd7.png)

### Timing Analysis using OpenSTA
  
The Static Timing Analysis(STA) of the design is carried out using the OpenSTA tool.


  ![Screenshot from 2022-08-08 18-31-28](https://user-images.githubusercontent.com/67609171/183495564-3e736ec0-9ece-4de8-b736-2fb2908e3cd3.png)

  The Slack violation has to be fixed.
  
### Floorplan and Placement
The following commands have to be executed in the given order.
  
  ![Screenshot from 2022-08-09 00-45-18](https://user-images.githubusercontent.com/67609171/183496430-f2799d4b-191b-4e57-99fd-d8a3796f010b.png)

  The following snippets are the results.
  
  
  ![Screenshot from 2022-08-08 03-20-16](https://user-images.githubusercontent.com/67609171/183496735-15085b90-6fc6-4102-a165-894b738e98b7.png)

  The inverter we created is added
  
 
  ![Screenshot from 2022-08-08 03-19-47](https://user-images.githubusercontent.com/67609171/183496889-b725ee83-75a1-469f-80b2-7d2b9bb178cf.png)
![Screenshot from 2022-08-08 03-23-35](https://user-images.githubusercontent.com/67609171/183497097-47985f5c-9d68-4d1f-8c44-9a8cd3a53ec7.png)
![Screenshot from 2022-08-08 03-24-30](https://user-images.githubusercontent.com/67609171/183497265-7aea82e4-8f13-4505-b43b-e52f9799559a.png)

  
In OpenLANE, clock tree synthesis is carried out using TritonCTS tool. CTS should always be done after the floorplanning and placement as the CTS is carried out on a (placement.def)ile that is created during placement stage.
The command used for running CTS in OpenLANE is (run_cts)
  
![Screenshot from 2022-08-08 19-38-35](https://user-images.githubusercontent.com/67609171/183498180-5b7003fc-67e7-4ec7-8e4b-b1fc894840f9.png)

  
If the above error occurs, follow the below steps and again execute (run_cts)
  
  ![Screenshot from 2022-08-09 00-45-18](https://user-images.githubusercontent.com/67609171/183498513-310c1bd4-ba18-4288-b548-9be29f9b0445.png)

  
## Day 5 - Final steps for RTL2GDS
### Generation of Power Distribution Network
Flow generation of PDN is carried out after the Clock Tree Synthesis(CTS).
The command used is (gen_pdn)
  
### Routing using TritonRoute
OpenLANE uses TritonRoute, an open source router for modern industrial designs. The router consists of several main building blocks, including pin access analysis, track assignment, initial detailed routing, search and repair, and a DRC engine.

The following command is used for routing: (run_routing)
  
  
## References
RISC-V: https://riscv.org/
VLSI System Design: https://www.vlsisystemdesign.com/
OpenLANE: https://github.com/The-OpenROAD-Project/OpenLane
https://github.com/nickson-jose/vsdstdcelldesign

## Acknowledgement
Kunal Ghosh, Co-founder, VSD Corp. Pvt. Ltd.
  
Nickson Jose

  
  
  















