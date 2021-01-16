# montecarbo
MonteCarbo is a software to construct simple 5-, 6- and 7-membered ring multifunctionalized monosaccharides and nucleobases and to dock them into target glyco- and RNA/DNA- active enzymes. The core code is a bash script and it executes simple orders to generate the Z-matrix of the neutral molecule of interest. After that, a Fortran90 code based on a pseudo-random number generator (MonteCarlo method) is executed to assign dihedral angles to the different rotamers present in the structure (5-, 6-, 7-membered ring conformers and OH, NH2, CH2OH and so on). In order to transform the Z-matrix into a PDB or XYZ file, the program requires the installation of an external code (Babel, for example, default). The program also has a GIC implementation of the Cremer and Pople puckering coordinates for 5-, 6- and 7-membered ring ready to use in Gaussian16. Once the structures are generated and optimized using the preferred Quantum Mechanics software for the user, a second code is ready to execute AutoDock Vina in order to dock the different conformers of the molecule of interest in the active site of a wide family of enzymes. (More information in README file, any further doubt, do not hesitate to contact me: dr.s.alonso-gil@gmx.com or santiago.alonso.gil@univie.ac.at )

First of all, untar the file MonteCarbo.tar.gz

tar -zxvf MonteCarbo.tar.gz

cd MonteCarbo

CONTENT OF THE README IN MonteCarbo folder:

************************************************************
*                                                          *
*                         MONTECARBO                       *
*                                                          *
************************************************************

MonteCarbo is an open-source bash script that requires:

-Fortran compiler f95

-Open Babel 2.4.1 or similar (NOPUCKPDB, NOPUCKXYZ)

-(Optional) Gaussian16 for optimizations (PUCK) or other 
version of Gaussian (09,03,98) (NOPUCKG)

-(Optional) To execute MCdock: AutoDock Vina

To execute MonteCarbo just:

./Montecarbo (and follow the instructions)

or

./MonteCarbo < input.dat

************************************************************
*                                                          *
*                         INPUT.DAT                        *
*                                                          *
************************************************************

In the main folder of the program, the user will find an 
example of an input.dat file. Feel free to play with it.

Structure of input.dat:

1) Title/name of the project
2) Kind of study --> output 
PUCK 
Find for minima in the puckering coordinate PES
Needs Gaussian 16.

NOPUCKG, NOPUCKPDB, NOPUCKXYZ - The program generates
structures for Gaussian16(G) or in PDB or XYZ formats 
Babel required.

RIGID - Optimization with Gaussian16 keeping fixed the 
conformation of the ring (recommended for docking) 

3) Size of the ring (5-, 6- or 7-membered) ($N)
4) Number of heteroatoms in the ring (0 or 1)
5) If 4)=1, name of heteroatom (Oxy,S,SO2,N,B,P)
6) Chosen functional groups for the N-num.het. positions
in the ring (C1-UP, C1-DOWN, C2-UP, etc.)

List of functional groups:

H - Hydrogen
F - fluor
Cl - chloride
Br - bromide
CH3 - methyl
C
O - C=O in the ring
NH2 - amino - 1 rotamer
OH - alcohol - 1 rotamer
SH - thiol - 1 rotamer
NO2 - nitro - 1 rotamer
CH2OH - methyl-alcohol - 2 rotamers
CH2OCH3 - methyl methyl ether - 2 rotamers
CO2H - Carboxylic acid - 1 rotamer
Bz - Benzyl - 1 rotamer
NAc - N-Acetyl - 1 rotamer
pNP - para-nitrophenyl -O-Bz-NO2 - 1 rotamer
OCH3 - methoxy - 1 rotamer
OAc - acetyl - 2 rotamers
SCH3 - methyl-thiol - 1 rotamer
H2PO4 - dihydrogen-phosphate - 1 rotamer
HSO4 - hydrogen-sulphate - 1 rotamer
Ade - adenine - 1 rotamer
Thy - thymine - 1 rotamer
Ura - Uracyl - 1 rotamer
Cyt - Cytosine - 1 rotamer
Gua - Guanine - 1 rotamer

7) Number of conformers to be generated

That's all! Enjoy!

After that, as I told you before, execute like:

./MonteCarbo < input.dat

The program will generate a folder in the main directory
whose name is $title$numofconformers (ex. testglucose99)

In case you chose PUCK: the program copies an analysis-$N.bash
file in the "testglucose99" folder. Once the Gaussian 
calculations are finished, execute:

./analysis-$N.bash (./analysis-6.bash for a 6-ring)

and you will get a file Results.dat with the energetics and
conformational information of your systems that you can plot
with gnuplot or similar software. You can see which is the 
favourite conformational space of your molecule.

Again, enjoy!

************************************************************
*                                                          *
*                          MCdock                          *
*                                                          *
************************************************************
First of all, download and decompress the MCdock.tar.gz:

tar -czvf MCdock.tar.gz

The content of this file is:

1) MCdock.bash

2) receptors folder with pdb, pdbqt and config files for
docking calculations with AutoDock Vina.

3) vina folder where the user must copy the ./vina program
that can be downloaded in
http://vina.scripps.edu/download.html.

To perform the docking calculations the following codes are
required:

1) Open Babel

2) AutoDock Vina

3) prepare_ligand and prepare_receptor executables from
AutoDock package.

MCdock.bash must be executed in the MonteCarbo folder:

/address/MonteCarbo/MCdock.bash

The script requires:

1) The name of the folder where the structures are
split into one folder per structure.

2) The format of the coordinates files (XYZ, PDB or G)

3) Which kind of enzymes will be the target for docking
(Feel free to create a new family of enzymes in the
receptor folder: mkdir receptors/NEW-ENZYMES)

Example:

./MCdock

testglucose10

XYZ

GLUCO

*This example will dock 10 structures of glucose in
some glucosidases.*

And enjoy!


