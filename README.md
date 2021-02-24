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

### Instructions：
create a md1 conf file, to be a simulation that would 
restart after the end of md0. 
To do so,
	remember to remove the lines with: 
	**CellBasisVector**, **CellOrigin**, **reinitvels**, **temperature $temperature**, **seta,b,c**, **minimize**, and **PME**
	comment in the first three lines (so that md1 will restart from md0);
just like I instructed you to do in the original tutorial on NAMD.

***Another recommendation**: you should not start md1 
if you don't see the "End of Program" statement at the end of the log of md0.*

- [x] Task 4: 
Create md2 as a restart from md1 (so that you'll have 15 ns in total) and visualise them with VMD. 
Try to set the representation so that the entire proline dipeptide (not just the PRO) is set with "Licorice". 
Remember: PRO is just the center residue among three, you must discover which are the resnames (or resids) of the other two, by clicking on any of their atoms in VMD.

**load prmtop, dcd**

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

### Install AmberTools 
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
- [x] Task 7: How to run MD on rosalind?
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

plummed.dat: 
```
psi: TORSION ATOMS=25,12,11,9 #they are not VMD indices; VMD indices: 24,11,10,8
(therefore, if you want to define atom's indices, go to VMD, take indices and always remember to add one)

meta: SIGMA=0.16 #width of Guassian in radius 
HEIGHT=0.02 #
PACE=500 #
BIASFACTOR=(5-15 range) #but recommand 15

PRINT AGE #print all arguments/variables

STRIDE, FILE
```
post-production -> free energy surface  
run metadynamics and reach converge (from graph psi vs. timestep)

**what if make a mistake?**
Again, very important, add the RESTART keyword at the beginning of the plumed.dat file if you are doing a restart, and keep in mind that HILLS and COLVAR may be wrongly overwritten if a restart needs to be redone, so please create safe-copies of both files after every restart.
you need also back up your files

``` plumed on
plumedfile plumed_dip.dat ```

###### install Plumed 

``` ./configure
make all install ```
```

## Metadynamics - Calcaulations of the free energy surface (FES)

script for the calculation of the free energy surface (FES) (named: hills_script.sh) 
FES as a function of two variables, it will need to be plotted with ```splot``` in gnuplot. In order to have it on 2D, and with the z dimension as colours, use ```set pm3d map```

Ideally, you may want to have an error on the free energy surface, and this is done with the block analysis, which I will explain another time.

Noticeable: Do at least 40 ns of metadynamics

Plot: 

``` 
plot 'COLVAR' u ($1*0.001):2
set ylabel '{/Symbol x}'
```
```
gnuplot> set title 'Energy Minimisation'
gnuplot> set title font set ylabs
gnuplot> set ylabel 'Potential Energy (kJ mol-1)'
gnuplot> set ylabel font "Helvetica,14"
gnuplot> set xlabel 'Energy Minimisation Step'
gnuplot> set xlabel font "Helvetica,14"
gnuplot> set label '1AKI, Steepest Decent' at screen 0.47, 0.94 font "Arial,8"
gnuplot> replot
gnuplot> plot 'ep.xvg' u 1:2 title 'Potential' with lines linetype -1 lw 3
```


## Key Takeways for Data Analysis for NAMD Tutorial

**Physical time = num_steps * timestep  
e.g. 1000000 * 1 fs = 1000000 * 0.000001 ns = 1 ns**

review the progress you have made in the proline dipeptide simulations (MD and metadynamics) 

Please prepare a few figures to show what you have done: 
 - [x] monitoring the torsional angles used as CV in the MD and metadynamics to see the difference and the convergence and the free energy surface
 - [ ] if you have also done the #projection that would be also good
(I attach a couple of paper where the proline dipeptide has been studies)
 - [ ] #hydrogen bonds in the MD (eg proline and water) or #g(r), but these are also quantities that can be of interest.

 - [ ] monitor as a function of time the torsional angle that determines the orientation of the rotatable phenyl (see attachment), as this is what needs to be biased in metadynamics

The phenyl also swings and you can find another angle to describe that motion or other motions you can see if you visualize the trajectories (the other figures have some ideas; you do not have to monitor all but chose the most representative(s) according to the motion in the MD).

-> phenyl motions: rotation (torsional angle) & swing (__) 

http://jswails.wikidot.com/using-gnuplot



### Using VMD to plot: 

###### Load the molecule at once: 
`browse` → `prmtop file` → `load`  
`browse` → dcd file → `load`  
`Frames: Load all at once`   
`load`

###### To Make Molecule Representative:
`Graphics` → `Representations`  
(there will be a window jump out)  
`Drawing Method` → `CPK`

###### To make the system centred and monitor the motions: 
`Extensions` → `Analysis` → `RMSD Trajectory Tool`  
(there will be a window jump out)  
left box typing to change 'protein' to 'all'  
`ALIGH`  
close the window  
`▶️` : the animation will move

###### Plot the dihedral angles: 
`Mouse` → `Label` → `dihedrals 4`  
`Graphics` → `Representations` → `Selected Atoms` change 'all' to 'index 24 11 10 8' (-1 from plummed.dat) → `Create Rep` → `Drawing Method` → `VDW`  
`Graphics` → `Labels` → `Dihedrals` → click on the four dihedrals → `Graph` → `preview`/`save`


###### How to Identity the Cell Periodic Boundary Condition: 
`Extension` → `Tk Console` 
```
buffer line limit: 512   max line length: unlimited
Main console display active (Tcl8.6.11 / Tk8.6.11)
(yixinli) 1 % set all [atomselect top "all"]
atomselect0
(yixinli) 5 % puts "[measure minmax $all]"
{2.4590001106262207 2.4590001106262207 2.4590001106262207} {11.288999557495117 11.616000175476074 6.377999782562256}
#{x_min y_min z_min} {x_max y_max z_max}
(yixinli) 6 % puts "[measure center $all]"
6.8668389320373535 7.750032424926758 4.440515995025635

```
###### To Calculation on conf file: 
```
temperature         $temperature  
  set a 26.489 (= x_max - x_min * 3/4/... and > cutoff)    
  set b 27.47 (= y_max - y_min * 3/4/... and > cutoff)  
  set c 11.756 (= z_max - z_min * 3/4/... and > cutoff)  
cellBasisVector1 $a 0 0  
cellBasisVector2 0 $b 0  
cellBasisVector3 0 0 $c  
  cellOrigin 6.8668389320373535 7.750032424926758 4.440515995025635  
    
  cutoff              10.0
```

### Using VMD to manipulate molecules: 

###### To manipulate the dihedral angels 
`Extensions` → `Modeling` → `Molefracture`  
`File` → `New molecule` → `From selection` → type 'all' in the `selection` section → `Start Molefracture`   
`OpenGL Display` → tab 'shift' on keyboard to select index on OpenGL Display → `Dihedral Angles` → `Move` → `Angles`

Useful Link: https://www.ks.uiuc.edu/Training/Tutorials/vmd-molefacture/tutorial-Molefacture.pdf
