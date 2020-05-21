# Instructions to build OpenFOAM-v5.x using ARM compiler on Isambard

1. Download OpenFOAM-5.x and ThirdParty-v5.x
     BUILD_DIR=$HOME/OpenFOAM
     mkdir $BUILD_DIR
     cd $BUILD_DIR
     git clone https://github.com/OpenFOAM/OpenFOAM-5.x.git
     git clone https://github.com/OpenFOAM/ThirdParty-5.x.git
2. Load ARM compiler[1](https://gitlab.com/arm-hpc/packages/wikis/packages/openfoam) (To use GNU compiler, follow steps in section A)
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
    cd $BUILD_DIR/OpenFOAM-5.x
    # create necessary files with compiler options
    echo -e "WM_COMPILER=Clang \nWM_MPLIB=CRAY-MPICH \nWM_LABEL_SIZE=64" > $BUILD_DIR/OpenFOAM-5.x/etc/prefs.sh
    echo -e "include \$(GENERAL_RULES)/mplibMPICH" > wmake/rules/General/mplibCRAY-MPICH
    cp -r $BUILD_DIR/OpenFOAM-5.x/wmake/rules/linux64Clang $BUILD_DIR/OpenFOAM-5.x/wmake/rules/linuxAArch64Clang
4. Download and apply patch to the configuration files
    # Download the two patch files and apply the patch running the below commands
    wget https://dl.dropboxusercontent.com/s/64pas8yjb51tdlh/OpenFOAM-5x.patch -P $BUILD_DIR
    wget https://dl.dropboxusercontent.com/s/froe0u5updqb2n5/ThirdParty-5x.patch -P $BUILD_DIR
    (cd $BUILD_DIR/OpenFOAM-5.x/; patch -p1 < $BUILD_DIR/OpenFOAM-5x.patch)
    (cd $BUILD_DIR/ThirdParty-5.x/; patch -p1 < $BUILD_DIR/ThirdParty-5x.patch)
5. Compile OpenFOAM-5.x
    # source configuration file
    . etc/bashrc
    # This step is optional to check if everything is okay to build OpenFOAM
    foamSystemCheck
    # Compile openfoam using all available processors (-j).
    ./Allwmake -j 64

Section A: Build OpenFOAM-5.x using GNU compiler[2,3](https://gitlab.com/arm-hpc/packages/wikis/packages/openfoam) on ARM processors

    # load GNU compiler if no compilers are loaded
    module load PrgEnv-gnu/6.0.5
    # or swap current cray compiler with gnu compiler
    module switch PrgEnv-cray/6.0.5 PrgEnv-gnu/6.0.5
    # export compiler variables
    export CC=gcc
    export CXX=g++
    export FC=gfortran
    export OMPI_CC=$CC
    export OMPI_CXX=$CXX
    export OMPI_FC=$FC
    # set other compiler variables 
    OF_COPT="-O3 -march=native -mtune=native"
    OF_CXXOPT="$OF_COPT -std=c++11"
    
    # make modifications to the configuration files
    cd $BUILD_DIR/OpenFOAM-5.x
    echo -e "WM_MPLIB=CRAY-MPICH \nWM_LABEL_SIZE=64" > $BUILD_DIR/OpenFOAM-5.x/etc/prefs.sh
    echo -e "include \$(GENERAL_RULES)/mplibMPICH" > $BUILD_DIR/OpenFOAM-5.x/wmake/rules/General/mplibCRAY-MPICH
    
    # Go to steps 4 and 5
    

**References:**

1. https://gitlab.com/arm-hpc/packages/wikis/packages/openfoam
2. https://openfoamwiki.net/index.php/Installation/Linux/OpenFOAM-5.x/CentOS_SL_RHEL
3. https://openfoamwiki.net/index.php/Installation/Linux/OpenFOAM-6.x/CentOS_SL_RHEL

