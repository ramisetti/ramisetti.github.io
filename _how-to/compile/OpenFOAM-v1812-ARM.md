---
layout: how2
title: Compile OpenFOAM v1812 with ARM
subtitle: On Isambard
group: compile
date: 26/08/2019
revised: 05/09/2019
---
In this article, I will present easy to follow intructions to build OpenFOAM v1812 with ARM and CRAY-MPICH compilers on Isambard, which is a Tier 2 HPC facility for UK-based researchers<sup>[1](#1)</sup>. Below are the instructions to build OpenFOAM-v1812 with ARM compiler on Isambard HPC.


1. Download OpenFOAM-v1812 and ThirdParty-v1812
  ```sh
     BUILD_DIR=$HOME/OpenFOAM
     mkdir $BUILD_DIR
     cd $BUILD_DIR
     wget https://sourceforge.net/projects/openfoamplus/files/v1812/OpenFOAM-v1812.tgz
     wget https://sourceforge.net/projects/openfoamplus/files/v1812/ThirdParty-v1812.tgz
     tar -xvf OpenFOAM-v1812.tgz -C $BUILD_DIR 
     tar -xvf ThirdParty-v1812.tgz -C $BUILD_DIR
  ```
  &#13;
2. Load ARM compilers<sup>[2](#2)</sup>
  ```sh
    # load ARM compiler if no compilers are loaded
    module load PrgEnv-allinea/6.0.5
    # or swap current cray compiler with arm compiler
    module switch PrgEnv-cray/6.0.5 PrgEnv-allinea/6.0.5
  ```
  &#13;
3. Make modifications to the configuration files required to build OF
  ```sh
    # Download the two patch files and apply the patch running the below commands
    wget https://dl.dropboxusercontent.com/s/jnpjop7kll5f3ph/OpenFOAM-v1812.patch
    wget https://dl.dropboxusercontent.com/s/q0iwu2qf7pv9s6n/ThirdParty-v1812.patch
    (cd $BUILD_DIR/OpenFOAM-v1812/; patch -p1 < $BUILD_DIR/OpenFOAM-v1812.patch)
    (cd $BUILD_DIR/ThirdParty-v1812/; patch -p1 < $BUILD_DIR/ThirdParty-v1812.patch)
    # create prefs.sh file with compiler options
    cd $BUILD_DIR/OpenFOAM-v1812
    echo -e "WM_COMPILER=Arm \nWM_MPLIB=CRAY-MPICH \nWM_LABEL_SIZE=64" > $BUILD_DIR/etc/prefs.sh
  ```
  &#13;
4. Compile OpenFOAM-v1812
  ```sh
    # source configuration file
    . etc/bashrc
    # This step is optional to check if everything is okay to build OpenFOAM
    foamSystemCheck
    # Compile openfoam using all available processors (-j) with reduced output (-s) and log the output (-l) to a file so that we can examine any compilation issues later.
    ./Allwmake -j -s -l
  ```
  &#13;
**References:**
<br>
<a id="1"></a>[1]: [https://gw4.ac.uk/isambard](https://gw4.ac.uk/isambard)
<br>
<a id="2"></a>[2]: [https://gitlab.com/arm-hpc/packages/wikis/packages/openfoam](https://gitlab.com/arm-hpc/packages/wikis/packages/openfoam)
<br>
<a id="3"></a>[3]: [https://openfoamwiki.net/index.php/Installation/Linux/OpenFOAM-5.x/CentOS_SL_RHEL](https://openfoamwiki.net/index.php/Installation/Linux/OpenFOAM-5.x/CentOS_SL_RHEL)
<br>
<a id="4"></a>[4]: [https://openfoamwiki.net/index.php/Installation/Linux/OpenFOAM-6.x/CentOS_SL_RHEL](https://openfoamwiki.net/index.php/Installation/Linux/OpenFOAM-6.x/CentOS_SL_RHEL)