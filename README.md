# nozzleDSMC
Repository for the PX915 Peer-to-peer assessment, containing necessary files to run the simulation(s) and a Jupyter notebook for the UQ part.

Note: this project was done using SPARTA version 18 July 2022.

## Installing SPARTA
1. Go to https://sjplimp.github.io//download.html and download SPARTA (under Other software).
2. Extract the files, prefarably on a remote server (e.g. ```hetmathsys1.scrtp.warwick.ac.uk```) with ability to parallel process.
3. Go to ```sparta/sparta-{Version date}/src``` and run the command ```make mpi```. This should produce the ```spa_mpi``` executable. This needs to be present inside whichever folder containing the input scripts for simulations.
4. If this does not work/ more details are needed, please visit https://sparta.github.io/doc/Section_start.html for more guidance.

## Running the simulation
1. Create a folder (henceforth ```nozzle``` in this document) within ```sparta/sparta-{Version date}```.
2. Copy the core files in this repository into the folder. These are:
   a. ```air.species``` - contains parameters for molecules that form the air mixture; e.g. molecular mass
   b. ```air.vss```     - contains collision parameters for different gases for the VSS model
   c. ```data.flatnz``` - contains the geometry used in the nozzle simulation
   d. ```flatnz.in```   - the input script for the simulation
3. Copy ```spa_mpi``` into the folder.
4. Run the simulation using the command ```./spa_mpi < flatnz.in```. A typical run takes 10-30 minutes.

## Altering the input file/setting variables
There are many parameters in the input file required to generate a valid simulation, all detailed at the SPARTA website (https://sparta.github.io/). However, for the purposes of UQ, there are only three sections for alteration and the rest may be ignored (do not alter!). The key numbers are indicated in asterisks as follows (remove the asterisks in the actual file):

1. Changing h (Channel width)
  - Go to the geometry file and alter the y coordinates (third column) of the 2nd and 3rd points. This value should not exceed those for the simulation box (0.0045).
  - An example row for the points should read ```2 0.0000 *0.0040*```.
2. Changing alpha (accommodation coefficient)
  - Look for the line starting ```surf_collide``` and change the final number in the row. This value should not exceed 1 and fall below 0.
  - The line should read ```surf_collide      diffuse_423K diffuse 423.15 *1*```
3. Changing T (temperature)
  - Look for the lines containing ```mixture``` and ```surf_collide```.
  - The numbers in hundreds represent the temperature - these can be altered. There should be two ```mixture``` lines; the one with a ```temp``` keyword is the one we are after.
  - These lines should read:  
    ```mixture           gas_in N2 vstream 0 0 0 temp *423.15* nrho 3.424e21```  
    ```surf_collide      diffuse_423K diffuse *423.15* 1```
4. Changing the output filename
   It is useful to be able to save multiple output files to be processed at once as is in the UQ Jupyter notebook. To do this:
   - Look for the line
     ```fix               mfr ave/time 1 10 10 c_mfr[*] mode vector file tmp.outH401```
   - This should be just after the comment line "After the simulation reaches steady state".
   - Change the final entry in the line to your preferred file name; for example, ```tmp.outT424``` for a temperature variation.

## Performing the UQ
The Jupyter notebook titled ```UQ - nozzle.ipynb``` should contain all explanations to perform the UQ. 
