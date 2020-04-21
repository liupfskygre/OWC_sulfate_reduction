##Phylogenomic tree of Medium and high quality MAGs with sulfur cycling genes

#

#




#slurm scripts
```
#!/bin/bash
#SBATCH --nodes=1 #always =1 on zenith, usually will be =1 on summit
#SBATCH --ntasks=20 #number of cores you are requesting
#SBATCH --time=14-00:00:00 #max 14 days, HH:MM:SS if you need to include days do D-HH:MM:SS i.e.requesting 7 days is done as 7-00:00:00
#SBATCH --mem=128gb #How much memory you are requesting.  i.e. 500gb.  For 1Tb use 1024gb.  Notice this is lower case.
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=liupf@colostate.edu
#SBATCH --partition=wrighton-hi,wrighton-low #The options are: wrighton-hi,wrighton-low,wilkins-hi,wilkins-low,debug


#code block for running
#



#
##call sbatch test_slurm.sh

#view

#note on partition
##This specifies which user group/servers you are targeting.  There are these 4 partitions plus one called ‘debug’.
## Wilkins partitions have priority on marmot, Wrighton partitions have priority on tardigrade and zenith.



```
