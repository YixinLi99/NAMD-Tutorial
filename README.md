# NAMD-Tutorials

Simulations of Dipeptide with Minimisation and Equilibrum 

###### Step 1: Install NAMD & Dipeptide_e Files

###### Step 2: Runing NAMD 
```
1. cd ~/dipeptide_e
	#make sure you've got the two files in the dipeptide_e
2. /Users/yixinli/NAMD_2.14/namd2 md0.dipwat.conf > Out_md0.Dipwat.log
```
`(base) Yixins-MacBook-Pro-2:dipeptide_e yixinli$ /Users/yixinli/NAMD_2.14/namd2 md0.dipwat.conf > Out_md0.Dipwat.logyx`


新建一个terminal
`tail -f Out_md0.Dipwat.logyx`
就会实时更新进展


Task1:
1. Installed VMD 
2. Visualise dcd projectroy file 
3. Upload the dcd projector file 
4. Movie 

###### Instructions：
Visualize the md0 dcd trajectory file on VMD, by first uploading the prmtop file and then uploading the dcd on top of it.


Task2:
Md0 minimisation of 
Completed simulated 
Create md1 and simulate from md0
(Continuation of md0)

新建一个file：
touch file_name.conf

###### Instructions：
create a md1 conf file, to be a simulation that would 
restart after the end of md0. To do so, remember to remove the lines with 
CellBasisVector, CellOrigin, reinitvels, temperature $temperature, and minimize,
just like I instructed you to do in the original tutorial on NAMD.

Task 3:
How’s the evolution of energy/temperture

`(base) Yixins-MacBook-Pro-2:dipeptide_e yixinli$ cat Out_md0.Dipwat.logyx | grep -i energy > Out_md0.Dipwat.logyx.en`

`(base) Yixins-MacBook-Pro-2:dipeptide_e yixinli$ cat Out_md0.Dipwat.logyx | grep -i etitle > Out_md0.Dipwat.logyx.etitle`

###### Instructions：
Inspect the md0 log file, there should be lines starting with "ENERGY:" 
followed by many values, one of which is the temperature, another is the total energy,
and so on... To know which is which, refer to the lines starting with "ETITLE:". 
If you find a way to plot temperature 
versus time and total energy versus time 
(for example with a software like gnuplot) it would be great, 
otherwise just get familiar with them for now.

###### How to install Gnuplot?

```
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"

brew install gnuplot

(base) Yixins-MacBook-Pro-2:~ yixinli$ gnuplot
```
>> http://people.duke.edu/~hpgavin/gnuplot.html

Another recommendation: you should not start md1 
if you don't see the "End of Program" statement at the end of the log of md0.



## Simulation with Non-Periodic Boundary Conditions
