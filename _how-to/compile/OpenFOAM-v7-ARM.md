---
layout: how-to
title: Compile OpenFOAM v7 with ARM
subtitle: On Isambard
group: compile
date: 06/09/2019
#revised: 05/09/2019
---

Instructions to build OpenFOAM-v7 with ARM and CRAY-MPICH compiler on Isambard


1. Download OpenFOAM-7 and ThirdParty-7
   ```sh
     BUILD_DIR=$HOME/OpenFOAM
     mkdir $BUILD_DIR
     cd $BUILD_DIR
     git clone https://github.com/OpenFOAM/OpenFOAM-7.git
     git clone https://github.com/OpenFOAM/ThirdParty-7.git
   ```
   &#13;
2. Load ARM compiler<sup>[1](#1)</sup>
   ```sh
    # load ARM compiler if no compilers are loaded
    module load PrgEnv-allinea/6.0.5
    # or swap current cray compiler with arm compiler
    module switch PrgEnv-cray/6.0.5 PrgEnv-allinea/6.0.5
    # export compiler variables
    export CC=armclang
    export CXX=armclang++
    export FC=armflang
    export OMPI_CC=$CC
    export OMPI_CXX=$CXX
    export OMPI_FC=$FC
    # set other compiler variables 
    OF_COPT="-march=armv8.1-a -mtune=thunderx2t99 -O3 -ffp-contract=fast"
    OF_CXXOPT="$OF_COPT -std=c++11"
    
    # make modifications to the configuration files
    cd $BUILD_DIR/OpenFOAM-7
    # create necessary files with compiler options
    echo -e "WM_COMPILER=Clang \nWM_MPLIB=CRAY-MPICH \nWM_LABEL_SIZE=64" > $BUILD_DIR/OpenFOAM-7/etc/prefs.sh
    echo -e "include \$(GENERAL_RULES)/mplibMPICH" > wmake/rules/General/mplibCRAY-MPICH
    cp -r $BUILD_DIR/OpenFOAM-7/wmake/rules/linux64Clang $BUILD_DIR/OpenFOAM-7/wmake/rules/linuxAArch64Clang
   ```
   &#13;
3. Download and apply patch to the configuration files
   ```sh
    # Download the two patch files and apply the patch running the below commands
    wget https://dl.dropboxusercontent.com/s/gbxz7e33qpi73hm/OpenFOAM-7.patch -P $BUILD_DIR
    wget https://dl.dropboxusercontent.com/s/0t0euvwta6o4mos/ThirdParty-7.patch -P $BUILD_DIR
    (cd $BUILD_DIR/OpenFOAM-7/; patch -p1 < $BUILD_DIR/OpenFOAM-7.patch)
    (cd $BUILD_DIR/ThirdParty-7/; patch -p1 < $BUILD_DIR/ThirdParty-7.patch)
   ```
   &#13;
4. Compile OpenFOAM-7
   ```sh
    # source configuration file
    . etc/bashrc
    # Compile openfoam using all available processors (-j).
    ./Allwmake -j 64
   ```
   &#13;
**References:**
<br>
<a id="1"></a>[1]: [https://gitlab.com/arm-hpc/packages/wikis/packages/openfoam](https://gitlab.com/arm-hpc/packages/wikis/packages/openfoam)
<br>
<a id="3"></a>[2]: [https://openfoamwiki.net/index.php/Installation/Linux/OpenFOAM-7/CentOS_SL_RHEL](https://openfoamwiki.net/index.php/Installation/Linux/OpenFOAM-7/CentOS_SL_RHEL)
<br>
<a id="3"></a>[3]: [https://openfoamwiki.net/index.php/Installation/Linux/OpenFOAM-6.x/CentOS_SL_RHEL](https://openfoamwiki.net/index.php/Installation/Linux/OpenFOAM-6.x/CentOS_SL_RHEL)


