# processSV_fMRS

processSV_fMRS

Program to process 7T SV MRS and 7T SV functional MRS data acquired w/ Siemens VB17 STEAM C2P sequence (https://www.cmrr.umn.edu/spect)

Requirements:

1) Data acquired with CMRR VB17 STEAM C2P sequence
2) LCModel (http://s-provencher.com/lcmodel.shtml) installed in default location.
3) Appropriate LCM basis set. We used steam7T_8ms_15Dec2016_Young.BASIS, which was graciously provided by Dr. Małgorzata Marjańska at CMRR.
“processSV_fMRS” looks for this basis set in ~/.lcmodel/basis-sets/7T/ location. If it is not found in this location, a pop-up window will prompt the user to select a basis set which can have a custom location and name. 

Recommended Packages: While not required, installation of parallel-gnu is recommended.
On Ubuntu:
>sudo apt-get install parallel
Linux Installation:

This was tested on Ubuntu installations on Windows 10/11 as well as standalone Ubuntu 22.04 LTS  but it should work on other Linux distributions (e.g. Centos, etc).
If files needed and Matlab Runtime are installed within a user’s home directory, no root/superuser permissions should be required. Local installation packages (which include the Matlab Runtime) are located at:
https://pitt-my.sharepoint.com/personal/cvsst5_pitt_edu/_layouts/15/onedrive.aspx?id=%2Fpersonal%2Fcvsst5%5Fpitt%5Fedu%2FDocuments%2FShared%20with%20Everyone%2FprocessSV%5FfMRS&ga=1

-Open a terminal and go (cd) to the directory which has the installation package
> ./SV_fMRS_LocalInstaller.install &
-Follow directions. The default installation directories require root permissions; on Ubuntu:
> sudo ./SV_fMRS_LocalInstaller.install

Add an alias to your .bashrc file (use your favorite editor or, as follows, replacing the path/username here with your installation directory):
> cd ~   								# switch to your home directory
> cp .bashrc .bashrc_org            					# make a copy of the .bashrc file
> echo “alias processSV_fMRS='/usr/UniversityOfPittsburgh/processSV_fMRS/application/run_processSV_fMRS.sh /usr/local/MATLAB/MATLAB_Runtime/R2022b'” >> .bashrc    # add alias to .bashrc file. Note1: For Ubuntu on Windows, the file to edit would be .bash_aliases

Note2: Use >>, to append the .bashrc file, rather than just >, which would create a new .bashrc file with just the alias line
-Open a new terminal window, to make use of your newly edited .bashrc file

Note3: Once the program is installed (including the Matlab Runtime needed to run it), to update it, one could just replace “processSV_fMRS” in /usr/UniversityOfPittsburgh/processSV_fMRS/application/ , with the most current one, found at github.com/FastMRSI

Help:

> processSV_fMRS -help

Usage for SV (single voxel) MRS:

> processSV_fMRS &

OR

> processSV_fMRS WaterSupressed_SV_MRS WaterReference_SV_MRS &

OR 

>  processSV_fMRS WaterSupressed_SV_MRS & (if no Water Reference scan was acquired)

OR

> processSV_fMRS WaterSupressed_SV_MRS WaterReference_SV_MRS ECCandWS &


Usage for SV functional MRS (fMRS):

> processSV_fMRS WaterSupressed_SV_MRS WaterReference_SV_MRS ECCandWS N_block&

OR

> processSV_fMRS WaterSupressed_SV_MRS WaterReference_SV_MRS ECCandWS N_block avgCHSfirst N_proc lcmBEFOREcomb savePicsTrueFalse &



1) WaterSupressed_SV_MRS is the water-supressed SV MRS twix file (relative path to cwd OK)
2) WaterReference_SV_MRS is the water reference SV MRS twix file; if not available/was not acquired, enter 0 here.
3) Eddie Current Correction (ECC) and Water Scaling (WS);  this is: 0 if (doecc=0 & dows=0), 1 if (doecc=1 & dows=0), 2 if (doecc=1 & dows=1)
4) N_block is the number of measurements averaged for each processed block. If -1 or no entry, N_block is the same as Navgs acquired
5) avgCHSfirst - enter 1 or 0 (default is 1), with N_block measurements data for each channel being averaged and LCModel processed first, before combining channels using a weighted average.
6) N_proc is the number of threads used for processing.
7) lcmBEFOREcomb can be either 1 (each channel data is processed with LCM before recombination) or 0. Default is 0 (less compute intensive).
8) savePics - default is 1. Set this to 0 if no X display available, to avoid generating pics.
