# Install ABAQUS 6.14-5 on Ubuntu 16.04 64bit

## Preparative

1. Install C shell (CSH or TCSH)

       sodu apt-get update
       sudo apt-get install csh tcsh

2. Create the necessary folders

       cd ~
       mkdir ~/abaqus
       mkdir ~/abaqustemp
       mkdir ~/abaqusworks

3. Install dependent packages

       sudo apt-get install gnome-session-flashback
       sudo apt-get install libjpeg62
       sudo apt-get install libstdc++5


## Starting installation via .iso file
1. Mount the .iso file

       sudo mkdir /media/virtualCD
       sudo mount FILE_LOCATION.iso /media/virtualCD -t iso9660 -o loop

where `FILE_LOCATION` is the .iso file name complete of his location.

2. Run `setup` file in .iso and install

  * Run csh

        csh

  a csh shell will be opened in your terminal.

  * Run setup

    insert this command in the terminal:

        /media/virtualCD/setup

    insert the scratch directory when required:

        ~/abaqustemp

    and follow the graphical installation using defaults settings.

  * Exit the csh shell

         exit

## License server configuration

1.  Copy the license file in the License directory of Abaqus and rename it as `ABAQUS.lic`.

2.  Edit the `ABAQUS.lic` file

        gedit ~/abaqus/License/ABAQUS.lic

  In the text editor that will be opened, edit the `HOST_NAME` in fist row of the file.

3. Create a log file in the License directory

        gedit ~/abaqus/License/ABAQUS.log

4. Edit the .bashrc file in the home.

        sudo gedit ~/.bashrc

  in the text editor that will be opened, add the following rows at the top:

        #abaqus
        export LM_LICENSE_FILE=27011@PCNAME
        alias abalic=/home/ACCOUNTNAME/abaqus/License/lmgrd\ -c\ /home/ACCOUNTNAME/abaqus/License/ABAQUS.lic\ -l\ +/home/ACCOUNTNAME/abaqus/License/ABAQUS.log
        alias abaqus='XLIB_SKIP_ARGB_VISUALS=1 /home/ACCOUNTNAME/abaqus/Commands/abaqus'
        alias cae='abaqus cae -mesa'

  where ACCOUNTNAME is the name of your pc account and PCNAME is the name of your pc.

5. Reboot the computer.

## ABAQUS Product Installation

1. Launch the license server:

        abalic

2. following the graphical installation using defaults settings.

3. Starting ABAQUS Cae

       cd ~/abaqusworks
       cae

## Compliling subrountines with gfortran

1. Install gfortran package:

        sudo apt-get install gfortran

2. Find ABAQUS setting file `abaqus_v6.env` at `(Abaqus path)/6.14-5/SMA/site/` and make a backup.

3. Find and change the following parameters(`fortCmd`, `compile_fortran`, `link_sl`) to

        fortCmd = "gfortran"
        compile_fortran = (fortCmd + ' -c -fPIC -I%I')
        link_sl = (fortCmd +
	                 " -gcc-version=%i -fPIC -shared " +
	                 "%E -Wl,-soname,%U -o %U %F %A %L %B -Wl,-Bdynamic " +
	                 " -lifport -lifcoremt")

  Alternative:
  You can also replace your `abaqus_v6.env` file for the file in this repository, and modify the last line with your own license setting like `abaquslm_license_file="27011@PCNAME"`.

4. Run the command in terminal

        abaqus verify -user_std

  to see if everything is working.

## Usefull references
1. [Install ABAQUS on Ubuntu 10.04 64bit](https://sites.google.com/site/abaqus2010/help_0)

2. [Using gfortran compiler in Abaqus 6.14](http://www.eng-tips.com/viewthread.cfm?qid=381690)

3. [GUI fonts for Abaqus under Linux](https://polymerfem.com/showthread.php?4906-GUI-fonts-for-Abaqus-under-Linux)

4. [Installing Abaqus to run off the Central License Server
](http://kb.mit.edu/confluence/display/istcontrib/Installing+Abaqus+to+run+off+the+Central+License+Server)
