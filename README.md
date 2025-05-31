# Installing new packages
First make sure that you have the current version of *enivronment.yml* by syncing via the Source Control tab in VS Code. Then update your conda environment to the newest version by running
```
conda env update --file environment.yml --prune
```
in the terminal. After this you can install a new package by executing
```
conda activate data_analysis
conda install *thepackageyouwanttoadd*
```  
If the installation was successfull you can now update the *enivronment.yml* via
```
conda export --no-builds -f environment.yml
```
Then commit (stating the package(s) you added) and sync the updated *enivronment.yml*.

# Conda Setup
To create the conda environment run
```
conda env create -f environment.yml
```
or update it using
```
conda env update --file environment.yml --prune
```

To test if the environment was installed successfully use
 ```
 conda activate data_analysis
 conda list
 ```
# ILC-cells-Team4
This is a repository for the current team that the students will work on &amp; submit.


Dear Team4, 

you can use this ReadMe file to construct your project, summarise ideas, and present results. 

The project guideline and requirements were kindly summarised by Alexander [here](https://github.com/maiwen-ch/2025_Data_Analysis_Topic_02_Gene_Regulation_of_Immune_Cells). Please read through this repo very carefully and start gathering your own ideas!

You will have to submit a Jupyter Notebook with your code & plotting/results, so make sure we can find it in this repository at the end. 

Also, do not forget to clean up this ReadMe and edit it, so that any external member of the course could read & comprehend what you did in your project. 

Good luck! 