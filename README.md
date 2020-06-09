# L2SI-Vac-ADS-interface-example
This repository is a working example of the ADS interface for gauges in L2SI-Vacuum-Lib (https://github.com/pcdshub/lcls-twincat-vacuum). In the interest of keeping this example simple, the communication is unidirectional. 

## Terms 
* **reader PLC**: This is the PLC that will be reading the gauge and valve status from the target PLC. This will PLC will run the `reading-plc` project.
* **target PLC**: This is the PLC that hosts the standard FB_MKS422 and FB_VGC structures to be read by another PLC. This PLC will run the `target-plc` project.

## Operation
1. Begin by selecting two PLCs on the same subnet. 
1. Select one PLC as the reader PLC (on which we will run the `reading-plc` project) and another as the PLC to be read from (on which we will run the `target-plc` project) 
1. From either PLC add a static route to the other PLC using the Static Routes tab on the Routes item in the solution explorer.  By selectinig the `static` option in the menu for the "Target Route" and "Remote Route" both PLCs will recognize one another in a single step. 
1. Add the AMS NET ID and ADS port of the reader PLC in the `ads_watch_dog` FB call in `MAIN` in the `target-plc` project.
1. Add the AMS NET ID and ADS port of the target PLC in the `vgc_reader` and `gcc_reader` FB call in `MAIN` in the `reading-plc` project.
1. Confirm that the interface is not producing errors by checking that `bError`, located in `vgc_reader` and `gcc_reader` located on the reader PLC and `ads_watch_dog` located on the `target-plc` is false.
1. Confirm that the `ADS_connection_counter` located on the reader PLC is rapidly escalating. 
1. Confirm that after manually changing a value in `MAIN.test_gcc.IG` or `MAIN.test_vgc.iq_stValve` on the target PLC the change propagates to the corresponding value in `MAIN.gcc_reader.IG` or `MAIN.vgc_reader.VGC` respectively
1. If the previous three checks succeeded, the interface is functioning correctly.
