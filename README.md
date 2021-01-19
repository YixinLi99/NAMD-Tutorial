# NAMD-Tutorials

<filename>.psf - protein structure file. A list of the atoms, masses, charges and connections between atoms.

<filename>.pdb - protein database file. The actual starting coordinates of the models. This has to be the same order as the psf file.

<filename>.conf - NAMD configuration file. Tells NAMD how to run the job.
	
Parameters file + configuration file -> MD trajectory
###### conf file explanation: https://www.ks.uiuc.edu/Training/Tutorials/namd/namd-tutorial-unix-html/node26.html
###### NAMD Explained: https://sci-hub.se/https://onlinelibrary.wiley.com/doi/abs/10.1002/jcc.20289

## Simulations of Dipeptide with Minimisation and Equilibrum, MD0

###### Step 1: Install NAMD & Dipeptide_e Files

###### Step 2: Runing NAMD 
```
1. cd ~/dipeptide_e
	#make sure you've got the two files in the dipeptide_e
2. /Users/yixinli/NAMD_2.14/namd2 md0.dipwat.conf > Out_md0.Dipwat.log
```
`(base) Yixins-MacBook-Pro-2:dipeptide_e yixinli$ /Users/yixinli/NAMD_2.14/namd2 md0.dipwat.conf > Out_md0.Dipwat.logyx`


start a new terminal
`tail -f Out_md0.Dipwat.logyx`
it will update all the changes to the file 
`tail Out_md0.Dipwat.logyx`
it will give the last 10 lines of the file


- [x] Task1:
1. Installed VMD 
2. Visualise dcd projectroy file 
3. Upload the dcd projector file 
4. Movie 

###### Instructions：
Visualize the md0 dcd trajectory file on VMD, by first uploading the prmtop file and then uploading the dcd on top of it.

- [x] Task 2:
How’s the evolution of energy/temperture

`(base) Yixins-MacBook-Pro-2:dipeptide_e yixinli$ cat Out_md0.Dipwat.logyx | grep -i energy > Out_md0.Dipwat.logyx.en`

`(base) Yixins-MacBook-Pro-2:dipeptide_e yixinli$ cat Out_md0.Dipwat.logyx | grep -i etitle > Out_md0.Dipwat.logyx.etitle`

how to put the two together? 

###### Instructions：
Inspect the md0 log file, there should be lines starting with "ENERGY:" 
followed by many values, one of which is the temperature, another is the total energy,
and so on... To know which is which, refer to the lines starting with "ETITLE:". 
If you find a way to plot temperature 
versus time and total energy versus time 
(for example with a software like gnuplot) it would be great, 
otherwise just get familiar with them for now.

script: comment out the followings: 
```
#   binCoordinates         ./md0.Dipwat.restart.coor
#   binVelocities          ./md0.Dipwat.restart.vel
#   extendedSystem
```
###### Step 1: Install Gnuplot

```
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"

brew install gnuplot

(base) Yixins-MacBook-Pro-2:~ yixinli$ gnuplot
```
	> http://people.duke.edu/~hpgavin/gnuplot.html

###### Step 1: Enter Gnuplot and Plot

```
(base) Yixins-MacBook-Pro-2:dipeptide_e yixinli$ gnuplot
gnuplot> plot "Out_md0.Dipwat.logyx.en" u 2:12
```

------------------------------------------------------

## Simulation MD1 (production run)


script: comment out the followings: 
```
# temperature         $temperature
#   set a 33.6
#   set b 33.0
#   set c 33.3

# PME
```

- [x] Task3:
Md0 minimisation of 
Completed simulated 
Create md1 and simulate from md0
(Continuation of md0)

Create a new md1 file via terminal:
`touch file_name.conf`

###### Instructions：
create a md1 conf file, to be a simulation that would 
restart after the end of md0. 
To do so,
	remember to remove the lines with: 
	**CellBasisVector**, **CellOrigin**, **reinitvels**, **temperature $temperature**, **seta,b,c**, **minimize**, and **PME**
	comment in the first three lines (so that md1 will restart from md0);
just like I instructed you to do in the original tutorial on NAMD.

*Another recommendation: you should not start md1 
if you don't see the "End of Program" statement at the end of the log of md0.*

- [x] Task 4: 
Create md2 as a restart from md1 (so that you'll have 15 ns in total) and visualise them with VMD. 
Try to set the representation so that the entire proline dipeptide (not just the PRO) is set with "Licorice". 
Remember: PRO is just the center residue among three, you must discover which are the resnames (or resids) of the other two, by clicking on any of their atoms in VMD.

(put dcd into pymol)

- [x] Task 5: 
Use gnuplot to make sure that the energy (u 2:12) and the temperature (u 2:13) behave as you expect. Do this for md0, md1, and md2.

- [x] Task 6: 
Use cpptraj to calculate the rmsd and hydrogen bonds of the proline dipeptide. a useful script: 
```
export AMBERHOME=/home/alessandro/Documents/amber18

source $AMBERHOME/amber.sh
(please, modify "/home/alessandro/Documents/amber18" with the appropriate path)

and then to execute cpptraj itself:

$AMBERHOME/bin/cpptraj -i example.cpptraj
```

###### Install AmberTools 
make sure you've installed conda (which I did)

```conda create --name AmberTools20```

```conda activate AmberTools20```

```conda install -c conda-forge ambertools=20```

<useful links>(https://ambermd.org/GetAmber.php)

To activate AmberTools20, you just simply need to type:
```conda activate AmberTools20```

`cpptraj -i example.cpptraj`

After Execution, you want to have a look at the files:

`ls *dat`
there will be: hbout_dipeptide.dat, hbavg_dipeptide.dat (to analyse the hydrogen bonds) , rmsd_dipeptide.dat(to check if simulation is working well)

MD2: same protocol 
- [ ] Task 7: How to run MD on rosalind?
See also: Tutorial on Rosalind

## Metadynamics 

Input: 
``` mtd0.Dipwat.conf 
md0.Dipwat.restart.coor
md0.Dipwat.restart.vel
md0.Dipwat.restart.xsc
```
Output: 
```
COLVAR
HILLS
```

2,6,8,17: they are not VMD indices
VMD indices: 1,5,7,16
(therefore, if you want to define atom's indices, go to VMD, take indices and always remember to add one)

Sigma: width of Guassian in radius 
biasfact (5-15 range) but recommand 15
print age*print all arguments/variables
stride: 
File: Hills & Restart at the beginings*

post-production -> free energy surface 
How to actually run metadynamics, and converge 

*what if make a mistake?
cannot stop and restart from scratch, 
you need to back-up files of Hills

``` plumed on
plumedfile plumed_dip.dat ```

###### install Plumed 

``` ./configure
make all install ```
```

###### Metadynamics - Calcaulations of the free energy surface (FES)

here's the script for the calculation of the free energy surface (FES). As a function of two variables, it will need to be plotted with "splot" in gnuplot. In order to have it on 2D, and with the z dimension as colours, use "**set pm3d map**". In case of doubt, gnuplot is very well supported online.

Ideally, you may want to have an error on the free energy surface, and this is done with the block analysis, which I will explain another time.
Do at least 40 ns of metadynamics for this. Again, very important, add the RESTART keyword at the beginning of the plumed.dat file if you are doing a restart, and keep in mind that HILLS and COLVAR may be wrongly overwritten if a restart needs to be redone, so please create safe-copies of both files after every restart.

