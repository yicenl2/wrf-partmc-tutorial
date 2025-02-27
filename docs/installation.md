# Installation Guide

## WRF-PartMC Supported Versions

| Model    | Version                                     | Link  |
|----------|---------------------------------------------|-------|
| WRF      | 3.9.1.1 (release date: August 28, 2017)    | [Website](https://www2.mmm.ucar.edu/wrf/users/download/get_sources.html) |
| WRF-Chem | 3.9.1 (release date: August 17, 2017)      | [Website](https://www2.mmm.ucar.edu/wrf/users/download/get_sources.html) |
| WPS      | 3.9.1+      | [Website](https://github.com/wrf-model/WPS) |
| MCIP     | 4.3+      | [Website](https://github.com/USEPA/CMAQ) |
| I/O API     | 3.2      | [Website](https://www.cmascenter.org/ioapi/) |


## Contents
* [WRF-Chem](#wrf-chem-installation-guide)
* [WPS](#wps-installation-guide)
* [MCIP](#mcip-installation-guide)
* [WRF-PartMC](#wrf-partmc)

## WRF-Chem Installation Guide
<span style="font-size: 13px;">_<b>New to WRF?</b> If you're just getting started with WRF or WRF-Chem, follow these steps to install WRF-Chem and WPS to explore and familiarize yourself with WRF._</span>

**1.** Download the correct version of WRF.
```shell
wget https://www2.mmm.ucar.edu/wrf/src/WRFV3.9.1.1.TAR.gz
```

**2.** Unzip the file and move to the directory.
```shell
tar -zxvf WRFV3.9.1.1.TAR.gz
cd WRFV3
```

**3.** Set environmnet variable for NETCDF.
```shell
export NETCDF=/sw/netcdf4-4.7.4-gnu-9.3.0
```
✨ TIP: You can check the path by typing `nc-config --prefix`

**4.** Configure WRF.
```shell
./configure
```
* Compiler choice: 34
* Nesting option: 1

✅ CHECK: This will generate a `configure.wrf` file.

**5.** Edit the `configure.wrf` file by applying the following change:

Change:
`DM_CC = mpicc`

To:
`DM_CC = mpicc -DMPI2_SUPPORT`

**6.** Download the correct version of WRF-Chem.
```shell
# Move to one-level upper directory
cd ../
wget https://www2.mmm.ucar.edu/wrf/src/WRFV3-Chem-3.9.1.TAR.gz
```

**7.** Extract the chemistry code into WRF directory.
```shell
tar -zxvf WRFV3-Chem-3.9.1.TAR.gz -C ./WRFV3/
```

✅ CHECK: There should be a `chem` folder inside `WRFV3` directory.

**6.** Compile the code.
```shell
./compile em_real
```

✅ CHECK: If sucessful, this will create `real.exe` and `wrf.exe` in directory `main/`

<img src="/assets/img/wrfbuildsuccess.jpg" class="img-wrfflow" alt="">

## WPS Installation Guide
<span style="font-size: 13px;">_Make sure that the WRF model has been compiled <u>one directory level up</u> (i.e., `../`) in a directory named `WRF`, `WRFV3`, etc., or specify the full path to the compiled WRF model with the environment variable `$WRF_DIR`._</span>

**1.** Download the latest version of WPS and move to the directory.
```shell
git clone git@github.com:wrf-model/WPS.git
cd WPS
```

**2.** Set environmnet variable for NETCDF.
```shell
export NETCDF=/sw/netcdf4-4.7.4-gnu-9.3.0
```
✨ TIP: You can check the path by typing `nc-config --prefix`

**3.** Follow the two-step build mechanism.
```shell
./configure
./compile
```
* Compiler choice: 1

✅ CHECK: If sucessful, this will create three executables: `geogrid.exe`, `metgrid.exe`, and `ungrib.exe`.

## MCIP Installation Guide
### Prerequiste: I/O API
❗ IMPORTANT: Before proceeding, ensure you have I/O API 3.2 installed. You can find the detailed installation instruction [here](https://cjcoats.github.io/ioapi/AVAIL.html#build).

**1.** Download the source code:
<img src="/assets/img/ioapidownload.png" class="img-wrfflow" alt="" style="width: 100%;">

```shell
wget https://www.cmascenter.org/ioapi/download/ioapi-3.2.tar.gz
```

**2.** Extract the code and set up environment variables.
```shell
mkdir ioapi-3.2
tar -xvzf ioapi-3.2.tar.gz -C ioapi-3.2
cd ioapi-3.2
export NETCDF=/sw/netcdf4-4.7.4-gnu-9.3.0
```

**3.** Build `ioapi/Makefile`.

```shell
# Move to the directory that contains `Makeinclude.*` files.
cd ioapi
cp Makefile.nocpl Makefile
```
Edit the `Makefile` with the following changes:
* Set the base directory:
    ```shell
    BASEDIR = ~/ioapi-3.2 # path to your ioapi-3.2 directory
    ```
* Add `-fPIC` flag to `FFLAGS`:
    ```shell
    FFLAGS  = $(DEFINEFLAGS) $(FOPTFLAGS) $(OMPFLAGS) $(ARCHFLAGS) -I${IODIR} -fPIC
    ```
* Edit make-targets (`bins`, `binclean`, `bindirs`, `binrelink`) according to your choice of BINs.

    📝 NOTE: Select appropriate binary-type(s) BIN according to the file-extensions of the `ioapi/Makeinclude.*`.

    It is recommended that you choose both **optimized** and **debug** BINs. For example:
    * `Linux2_x86_64gfort`
    * `Linux2_x86_64gfortdbg`

<center><img src="/assets/img/ioapimakefile.png" class="img-wrfflow" alt="" style="width: 50%;"></center>

**4.** Similarly, for `m3tools/Makefile`.
```shell
cd ../m3tools
cp Makefile.nocpl Makefile
```
Edit the `Makefile`:
* Set the base directory:
    ```shell
    BASEDIR = ~/ioapi-3.2 # path to your ioapi-3.2 directory
    ```
* Add `-fPIC` flag to `FFLAGS`:
    ```shell
    FFLAGS = -I$(IODIR) ${MODI}$(OBJDIR) $(ARCHFLAGS) $(FOPTFLAGS) $(ARCHFLAGS) -fPIC
    ```
* Update `LIBS` by including path to NETCDF4 library:
    ```shell
    LIBS = -L${OBJDIR} -lioapi -L/sw/netcdf4-4.7.4-gnu-9.3.0/lib -lnetcdff -lnetcdf $(OMPLIBS) $(ARCHLIB) $(ARCHLIBS)
    ```
    ❗ IMPORTANT: must change this line to avoid 'netCDF4 library issues'.
* Edit make-targets `bins`, `binclean`, `bindirs`, `binrelink` according to your choice of BINs.


**4.** Compile `ioapi` and `m3tools`.

For *each* `BIN` type: 
```shell
cd ioapi
export BIN='Linux2_x86_64gfort'
make dir
make
cd ../m3tools
make
``` 

✅ CHECK: If sucessful, this will create directories with the same name as the `BIN` type, e.g., `Linux2_x86_64gfort` & `Linux2_x86_64gfortdbg` (make sure they are not empty).

### Build and compile MCIP
**1.** Grab the code from Github and move to the directory.
```shell
git clone git@github.com:USEPA/CMAQ.git
cd CMAQ/PREP/mcip/
```

**2.** Modify `Makefile` under `\src` directory.
Choose correct compiler: Uncomment contents under `#...gfortran` and change according to the location you installed I/O API from previous steps.

<img src="/assets/img/mcipmakefile1.jpg" class="img-wrfflow" alt="" style="width: 100%;">

Comment contents under `#...Intel Fortran`

<img src="/assets/img/mcipmakefile2.png" class="img-wrfflow" alt="" style="width: 80%;">

**3.** Compile the code under `\src` directory.
```shell
make
```

✅ CHECK: If sucessful, this will create the executable `mcip.exe`.

---
[⬆️ Back to Top](#wrf-partmc-supported-versions)

[⏪ Return to Home](readme.md)